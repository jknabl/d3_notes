# Chapter 2: The Ubiquitous Language

[<-- Back](README.md)

* There is usually a communication barrier between developers and domain experts. Developers are thinking in terms of technical stuff, classes and code. Domain experts are thinking in terms of their own mental models. The two parties need a common language. 
* "Translating" one type of specialized knowledge to another does not help.
  * e.g., "translating" what you mean by some design pattern you mentioned to a domain expert isn't going to help things, it's going to confuse them. 
    * Need to speak the SAME language, not translate from one to another.
* What is the common language?
  * It's based on the domain model. 
  * Explicitly request that the team only use domain model concepts when communicating.
    * If you run into a situation where you can't communicate something, you've identified a gap in the model that needs to be filled in. 
  * Model and language are dependent on each other. Change one, change the other. 

## Creating the ubiquitous language

* When starting to build a model, everyone should explicitly be aware that, as a team, we're building a model.
  * Need to stay focused on that. It's OK to point out when a discussion has gone off track, or when people have reverted to using specialized jargon. 
* Code should express the model.
* How to express the model? 
  * Diagrams
    * When a project gets big, it's hard to fully express a model in diagrams. Too many diagrams required.
    * Diagrams are good at expressing entities and attributes and relationships, behaviour not so much. 
  * Documents
    * For expressing the _meaning_ of domain model concepts, and _behaviour_.
* Summary of how to express: diagrams for entities/relationships, writing for meaining/behaviour.
* Long documents tend to go out of date. Documents should be short enough that they can be kept up to date as the domain model changes. 
* Mix diagrams and documents: e.g. reference a document from within a diagram when you need to explain behaviour or expand on a concept. 
* Code communicates domain model: name things expressively of the domain model. 

