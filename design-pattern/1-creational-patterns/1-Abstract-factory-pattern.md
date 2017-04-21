[Abstract factory pattern](https://en.wikipedia.org/wiki/Abstract_factory_pattern)
----------

The **abstract factory pattern** provides a way to encapsulate a group of individual [factories](https://en.wikipedia.org/wiki/Factory_object) that have a common theme without specifying their concrete classes.

Client software creates a concrete implementation of the abstract factory and then uses the generic [interface](https://en.wikipedia.org/wiki/Interface_(computer_science)) of the factory to create the concrete [objects](https://en.wikipedia.org/wiki/Object_(computer_science))

This pattern separates the details of implementation of a set of objects from their general usage and relies on object composition, as object creation is implemented in methods exposed in the factory interface.

A **factory** is the location of a concrete class in the code at which objects are constructed. The intent in employing the pattern is to insulate the creation of objects from their usage and to create families of related objects without having to depend on their concrete classes

The *factory* determines the actual *concrete* type of [object](https://en.wikipedia.org/wiki/Object_(computer_science)) to be created, and it is here that the object is actually created. A factory is an object for creating other objects.

Factory only returns an abstract type reference to the created concrete object.

>	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Abstract_factory.svg/640px-Abstract_factory.svg.png" alt="Abstract factory.svg" width="613" height="267" />


>	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Abstract_Factory_in_LePUS3_vector.svg/640px-Abstract_Factory_in_LePUS3_vector.svg.png" alt="Abstract Factory in LePUS3 vector.svg" width="451" height="200" />


*Java example:*

```java

public interface IButton {
	void paint();
}

public interface IGUIFactory {
	public IButton createButton();
}

public class WinFactory implements IGUIFactory {
	 @ Override
	public IButton createButton() {
		return new WinButton();
	}
}

public class OSXFactory implements IGUIFactory {
	 @ Override
	public IButton createButton() {
		return new OSXButton();
	}
}

public class WinButton implements IButton {
	 @ Override
	public void paint() {
		System.out.println("WinButton");
	}
}

public class OSXButton implements IButton {
	 @ Override
	public void paint() {
		System.out.println("OSX button");
	}
}

public class Main {
	public static void main(String [] args)throws Exception {
		IGUIFactory factory = null;
		String appearance = randomAppearance();
		 * //current operating system*
		if (appearance.equals("osx")) {
			factory = new OSXFactory();
		} else if (appearance.equals("windows")) {
			factory = new WinFactory();
		} else {
			throw new Exception("No such operating system");
		}
		IButton button = factory.createButton();
		button.paint();
	}
	
	/*
	 * This is just for the sake of testing this program, and doesn't have to do
	 * with Abstract Factory pattern.
	 */
	public static String randomAppearance() {
		String \ [ \ ]appearanceArr = new String \ [3 \ ];
		appearanceArr \ [0 \ ] = "osx";
		appearanceArr \ [1 \ ] = "windows";
		appearanceArr \ [2 \ ] = "error";
		java.util.Random rand = new java.util.Random();
		int randNum = rand.nextInt(3);
		return appearanceArr \ [randNum \ ];
	}
}

```