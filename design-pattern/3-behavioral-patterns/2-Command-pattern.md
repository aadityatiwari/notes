[Command pattern](https://en.wikipedia.org/wiki/Command_pattern)
------------

The **command pattern** is a [behavioral](https://en.wikipedia.org/wiki/Behavioral_Pattern) [design pattern](https://en.wikipedia.org/wiki/Design_pattern_(computer_science)) in which an object is used to [encapsulate](https://en.wikipedia.org/wiki/Information_Hiding) all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

Four terms always associated with the command pattern are *command*, *receiver*, *invoker* and *client*. 

A *command* object knows about *receiver* and invokes a method of the receiver. Values for parameters of the receiver method are stored in the command. The *receiver* then does the work.

An *invoker* object knows how to execute a command, and optionally does bookkeeping about the command execution. The invoker does not know anything about a concrete command, it knows only about command interface.

Both an invoker object and several command objects are held by a *client* object. The client decides which commands to execute at which points. To execute a command, it passes the command object to the invoker object.

Using command objects makes it easier to construct general components that need to delegate, sequence or execute method calls at a time of their choosing without the need to know the class of the method or the method parameters.

Using an invoker object allows bookkeeping about command executions to be conveniently performed, as well as implementing different modes for commands, which are managed by the invoker object, without the need for the client to be aware of the existence of bookkeeping or modes.

Command objects are useful for implementing

-   **GUI buttons and menu items:** Java Swing, Action is a command object

-   **Macro recording**: represent user actions using command object and record action sequence

-   **Mobile code**: In Java where code can be streamed/slurped from one location to another via URLClassloaders and Codebases, the commands can enable new behavior to be delivered to remote locations (EJB Command, Master Worker)

-   **Multi-level undo:** If all user actions in a program are implemented as command objects, the program can keep a stack of the most recently executed commands. When the user wants to undo a command, the program simply pops the most recent command object and executes its undo() method.

-   **Networking:** Send whole command objects across the network to be executed on the other machines, for example player actions in computer games.

-   **Parallel processing:** Where the commands are written as tasks to a shared resource and executed by many threads in parallel (possibly on remote machines -this variant is often referred to as the Master/Worker pattern)

-   **Progress bars:** Suppose a program has a sequence of commands that it executes in order. If each command object has a getEstimatedDuration() method, the program can easily estimate the total duration. It can show a progress bar that meaningfully reflects how close the program is to completing all the tasks.

-   **Thread pools:** A typical, general-purpose thread pool class might have a public addTask() method that adds a work item to an internal queue of tasks waiting to be done. It maintains a pool of threads that execute commands from the queue. The items in the queue are command objects. Typically these objects implement a common interface such as java.lang.Runnable that allows the thread pool to execute the command even though the thread pool class itself was written without any knowledge of the specific tasks for which it would be used.

-   **Transactional behavior:** Similar to undo, a database engine or software installer may keep a list of operations that have been or will be performed. Should one of them fail, all others can be reversed or discarded (usually called *rollback*). For example, if two database tables that refer to each other must be updated, and the second update fails, the transaction can be rolled back, so that the first table does not now contain an invalid reference.

-   **Wizards:** Often a wizard presents several pages of configuration for a single action that happens only when the user clicks the "Finish" button on the last page. In these cases, a natural way to separate user interface code from application code is to implement the wizard using a command object. The command object is created when the wizard is first displayed. Each wizard page stores its GUI changes in the command object, so the object is populated as the user progresses. "Finish" simply triggers a call to execute(). This way, the command class will work.

> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Command_pattern.svg/700px-Command_pattern.svg.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Command_pattern.svg/700px-Command_pattern.svg.png" width="616" height="231" />

Consider a "simple" switch. In this example we configure the Switch with two commands: to turn the light on and to turn the light off.

A benefit of this particular implementation of the command pattern is that the switch can be used with any device, not just a light - the Switch in the following example turns a light on and off, but the Switch's constructor is able to accept any subclasses of Command for its two parameters. For example, you could configure the Switch to start an engine.

*Before Java 8:*

```java
import java.util.List;
import java.util.ArrayList;

/** The Command interface */
public interface Command {
   void execute();
}

/** The Invoker class */
public class Switch {
   private List<Command> history = new ArrayList<Command>();

   public void storeAndExecute(final Command cmd) {
      this.history.add(cmd); // optional
      cmd.execute();
   }
}

/** The Receiver class */
public class Light {

   public void turnOn() {
      System.out.println("The light is on");
   }

   public void turnOff() {
      System.out.println("The light is off");
   }
}

/** The Command for turning on the light - ConcreteCommand #1 */
public class FlipUpCommand implements Command {
   private Light theLight;

   public FlipUpCommand(final Light light) {
      this.theLight = light;
   }

   @Override    // Command
   public void execute() {
      theLight.turnOn();
   }
}

/** The Command for turning off the light - ConcreteCommand #2 */
public class FlipDownCommand implements Command {
   private Light theLight;

   public FlipDownCommand(final Light light) {
      this.theLight = light;
   }

   @Override    // Command
   public void execute() {
      theLight.turnOff();
   }
}

/* The test class or client */
public class PressSwitch {
   public static void main(final String[] arguments){
      // Check number of arguments
      if (arguments.length != 1) {
         System.err.println("Argument \"ON\" or \"OFF\" is required.");
         System.exit(-1);
      }

      final Light lamp = new Light();
	  
      final Command switchUp = new FlipUpCommand(lamp);
      final Command switchDown = new FlipDownCommand(lamp);

      final Switch mySwitch = new Switch();

      switch(arguments[0]) {
         case "ON":
            mySwitch.storeAndExecute(switchUp);
            break;
         case "OFF":
            mySwitch.storeAndExecute(switchDown);
            break;
         default:
            System.err.println("Argument \"ON\" or \"OFF\" is required.");
            System.exit(-1);
      }
   }
}
```

*Java 8: Using a functional interface*

```java
/**
 * The Command functional interface.<br/>
 */
@FunctionalInterface
public interface Command {
	public void apply();
}

/**
 * The CommandFactory class.<br/>
 */
import java.util.HashMap;
import java.util.stream.Collectors;

public final class CommandFactory {
	private final HashMap<String, Command>	commands;

	private CommandFactory() {
		commands = new HashMap<String,Command>();
	}

	public void addCommand(final String name, final Command command) {
		commands.put(name, command);
	}

	public void executeCommand(String name) {
		if (commands.containsKey(name)) {
			commands.get(name).apply();
		}
	}

	public void listCommands() {
		System.out.println("Enabled commands: " + commands.keySet().stream().collect(Collectors.joining(", ")));
	}

	/* Factory pattern */
	public static CommandFactory init() {
		final CommandFactory cf = new CommandFactory();

		// commands are added here using lambdas. It is also possible to dynamically add commands without editing the code.
		cf.addCommand("Light on", () -> System.out.println("Light turned on"));
		cf.addCommand("Light off", () -> System.out.println("Light turned off"));

		return cf;
	}
}

public final class Main {
	public static void main(final String[] arguments) {
		final CommandFactory cf = CommandFactory.init();
		
		cf.executeCommand("Light on");
		cf.listCommands();
	}
}
```