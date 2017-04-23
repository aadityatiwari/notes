[Factory method pattern](https://en.wikipedia.org/wiki/Factory_method_pattern)
--------------

**Factory method pattern** is a [creational pattern](https://en.wikipedia.org/wiki/Creational_pattern) that uses factory methods to deal with the problem of [creating objects](https://en.wikipedia.org/wiki/Object_creation) without having to specify the exact class of the object that will be created. 

For further details, refer to notes [Factory (OOP)](../oop-factory.md)

```java
// Example of Abstract Factory and Factory design pattern  in Java
DocumentBuilderFactory abstractFactory = DocumentBuilderFactory.newInstance();
DocumentBuilder factory = abstractFactory.newDocumentBuilder();
Document doc = factory.parse(stocks)
```

In this example DocumentBuilderFactory (Abstract Factory) creates DocumentBuilder (Factory) which creates Documents (Products).

**AbstractFactory pattern uses composition** to delegate responsibility of creating object to another class while 
**Factory method design pattern uses inheritance** and relies on derived class or sub class to create object.

Factory should be an interface and clients first either creates factory or get factory which later used to create objects.
