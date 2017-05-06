[Strategy pattern](https://en.wikipedia.org/wiki/Strategy_pattern)
----------------

**Strategy pattern** (aka **policy pattern**) enables an [algorithm](https://en.wikipedia.org/wiki/Algorithm)'s behavior to be selected at runtime.

The strategy pattern
-   defines a family of algorithms,
-   encapsulates each algorithm, and
-   makes the algorithms interchangeable within that family.

Strategy lets the algorithm vary independently from clients that use it

For instance, a class that performs validation on incoming data may use a strategy pattern to select a validation algorithm based on the type of data, the source of the data, user choice, or other discriminating factors. These factors are not known for each case until run-time, and may require radically different validation to be performed.

The validation strategies, encapsulated separately from the validating object, may be used by other validating objects in different areas of the system (or even different systems) without [code duplication](https://en.wikipedia.org/wiki/Duplicate_code).

The essential requirement in the [programming language](https://en.wikipedia.org/wiki/Programming_language) is the ability to store a reference to some code in a data structure and retrieve it. This can be achieved by mechanisms such as the native [function pointer](https://en.wikipedia.org/wiki/Function_pointer), the [first-class function](https://en.wikipedia.org/wiki/First-class_function), classes or class instances in [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) languages, or accessing the language implementation's internal storage of code via [reflection](https://en.wikipedia.org/wiki/Reflection_(computer_science)).

Think twice before implementing this pattern. You have to be sure your need is to frequently change an algorithm. You have to clearly anticipate the future; otherwise, this pattern will be more expensive than a basic implementation.

> <img src="https://upload.wikimedia.org/wikipedia/commons/3/39/Strategy_Pattern_in_UML.png" alt="https://upload.wikimedia.org/wikipedia/commons/3/39/Strategy_Pattern_in_UML.png" width="255" height="160" />


```java
import java.util.ArrayList;
import java.util.List;

public class StrategyPatternWiki {

    public static void main(final String[] arguments) {
        Customer firstCustomer = new Customer(new NormalStrategy());

        // Normal billing
        firstCustomer.add(1.0, 1);

        // Start Happy Hour
        firstCustomer.setStrategy(new HappyHourStrategy());
        firstCustomer.add(1.0, 2);

        // New Customer
        Customer secondCustomer = new Customer(new HappyHourStrategy());
        secondCustomer.add(0.8, 1);
        // The Customer pays
        firstCustomer.printBill();

        // End Happy Hour
        secondCustomer.setStrategy(new NormalStrategy());
        secondCustomer.add(1.3, 2);
        secondCustomer.add(2.5, 1);
        secondCustomer.printBill();
    }
}

class Customer {

    private List<Double> drinks;
    private BillingStrategy strategy;

    public Customer(final BillingStrategy strategy) {
        this.drinks = new ArrayList<Double>();
        this.strategy = strategy;
    }

    public void add(final double price, final int quantity) {
        drinks.add(strategy.getActPrice(price*quantity));
    }

    // Payment of bill
    public void printBill() {
        double sum = 0;
        for (Double i : drinks) {
            sum += i;
        }
        System.out.println("Total due: " + sum);
        drinks.clear();
    }

    // Set Strategy
    public void setStrategy(final BillingStrategy strategy) {
        this.strategy = strategy;
    }

}

interface BillingStrategy {
    double getActPrice(final double rawPrice);
}

// Normal billing strategy (unchanged price)
class NormalStrategy implements BillingStrategy {

    @Override
    public double getActPrice(final double rawPrice) {
        return rawPrice;
    }

}

// Strategy for Happy hour (50% discount)
class HappyHourStrategy implements BillingStrategy {

    @Override
    public double getActPrice(final double rawPrice) {
        return rawPrice*0.5;
    }

}
```


*An example in Java 8 using Lambdas*
-------------
```java

/** Imports a type of lambdas taking two arguments of the same type T and returning one argument of same type T */
import java.util.function.BinaryOperator;


/** Implements and assigns to variables the lambdas to be used later in configuring Context.
*   FunctionalUtils is just a convenience class, as the code of a lambda
*   might be passed directly to Context constructor, as for ResultD below, in main().
*/
class FunctionalUtils {
	static final BinaryOperator<Integer> add = (final Integer a, final Integer b) -> {
		System.out.println("Called add's apply().");
		return a + b;
	};

	static final BinaryOperator<Integer> subtract = (final Integer a, final Integer b) -> {
		System.out.println("Called subtract's apply().");
		return a - b;
	};

	static final BinaryOperator<Integer> multiply = (final Integer a, final Integer b) -> {
		System.out.println("Called multiply's apply().");
		return a * b;
	};
}


/** Configured with a lambda and maintains a reference to a lambda */
class Context {
	/** a variable referencing a lambda taking two Integer arguments and returning an Integer: */
	private final BinaryOperator<Integer> strategy;
    	
	public Context(final BinaryOperator<Integer> lambda) {
		strategy = lambda;
	}

	public int executeStrategy(final int a, final int b) {
		return strategy.apply(a, b);
	}
}


/** Tests the pattern */
public class StrategyExample {

	public static void main(String[] args) {
		Context context;

		context = new Context(FunctionalUtils.add);
		final int resultA = context.executeStrategy(3,4);

		context = new Context(FunctionalUtils.subtract);
		final int resultB = context.executeStrategy(3, 4);

		context = new Context(FunctionalUtils.multiply);
		final int resultC = context.executeStrategy(3, 4);

		context = new Context((final Integer a, final Integer b) -> a * b + 1);
		final int resultD = context.executeStrategy(3,4);

		System.out.println("Result A: " + resultA );
		System.out.println("Result B: " + resultB );
		System.out.println("Result C: " + resultC );
		System.out.println("Result D: " + resultD );
	}}

```

According to the strategy pattern, the behaviors of a class should not be inherited. Instead they should be encapsulated using interfaces. 

The strategy pattern uses composition instead of inheritance. 

As an example, consider a car class. Two possible functionalities for car are *brake* and *accelerate*.

Since accelerate and brake behaviors change frequently between models, a common approach is to implement these behaviors in subclasses. This approach has significant drawbacks: accelerate and brake behaviors must be declared in each new Car model. The work of managing these behaviors increases greatly as the number of models increases, and requires code to be duplicated across models. Additionally, it is not easy to determine the exact nature of the behavior for each model without investigating the code in each.

The  In the strategy pattern, behaviors are defined as separate interfaces and specific classes that implement these interfaces. This allows better decoupling between the behavior and the class that uses the behavior. The behavior can be changed without breaking the classes that use it, and the classes can switch between behaviors by changing the specific implementation used without requiring any significant code changes. Behaviors can also be changed at run-time as well as at design-time.

For instance, a car object’s brake behavior can be changed from BrakeWithABS() to Brake() by changing the brakeBehavior member.

```java
/* Encapsulated family of Algorithms
 * Interface and its implementations
 */
public interface IBrakeBehavior {
    public void brake();
}


public class BrakeWithABS implements IBrakeBehavior {
    public void brake() {
        System.out.println("Brake with ABS applied");
    }
}


public class Brake implements IBrakeBehavior {
    public void brake() {
        System.out.println("Simple Brake applied");
    }
}


/* Client that can use the algorithms above interchangeably */
public abstract class Car {
    protected IBrakeBehavior brakeBehavior;

    public void applyBrake() {
        brakeBehavior.brake();
    }

    public void setBrakeBehavior(final IBrakeBehavior brakeType) {
        this.brakeBehavior = brakeType;
    }
}

/* Client 1 uses one algorithm (Brake) in the constructor */
public class Sedan extends Car {
    public Sedan() {
        this.brakeBehavior = new Brake();
    }
}

/* Client 2 uses another algorithm (BrakeWithABS) in the constructor */
public class SUV extends Car {
    public SUV() {
        this.brakeBehavior = new BrakeWithABS();
    }
}

/* Using the Car example */
public class CarExample {
    public static void main(final String[] arguments) {
        Car sedanCar = new Sedan();
        sedanCar.applyBrake();  // This will invoke class "Brake"

        Car suvCar = new SUV();
        suvCar.applyBrake();    // This will invoke class "BrakeWithABS"

        // set brake behavior dynamically
        suvCar.setBrakeBehavior( new Brake() );
        suvCar.applyBrake();    // This will invoke class "Brake"
    }
}

```
