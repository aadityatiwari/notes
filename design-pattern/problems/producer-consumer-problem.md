[Producer-consumer problem](https://en.wikipedia.org/wiki/Producer–consumer_problem)
-------------

The **Producer–consumer problem** (also known as the **bounded-buffer problem**) is a classic example of a multi-[process](https://en.wikipedia.org/wiki/Process_(computing)) [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) problem. The problem describes two processes, the producer and the consumer, who share a common, fixed-size [buffer](https://en.wikipedia.org/wiki/Buffer_(computer_science)) used as a [queue](https://en.wikipedia.org/wiki/Queue_(data_structure)).

The producer's job is to generate data, put it into the buffer, and start again. At the same time, the consumer is consuming the data (i.e., removing it from the buffer), one piece at a time. The problem is to make sure that the producer won't try to add data into the buffer if it's full and that the consumer won't try to remove data from an empty buffer.

The solution can be reached by means of [inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication), typically using [semaphores](https://en.wikipedia.org/wiki/Semaphore_(programming)).

An inadequate solution could result in a [deadlock](https://en.wikipedia.org/wiki/Deadlock) where both processes are waiting to be awakened.

The problem can also be generalized to have multiple producers and consumers.


*Below is an inadequate implementation which contains a race condition that could lead to a **deadlock***

```java

int itemCount = 0;

procedure producer() {
    while (true) {
        item = produceItem();

        if (itemCount == BUFFER_SIZE) {
            sleep();
        }

        putItemIntoBuffer(item);
        itemCount = itemCount + 1;

        if (itemCount == 1) {
            wakeup(consumer);
        }
    }
}

procedure consumer() {
    while (true) {

        if (itemCount == 0) {
            sleep();
        }

        item = removeItemFromBuffer();
        itemCount = itemCount - 1;

        if (itemCount == BUFFER_SIZE - 1) {
            wakeup(producer);
        }

        consumeItem(item);
    }
}

```

The problem with this solution is that it contains a [race condition](https://en.wikipedia.org/wiki/Race_condition) that can lead to a [deadlock](https://en.wikipedia.org/wiki/Deadlock). 

Consider the following scenario:

-   The consumer has just read the variable itemCount, noticed it's zero and is just about to move inside the if block.

-   Just before calling sleep, the consumer is interrupted and the producer is resumed.

-   The producer creates an item, puts it into the buffer, and increases itemCount.

-   Because the buffer was empty prior to the last addition, the producer tries to wake up the consumer.

-   Unfortunately the consumer wasn't yet sleeping, and the wakeup call is lost. When the consumer resumes, it goes to sleep and will never be awakened again. This is because the consumer is only awakened by the producer when itemCount is equal to 1.

-   The producer will loop until the buffer is full, after which it will also go to sleep.

Since both processes will sleep forever, we have run into a deadlock. This solution therefore is *unsatisfactory.*

**Using semaphores:** 
--------------

*Solves the problem of lost wakeup calls. Below solution works fine only when there is only one producer and consumer, otherwise contains a serious race condition.)*

```java

semaphore fillCount = 0; // items produced
semaphore emptyCount = BUFFER_SIZE; // remaining space

procedure producer() {
    while (true) {
        item = produceItem();
        down(emptyCount);
        putItemIntoBuffer(item);
        up(fillCount);
    }
}

procedure consumer() {
    while (true) {
        down(fillCount);
        item = removeItemFromBuffer();
        up(emptyCount);
        consumeItem(item);
    }
}

```

With multiple producers sharing the same memory space for the item buffer, or multiple consumers sharing the same memory space, this solution contains a serious race condition that could result in two or more processes reading or writing into the same slot at the same time.

If procedure can be executed concurrently by multiple producers, then consider following scenario:

-   Two producers decrement *emptyCount*

-   One of the producers determines the next empty slot in the buffer

-   Second producer determines the next empty slot and gets the same result as the first producer

-   Both producers write into the same slot

To overcome this problem, we need a way to execute a [critical section](https://en.wikipedia.org/wiki/Critical_section) with [mutual exclusion](https://en.wikipedia.org/wiki/Mutual_exclusion). 

**Using semaphores and mutex:** 
----------------
*Solution for multiple producers and consumers:*

```java

mutex buffer_mutex; // similar to "semaphore buffer_mutex = 1", but different (see notes below)
semaphore fillCount = 0;
semaphore emptyCount = BUFFER_SIZE;

procedure producer() {
    while (true) {
        item = produceItem();
        down(emptyCount);
            down(buffer_mutex);
                putItemIntoBuffer(item);
            up(buffer_mutex);
        up(fillCount);
    }
}

procedure consumer() {
    while (true) {
        down(fillCount);
            down(buffer_mutex);
                item = removeItemFromBuffer();
            up(buffer_mutex);
        up(emptyCount);
        consumeItem(item);
    }
}

```

**Using monitors:** 
-----------
*Since mutual exclusion is implicit with monitors, no extra effort is necessary to protect the critical section.*

```java

monitor ProducerConsumer {
    int itemCount;
    condition full;
    condition empty;

    procedure add(item) {
        while (itemCount == BUFFER_SIZE) {
            wait(full);
        }

        putItemIntoBuffer(item);
        itemCount = itemCount + 1;

        if (itemCount == 1) {
            notify(empty);
        }
    }
    procedure remove() {
        while (itemCount == 0) {
            wait(empty);
        }

        item = removeItemFromBuffer();
        itemCount = itemCount - 1;

        if (itemCount == BUFFER_SIZE - 1) {
            notify(full);
        }


        return item;
    }
}

procedure producer() {
    while (true) {
        item = produceItem();
        ProducerConsumer.add(item);
    }
}

procedure consumer() {
    while (true) {
        item = ProducerConsumer.remove();
        consumeItem(item);
    }
}

```