# Chapter 5: Preserving Model Integrity

[<-- Back](README.md)

* First requirement of a model: it must be consistent.
  * No contradictions
  * No variable terms
* "unified model": one that is consistent.
* Maintaining a giant unified model for an entire large corporation is not feasible
* Divide into several small unified models, each integrating with others.

## Bounded Context

* A model should be small enough to assign to one team
  * Q opportunity: do we push this in some ways in our codebases?
* "Context" of a model: set of conditions that need to be applied to make sure that the terms used in the model have meaning.
* Bounded context is not a module
  * Bounded context is the logical frame that groups model concepts
    * Can be multiple modules in a bounded context
* What's the price for multiple models?
  * Need to define+enforce borders
    * Q: how?
  * Boundaries inherently introduce friction -- can't just use a concept from some other model, need to use it based on the interface and rules of its own bounded context.
  
> For example, we want to create an e-commerce application used to sell stuff on the Internet. This application allows the customers to register, and we collect their personal data, including credit card numbers. The data is kept in a relational database. The customers are allowed to log in, browse the site looking for merchandise, and place orders. The application will need to publish an event whenever an order has been placed, because somebody will have to mail the requested item. We also want to build a reporting interface used to create reports, so we can monitor the status of available goods, what the customers are interested in buying, what they don’t like, etc. In the beginning we start with one model which covers the entire domain of e- commerce. We are tempted to do so, because after all we have been requested to create one big application. But if we consider the task at hand more carefully, we discover that the e-shop application is not really related to the reporting one. They have separate concerns, they operate with different concepts, and they may even need to use different technologies. The only thing really common is that the customer and merchandise data is kept in the database, and both applications access it.

> A messaging system is needed to inform the warehouse personnel about the orders placed, so they can mail the purchased merchandise. The mail personnel will use an application which gives them detailed information about the item purchased, the quantity, the customer address, and the delivery requirements. There is no need to have the e-shop model cover both domains of activity. It is much simpler for the e-shop application to send Value Objects containing purchase information to the warehouse using asynchronous messaging. There are definitely two models which can be developed separately, and we just need to make sure that the interface between them works well.

* Bounded context has a name that is part of the Ubiquitous Language

## Continuous integration

* A _defined process_ for integrating model changes. 
* Why process? Because changes potentially introduce inconsistencies into the model. Need to make sure they don't.
* Merge + build + test

## Context map

* A diagram that illustrates the relationships between different bounded contexts 
* Gives high-level picture to people who don't necessarily work with a given model day-to-day
* Shows the _integration_ of multiple models (i.e., interfaces, contract) 

## Shared kernel

* Subset of the domain model that teams agree to share. 
  * Any change requires _all teams_ to approve the change. 
* Seems geared towards small teams that don't have the desire to implement CI

## Customer-Supplier

* Two subsystems are closely coupled, one depending on the other. Inputs of one subsystem are the outputs of another.
  * Not possible/logical to implement as shared kernel. 
* e.g. e-Commerce application: e-Com subsystem vs. reporting subsystem 
  * reporting depends on e-Com, e-Com doesn't care at all about reporting. Reporting is the customer, e-Com is the supplier
    * Supplier may need to implement some specifciations needed by the customer system. 
  * Shared database: how to manage?
    * Supplier system assumes control of the DB. Meets with Customer to meet their specifications. When Supplier makes a change, it does so in consultation with the Customer. 
* Interface between subsystems needs to be _precisely_ defined. 

## Conformist 

* In Customer-Supplier relationship, Customer tends to suffer -- supplier marches on making its own changes, due to dealine pressures etc., and customer has to adapt. 
* Conformist: customer simply pulls in the supplier's model and uses it themselves. 
  * e.g., a gem for a supplier? 

## Anticorruption layer


