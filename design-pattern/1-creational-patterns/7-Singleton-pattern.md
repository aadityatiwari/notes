[Singleton pattern](https://en.wikipedia.org/wiki/Singleton_pattern)
-----------------

**Singleton pattern** is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) that restricts the [instantiation](https://en.wikipedia.org/wiki/Instantiation_(computer_science)) of a [class](https://en.wikipedia.org/wiki/Class_(computer_programming)) to one [object](https://en.wikipedia.org/wiki/Object_(computer_science)). 

An implementation of the singleton pattern must:
-   Ensure that only one instance of the singleton class ever exists
-   Provide global access to that instance.

Typically, this is done by:
-   Declaring all constructors of the class to be private
-   Providing a static method that returns a reference to the instance.

A singleton implementation may use [lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization), where the instance is created when the static method is first invoked.

*Example 1:* Double checked locking using volatile

```java
class Foo {
	private volatile Helper helper = null;

	public Helper getHelper() {
		if (helper == null) {
			synchronized(this) {
				if (helper == null)
					helper = new Helper();
			}
		}
		return helper;
	}
}
```

*Example 2:* Using static singletons

If the singleton you are creating is static (i.e., there will only be one Helper created), as opposed to a property of another object (e.g., there will be one Helper for each Foo object, there is a simple and elegant solution.

Just define the singleton as a static field in a separate class. The semantics of Java guarantee that the field will not be initialized until the field is referenced, and that any thread which accesses the field will see all of the writes resulting from initializing that field.

```java
class HelperSingleton {
	static Helper singleton = new Helper();
}
```

*Example 3:* Using Thread Local storage

```java
class Foo {

	/* 	If perThreadInstance.get() returns a non-null value, this thread
		has done synchronization needed to see initialization of helper
	*/
	private final ThreadLocal perThreadInstance = new ThreadLocal();
	private Helper helper = null;

	public Helper getHelper() {	
		if (perThreadInstance.get() == null)
			createHelper();			  
		return helper;
	}

	private final void createHelper() {

		synchronized(this) {
			if (helper == null)
				helper = new Helper();
		}

		// Any non-null value would do as the argument here
		perThreadInstance.set(perThreadInstance);
	}
}
```