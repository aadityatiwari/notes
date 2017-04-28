[Java programming language](https://en.wikipedia.org/wiki/Java_(programming_language))
--------------------------------------------------------------------------------------

[Concurrent](https://en.wikipedia.org/wiki/Concurrent_computing), [class-based](https://en.wikipedia.org/wiki/Class-based_programming), [object-oriented](https://en.wikipedia.org/wiki/Object-oriented_programming), and specifically designed to have less implementation dependencies.

As of 2016, only Java 8 is supported ("publicly"). Major release versions of Java, along with their release dates:

    - JDK 1.0 (January 21, 1996)

    -   JDK 1.1 (February 19, 1997)

    -   J2SE 1.2 (December 8, 1998)

    -   J2SE 1.3 (May 8, 2000)

    -   J2SE 1.4 (February 6, 2002)

    -   J2SE 5.0 (September 30, 2004)

    -   Java SE 6 (December 11, 2006)

    -   Java SE 7 (July 28, 2011)

    -   Java SE 8 (March 18, 2014)

	
Java contains multiple types of garbage collectors. By default, HotSpot uses the parallel scavenge garbage collector (Enabled using -XX:UseParallelGC). For 90% of applications in Java, the Concurrent Mark-Sweep (CMS) garbage collector (-XX:+UseConcMarkSweepGC) is sufficient.

All code is written inside classes, and every data item is an object, with the exception of the primitive data types, i.e. integers, floating-point numbers, [boolean values](https://en.wikipedia.org/wiki/Boolean_data_type), and characters, which are not objects for performance reasons.

Unlike C++, Java does not support [operator overloading](https://en.wikipedia.org/wiki/Operator_overloading) or [multiple inheritance](https://en.wikipedia.org/wiki/Multiple_inheritance) for classes, though multiple inheritance is supported for [interfaces](https://en.wikipedia.org/wiki/Interface_(Java)). This simplifies the language and aids in preventing potential errors and [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern) design.

Anonymous classes post compilation have class file names as concatenation of the name of their enclosing class, $, an integer.

Static methods cannot access any class members that are not also static.

The method name "main" is not a keyword in the Java language. It is simply the name of the method the Java launcher calls to pass control to the program. public static void main(String... args)

The [**System**](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html) class defines a public static field called [**out**](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#out). The out object is an instance of the [PrintStream](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html) class and provides many methods for printing data to [standard out](https://en.wikipedia.org/wiki/Standard_streams), including [**println(String)**](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html#println(java.lang.String)) which also appends a new line to the passed string.

Servlets are server-side Java EE components that generate responses (typically HTML pages) to requests (typically HTTP requests) from clients. A servlet runs on the server side.


```java
import java.io.* ;
import javax.servlet.* ;

public class Hello extends GenericServlet {

	public void service(final ServletRequest request, final ServletResponse response)
	throws ServletException,
	IOException {
		response.setContentType("text/html");
		final PrintWriter pw = response.getWriter();
		try {
			pw.println("Hello, world!");
		}
		finally {
			pw.close();
		}
	}
}
```


GenericServlet class provides the interface for the [server](https://en.wikipedia.org/wiki/Server_(computing)) to forward requests to the servlet and control the servlet's lifecycle.

[**service(ServletRequest, ServletResponse)**](https://docs.oracle.com/javaee/7/api/javax/servlet/Servlet.html#service(javax.servlet.ServletRequest,javax.servlet.ServletResponse)) method defined by the [Servlet](https://docs.oracle.com/javaee/7/api/javax/servlet/Servlet.html) [interface](https://en.wikipedia.org/wiki/Interface_(Java)) 

The service() method is passed: a [**ServletRequest**](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html) object that contains the request from the client and a [**ServletResponse**](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletResponse.html) object used to create the response returned to the client.

JavaServer Pages (JSP) are [server-side](https://en.wikipedia.org/wiki/Server-side) Java EE components. JSPs embed Java code in an HTML page by using the special [delimiters](https://en.wikipedia.org/wiki/Delimiter) &lt;% and %&gt;.

A JSP is compiled to a Java servlet, a Java application in its own right, the first time it is accessed. After that, the generated servlet creates the response.

The [Java Class Library](https://en.wikipedia.org/wiki/Java_Class_Library) is the [standard library](https://en.wikipedia.org/wiki/Standard_library), developed to support application development in Java. The class library contains features such as:

-   The core libraries, which include:

    -   IO/NIO

    -   Networking

    -   [Reflection](https://en.wikipedia.org/wiki/Reflection_(computer_programming))

    -   [Concurrency](https://en.wikipedia.org/wiki/Concurrent_computing)

    -   [Generics](https://en.wikipedia.org/wiki/Generics_in_Java)

    -   Scripting/Compiler

    -   [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming) (Lambda, Streaming)

    -   [Collection libraries](https://en.wikipedia.org/wiki/Java_collections_framework) that implement [data structures](https://en.wikipedia.org/wiki/Data_structure) such as [lists](https://en.wikipedia.org/wiki/List_(abstract_data_type)), [dictionaries](https://en.wikipedia.org/wiki/Associative_array), [trees](https://en.wikipedia.org/wiki/Tree_structure), [sets](https://en.wikipedia.org/wiki/Set_(abstract_data_type)), [queues](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) and [double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue), or [stacks](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))

    -   [XML](https://en.wikipedia.org/wiki/XML) Processing (Parsing, Transforming, Validating) libraries

    -   [Security](https://en.wikipedia.org/wiki/Computer_security)

    -   [Internationalization and localization](https://en.wikipedia.org/wiki/Internationalization_and_localization) libraries

-   The integration libraries, which allow the application writer to communicate with external systems. These libraries include:

    -   The [Java Database Connectivity](https://en.wikipedia.org/wiki/Java_Database_Connectivity) (JDBC) [API](https://en.wikipedia.org/wiki/Application_programming_interface) for database access

    -   [Java Naming and Directory Interface](https://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface) (JNDI) for lookup and discovery

    -   [RMI](https://en.wikipedia.org/wiki/Java_remote_method_invocation) and [CORBA](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture) for distributed application development

    -   [JMX](https://en.wikipedia.org/wiki/Java_Management_Extensions) for managing and monitoring applications

-   [User interface](https://en.wikipedia.org/wiki/User_interface) libraries, which include:

    -   The (heavyweight, or [native](https://en.wikipedia.org/wiki/Native_(computing))) [Abstract Window Toolkit](https://en.wikipedia.org/wiki/Abstract_Window_Toolkit) (AWT), which provides [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface) components, the means for laying out those components and the means for handling events from those components

    -   The (lightweight) [Swing](https://en.wikipedia.org/wiki/Swing_(Java)) libraries, which are built on AWT but provide (non-native) implementations of the AWT widgetry

    -   APIs for audio capture, processing, and playback

    -   [JavaFX](https://en.wikipedia.org/wiki/JavaFX)

-   A platform dependent implementation of the Java virtual machine that is the means by which the bytecodes of the Java libraries and third party applications are executed

-   Plugins, which enable [applets](https://en.wikipedia.org/wiki/Java_applet) to be run in web browsers

-   [Java Web Start](https://en.wikipedia.org/wiki/Java_Web_Start), which allows Java applications to be efficiently distributed to [end users](https://en.wikipedia.org/wiki/End_user) across the [Internet](https://en.wikipedia.org/wiki/Internet)

-   Licensing and documentation
