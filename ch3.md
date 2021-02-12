# Chapter 3: Model-Driven Design

* Use the domain model to drive the code.
  * How?
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


