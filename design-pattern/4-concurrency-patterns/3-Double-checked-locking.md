[Double-checked locking](https://en.wikipedia.org/wiki/Double-checked_locking)
----------------

**Double-checked locking** (also known as "double-checked locking optimization) is used to reduce the overhead of acquiring a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science)) by first testing the locking criterion (the "lock hint") without actually acquiring the lock. Only if the locking criterion check indicates that locking is required does the actual locking logic proceed.

It is typically used to reduce locking overhead when implementing "[lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization)" in a multi-threaded environment, especially as part of the [Singleton pattern](https://en.wikipedia.org/wiki/Singleton_pattern). Lazy initialization avoids initializing a value until the first time it is accessed.


```java
// Works with acquire/release semantics for volatile in Java 1.5 and later
// Broken under Java 1.4 and earlier semantics for volatile
class Foo {
    private volatile Helper helper;
    public Helper getHelper() {
        Helper result = helper;
        if (result == null) {
            synchronized(this) {
                result = helper;
                if (result == null) {
                    helper = result = new Helper();
                }
            }
        }
        return result;
    }
    // other functions and members...
}
```

The [volatile](https://en.wikipedia.org/wiki/Volatile_variable) keyword now ensures that multiple threads handle the singleton instance correctly.

Note the local variable result, which seems unnecessary. The effect of this is that in cases where helper is already initialized (i.e., most of the time), the volatile field is only accessed once (due to "return result;" instead of "return helper;"), which can improve the method's overall performance by as much as 25 percent.

If the helper object is static (one per class loader), an alternative is the [initialization-on-demand holder idiom](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom). This relies on the fact that nested classes are not loaded until they are referenced.


```java
// Correct lazy initialization in Java
class Foo {
    private static class HelperHolder {
       public static final Helper helper = new Helper();
    }

    public static Helper getHelper() {
        return HelperHolder.helper;
    }
}

```
