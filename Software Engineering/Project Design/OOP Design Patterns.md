# SOLID Principles
**Single Responsibility**: one class one responsibility
**Open/Closed**: a class should be open to extension, closed to modification
**Liscov Substitution**: subclass can substitute its superclass without introducing additional effects (e.g. exceptions), preconditions or postconditions
**Interface Segregation**: a class should not forced to implement methods that it does not need; interface segregation is the solution
**Dependency Inversion**: high-level modules should depend on abstractions rather than concrete implementations
# Design Pattern
## Creational ~: Object Creation
### Singleton & Multiton
The class can have only a specific amount of instances (Singleton: 1, Mutiton: 1+)
### Factory
### Builder
## Structural ~: Object Relationship
### Adaptor/Wrapper
### Decorator
## Behavioral ~: Object Communication
### Observer
The observed object upon event will notify its observers
### Iterator
### Memento
3 Classes
	Originator: main object
	Memento: store the state of originator, can only be accessed by originator
	Caretaker: 