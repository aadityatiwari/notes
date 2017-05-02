[Flyweight Pattern](https://en.wikipedia.org/wiki/Flyweight_pattern)
-------------

A flyweight is an [object](https://en.wikipedia.org/wiki/Object_(computer_science)) that minimizes [memory](https://en.wikipedia.org/wiki/Computer_memory) usage by sharing as much data as possible with other similar objects

It is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.

A classic example usage of the flyweight pattern is the data structures for graphical representation of characters in a [word processor](https://en.wikipedia.org/wiki/Word_processor). It might be desirable to have, for each character in a document, a [glyph](https://en.wikipedia.org/wiki/Glyph) object containing its font outline, font metrics, and other formatting data, but this would amount to hundreds or thousands of bytes for each character. Instead, for every character there might be a [reference](https://en.wikipedia.org/wiki/Reference_(computer_science)) to a flyweight glyph object shared by every instance of the same character in the document; only the position of each character (in the document and/or the page) would need to be stored internally.

Another example is [string interning](https://en.wikipedia.org/wiki/String_interning).

In other contexts the idea of sharing identical data structures is called [hash consing](https://en.wikipedia.org/wiki/Hash_consing).

To enable safe sharing, between clients and threads, Flyweight objects must be [immutable](https://en.wikipedia.org/wiki/Immutable_object). Flyweight objects are by definition value objects. The identity of the object instance is of no consequence therefore two Flyweight instances of the same value are considered equal.

Flyweight allows you to share bulky data that are common to each object. In other words, if you think that same data is repeating for every object, you can use this pattern to point to the single object and hence can easily save space.

*Example:*
---------

```java
import java.util.List;
import java.util.Map;
import java.util.Vector;
import java.util.concurrent.ConcurrentHashMap;

// Instances of CoffeeFlavour will be the Flyweights
class CoffeeFlavour {
  private final String name;

  CoffeeFlavour(final String newFlavor) {
    this.name = newFlavor;
  }

  @Override
  public String toString() {
    return name;
  }
}


// Menu acts as a factory and cache for CoffeeFlavour flyweight objects
class Menu {
  private Map<String, CoffeeFlavour> flavours = new ConcurrentHashMap<String, CoffeeFlavour>();

  synchronized CoffeeFlavour lookup(final String flavorName) {
    if (!flavours.containsKey(flavorName))
      flavours.put(flavorName, new CoffeeFlavour(flavorName));
    return flavours.get(flavorName);
  }

  int totalCoffeeFlavoursMade() {
    return flavours.size();
  }
}



// Order is the context of the CoffeeFlavour flyweight.
class Order {
  private final int tableNumber;
  private final CoffeeFlavour flavour;

  Order(final int tableNumber, final CoffeeFlavour flavor) {
    this.tableNumber = tableNumber;
    this.flavour = flavor;
  }

  void serve() {
    System.out.println("Serving " + flavour + " to table " + tableNumber);
  }
}



public class CoffeeShop {
  private final List<Order> orders = new Vector<Order>();
  private final Menu menu = new Menu();

  void takeOrder(final String flavourName, final int table) {
    CoffeeFlavour flavour = menu.lookup(flavourName);
    Order order = new Order(table, flavour);
    orders.add(order);
  }

  void service() {
    for (Order order : orders)
      order.serve();
  }

  String report() {
    return "\ntotal CoffeeFlavour objects made: "
        + menu.totalCoffeeFlavoursMade();
  }

  public static void main(final String[] args) {
    CoffeeShop shop = new CoffeeShop();

    shop.takeOrder("Cappuccino", 2);
    shop.takeOrder("Frappe", 1);
    shop.takeOrder("Espresso", 1);
    shop.takeOrder("Frappe", 897);
    shop.takeOrder("Cappuccino", 97);
    shop.takeOrder("Frappe", 3);
    shop.takeOrder("Espresso", 3);
    shop.takeOrder("Cappuccino", 3);
    shop.takeOrder("Espresso", 96);
    shop.takeOrder("Frappe", 552);
    shop.takeOrder("Cappuccino", 121);
    shop.takeOrder("Espresso", 121);

    shop.service();
    System.out.println(shop.report());
  }
}
```
