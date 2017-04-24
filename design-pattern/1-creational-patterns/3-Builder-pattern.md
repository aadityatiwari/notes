[Builder pattern](https://en.wikipedia.org/wiki/Builder_pattern)

The **builder pattern** is an [object creation](https://en.wikipedia.org/wiki/Creational_pattern) software [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science))

Unlike the [abstract factory pattern](https://en.wikipedia.org/wiki/Abstract_factory_pattern) and the [factory method pattern](https://en.wikipedia.org/wiki/Factory_method_pattern) whose intention is to enable [polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)), the intention of the builder pattern is to find a solution to the telescoping constructor [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern)

The telescoping constructor anti-pattern occurs when the increase of object constructor parameter combination leads to an exponential list of constructors.

Instead of using numerous constructors, the builder pattern uses another object, a builder, which receives each initialization parameter step by step and then returns the resulting constructed object at once.

Builders are good candidates for a [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface)

The intent of the Builder design pattern is to separate the construction of a complex object from its representation. By doing so the same construction process can create different representations.

Example: We have a Car class. The problem is that a car has many options. The combination of each option would lead to a huge list of constructors for this class. So we will create a builder class, CarBuilder. We will send to the CarBuilder each car option step by step and then construct the final car with the right options.

```java

class Car {

	private int wheels;
	private String color;

	public Car() {
	}

	@Override
	public String toString() {
		return "Car [wheels = " + wheels + ", color = " + color + "]";
	}

	public int getWheels() {
		return wheels;
	}

	public void setWheels(final int wheels) {
		this.wheels = wheels;
	}

	public String getColor() {
		return color;
	}

	public void setColor(final String color) {
		this.color = color;
	}
}

// The builder abstraction.

interface CarBuilder {

	void setWheels(final int wheels);

	void setColor(final String color);

	Car getResult();
}

public class CarBuilderImpl implements CarBuilder {
	private Car car;

	public CarBuilderImpl() {
		car = new Car();
	}

	@Override
	public void setWheels(final int wheels) {
		car.setWheels(wheels);
	}

	@Override
	public void setColor(final String color) {
		car.setColor(color);
	}

	@Override
	public Car getResult() {
		return car;
	}
}

public class CarBuildDirector {

	private CarBuilder builder;

	public CarBuildDirector(final CarBuilder builder) {

		this.builder = builder;

	}

	Car construct() {

		builder.setWheels(4);

		builder.setColor("Red");

		return builder.getResult();

	}

	public static void main(final String[] arguments) {

		CarBuilder builder = new CarBuilderImpl();

		CarBuildDirector carBuildDirector = new CarBuildDirector(builder);

		System.out.println(carBuildDirector.construct());

	}

}

```