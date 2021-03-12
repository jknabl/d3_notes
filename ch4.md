# Refactoring towards deeper insight

* Refactoring: 'redesign' code to make it better without changing behaviour.
  * Q: what is 'better'? 
* Technical vs. insight refactoring
  * Technical: make code easier to change, use design patterns, etc.
  * Insight: no patterns, can't be systematized
    * Expressive code, easy to read, easy to understand _what_ happens AND _why_. 
* Iterate on design in small steps, more clarity with each step, until you reach a Breakthrough
  * Breakthrough: Involves a "change in thinking" but otherwise somewhat foggily defined.
* Make implicit concepts explicit
  * Things we've buried in the model but that did not necessarily arise from conscious design
    * Once discovered, make them explicit -- include them in the model. 
  * Complex logic, conditionals, logic branching: good places to look for implicit concepts
* When discussing the domain, contradictions may arise -- one domain expert saying something another disagrees with, for example
  * Try to reconcile contradictions: they may be different ways of expressing the same thing. Find the commonalities.
* Dig out model concepts using domain literature. e.g., accounting books for accounting software.
* Useful concepts to make explicit
  * Constraint: an invariant. Object data is modified, but can't violate invariant constraints.
    * Its purpose is to explain the concept it is constraining. A limit on books per shelf describes the shelf.
  * Process: operation in steps. 
    * weakly explained...
  * Specification: "used to test an object to see if it satisfies a certain criteria"
    * Encapsulates more complex rules/constraints; e.g., can I create a credit given this customer? Check customer's credit history, account balance, pending credits, ...
 
