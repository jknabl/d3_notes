# Chapter 3: Model-Driven Design

[<-- Back](README.mdt)

* Use the domain model to drive the code.
* A gap can grow between the domain model and the code. Developers run into a domain concept they do not understand or that can't be expressed properly in code. Developers invent their own code models to cover these cases. The code grows farther and farther apart from the domain model.
  * THIS IS DOING IT WRONG
* Analysis model
  * Have a business analyst translate the domain model into an intermediary 'analysis model' -- this is NOT code. Then present the analysis model to devs, who code from the analysis model. 
    * Software design not taken into account so the model probably won't match with what needs to be coded. 
    * Flaws in the analysis model express themselves in unextensible or unexpressive code -- hard to change, hard to understand. 
* Instead of having analysts build a model in isolation, include devs. 
  * Similar to building a ubiquitous language: everyone has conversations, asks questions, builds the model. 
    * _Tight feeback loop_
* Object-oriented programming allows expressing domain concepts as entities and relating them to other entities. 
  * Procedural/functional languages don't offer constructs necessary for tightly mapping code to a real-world domain model.   
    * Objects can be simulated using data structures but there is still no coupling of behaviour with data, which is essential for mapping to a domain model. 

## Building blocks of a model-driven design

### Layered architecture

* Layer code by function. 
* Each layer loosely coupled to any other. 
* Stick to common architectural layers. For example: 
  * UI/presentation layer: interact with user
  * Application layer: co-ordinate application activity (_not_ business logic)
  * Domain layer: business logic
  * Infrastructure: DB, support libraries, messaging, etc. 
* Establish _rules of interaction_ for layers. They communicate only via well-defined interfaces. 

#### Entities

* Some objects seem to have an identity, which stays the same through e.g. the lifecycle of a request. 
* Not the _attributes_ that matter, but the _continuity and identity_ of the object. 
* Note: an object instance is _not_ the entity. An object can be instantiated and destroyed and instantiated again, but still represent the same entity.
* Think: what identifies one object of this type from another? Might be a clue to _identity_. 
* Can be: combo of attributes, unique ID, ...
* Entity class definition should be simple and _focused on life cycle continuity and identity_. 
  * Define means of distinguishing between entity
  * Define an operation that is guaranteed to produce a unique result for each entity
* Ask: does the object I'm modelling really _need_ to be an entity? 

#### Value objects

* Tracking identity has a cost. Needing to maintain object continuity can be expensive.
* When we're not interested in what the object _is_, but rather what attributes it _has_, that's a Value Object.
* Value objects can be easily created and discarded. No need to track them, since they don't have identity.
* Should be *immutable*. This simplifies design as it allows value objects to be _shared_. When you don't have to worry about an object changing state, you don't have to worry about conflicts when multiple other objects depend on them. 
* Golden rule: if value objects are _shareable_, they should be _immutable_. 
* Value objects can be nested within each other.
* Value objects can contain references to entities. 

#### Services

* Some 'verbs' in the domain model are behaviours that can't be mapped to entities or value objects.
* Since we're working in an object-oriented language, we still have to map non-entity behaviours to a code class. 
* Declare a service object for these kinds of behaviours.
* No state. 
* Groups functionality that serves entities and value objects.
* Encapsulates a concept (the behaviour) clearly separating it from other domain model concepts. 
* Often involves collaborations between many types of entities/VOs -- which is a good reason to not include these kinds of behaviours in entity/VO classes. 
* 3 characteristics: 
  1. The operation performed is a domain concept that doesn't naturally belong to an entity/VO. 
  2. The operation refers to other objects in the domain (has collaborators)
  3. No state. 
* Don't mix domain layer services with infrastructure layer services.

#### Modules

* Group related concepts into components. Don't cross component boundaries.
* Question: "group related concepts into modules, have the module provide an interface" -- fine, but in code, what does the interface look like? 
* Way of obtaining high cohesion
* 2 types of cohesion
  1. Communicational cohesion: when parts of a module operate on the same data. 
  2. Functional cohesion: all parts of a module work together to perform a well-defined task. 
