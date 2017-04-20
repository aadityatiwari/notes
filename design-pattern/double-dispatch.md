[Double dispatch](https://en.wikipedia.org/wiki/Double_dispatch)
-----------

**Double dispatch** is a special form of [multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch), and a mechanism that dispatches a function call to different concrete functions depending on the runtime types of two objects involved in the call.

In most [object-oriented](https://en.wikipedia.org/wiki/Object-oriented) systems, the concrete function that is called from a function call in the code depends on the dynamic type of a single object and therefore they are known as [single dispatch](https://en.wikipedia.org/wiki/Single_dispatch) calls, or simply [virtual function](https://en.wikipedia.org/wiki/Virtual_function) calls.

Double dispatch is useful in situations where the choice of computation depends on the runtime types of its arguments. 

Selection of the appropriate algorithm is based on the call's argument types at runtime. 

The **Overridden methods are dispatched dynamically but method overloading is done statically.**

Double dispatch is more than function overloading. At first glance, double dispatch appears to be a natural result of [function overloading](https://en.wikipedia.org/wiki/Function_overloading). Function overloading allows the function called to depend on the type of the argument.

Function overloading, however, is done at compile time using "[name mangling](https://en.wikipedia.org/wiki/Name_mangling)" where the internal name of the function encodes the argument's type. For example, a function foo(int) may internally be called *\_\_foo\_i* and the function foo(double) may be called *\_\_foo\_d*. Thus, there is no name collision, and no virtual table lookup.

By contrast, dynamic dispatch is based on the type of the calling object, meaning it uses [virtual functions](https://en.wikipedia.org/wiki/Virtual_functions) (overriding) instead of [function overloading](https://en.wikipedia.org/wiki/Function_overloading), and does result in a vtable lookup.

**An example of collisions in a game:**


```java
public class Spaceship {}

public class ApolloSpacecraft extends Spaceship {}

public class Asteroid {
    public void collideWith(Spaceship spaceship) {
        System.out.println("Asteroid hits a Spaceship");
    }
    public void collideWith(ApolloSpacecraft apolloSpacecraft) {
        System.out.println("Asteroid hits a ApolloSpacecraft");
    }
    public void collideWith(ExtendedSpaceship spaceship) {
        System.out.println("Asteroid hits a Spaceship");
    }
    public void collideWith(ExtendedApolloSpacecraft apolloSpacecraft) {
        System.out.println("Asteroid hits a ApolloSpacecraft");
    }
 }
 
public class ExplodingAsteroid extends Asteroid {
    @Override
    public void collideWith(Spaceship spaceship) {
        System.out.println("ExplodingAsteroid hits a Spaceship");
    }
    @Override
    public void collideWith(ApolloSpacecraft apolloSpacecraft) {
        System.out.println("ExplodingAsteroid hits a ApolloSpacecraft");
    }
    public void collideWith(ExtendedSpaceship spaceship) {
        System.out.println("ExplodingAsteroid hits a Spaceship");
    }
    public void collideWith(ExtendedApolloSpacecraft apolloSpacecraft) {
        System.out.println("ExplodingAsteroid hits a ApolloSpacecraft");
    }
 }
 
public class ExtendedSpaceship {
    public void collideWith(Asteroid asteroid) {
    asteroid.collideWith(this);
    }
 }
 
public class ExtendedApolloSpacecraft extends ExtendedSpaceship {
    public void collideWith(Asteroid asteroid) {
        asteroid.collideWith(this);
    }
}
 
public class MainClient {
    public static void main(String[] args) {
        withoutDynamicDispatch();
        withDynamicDispatch();
        withoutDoubleDispatch();
        withDoubleDispatch();
    }
    
    public static void withoutDynamicDispatch() {
        System.out.println("\nBEGIN-- Without dynamic dispatch");
        Asteroid asteroid = new Asteroid();
        ExplodingAsteroid explodingAsteroid = new ExplodingAsteroid();
        Spaceship spaceShip = new Spaceship();
        ApolloSpacecraft apolloSpacecraft = new ApolloSpacecraft();
        asteroid.collideWith(spaceShip);
        asteroid.collideWith(apolloSpacecraft);
        explodingAsteroid.collideWith(spaceShip);
        explodingAsteroid.collideWith(apolloSpacecraft);
        System.out.println("\\nEND-- Without dynamic dispatch");
        
         // -- OUTPUT --
         // BEGIN-- Without dynamic dispatch
         // Asteroid hits a Spaceship
         // Asteroid hits a ApolloSpacecraft
         // ExplodingAsteroid hits a Spaceship
         // ExplodingAsteroid hits a ApolloSpacecraft
         // END-- Without dynamic dispatch
    }
    
    public static void withDynamicDispatch() {
        System.out.println("\\nBEGIN-- With dynamic dispatch");
        Spaceship spaceShip = new Spaceship();
        ApolloSpacecraft apolloSpacecraft = new ApolloSpacecraft();
        Asteroid asteroidReference = new ExplodingAsteroid();
         asteroidReference.collideWith(spaceShip);
         asteroidReference.collideWith(apolloSpacecraft);
        System.out.println("\\nEND-- With dynamic dispatch");
         // -- OUTPUT --
         // BEGIN-- With dynamic dispatch
         // ExplodingAsteroid hits a Spaceship
         // ExplodingAsteroid hits a ApolloSpacecraft
         // END-- With dynamic dispatch
     }
     
    public static void withoutDoubleDispatch() {
        System.out.println("\\nBEGIN-- Without double dispatch");
        Asteroid asteroid = new Asteroid();
        Asteroid asteroidReference = new ExplodingAsteroid();
        Spaceship spaceShipReference = new ApolloSpacecraft();
         asteroid.collideWith(spaceShipReference);
         asteroidReference.collideWith(spaceShipReference);
        System.out.println("\\nEND-- Without double dispatch");
         // -- OUTPUT --
         // BEGIN-- Without double dispatch
         // Asteroid hits a Spaceship
         // ExplodingAsteroid hits a Spaceship
         // END-- Without double dispatch
         // Does not work as desired, Expected output was
         // Asteroid hits a ApolloSpacecraft
         // ExplodingAsteroid hits a ApolloSpacecraft
         // The problem is overridden methods are dispatched dynamically but
         // method overloading is done statically.
     }

// The problem described in withoutDynamicDispatch method can be resolved by simulating 
// double dispatch, for example by using a visitor pattern.

    public static void withDoubleDispatch() {
        System.out.println("\\nBEGIN-- With double dispatch");
        ExtendedSpaceship extSpaceshipReference = new ExtendedApolloSpacecraft();
        Asteroid asteroid = new Asteroid();
        Asteroid asteroidReference = new ExplodingAsteroid();
         extSpaceshipReference.collideWith(asteroid);
         extSpaceshipReference.collideWith(asteroidReference);
        System.out.println("\\nEND-- With double dispatch");
         // -- OUTPUT --
         // BEGIN-- With double dispatch
         // Asteroid hits a ApolloSpacecraft
         // ExplodingAsteroid hits a ApolloSpacecraft
         // END-- With double dispatch
         }
     }
```