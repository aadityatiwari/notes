[Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern)
----------------

**Decorator pattern** (also known as **Wrapper**, an alternative naming shared with the **Adapter pattern**) is a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class.

The decorator pattern is often useful for adhering to the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), as it allows functionality to be divided between classes with unique areas of concern

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/400px-Decorator_UML_class_diagram.svg.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/400px-Decorator_UML_class_diagram.svg.png" width="400" height="317" />

This pattern is designed so that multiple decorators can be stacked on top of each other, each time adding a new functionality to the overridden method(s).

Decorators and the original class object share a common set of features.

The decoration features (e.g., methods, properties, or other members) are usually defined by an interface, [mixin](https://en.wikipedia.org/wiki/Mixin) (a.k.a. [trait](https://en.wikipedia.org/wiki/Trait_(computer_programming))) or class inheritance which is shared by the decorators and the decorated object.

The decorator pattern is an alternative to [subclassing](https://en.wikipedia.org/wiki/Subclass_(computer_science)). Sub-classing adds behavior at [compile time](https://en.wikipedia.org/wiki/Compile_time), and the change affects all instances of the original class; decorating can provide new behavior at [run-time](https://en.wikipedia.org/wiki/Run_time_(program_lifecycle_phase))  for selective objects.

This difference becomes most important when there are several *independent* ways of extending functionality. In some [object-oriented programming languages](https://en.wikipedia.org/wiki/Object-oriented_programming_language), classes cannot be created at runtime, and it is typically not possible to predict, at design time, what combinations of extensions will be needed. This would mean that a new class would have to be made for every possible combination.

By contrast, decorators are objects, created at runtime, and can be combined on a per-use basis.

The I/O Streams implementations of both [Java](https://en.wikipedia.org/wiki/Java_Platform,_Standard_Edition#java.io) and [.NET Framework](https://en.wikipedia.org/wiki/.NET_Framework) incorporate the decorator pattern.

> <img src="https://upload.wikimedia.org/wikipedia/commons/c/c6/UML2_Decorator_Pattern.png" alt="https://upload.wikimedia.org/wikipedia/commons/c/c6/UML2_Decorator_Pattern.png" width="616" height="314" />


| **Pattern**                                                 | **Intent**                                                                        |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------|
| [Adapter](https://en.wikipedia.org/wiki/Adapter_pattern)    | Converts one interface to another so that it matches what the client is expecting |
| Decorator                                                   | Dynamically adds responsibility to the interface by wrapping the original code    |
| [Façade](https://en.wikipedia.org/wiki/Fa%C3%A7ade_pattern) | Provides a simplified interface                                                   |


A decorator makes it possible to add or alter behavior of an interface at run-time

Alternatively, [adapter](https://en.wikipedia.org/wiki/Adapter_pattern) pattern can be used when the wrapper must respect a particular interface and must support [polymorphic](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) behavior

Use the [Facade](https://en.wikipedia.org/wiki/Facade_pattern) pattern when an easier or simpler interface to an underlying object is desired.


*#FirstExample (window/scrolling scenario)*
-----------

```java
// The Window interface class
public interface Window {
    void draw(); // Draws the Window
    String getDescription(); // Returns a description of the Window
}

// Implementation of a simple Window without any scrollbars
class SimpleWindow implements Window {
    public void draw() {
        // Draw window
    }

    public String getDescription() {
        return "simple window";
    }
}

// abstract decorator class - note that it implements Window
abstract class WindowDecorator implements Window {
    protected Window windowToBeDecorated; // the Window being decorated

    public WindowDecorator (Window windowToBeDecorated) {
        this.windowToBeDecorated = windowToBeDecorated;
    }
    public void draw() {
        windowToBeDecorated.draw(); //Delegation
    }
    public String getDescription() {
        return windowToBeDecorated.getDescription(); //Delegation
    }
}

// The first concrete decorator which adds vertical scrollbar functionality
class VerticalScrollBarDecorator extends WindowDecorator {
    public VerticalScrollBarDecorator (Window windowToBeDecorated) {
        super(windowToBeDecorated);
    }

    @Override
    public void draw() {
        super.draw();
        drawVerticalScrollBar();
    }

    private void drawVerticalScrollBar() {
        // Draw the vertical scrollbar
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", including vertical scrollbars";
    }
}

// The second concrete decorator which adds horizontal scrollbar functionality
class HorizontalScrollBarDecorator extends WindowDecorator {
    public HorizontalScrollBarDecorator (Window windowToBeDecorated) {
        super(windowToBeDecorated);
    }
    @Override
    public void draw() {
        super.draw();
        drawHorizontalScrollBar();
    }

    private void drawHorizontalScrollBar() {
        // Draw the horizontal scrollbar
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", including horizontal scrollbars";
    }
}

/**
* Here's a test program that creates a Window instance which is fully decorated
* (i.e., with vertical and horizontal scrollbars), and prints its description:
*/
public class DecoratedWindowTest {
    public static void main(String[] args) {
        // Create a decorated Window with horizontal and vertical scrollbars
        Window decoratedWindow = new HorizontalScrollBarDecorator (
                new VerticalScrollBarDecorator (new SimpleWindow()));

        // Print the Window's description
        System.out.println(decoratedWindow.getDescription());
    }
}
```

The **output** of this program is **"simple window, including vertical scrollbars, including horizontal scrollbars"**. Notice how the getDescription() method of the above two decorators first retrieve the decorated Window's description and *decorates* it with a suffix.


*#SecondExample (coffee making scenario)*
-----------
```java
// The interface Coffee defines the functionality of Coffee implemented by decorator
public interface Coffee {
    public double getCost(); // Returns the cost of the coffee
    public String getIngredients(); // Returns the ingredients of the coffee
}

// Extension of a simple coffee without any extra ingredients
public class SimpleCoffee implements Coffee {
    @Override
    public double getCost() {
        return 1;
    }

    @Override
    public String getIngredients() {
        return "Coffee";
    }
}

// Abstract decorator class - note that it implements Coffee interface
public abstract class CoffeeDecorator implements Coffee {
    protected final Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee c) {
        this.decoratedCoffee = c;
    }

    public double getCost() { // Implementing methods of the interface
        return decoratedCoffee.getCost();
    }

    public String getIngredients() {
        return decoratedCoffee.getIngredients();
    }
}

// Decorator WithMilk mixes milk into coffee.
// Note it extends CoffeeDecorator.
class WithMilk extends CoffeeDecorator {
    public WithMilk(Coffee c) {
        super(c);
    }

    public double getCost() { // Overriding methods defined in the abstract superclass
        return super.getCost() + 0.5;
    }

    public String getIngredients() {
        return super.getIngredients() + ", Milk";
    }
}

// Decorator WithSprinkles mixes sprinkles onto coffee.
// Note it extends CoffeeDecorator.
class WithSprinkles extends CoffeeDecorator {
    public WithSprinkles(Coffee c) {
        super(c);
    }

    public double getCost() {
        return super.getCost() + 0.2;
    }

    public String getIngredients() {
        return super.getIngredients() + ", Sprinkles";
    }
}

// MAIN class
public class Main {
    public static void printInfo(Coffee c) {
        System.out.println("Cost: " + c.getCost() + "; Ingredients: " + c.getIngredients());
    }

    public static void main(String[] args) {
        Coffee c = new SimpleCoffee();
        printInfo(c);

        c = new WithMilk(c);
        printInfo(c);

        c = new WithSprinkles(c);
        printInfo(c);
    }
}
```

The output of the above program is :
-   Cost: 1.0; Ingredients: Coffee
-   Cost: 1.5; Ingredients: Coffee, Milk
-   Cost: 1.7; Ingredients: Coffee, Milk, Sprinkles
