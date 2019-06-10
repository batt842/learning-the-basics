# OOP SOLID Principles

Even though some people say that Imperative Programming is more flexible and more readable, it is not true if OOP-style coding is done correctly. OOP could be more flexible and more readable. Furthermore, it is way more extensible.

## SRP
: Single Responsibility Principle

* There should never be more than one reason for a class to change. (Robert C. Martin)
* A class should only have a single responsibility, that is, only one changes to one part of the software's specification should be able to affect the specification of the class.
* A class should have one and only one reason to change, meaning that a class should have only one job.

### How
* Divergent Change
  * Extract a class a superclass
* Shotgun Surgery
  * Move field or method
  * Releated to AoP (cross-cutting)

### Effect
* Less coupling and more coherence
* Free from collateral changes
* Better readability and maintainability

## OCP
: Open/Closed Principle

* Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification (Bertrand Meyer)
* A module should be open for extensions but closed for modification. (Robert C. Martin)

### How
* Differentiate what is extensible or not
* Abstraction and polymorphism are keys

### Effect
* More extensibility, maintainability, and reusability

## LSP
: Liskov Substitution Principle

* Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T. (Barbara Liskov)
* Subclasses should be substitutable for their base classes. (Robert C. Martin)

Liskov Substitution Principle is about a requirement specification. The following are common violations.
* Returning a value which is out of the specification
* Throwing an exception which is out of the specification
* Executing a functionality which is out of the specification

### How
* Write one class for two objects behaving the same
* Write one interface and two classes for two objects behaving similarly
* Write two classes for two objects behaving differently
* Use inheretance to add some behavior to an existing object

### Effect
* More compliant and robustness for inheritance and polymorphism

## ISP
: Interface Segregation Principle

* Many client-specific interfaces are better than one general-purpose interface. (Robert C.Martin) 
* A client should never be focused to implement an interface that it doesn't use or clients shouldn't be forced to depend on they do not use.

This principle is related to the Adapter Pattern. It also allows multiple responsibilities for a single interface or class in contrast to SRP.

### How
* Separation through Multiple Inheritance (class interface)
* Separation through Delegation (object interface)

### Effect
* Similar to SRP, less coupling and more coherence

## DIP
: Dependency Inversion Principle

* High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).<br> Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions. (Robert C. Martin)
* Depend upon Abstractions. Do not depend upon concretion. (Robert C. Martin)
* Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions.

### How
* Layering
  * "...all well structured object-oriented architectures have clearly-defined layers, with each layer providing some coherent set of services though a well-defined and controlled interface." (Grady Booch)

### Effect
* More extensibility, maintainability, and reusability

## References
* http://www.nextree.co.kr/p6960/
* https://web.archive.org/web/20150202200348/http://* www.objectmentor.com/resources/articles/srp.pdf
* https://web.archive.org/web/20150905081105/http://* www.objectmentor.com/resources/articles/ocp.pdf
* https://web.archive.org/web/20150905081111/http://* www.objectmentor.com/resources/articles/lsp.pdf
* https://web.archive.org/web/20150905081110/http://* www.objectmentor.com/resources/articles/isp.pdf
* https://web.archive.org/web/20150905081103/http://* www.objectmentor.com/resources/articles/dip.pdf
