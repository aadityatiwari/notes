[Prototype pattern](https://en.wikipedia.org/wiki/Prototype_pattern)
------------------

It is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.

This pattern is used to:
-   Avoid subclasses of an object creator in the client application, like the abstract factory pattern does.
-   Avoid the inherent cost of creating a new object in the standard way (e.g., using the 'new' keyword) when it is prohibitively expensive for a given application.

The rule of thumb could be that you would need to clone() an Object when you want to create another Object at runtime that is a true copy of the Object you are cloning.

True copy means all the attributes of the newly created Object should be the same as the Object you are cloning. If you could have instantiated the class by using new instead, you would get an Object with all attributes as their initial values.

For example, if you are designing a system for performing bank account transactions, then you would want to make a copy of the Object that holds your account information, perform transactions on it, and then replace the original Object with the modified one. In such cases, you would want to use clone() instead of new.

Designs that make heavy use of the [composite](https://en.wikipedia.org/wiki/Composite_pattern) and [decorator](https://en.wikipedia.org/wiki/Decorator_pattern) patterns often can benefit from Prototype as well.
