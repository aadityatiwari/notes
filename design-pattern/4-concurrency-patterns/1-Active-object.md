[Active object](https://en.wikipedia.org/wiki/Active_object)
---------------

The **Active object** [design pattern](https://en.wikipedia.org/wiki/Design_pattern) decouples method execution from method invocation for objects that each reside in their own [thread](https://en.wikipedia.org/wiki/Thread_(computing)) of control. The goal is to introduce [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)), by using [asynchronous method invocation](https://en.wikipedia.org/wiki/Asynchronous_method_invocation) and a [scheduler](https://en.wikipedia.org/wiki/Scheduling_(computing)) for handling requests

The pattern consists of six elements:
-   A [proxy](https://en.wikipedia.org/wiki/Proxy_pattern), which provides an interface towards clients with publicly accessible methods.
-   An interface which defines the method request on an active object.
-   A list of pending requests from clients.
-   A [scheduler](https://en.wikipedia.org/wiki/Scheduling_(computing)), which decides which request to execute next.
-   The implementation of the active object method.
-   A [callback](https://en.wikipedia.org/wiki/Callback_(computer_science)) or [variable](https://en.wikipedia.org/wiki/Variable_(computer_science)) for the client to receive the result.


```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;
import java.util.concurrent.TimeUnit;

public class ActiveObjectClass {

	private int val = 1;

	private BlockingQueue<Runnable> dispatchQueue = new LinkedBlockingQueue<Runnable>();

	public ActiveObjectClass() {
		new Thread() {
			int i = 0;

			@Override
			public void run() {
				while (true) {
					try {
						System.out
								.println("Thread loop inside ActiveObjectClass constructor. Iteration count: " + (++i));
						if (dispatchQueue.isEmpty()) {
							System.out.println("DispatchQueue is empty");
						} else {
							System.out.println("DispatchQueue size: " + dispatchQueue.size());
						}
						dispatchQueue.take().run();

						System.out.println(
								"Thread loop inside ActiveObjectClass constructor: Sleep for 2 seconds -- BEGINS");
						TimeUnit.MILLISECONDS.sleep(500);
						System.out.println(
								"Thread loop inside ActiveObjectClass constructor: Sleep for 2 seconds -- ENDS");

					} catch (InterruptedException itEx) {
						System.out.println("Caught InterruptedException inside ActiveObjectClass constructor loop");
					}
				}
			}
		}.start();
		System.out.println("---- Coming out of ActiveObjectClass constructor");
	}

	void doSomething(int input) throws InterruptedException {
		dispatchQueue.put(new Runnable() {
			@Override
			public void run() {
				System.out.println("Inside doSomething: Before val " + val);
				val = val * 1 + input;
				System.out.println("Inside doSomething: After val " + val);
			}
		}

		);
	}

	void doSomethingElse(int input) throws InterruptedException {
		dispatchQueue.put(new Runnable() {
			@Override
			public void run() {
				System.out.println("Inside doSomethingElse: Before val " + val);
				val = val * 2 + input;
				System.out.println("Inside doSomethingElse: After val " + val);
			}
		}

		);
	}
}


import java.util.concurrent.TimeUnit;

public class MainClientActiveObject {

	public static void main(String[] args) {

		System.out.println("MainClient: Before New ActiveObject");
		ActiveObjectClass activeObject = new ActiveObjectClass();
		System.out.println("MainClient: After New ActiveObject");
		try {
			System.out.println("MainClient: Sleep for 2 seconds -- BEGINS");
			TimeUnit.MILLISECONDS.sleep(100);
			System.out.println("MainClient: Sleep for 2 seconds -- ENDS");
		} catch (InterruptedException e1) {
			System.out.println("Caught InterruptedException inside MainClient " + "while keeping "
					+ "Active object thread to sleep");
		}
		int i = 0;
		while (++i < 10) {
			System.out.println("MainClient Iteration: " + i);
			try {
				activeObject.doSomething(i);
				TimeUnit.MILLISECONDS.sleep(10);

				activeObject.doSomethingElse(i);
				TimeUnit.MILLISECONDS.sleep(10);
			} catch (InterruptedException e) {
				System.out.println("Caught InterruptedException inside MainClient");
			}
		}

		try {
			System.out.println("MainClient: Sleep for 4 seconds -- BEGINS");
			TimeUnit.SECONDS.sleep(4);
			System.out.println("MainClient: Sleep for 4 seconds -- ENDS");
		} catch (InterruptedException e1) {
			System.out.println("Caught InterruptedException inside MainClient before second loop");
		}

		while (++i < 20) {
			System.out.println("MainClient Iteration: " + i);
			try {
				activeObject.doSomething(i);
				TimeUnit.MILLISECONDS.sleep(800);

				activeObject.doSomethingElse(i);
				TimeUnit.MILLISECONDS.sleep(800);
			} catch (InterruptedException e) {
				System.out.println("Caught InterruptedException inside MainClient");
			}
		}
	}
}

```
