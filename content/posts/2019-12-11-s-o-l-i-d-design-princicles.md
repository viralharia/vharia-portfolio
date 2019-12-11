---
template: post
title: S.O.L.I.D. Design Princicles
date: '2019-12-11 15:12'
category: Random
tags:
  - oops
---
Robert C. Martin defined five object-oriented design principles:

1. S - Single Responsibility Principle
2. O - Open-Closed Principle
3. L - Liskov’s Substitution Principle
4. I - Interface Segregation Principle, and
5. D - Dependency Inversion Principle

These together are popularly known as the **_SOLID principles._** 

When designing an object-oriented system, we should try to stick to these principles wherever possible.

These principles help us design a system that’s more **extensible, understandable and maintainable.**

## Single Responsibility Principle
* The Single-Responsibility Principle(SRP) states that every class must exactly do just one thing. OR there should not be more than one reason for us to modify a class.
* If a class has more than one responsibility, it becomes coupled. A change to one responsibility results to modification of the other responsibility.
* When designing our classes, we should aim to put related features together, so whenever they tend to change, they change for the same reason. And we should try to separate features if they will change for different reasons.
* The Single-Responsibility Principle provides us with the following benefits:   
  - Less coupling: Since every class would be doing just one thing, there’ll be far fewer dependencies.
  - Easier to test: the code will more likely be easier to test with far fewer test cases covering the system in entirety.

## Open-Closed Principle
* The Open-Closed Principle states that the software components (classes, functions, modules) must be open for extension but closed for modification.
* The intention here is to avoid introducing bugs in the system by breaking some existing working functionality due to code modifications. We should rather extend the existing class to support any additional functionality.
* This rule applies to the more stable classes of our system which have passed through the testing phases and is working well in production.
* Also, For large-systems, it’ll be not very straight-forward to identify for what all purposes a class might have been used. And so by only extending the functionality, we’re reducing chances of us dealing with the unknowns of the system.

## Liskov’s Substitution Principle
* The Liskov’s Substitution Principle says that a derived type must be able to completely substitute its base type without altering the existing behavior. So, if we have two classes A and B such that B extends A, we should be able to replace A with B in our entire code base without impacting the system’s behavior.
* At its heart LSP is about interfaces and contracts as well as how to decide when to extend a class vs. use another strategy such as composition to achieve your goal.
* A great example illustrating LSP is how sometimes something that sounds right in natural language doesn't quite work in code. In mathematics, a Square is a Rectangle. Indeed it is a specialization of a rectangle. The "is a" makes you want to model this with inheritance. However if in code you made Square derive from Rectangle, then a Square should be usable anywhere you expect a Rectangle. This makes for some strange behavior. Imagine you had `SetWidth` and `SetHeight` methods on your Rectangle base class; this seems perfectly logical. However if your Rectangle reference pointed to a Square, then `SetWidth` and `SetHeight` doesn't make sense because setting one would change the other to match it. In this case Square fails the Liskov Substitution Test with Rectangle and the abstraction of having Square inherit from Rectangle is a bad one.
* Well, a square clearly `IS a` type of rectangle in the real world. Whether we can model this in our code depends on the spec. **What the LSP indicates is that subtype behavior should match base type behavior as defined in the base type specification.** If the rectangle base type spec says that height and width can be set independently, then LSP says that square cannot be a subtype of rectangle. If the rectangle spec says that a rectangle is immutable, then a square can be a subtype of rectangle. **It's all about subtypes maintaining the behavior specified for the base type.**

## Interface Segregation Principle:
* As per the Interface Segregation Principle, the clients should not be forced to deal with the methods that they don’t use. We should split the larger interface into smaller ones, wherever needed.
* The Interface Segregation Principle(ISP) also reinforces other principles:
  - Single Responsibility Principle: Classes that implement smaller interfaces are usually more focused and usually have a single purpose
  - Liskov Substitution Principle: With smaller interfaces, there are more chances of us having classes implementing them to fully substitute the interface

## Dependency Inversion
* Dependency should be on Abstraction and not on concretions i.e coding to the interface.
* The Dependency Inversion Principle states that the high-level modules should not depend on low-level modules; both should depend on the abstractions.

