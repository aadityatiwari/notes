[Bridge pattern](https://en.wikipedia.org/wiki/Bridge_pattern)
------------------

The bridge pattern is meant to "decouple an abstraction from its implementation so that the two can vary independently". The *bridge* uses [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_science)),[aggregation](https://en.wikipedia.org/wiki/Object_composition#Aggregation), and can use [inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to separate responsibilities into different [classes](https://en.wikipedia.org/wiki/Class_(computer_science)).

The class itself can be thought of as the *abstraction* and what the class can do as the *implementation*.

The **bridge pattern** is often confused with the adapter pattern. In fact, the **bridge pattern** is often implemented using the [**class adapter pattern**](https://en.wikipedia.org/wiki/Adapter_pattern#Class_Adapter_pattern)

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Bridge_UML_class_diagram.svg/750px-Bridge_UML_class_diagram.svg.png" alt="Bridge UML class diagram.svg" width="499" height="249" />

**Abstraction (abstract class)**
-   defines the abstract interface
-   Maintains the Implementor reference.

**RefinedAbstraction (normal class)**
-   extends the interface defined by Abstraction

**Implementor (interface)**
-   defines the interface for implementation classes

**ConcreteImplementor (normal class)**
-   implements the Implementor interface

Bridge classes are the Implementation that uses the same interface-oriented architecture to create objects.

Abstraction takes an object of the implementation phase and runs its method.

```java
/** "Implementor" */
interface DrawingAPI {
    public void drawCircle(final double x, final double y, final double radius);
}

/** "ConcreteImplementor"  1/2 */
class DrawingAPI1 implements DrawingAPI {
    public void drawCircle(final double x, final double y, final double radius) {
        System.out.printf("API1.circle at %f:%f radius %f\n", x, y, radius);
    }
}

/** "ConcreteImplementor" 2/2 */
class DrawingAPI2 implements DrawingAPI {
    public void drawCircle(final double x, final double y, final double radius) {
        System.out.printf("API2.circle at %f:%f radius %f\n", x, y, radius);
    }
}

/** "Abstraction" */
abstract class Shape {
    protected DrawingAPI drawingAPI;

    protected Shape(final DrawingAPI drawingAPI){
        this.drawingAPI = drawingAPI;
    }

    public abstract void draw();                                 // low-level
    public abstract void resizeByPercentage(final double pct);   // high-level
}



/** "Refined Abstraction" */
class CircleShape extends Shape {
    private double x, y, radius;
    public CircleShape(final double x, final double y, final double radius, final DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;  this.y = y;  this.radius = radius;
    }

    // low-level i.e. Implementation specific
    public void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }
    // high-level i.e. Abstraction specific
    public void resizeByPercentage(final double pct) {
        radius *= (1.0 + pct/100.0);
    }
}



/** "Client" */
class BridgePattern {
    public static void main(final String[] args) {
        Shape[] shapes = new Shape[] {
            new CircleShape(1, 2, 3, new DrawingAPI1()),
            new CircleShape(5, 7, 11, new DrawingAPI2())
        };

        for (Shape shape : shapes) {
            shape.resizeByPercentage(2.5);
            shape.draw();
        }
    }
}
```

Output:

```
API1.circle at 1.000000:2.000000 radius 3.075000
API2.circle at 5.000000:7.000000 radius 11.275000
```