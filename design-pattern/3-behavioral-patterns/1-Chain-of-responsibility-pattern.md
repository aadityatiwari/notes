[Chain-of-responsibility pattern](https://en.wikipedia.org/wiki/Chain-of-responsibility_pattern)
------------

**Chain-of-responsibility pattern** is a [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)) consisting of a source of [command objects](https://en.wikipedia.org/wiki/Command_pattern) and a series of **processing objects**. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain. A mechanism also exists for adding new processing objects to the end of this chain.


In a variation of the standard chain-of-responsibility model, some handlers may act as [dispatchers](https://en.wikipedia.org/wiki/Dynamic_dispatch), capable of sending commands out in a variety of directions, forming a *tree of responsibility*. 


This pattern promotes the idea of [loose coupling](https://en.wikipedia.org/wiki/Loose_coupling).


In this example we have different roles, each having a fixed purchasing limit and a successor. Every time a user in a role receives a purchase request that exceeds his or her limit, the request is passed to his or her successor.


*Example*
---------

```java
abstract class PurchasePower {
    protected static final double BASE = 500;
    protected PurchasePower successor;

    abstract protected double getAllowable();
    abstract protected String getRole();

    public void setSuccessor(PurchasePower successor) {
        this.successor = successor;
    }

    public void processRequest(PurchaseRequest request){
        if (request.getAmount() < this.getAllowable()) {
            System.out.println(this.getRole() + " will approve $" + request.getAmount());
        } else if (successor != null) {
            successor.processRequest(request);
        }
    }
}
```

Four implementations of the abstract class above: Manager, Director, Vice President, President


```java
class ManagerPPower extends PurchasePower {
    
    protected double getAllowable() {
        return BASE * 10;
    }

    protected String getRole() {
        return "Manager";
    }
}

class DirectorPPower extends PurchasePower {
    
    protected double getAllowable() {
        return BASE * 20;
    }

    protected String getRole() {
        return "Director";
    }
}

class VicePresidentPPower extends PurchasePower {
    
    protected double getAllowable() {
        return BASE * 40;
    }

    protected String getRole() {
        return "Vice President";
    }
}

class PresidentPPower extends PurchasePower {

    protected double getAllowable() {
        return BASE * 60;
    }

    protected String getRole() {
        return "President";
    }
}
```

The following code defines the PurchaseRequest class that keeps the request data in this example.

```java
class PurchaseRequest {

    private double amount;
    private String purpose;

    public PurchaseRequest(double amount, String purpose) {
        this.amount = amount;
        this.purpose = purpose;
    }

    public double getAmount() {
        return this.amount;
    }

    public void setAmount(double amount)  {
        this.amount = amount;
    }

    public String getPurpose() {
        return this.purpose;
    }
    public void setPurpose(String purpose) {
        this.purpose = purpose;
    }
}
```

In the following usage example, the successors are set as follows: Manager -> Director -> Vice President -> President

```java
class CheckAuthority {

    public static void main(String[] args) {
        ManagerPPower manager = new ManagerPPower();
        DirectorPPower director = new DirectorPPower();
        VicePresidentPPower vp = new VicePresidentPPower();
        PresidentPPower president = new PresidentPPower();
        manager.setSuccessor(director);
        director.setSuccessor(vp);
        vp.setSuccessor(president);

        // Press Ctrl+C to end.
        try {
            while (true) {
                System.out.println("Enter the amount to check who should approve your expenditure.");
                System.out.print(">");
                double d = Double.parseDouble(new BufferedReader(new InputStreamReader(System.in)).readLine());
                manager.processRequest(new PurchaseRequest(d, "General"));
            }
        }
        catch (Exception exc) {
            System.exit(1);
        }
    }
}
```