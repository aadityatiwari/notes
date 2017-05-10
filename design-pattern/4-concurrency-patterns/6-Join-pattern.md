[Join pattern](https://en.wikipedia.org/wiki/Join-pattern)
--------------

**Join-patterns** provide a way to write [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing), [parallel](https://en.wikipedia.org/wiki/Parallel_algorithm) and [distributed](https://en.wikipedia.org/wiki/Distributed_programming) computer programs by [message passing](https://en.wikipedia.org/wiki/Message_passing). Compared to the use of [threads](https://en.wikipedia.org/wiki/Thread_(computing)) and locks, this is a high level programming model using communication constructs model to abstract the complexity of concurrent environment and to allow [scalability](https://en.wikipedia.org/wiki/Scalability). Its focus is on the execution of a [chord](https://en.wikipedia.org/wiki/Chord_(concurrency)) between messages atomically consumed from a group of channels.

Join-patterns can be used to easily encode related concurrency idioms like actors and active objects.

[Join Java](https://en.wikipedia.org/wiki/Join_Java) is a language based on the [Java programming language](https://en.wikipedia.org/wiki/Java_(programming_language)) allowing the use of the join calculus. It introduces three new language constructs: Join methods, Asynchronous methods, and ordering modifiers.

[**Barriers**](https://en.wikipedia.org/wiki/Barrier_(computer_science))
--------------

```csharp
class SymmetricBarrier {

	public readonly Synchronous.Channel Arrive;
	
	public SymmetricBarrier(int n) {
		// create j and init channels (elided)
		var pat = j.When(Arrive);
		for (int i = 1; i < n; i++) 
			pat = pat.And(Arrive);
		pat.Do(() => { });
	}
}

```

[**Dining philosophers problem**](https://en.wikipedia.org/wiki/Dining_philosophers_problem)
--------------

```csharp
var j = Join.Create();
Synchronous.Channel[] hungry;
Asynchronous.Channel[] chopstick;
j.Init(out hungry, n); 
j.Init(out chopstick, n);

for (int i = 0; i < n; i++) {
    var left = chopstick[i];
    var right = chopstick[(i+1) % n];
    j.When(hungry[i]).And(left).And(right).Do(() => {
    eat(); left(); right(); // replace chopsticks
    });
}
```

[**Mutual exclusion**](https://en.wikipedia.org/wiki/Mutual_exclusion)
--------------

```csharp
class Lock {
	public readonly Synchronous.Channel Acquire;
	public readonly Asynchronous.Channel Release;

    public Lock() {
        // create j and init channels (elided)
        j.When(Acquire).And(Release).Do(() => { });
        Release(); // initially free
    }
}
```

[**Producers/Consumers**](https://en.wikipedia.org/wiki/Producer-consumer_problem)
--------------

```csharp
class Buffer<T> {

	public readonly Asynchronous.Channel<T> Put;
	public readonly Synchronous<T>.Channel Get;
	
    public Buffer() {
        Join j = Join.Create(); // allocate a Join object
        j.Init(out Put);
        // bind its channels
        j.Init(out Get);
        j.When(Get).And(Put).Do // register chord
        (t => { return t; });
    }
}
```

[**Reader-writer locking**](https://en.wikipedia.org/wiki/Readers%E2%80%93writer_lock) 
--------------

```csharp
class ReaderWriterLock {

	private readonly Asynchronous.Channel idle;
	private readonly Asynchronous.Channel<int> shared;
	public readonly Synchronous.Channel AcqR, AcqW,
	RelR, RelW;
	
	public ReaderWriterLock() {
		// create j and init channels (elided)
		j.When(AcqR).And(idle).Do(() => shared(1));
		j.When(AcqR).And(shared).Do(n => shared(n+1));
		j.When(RelR).And(shared).Do(n => {
			if (n == 1) idle(); else shared(n-1);
		});
		j.When(AcqW).And(idle).Do(() => { });
		j.When(RelW).Do(() => idle());
		idle(); // initially free
	}
}
```

[**Semaphores**](https://en.wikipedia.org/wiki/Semaphore_(programming))
--------------

```csharp
class Semaphore {

	public readonly Synchronous.Channel Acquire;
	public readonly Asynchronous.Channel Release;
	
    public Semaphore(int n) {
        // create j and init channels (elided)
        j.When(Acquire).And(Release).Do(() => { });
        for (; n > 0; n--) 
			Release(); // initially n free
    }
}
```
