[Immutable object](https://en.wikipedia.org/wiki/Immutable_object)
-------------

**Immutable object** (unchangeable object) is an [object](https://en.wikipedia.org/wiki/Object_(computer_science)) whose state cannot be modified after it is created

-   In some cases, an object is considered immutable even if some internally used attributes change but the object's state appears to be unchanging from an external point of view. For example, an object that uses [memoization](https://en.wikipedia.org/wiki/Memoization) to cache the results of expensive computations could still be considered an immutable object.

-   Immutable objects are also useful because they are inherently [thread-safe](https://en.wikipedia.org/wiki/Thread_safety).

-   *Weakly immutable:* When only certain fields of an object being immutable

-   *Immutable:* When all the fields are immutable

-   *Strongly immutable:* When whole object cannot be extended by another class

***Copy-on-write (COW):*** Using this technique, when a user asks the system to copy an object, it will instead merely create a new reference that still points to the same object. As soon as a user attempts to modify the object through a particular reference, the system makes a real copy, applies the modification to that, and sets the reference to refer to the new copy. The other users are unaffected, because they still refer to the original object. Under COW, all users appear to have a mutable version of their object, the space-saving and speed advantages of immutable objects are preserved.

Strings are immutable objects in Java but Java also have mutable versions of Java [String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) which are [StringBuffer](https://en.wikipedia.org/wiki/StringBuffer) and [StringBuilder](https://en.wikipedia.org/wiki/StringBuilder)

Additionally, all of the [primitive wrapper classes](https://en.wikipedia.org/wiki/Primitive_wrapper_class) in Java are immutable.

The reference types cannot be made immutable just by using the final keyword; **final only prevents reassignment.** Primitive wrappers (Integer, Long, Short, Double, Float, Character, Byte, Boolean) are also all immutable.

```java
final MyObject m = new MyObject();   //m is of reference type
m.data = 100;  // OK. We can change state of object m (m is mutable and final doesn't change this fact)
m = new MyObject();  // does not compile. m is final so can't be reassigned
```

**Example of an Immutable class:**
----------

```java
import java.util.Date;

/**
* Planet is an immutable class, since there is no way to change
* its state after construction.
*/
public final class Planet {

  public Planet (double aMass, String aName, Date aDateOfDiscovery) {
     fMass = aMass;
     fName = aName;
     //make a private copy of aDateOfDiscovery
     //this is the only way to keep the fDateOfDiscovery
     //field private, and shields this class from any changes that 
     //the caller may make to the original aDateOfDiscovery object
     fDateOfDiscovery = new Date(aDateOfDiscovery.getTime());
  }

  /**
  * Returns a primitive value.
  *
  * The caller can do whatever they want with the return value, without 
  * affecting the internals of this class. Why? Because this is a primitive 
  * value. The caller sees its "own" double that simply has the
  * same value as fMass.
  */
  public double getMass() {
    return fMass;
  }

  /**
  * Returns an immutable object.
  *
  * The caller gets a direct reference to the internal field. But this is not 
  * dangerous, since String is immutable and cannot be changed.
  */
  public String getName() {
    return fName;
  }

	//  /**
	//  * Returns a mutable object - likely bad style.
	//  *
	//  * The caller gets a direct reference to the internal field. This is usually dangerous, 
	//  * since the Date object state can be changed both by this class and its caller.
	//  * That is, this class is no longer in complete control of fDate.
	//  */
	//  public Date getDateOfDiscovery() {
	//    return fDateOfDiscovery;
	//  }

  /**
  * Returns a mutable object - good style.
  * 
  * Returns a defensive copy of the field.
  * The caller of this method can do anything they want with the
  * returned Date object, without affecting the internals of this
  * class in any way. Why? Because they do not have a reference to 
  * fDate. Rather, they are playing with a second Date that initially has the 
  * same data as fDate.
  */
  public Date getDateOfDiscovery() {
    return new Date(fDateOfDiscovery.getTime());
  }

  // PRIVATE

  /**
  * Final primitive data is always immutable.
  */
  private final double fMass;

  /**
  * An immutable object field. (String objects never change state.)
  */
  private final String fName;

  /**
  * A mutable object field. In this case, the state of this mutable field
  * is to be changed only by this class. (In other cases, it makes perfect
  * sense to allow the state of a field to be changed outside the native
  * class; this is the case when a field acts as a "pointer" to an object
  * created elsewhere.)
  */
  private final Date fDateOfDiscovery;
}

```

***Final and inner classes:*** When an anonymous [inner class](https://en.wikipedia.org/wiki/Inner_class) is defined within the body of a method, all variables declared final in the scope of that method are accessible from within the inner class. For scalar values, once it has been assigned, the value of the final variable cannot change. For object values, the reference cannot change. This allows the Java compiler to "capture" the value of the variable at run-time and store a copy as a field in the inner class. Once the outer method has terminated and its [stack frame](https://en.wikipedia.org/wiki/Call_stack) has been removed, the original variable is gone but the inner class's private copy persists in the class's own memory.
