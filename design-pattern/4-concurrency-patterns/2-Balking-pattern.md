[Balking pattern](https://en.wikipedia.org/wiki/Balking_pattern)
---------------

"**Balk**": Hesitate or be unwilling to accept an idea or undertaking.

The **balking pattern** only executes an action on an [object](https://en.wikipedia.org/wiki/Object_(computer_science)) when the object is in a particular state. 

For e.g, if an object reads [ZIP](https://en.wikipedia.org/wiki/ZIP_file_format) files and a calling method invokes a get method on the object when the ZIP file is not open, the object would "balk" at the request by throwing an IllegalStateException

There are some specialists in this field who consider balking more of an [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern) than a design pattern.

Objects that use this pattern are generally only in a state that is prone to balking temporarily but for an unknown amount of time. If objects are to remain in a state which is prone to balking for a known, finite period of time, then the [guarded suspension pattern](https://en.wikipedia.org/wiki/Guarded_suspension_pattern) may be preferred.


*Example*
-----------

> If there are multiple calls to the job method, only one will proceed while the other calls will return with nothing. 
> Another thing to note is the jobCompleted() method. 
> The reason it is synchronized is because the only way to guarantee another thread will see a change to a field is to synchronize all access to it or declare it as volatile.

```java
public class Example {
    private boolean jobInProgress = false;

    public void job() {
        synchronized(this) {
           if (jobInProgress) {
               return;
           }
           jobInProgress = true;
        }
        // Code to execute job goes here
        // ...
    }

    void jobCompleted() {
        synchronized(this) {
            jobInProgress = false;
        }
    }
}

```
