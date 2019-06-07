# OOP SOLID Principles

## SRP
: Single Responsibility Principle

There should never be more than one reason for a class to change. (Robert C. Martin) </br>
= A class should only have a single responsibility, that is, only one changes to one part of the software's specification should be able to affect the specification of the class. </br>
= A class should have one and only one reason to change, meaning that a class should have only one job.

## OCP
: Open/Closed Principle

A module should be open for extensions but closed for modification. (Robert C. Martin)

## LSP
: Liskov Substitution Principle

Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T. (Barbara Liskov) </br>
= Subclasses should be substitutable for their base classes. (Robert C. Martin)

Liskov Substitution Principle is about a requirement specification. The following are common violations.

* Returning a value which is out of the specification.
* Throwing an exception which is out of the specification.
* Executing a functionality which is out of the specification.

## ISP
: Interface Segregation Principle

Many client-specific interfaces are better than one general-purpose interface. (Robert C.Martin) </br>
= A client should never be focused to implement an interface that it doesn't use or clients shouldn't be forced to depend on they do not use.

## DIP
: Dependency Inversion Principle

Depend upon Abstractions. Do not depend upon concretion. (Robert C. Martin)

Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions.


## References
https://web.archive.org/web/20150202200348/http://www.objectmentor.com/resources/articles/srp.pdf

https://web.archive.org/web/20150905081105/http://www.objectmentor.com/resources/articles/ocp.pdf

https://web.archive.org/web/20150905081111/http://www.objectmentor.com/resources/articles/lsp.pdf

https://web.archive.org/web/20150905081110/http://www.objectmentor.com/resources/articles/isp.pdf

https://web.archive.org/web/20150905081103/http://www.objectmentor.com/resources/articles/dip.pdf
