# Chapter 1: What is DDD? 

* *Domain* of software: the _business_ process being automated.
* You need to understand the domain of the software you're building in order to build useful software.
  * You won't get this alone, or by talking to people who aren't deeply specialized in the domain.
    * e.g. if you're building software to automate banking processes, you need to talk to people who deeply understand the processes, not just people on the periphery. 
* Somebody without knowledge of your software's domain should be able to read your code and learn a lot about the domain.
  * N.B.: if someone looks at your code and has no idea what's going on, or if your concepts are unclear, you may have a design problem.
* *Domain model*: product of gaining knowledge of the domain.
  * Not a particular diagram. But can be diagrams. 
  * Refer back to the domain model frequently, and revise it, while building the software.
  * What to include from the domain? What's useful for the abstraction we need to build? Keep it. What's not needed from the domain? Throw it out. 
* Need to _communicate the model_
  * Writing, diagrams, verbal.
* Code design follows from communicating the model. 
* A mistake modelling the domain is more costly than a mistake modelling the code -- the former can be caught and fixed early in the process, the latter is a dependency of the former and a revision costs more. 
* Waterfall approach to design: top down, step by step. 
  * No feedback from the bottom to top or top to bottom. 
* Agile approach
  * Recognizes that you can't see everything up front, there will be iteration
  * Solves analysis paralysis -- keep moving, no time to build the perfect design.
  * Downsides
    * Everyone has their own idea of what agile means -- how tight feedback loops should be, who should be involved, etc.
    * Continuous refactoring done by developers who don't have solid design principles can lead to code nobody can understand or change. 
    * Fear of over-engineering can lead to fear of _any_ design, which is bad.

## Building domain knowledge 

* Talk with domain experts about how they understand the domain. Ask questions. Drill down on concepts that come up over and over again. 
  * Not trying to invent code abstractions -- trying to understand the domain expert's own model of the domain. 
* After talking with domain experts, developers build a prototype. Questions arise. Back to the domain experts for feedback. Iterate on the prototype. And so on. 

