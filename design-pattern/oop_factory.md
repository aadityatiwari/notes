
[Factory (object-oriented programming)](https://en.wikipedia.org/wiki/Factory_(object-oriented_programming))
----------

A factory is an object for creating other objects.

In [class-based programming](https://en.wikipedia.org/wiki/Class-based_programming), a factory is an [abstraction](https://en.wikipedia.org/wiki/Abstraction_(computer_science)) of a [constructor](https://en.wikipedia.org/wiki/Constructor_(object-oriented_programming)) of a class, while in [prototype-based programming](https://en.wikipedia.org/wiki/Prototype-based_programming) a factory is an abstraction of a prototype object.

OOP provides polymorphism on object *use* by [method dispatch](https://en.wikipedia.org/wiki/Method_dispatch), formally [subtype polymorphism](https://en.wikipedia.org/wiki/Subtype_polymorphism) via [single dispatch](https://en.wikipedia.org/wiki/Single_dispatch) determined by the type of the object on which the method is called. However, this does not work for constructors, as constructors *create* an object of some type, rather than *use* an existing object. More concretely, when a constructor is called, there is no object yet on which to dispatch.

Using factories instead of constructors or prototypes allows one to use [polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) for object creation, not only object use. Also, using Factories provides [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming)), and means the code is not tied to specific classes or objects, and thus the class hierarchy or prototypes can be changed or [refactored](https://en.wikipedia.org/wiki/Refactored) without needing to change code that uses them.

**Dynamic dispatch** is the process of selecting which implementation of a [polymorphic](https://en.wikipedia.org/wiki/Subtyping) method to call at run time.

The decision of which version of a method to call may be based either on a single object, or on a combination of objects. The former is called *single dispatch* and is directly supported by common object-oriented languages such as [C++](https://en.wikipedia.org/wiki/C%2B%2B), [Java](https://en.wikipedia.org/wiki/Java_(programming_language)), [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Swift](https://en.wikipedia.org/wiki/Swift_(programming_language)), [JavaScript](https://en.wikipedia.org/wiki/JavaScript), and [Python](https://en.wikipedia.org/wiki/Python_(programming_language)).

Dynamic dispatch contrasts with *static dispatch*, in which the implementation of a polymorphic operation is selected at [compile-time](https://en.wikipedia.org/wiki/Compile-time).

Dynamic dispatch is different from [late binding](https://en.wikipedia.org/wiki/Late_binding). While dynamic dispatch does not imply late binding, late binding does imply dynamic dispatching since the binding is what determines the set of available dispatches.

Factory objects are used in situations where getting hold of an object of a particular kind is a more complex process than simply creating a new object, notably if complex allocation or initialization is desired.

Factories, specifically factory methods, are common in [toolkits](https://en.wikipedia.org/wiki/Toolkit) and [frameworks](https://en.wikipedia.org/wiki/Software_framework), where library code needs to create objects of types that may be sub-classed by applications using the framework.

Factories can be used when:
>   -   The creation of an object makes reuse impossible without significant duplication of code.
>   -   The creation of an object requires access to information or resources that should not be contained within the composing class.
>   -   The lifetime management of the generated objects must be centralized to ensure a consistent behavior within the application.
