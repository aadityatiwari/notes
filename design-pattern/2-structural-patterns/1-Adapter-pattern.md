[Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern)

-   Adapter pattern also known as Wrapper, allows the interface of an existing class to be used as another interface so as to make existing classes work with others without modifying their source code.

-   An example is an adapter that converts the interface of a [Document Object Model](https://en.wikipedia.org/wiki/Document_Object_Model) of an [XML](https://en.wikipedia.org/wiki/XML)  document into a tree structure that can be displayed.

-   The Adapter design pattern allows otherwise incompatible classes to work together by converting the interface of one class into an interface expected by the clients.

| **Pattern**                                                    | **Intent**                                                                        |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Adapter or Wrapper                                             | Converts one interface to another so that it matches what the client is expecting |
| [Decorator](https://en.wikipedia.org/wiki/Decorator_pattern)   | Dynamically adds responsibility to the interface by wrapping the original code    |
| [Delegation](https://en.wikipedia.org/wiki/Delegation_pattern) | Support "composition over inheritance"                                            |
| [Facade](https://en.wikipedia.org/wiki/Facade_pattern)         | Provides a simplified interface                                                   |

-   There are two adapter patterns:
>-   **Object adapter pattern**
>-   **Class adapter pattern**


-   In Object Adapter pattern, the Adapter contains an instance of the class it wraps. In this situation, the adapter makes calls to the instance of the wrapped object.
----
>	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/ObjectAdapter.png/300px-ObjectAdapter.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/ObjectAdapter.png/300px-ObjectAdapter.png" width="331" height="173" />
----
>	<img src="https://upload.wikimedia.org/wikipedia/commons/1/1a/Adapter%28Object%29_pattern_in_LePUS3.png" alt="https://upload.wikimedia.org/wikipedia/commons/1/1a/Adapter%28Object%29_pattern_in_LePUS3.png" width="467" height="151" />
----

-   Class adapter pattern uses multiple [polymorphic interfaces](https://en.wikipedia.org/wiki/Subtyping) implementing or inheriting both the interface that is expected and the interface that is pre-existing.

----
>	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/ClassAdapter.png/300px-ClassAdapter.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/ClassAdapter.png/300px-ClassAdapter.png" width="332" height="195" />
----
>	<img src="https://upload.wikimedia.org/wikipedia/en/b/b3/Adapter%28Class%29_pattern_in_LePUS3.png" alt="https://upload.wikimedia.org/wikipedia/en/b/b3/Adapter%28Class%29_pattern_in_LePUS3.png" width="556" height="175" />
----
	
-   When implementing the adapter pattern, for clarity one can apply the class name **\[ClassName\]To\[Interface\]Adapter** to the provider implementation. *e.g*: DAOToProviderAdapter.

-   It should have a constructor method with an adaptee class variable as a parameter. This parameter will be passed to an instance member of *\[ClassName\]To\[Interface\]Adapter*.

-   When the clientMethod is called, it will have access to the adaptee instance that allows for accessing the required data of the adaptee and performing operations on that data that generates the desired output.

```java
public class AdapteeToClientAdapter implements Adapter {

	private final Adaptee instance;

	public AdapteeToClientAdapter(final Adaptee instance) {
		this.instance = instance;
	}

	@Override
	public void clientMethod() {
		// Call Adaptee's method(s) to implement Client's clientMethod
	} 
}
```