* If an external module is constantly calling multiple objects within a module in a single request, maybe those objects are related and the module should provide an overarching interface for them. 

* 3 patterns for lifecycle management of objects: aggregates, factories, repositories.

#### Aggregates

* The biggest challenge of modelling is generally not to make it complete enough -- it's easy to stuff a bunch of logic in classes -- but to make it simple and understandable and flexible. 
* Most of the time it's good to eliminate or simplify relations between models. 
  * Unless they embed a fundamental domain concept.
* To simplify:
  1. If a relation can be eliminated, it should be.
  2. Multiplicity can be eliminated by adding a constraint -- can you formulate a constraint such that only one object can satisfy a relationship rather than many? 
  3. Bidirectional relationships can often be simplified. e.g., car has engine, engine has car --> car has engine, engine does not necessarily have car. 

* There are cases where it's hard to simplify any futrher. 
  * Example: banking system, lots of collaborators, needs to be transactioal -- poor relational design can lead to DB contention. 
* Also need to enforce data invariants -- when data changes on A, this might affect the integrity of B, C, D, ..., so they need to be checked. 
* In these cases, *define an aggregate*. 

* Aggregate: group of associated objects that are considered a single unit w/r/t data changes. 
* Each aggregate has a root. The root is an entity. It is the _only_ object accessible from the outside. 
  * Root can hold references to all the other objects in the aggregate. 
  * Outside object can only hold reference to the root. 
* Implication: external objects can't directly change members of the aggregate. The root entity co-ordinates all changes. 
  * The root can encapsulate enforcement of invariants, relationships, etc. 
* n.b. compared to e.g., every ActiveRecord model having its own validations, associations, etc. -- a way to centralize that mess. 

* If objects of an aggregate are stored in DB, they should _not be accessible by queries_ -- they should only be accessible by traversing the root entity. 
* Root entity has _global_ identity; aggregate members have _local_ identity. 

#### Factories
 
* Trying to construct a complex aggregate in a root entity's constructor goes against how constructing things actually works in the real world. An Xbox wouldn't attempt to construct itself. 
* **Factory**: Encapsulates the process of complex object creation. 
  * Especially useful for creating aggregates. 
* Creation must be atomic -- can't leave half-constructed stuff. 
* When the root is created, we need to create all associated objects subject to _invariants_ -- or else we can't enforce the invariants. 
* Clients of factories should never reference concrete classes of objects being instantiated. 
* Don't use a factory when: 
  1. Construction is not complicated.
  2. Not creating multiple collaborating objects.
  3. The client cares about the implemention, e.g. different construction strategies. 
  4. The class _is_ the type -- no need to choose between concrete implementations. 

#### Repositories

* Problem: To use an object, we need to obtain a reference to it. How do we get a reference to an aggregate root?
  * If the object we need is an entity, it's probably in the DB. If it's a value object, it's probably obtainable by traversing the root of an entity. 
    * Problem: DB is infrastructure. We shouldn't access it from non-infrastructure code layers. Accessing it from other layers makes it very hard to swap out or change, and pollutes the other layers with infra code that those layers shouldn't be concerned with. 
* Solution: use a **repository**: encapsulation of the logic needed to obtain references to objects. 
* Repository can store references to objects in-memory -- e.g. some value objects, some entities. 
* Repository can provide interface for accessing other objects, e.g. from the DB. 
* Repository fits nicely with strategy pattern -- can abstract different persistence strategies and isolate them from client domain model code. 
* For each type of object that needs global access: 
  * Create an object that gives illusion of being an in-memory collection of all objects of that type. 
  * Set up object access via a well-known interface. 
  * Getters and setters
  * Methods that select objects based on given criteria
* Don't mix repository and factory. Factory creates, repository operates on existing. 
  * Factories are 'pure domain', repositories interact with infrastructure layer. 
