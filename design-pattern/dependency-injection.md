[Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
---------

**Dependency injection** is a design principle where [objects](https://en.wikipedia.org/wiki/Object_(computer_science)) are not responsible for constructing the other objects on which they depend. Instead, external code must supply the needed dependencies when instantiating an object. This is an example of [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control).

Often a dependency injection framework (or "container") is used to manage and automate the construction and lifetimes of interdependent objects.

A dependency is an object that can be used (a [service](https://en.wikipedia.org/wiki/Service_(systems_architecture))). 

An injection is the passing of a dependency to a dependent object (a [client](https://en.wikipedia.org/wiki/Client_(computing))) that would use it. The service is made part of the client's [state](https://en.wikipedia.org/wiki/State_(computer_science)). 

Passing the service to the client, rather than allowing a client to build or [find the service](https://en.wikipedia.org/wiki/Service_locator_pattern), is the fundamental requirement of the pattern.

This fundamental requirement means that using values (services) produced within the class from [new](https://en.wikipedia.org/wiki/New_and_delete_(C%2B%2B)) or [static methods](https://en.wikipedia.org/wiki/Static_methods) is prohibited. The class should accept values passed in from outside.

Dependency injection allows a program design to follow the [dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle).

**Dependency inversion principle (DIP) states:**
-   High-level modules should not depend on low-level modules. Both should depend on [abstractions](https://en.wikipedia.org/wiki/Abstraction_(computer_science)).
-   Abstractions should not depend on details. Details should depend on abstractions.

The client delegates the responsibility of providing its dependencies to external code (the injector). The client is not allowed to call the injector code. It is the injecting code that constructs the services and calls the client to inject them. 

The client only needs to know about the intrinsic interfaces of the services because these define how the client may use the services. This separates the responsibilities of use and construction.

There are three common means for a client to accept a dependency injection: [setter](https://en.wikipedia.org/wiki/Mutator_method)-, [interface](https://en.wikipedia.org/wiki/Interface_(object-oriented_programming))- and [constructor](https://en.wikipedia.org/wiki/Constructor_(object-oriented_programming))-based injection.

>-  ***constructor injection***: the dependencies are provided through a class constructor.
>-  ***setter injection***: the client exposes a setter method that the injector uses to inject the dependency.
>-  ***interface injection**:* the dependency provides an injector method that will inject the dependency into any client passed to it. Clients must implement an interface that exposes a [setter method](https://en.wikipedia.org/wiki/Setter_method) that accepts the dependency.

Setter and constructor injection differ mainly by when they can be used. Interface injection differs in that the dependency is given a chance to control its own injection.

Dependency injection involves four roles:
>-   the **service** object(s) to be used
>-   the **client** object that is depending on the services it uses
>-   the [**interfaces**](https://en.wikipedia.org/wiki/Interface_(object-oriented_programming)) that define how the client may use the services
>-   the **injector**, which is responsible for constructing the services and injecting them into the client

The injector may be referred to by other names such as: assembler, provider, container, factory, builder, spring, construction code, or main.
