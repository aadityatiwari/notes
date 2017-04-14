[Creational patterns](https://en.wikipedia.org/wiki/Creational_pattern)
------------

Creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

Creational design patterns are composed of two dominant ideas. One is encapsulating knowledge about which concrete classes the system uses. Another is hiding how instances of these concrete classes are created and combined.

Creational design patterns are further categorized into Object-creational patterns and Class-creational patterns.

Object-creational patterns defer part of its object creation to another object, while Class-creational patterns defer its object creation to subclasses.

The creational patterns aim to separate a system from how its objects are created, composed, and represented. They increase the system's flexibility in terms of the what, who, how, and when of object creation.

Consider applying creational patterns when:
-   A system should be independent of how its objects and products are created.
-   A set of related objects is designed to be used together.
-   Hiding the implementations of a class library or product, revealing only their interfaces.
-   Constructing different representation of independent complex objects.
-   A class wants its subclass to implement the object it creates.
-   The class instantiations are specified at run-time.
-   There must be a single instance and client can access this instance at all times.
-   Instance should be extensible without being modified.


	<img src="https://upload.wikimedia.org/wikipedia/en/thumb/f/f6/Creational_Pattern_Simple_Structure.png/220px-Creational_Pattern_Simple_Structure.png" alt="https://upload.wikimedia.org/wikipedia/en/thumb/f/f6/Creational_Pattern_Simple_Structure.png/220px-Creational_Pattern_Simple_Structure.png" width="219" height="273" align="centre"/>


**Creator** => Declares object interface. Returns object. 

**ConcreteCreator** => Implements object's interface.


### *List of creational design patterns:*


| **Name**                                                                                                             | **Description**                                                                                                                                                                                                                                                                                           
|----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| [Abstract factory](https://en.wikipedia.org/wiki/Abstract_factory_pattern)                                           | Provide an interface for creating *families* of related or dependent objects without specifying their concrete classes.                                                                                                                                                                                   
| [Builder](https://en.wikipedia.org/wiki/Builder_pattern)                                                             | Separate the construction of a complex object from its representation, allowing the same construction process to create various representations.                                                                                                                                                          
| [Factory method](https://en.wikipedia.org/wiki/Factory_method_pattern)                                               | Define an interface for creating a *single* object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses ([dependency injection](https://en.wikipedia.org/wiki/Dependency_injection)).                                                     
| [Lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization)                                             | Tactic of delaying the creation of an object, the calculation of a value, or some other expensive process until the first time it is needed. This pattern appears in the GoF catalog as "virtual proxy", an implementation strategy for the [Proxy](https://en.wikipedia.org/wiki/Proxy_pattern) pattern. 
| [Multiton](https://en.wikipedia.org/wiki/Multiton_pattern)                                                           | Ensure a class has only named instances, and provide a global point of access to them.                                                                                                                                                                                                                    
| [Object pool](https://en.wikipedia.org/wiki/Object_pool_pattern)                                                     | Avoid expensive acquisition and release of resources by recycling objects that are no longer in use. Can be considered a generalization of [connection pool](https://en.wikipedia.org/wiki/Connection_pool) and [thread pool](https://en.wikipedia.org/wiki/Thread_pool) patterns.                        
| [Prototype](https://en.wikipedia.org/wiki/Prototype_pattern)                                                         | Specify the kinds of objects to create using a prototypical instance, and create new objects from the 'skeleton' of an existing object, thus boosting performance and keeping memory footprints to a minimum.                                                                                             
| [Resource acquisition is initialization](https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization)(RAII) | Ensure that resources are properly released by tying them to the lifespan of suitable objects.                                                                                                                                                                                                            
| [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern)                                                         | Ensure a class has only one instance, and provide a global point of access to it.                                                                                                                                                                                                                         


