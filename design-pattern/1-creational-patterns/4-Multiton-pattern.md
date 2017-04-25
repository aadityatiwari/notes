[Multiton pattern](https://en.wikipedia.org/wiki/Multiton_pattern)
-----------------

**Multiton pattern** is a [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)) similar to the [singleton](https://en.wikipedia.org/wiki/Singleton_pattern). 

It expands on the singleton concept to manage a [map](https://en.wikipedia.org/wiki/Associative_array) of named instances as key-value pairs. It ensures a single instance per key.

While it may appear that the multiton is no more than a simple [hash table](https://en.wikipedia.org/wiki/Hash_table) with synchronized access there are two important distinctions.

First, the multiton does not allow clients to add mappings.

Secondly, the multiton never returns a null or empty reference; instead, it creates and stores a multiton instance on the first request with the associated key.
