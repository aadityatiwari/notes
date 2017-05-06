[Visitor Pattern](https://en.wikipedia.org/wiki/Visitor_pattern)
----------------

The **visitor** [design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) is a way of separating an [algorithm](https://en.wikipedia.org/wiki/Algorithm) from an object structure on which it operates.

A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the [open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle).

In essence, the visitor allows one to add new [virtual functions](https://en.wikipedia.org/wiki/Virtual_function) to a family of classes without modifying the classes themselves;

Instead, one creates a visitor class that implements all of the appropriate specializations of the virtual function. The visitor takes the instance reference as input, and implements the goal through [double dispatch](https://en.wikipedia.org/wiki/Double_dispatch).

Visitor represents an operation to be performed on elements of an object structure.

Visitor lets you define a new operation without changing the classes of the elements on which it operates.

The nature of Visitor makes it an ideal pattern to plug into public APIs thus allowing its clients to perform operations on a class using a "visiting" class without having to modify the source.

The Visitor pattern encodes a logical operation on the whole hierarchy into a single class containing one method per type.

For example, iterating over a directory structure could be implemented with a visitor pattern. This would allow you to create file searches, file backups, directory removal, etc. by implementing a visitor for each function while reusing the iteration code.

> <img src="https://upload.wikimedia.org/wikipedia/en/thumb/e/eb/Visitor_design_pattern.svg/500px-Visitor_design_pattern.svg.png" alt="https://upload.wikimedia.org/wikipedia/en/thumb/e/eb/Visitor_design_pattern.svg/500px-Visitor_design_pattern.svg.png" width="607" height="366" />

The visitor pattern requires a programming language that supports [single dispatch](https://en.wikipedia.org/wiki/Single_dispatch). 

Consider two objects, each of some class type; one is called the "element", and the other is called the "visitor". An element has an accept method that can take the visitor as an argument. The accept method calls a visit method of the visitor; the element passes itself as an argument to the visit method.

When the accept method is called in the program, its implementation is chosen based on both:
-   The dynamic type of the element.
-   The static type of the visitor.

When the associated visit method is called, its implementation is chosen based on both:
-   The dynamic type of the visitor.
-   The static type of the element as known from within the implementation of the accept method, which is the same as the dynamic type of the element. (As a bonus, if the visitor can't handle an argument of the given element's type, then the compiler will catch the error.)

Consequently, the implementation of the visit method is chosen based on both:
-   The dynamic type of the element.
-   The dynamic type of the visitor.

This effectively implements [double dispatch](https://en.wikipedia.org/wiki/Double_dispatch)

In this way, a single algorithm can be written for traversing a graph of elements, and many different kinds of operations can be performed during that traversal by supplying different kinds of visitors to interact with the elements based on the dynamic types of both the elements and the visitors.

Following example shows how the contents of a tree of nodes (in this case describing the components of a car) can be printed.

Instead of creating "print" methods for each node subclass (Wheel, Engine, Body, and Car), a single visitor class (CarElementPrintVisitor) performs the required printing action.

Because different node subclasses require slightly different actions to print properly,  CarElementPrintVisitor dispatches actions based on the class of the argument passed to its visit method. 

> <img src="https://upload.wikimedia.org/wikipedia/en/d/d9/UML_diagram_of_an_example_of_the_Visitor_design_pattern.png" alt="UML diagram of the Visitor pattern example with Car Elements" width="595" height="239" />


*Example*
----------

```java
interface CarElement {
    void accept(CarElementVisitor visitor);
}

interface CarElementVisitor {
    void visit(Body body);
    void visit(Car car);
    void visit(Engine engine);
    void visit(Wheel wheel);
}

class Body implements CarElement {
    public void accept(final CarElementVisitor visitor) {
        visitor.visit(this);
    }
}

class Car implements CarElement {
    CarElement[] elements;

    public Car() {
        this.elements = new CarElement[] { 
            new Wheel("front left"), new Wheel("front right"),
            new Wheel("back left"), new Wheel("back right"),
            new Body(), new Engine()
        };
    }

    public void accept(final CarElementVisitor visitor) {
        for (CarElement elem : elements) {
            elem.accept(visitor);
        }
        visitor.visit(this);
    }
}

class Engine implements CarElement {
    public void accept(final CarElementVisitor visitor) {
        visitor.visit(this);
    }
}

class Wheel implements CarElement {
    private String name;

    public Wheel(final String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void accept(final CarElementVisitor visitor) {
        /*
         * accept(CarElementVisitor) in Wheel implements
         * accept(CarElementVisitor) in CarElement, so the call
         * to accept is bound at run time. This can be considered
         * the *first* dispatch. However, the decision to call
         * visit(Wheel) (as opposed to visit(Engine) etc.) can be
         * made during compile time since 'this' is known at compile
         * time to be a Wheel. Moreover, each implementation of
         * CarElementVisitor implements the visit(Wheel), which is
         * another decision that is made at run time. This can be
         * considered the *second* dispatch.
         */
        visitor.visit(this);
    }
}

class CarElementDoVisitor implements CarElementVisitor {
    public void visit(final Body body) {
        System.out.println("Moving my body");
    }

    public void visit(final Car car) {
        System.out.println("Starting my car");
    }

    public void visit(final Wheel wheel) {
        System.out.println("Kicking my " + wheel.getName() + " wheel");
    }

    public void visit(final Engine engine) {
        System.out.println("Starting my engine");
    }
}

class CarElementPrintVisitor implements CarElementVisitor {
    public void visit(final Body body) {
        System.out.println("Visiting body");
    }

    public void visit(final Car car) {
        System.out.println("Visiting car");
    }

    public void visit(final Engine engine) {
        System.out.println("Visiting engine");
    }

    public void visit(final Wheel wheel) {
        System.out.println("Visiting " + wheel.getName() + " wheel");
    }
}

public class VisitorDemo {
    public static void main(final String[] args) {
        final Car car = new Car();
		
        car.accept(new CarElementPrintVisitor());
        car.accept(new CarElementDoVisitor());
    }
}
```
