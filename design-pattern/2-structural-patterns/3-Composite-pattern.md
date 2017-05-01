[Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)
-------------

**Composite pattern** is a partitioning [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)). The composite pattern describes that a group of objects is to be treated in the same way as a single instance of an object.

The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies.

Implementing the composite pattern lets clients treat individual objects and compositions uniformly

In [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming), a composite is an object designed as a composition of one-or-more similar objects, all exhibiting similar functionality. This is known as a "[has-a](https://en.wikipedia.org/wiki/Has-a)" relationship between objects.

The key concept is that you can manipulate a single instance of the object just as you would manipulate a group of them.

For example, if defining a system to portray grouped shapes on a screen, it would be useful to define resizing a group of shapes to have the same effect (in some sense) as resizing a single shape.

Composite should be used when clients ignore the difference between compositions of objects and individual objects. If programmers find that they are using multiple objects in the same way, and often have nearly identical code to handle each of them, then composite is a good choice; it is less complex in this situation to treat primitives and composites as homogeneous.

>    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Composite_UML_class_diagram_%28fixed%29.svg/600px-Composite_UML_class_diagram_%28fixed%29.svg.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Composite_UML_class_diagram_%28fixed%29.svg/600px-Composite_UML_class_diagram_%28fixed%29.svg.png" width="578" height="301" />

**Component**
-   is the abstraction for all components, including composite ones
-   declares the interface for objects in the composition
-   (optional) defines an interface for accessing a component's parent in the recursive structure, and implements it if that's appropriate


**Leaf**
-   represents leaf objects in the composition
-   implements all Component methods


**Composite**
-   represents a composite Component (component having children)
-   implements methods to manipulate children
-   implements all Component methods, generally by delegating them to its children


```java
/** "Component" */
interface Graphic {

    //Prints the graphic.
    public void print();
}

/** "Composite" */
import java.util.List;
import java.util.ArrayList;
class CompositeGraphic implements Graphic {

    //Collection of child graphics.
    private List<Graphic> childGraphics = new ArrayList<Graphic>();

    //Prints the graphic.
    public void print() {
        for (Graphic graphic : childGraphics) {
            graphic.print();
        }
    }

    //Adds the graphic to the composition.
    public void add(Graphic graphic) {
        childGraphics.add(graphic);
    }

    //Removes the graphic from the composition.
    public void remove(Graphic graphic) {
        childGraphics.remove(graphic);
    }
}



/** "Leaf" */
class Ellipse implements Graphic {

    //Prints the graphic.
    public void print() {
        System.out.println("Ellipse");
    }
}

/** Client */
public class Program {

    public static void main(String[] args) {
        //Initialize four ellipses
        Ellipse ellipse1 = new Ellipse();
        Ellipse ellipse2 = new Ellipse();
        Ellipse ellipse3 = new Ellipse();
        Ellipse ellipse4 = new Ellipse();

        //Initialize three composite graphics
        CompositeGraphic graphic = new CompositeGraphic();
        CompositeGraphic graphic1 = new CompositeGraphic();
        CompositeGraphic graphic2 = new CompositeGraphic();

        //Composes the graphics
        graphic1.add(ellipse1);
        graphic1.add(ellipse2);
        graphic1.add(ellipse3);

        graphic2.add(ellipse4);

        graphic.add(graphic1);
        graphic.add(graphic2);

        //Prints the complete graphic (four times the string "Ellipse").
        graphic.print();
    }
}
```