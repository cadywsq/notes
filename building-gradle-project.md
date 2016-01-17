---
title: Building a Gradle Project by Example
date: 2016-01-11
---

* Building a full-stack Java project with Gradle
* Practicing efficient web application development
* Customizing default conventions to adapt to custom requirements
* Using the Gradle wrapper

Gradle provides a build-by-convention approach for certain domains like Java projects by introducing predefined project layouts with sensible defaults. But Gradle also allows for adapting its conventions to your needs.

# To Do application

We use a simple application to illustrate the use of Gradle: a To Do application.

The use case starts out as a plain Java application without a GUI, simply controlled through console input. We'll extend this application by adding components to learn more advanced concepts.

![enter image description here](https://i.imgur.com/h1n9CLe.png)
![enter image description here](https://i.imgur.com/sHYniaK.png)
* consists of an ordered list of action items or tasks
* a task has a title to represent the action needed to complete it
* tasks can be added to the list and removed from the list
* tasks can be marked active or completed to indicate their status
* a task’s title can be changed later
* changes to a task should automatically get persisted to a data store.
* filter tasks by their status: active or completed

In its first version, you’ll lay out its foundation by implementing the basic functionality controlled through the command line.

#### Components and the interactions between them

 A To Do application implements the typical create, read, update, and delete (**CRUD**) functionality.

For data to be persisted, you need to represent it by a **model**.

You’ll create a new Java class called **ToDoItem**, a plain old Java object (**POJO**) acting as a model.

To keep the first iteration of the solution as simple as possible, we won’t introduce a traditional data store like a database to store the model data. Instead, you’ll keep it in memory, which is easy to implement. The class implementing the persistence contract is called **InMemoryToDoRepository**.

Every **standalone** Java program is required to implement a main class, the applica- tion’s entry point. Your main class will be called **ToDoApp** and will run until the user decides to exit the program.

Each action command is mapped to an enum called **CommandLineInput**.

**CommandLineInputHandler** represents the glue between user interaction and command execution.

![enter image description here](https://i.imgur.com/xn5zvw7.png)

#### Building the application’s functionality

```java
package todo.model;

public class ToDoItem implements Comparable<ToDoItem>{
	private Long id;
	private String name;
	private boolean isCompleted;
	// getters and setters
	...
}
```

```java
// in memory persistence of the model
package todo.repository;

import todo.model.ToDoItem;
import java.util.Collection;

public interface ToDoRepository {
	List<ToDoItem> findAll();
	ToDoItem findById(Long id);
	Long insert(ToDoItem toDoItem);
	void update(ToDoItem toDoItem);
	void delete(ToDoItem toDoItem);
}
```

Next, you’ll create a **scalable and thread-safe implementation** of this interface.

```java
package todo.repository;

import todo.model.ToDoItem;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ConcurrentMap;
import java.util.concurrent.atomic.AtomicLong;

public class InMemoryToDoRepository implements ToDoRepository {
	private AtomicLong currentId = new AtomicLong();
	private ConcurrentMap<Long, ToDoItem> toDos
		= new ConcurrentHashMap<Long, ToDoItem>(); // in memory data store
	
	@Override
	public List<ToDoItem> findAll() {
		// convert map values to list
		List<ToDoItem> toDoItems = new ArrayList<ToDoItem>(toDos.values());
		Collections.sort(toDoItems);
		return toDoItems;
	}
	
	@Override
	public ToDoItem findById(Long id) {
		return toDos.get(id);
	}
	
	@Override
	public Long insert(ToDoItem toDoItem) {
		Long id = currentId.incrementAndGet();
		toDoItem.setId(id);
		toDos.putIfAbsent(id, toDoItem);
		return toDoItem.getId();
	}
	
	@Override
	public void update(ToDoItem toDoItem) {
		toDos.replace(toDoItem.getId(), toDoItem);
	}
	
	@Override
	public void delete(ToDoItem toDoItem) {
		toDos.remove(toDoItem.getId());
	}
}
```

The class ToDoApp prints the application’s options on the console, reads the user’s input from the prompt, **translates** the one-letter input into a command object, and handles it accordingly.

```java
package todo;
import todo.utils.CommandLineInput;
import todo.utils.CommandLineInputHandler;

public class ToDoApp {
	public static final char DEFAULT_INPUT = '\u0000';
	
	public static void main(String args[]) {
		CommandLineInputHandler commandLineInputHandler
			= new CommandLineInputHandler();
		char command = DEFAULT_INPUT;
		while(CommandLineInput.EXIT.getShortCmd() != command) {
			commandLineInputHandler.printOptions();
			String input = commandLineInputHandler.readInput();
			char[] inputChars = input.length() == 1 ? input.toCharArray() : new char[] { DEFAULT_INPUT };
			command = inputChars[0];	
			CommandLineInput commandLineInput = CommandLineInput.getCommandLineInputForInput(command);
			commandLineInputHandler.processInput(commandLineInput);
		}
	}
}
```

We’ll look at specific concerns like setting up the project with Gradle, compiling the source code, assembling the JAR file, and running the application.

#### Building a Java project

To assemble an executable program, the source code needs to be com- piled and the classes need to be packaged into a JAR file. The Java Development Kit (JDK) provides development tools like javac and jar that help with implementing these tasks. **You don’t want to run these tasks manually each and every time your source code changes.**

Gradle plugins act as enablers to automate these tasks. A plugin extends your proj- ect by introducing domain-specific conventions and tasks with sensible defaults.

The Java plugin  establishes a standard layout for your project and makes sure that tasks are executed in the correct order so they make sense in the context of a Java project.

First create the  `build.gradle` file and tell your project to use the Java plugin.

```
apply plugin: 'java'
```

By default, the plugin searches for production source code in the directory src/main/java.

One of the tasks the Java plugin adds to your project is named build. The build task compiles your code, runs your tests, and assembles the JAR file.

![enter image description here](https://i.imgur.com/6tJUQ0b.png)

Each line of the output represents an executed task provided by the Java plugin.

Some of the tasks are marked with the message UP-TO-DATE. That means that the task was skipped.

![enter image description here](https://i.imgur.com/z4AFO6J.png)

#### Customizing your project

Gradle gives you the option of customizing the conventions. Refer to Gradle’s Build Language Reference, available at http://www.gradle.org/docs/current/dsl/ to see what is configurable.

Running gradle properties gives you a list of configurable standard and plugin properties, plus their default values.

Specify a version number for your project and indi- cate the Java source compatibility.

Previously, you ran the To Do application using the java command. You told the Java runtime where to find the classes by assigning the build output directory to the classpath command-line option via -cp build/classes/ main.

To be able to start the application from the **JAR** file, the manifest MANIFEST.MF needs to contain the header Main-Class. Change the default values in the build script and add a header attribute to the JAR manifest.

```groovy
version = 0.1
sourceCompatibility = 1.6

jar {
	manifest {
		attributes 'Main-Class': "todo.ToDoApp'
	}
}
```

After assembling the JAR file, you’ll notice that the version number has been added to the JAR filename.

Rarely do enterprises start new software projects with a clean slate. All too often, you’ll have to integrate with a legacy system, migrate the technology stack of an existing project, or adhere to internal standards or limitations. A build tool has to be flexible enough to adapt to external constraints by configuring the default settings.

Let’s assume you started the project with a different directory layout.

![enter image description here](https://i.imgur.com/AU6tfTK.png)


**The key to customizing a build is knowledge of the underlying properties and DSL ele- ments.**

```java
String input = commandLineInputHandler.readInput();
char[] inputChars = input.length() == 1 ?
	input.toCharArray() :
	new char[] { DEFAULT_INPUT }; command = inputChars[0];
```

You can improve on this implementation by reusing a library that wraps this logic. The perfect match is the class CharUtils from the Apache Commons Lang library.

It provides a method called toChar that converts a String to a char by using just the first character, or a default character if the string’s value is empty.

```java
import org.apache.commons.lang3.CharUtils;

String input = commandLineInputHandler.readInput();
command = CharUtils.toChar(input, DEFAULT_INPUT);
```

In the Java world, dependencies are distributed and used in the form of JAR files. Many libraries are available in a **repository**, such as a file system or central server. Gradle requires you to define at least one repository to use a dependency.

We are going to use the publicly available, internet-accessible repository Maven Central.

```groovy
repositories {
	// Shortcut notation for configuring Maven Central 2 repository accessible under http://repo1.maven.org/maven2
	mavenCentral()
}

dependencies {
	compile group: 'org.apache.commons',
	name: 'commons-lang3',
	version: '3.1'
}
```

In Gradle, dependencies are grouped by configurations. One of the configurations that the Java plugin introduces is compile. It’s used for dependencies needed for compiling source code.

#### Web development with Gradle

In Java, server-side web components of the Enterprise Edition (Java EE) provide the dynamic extension capabilities for running your application within a web container or application server. As the name Servlet may already indicate, it serves a client request and constructs the response. It acts as the controller component in a Model-View-Controller (MVC) architecture. The response of a Servlet is rendered by the view component— the Java Server Page (JSP).

![enter image description here](https://i.imgur.com/1KaBuCD.png)

A WAR (web application archive) file is used to bundle web components, compiled classes, and other resource files like deployment descriptors, HTML, JavaScript, and CSS files. Together they form a web application. To run a Java web application, the WAR file needs to be deployed to the server environment, a web container.

Gradle provides out-of-the-box plugins for assembling WAR files and deploying web applications to a local Servlet container.

The Java enterprise landscape is dominated by a wide range of web frameworks, such as **Spring MVC** and Tapestry. Web frameworks are designed to abstract the standard web components and reduce boilerplate code. To keep the example as simple and understandable as possible, we’ll stick to the standard Java enterprise web components.

The Servlet class you’re going to create is called ToDoServlet. It’s responsible for accepting HTTP requests, executing a CRUD operation mapped to a URL endpoint, and forwarding the request to a JSP.

![enter image description here](https://i.imgur.com/SAsvbIX.png)

Gradle provides extensive support for building and running web applications such as **War** and **Jetty**.

The War plugin extends the Java plugin by adding conventions for web application devel- opment and support for assembling WAR files. 

Jetty comes with an embedded implementation by adding an HTTP module to your application. Gradle’s Jetty plugin extends the War plugin, provides tasks for deploying a web application to the embedded container, and runs your application.

The War plugin extends the Java plugin. So don’t have to apply the Java plugin anymore.

```groovy
apply plugin: 'war'
```

In addition to the conventions provided by the Java plugin, your project becomes aware of a source directory for web applica- tion files and knows how to assemble a WAR file instead of a JAR file.

The default convention for web application sources is the directory src/main/webapp.

![enter image description here](https://i.imgur.com/akxX1CY.png)

You implemented your web application with the help of classes that aren’t part of the Java Standard Edition, such javax.servlet.HttpServlet. Before you run the build, you’ll need to make sure that you declare those external dependencies.

The War plugin introduces two new **dependency configurations**. The configuration you’ll use for the Servlet dependency is providedCompile.

It’s used for dependencies that are required for compilation but provided by the runtime environment. The runtime environment in this case is **Jetty**. As a consequence, dependencies marked provided aren’t going to be packaged with the WAR file. Runtime dependencies like the JSTL library aren’t needed for the compilation process, but are needed at **runtime**. They’ll become part of the WAR file.

```groovy
dependencies {
	providedCompile 'javax.servlet:servlet-api:2.5'
	runtime 'javax.servlet:jstl:1.1.2' }
```

Even if your project doesn’t adhere to Gradle’s standard conventions, the plugin can be used to build a WAR file. Let’s look at some customization options.

![enter image description here](https://i.imgur.com/VAg3CsG.png)

![enter image description here](https://i.imgur.com/HFWlgCv.png)

The War plugin exposes the convention property webAppDirName. The default value src/ main/webapp is easily switched to webfiles by assigning a new value.

Directories can be selectively added to the WAR file by invoking the from method.

If you’re looking for a configuration parameter, the best place to check is the War plugin DSL guide

An embedded Servlet container doesn’t know anything about your application until you provide the exact classpath and relevant source directories of your web application. Usually, you’d do that programmatically. Internally, the **Jetty plugin** does all this work for you. As the War plugin exposes all this information, it can be accessed at run- time by the Jetty plugin. This is a typical example of **a plugin using another plugin’s configuration** through the Gradle API.

```groovy
apply plugin: 'jetty'
```

The task you’re going to use to run the web application is jettyRun. It’ll start the Jetty container without even having to create a WAR file. The output of running the task on the command line should look similar to this:

```
$ gradle jettyRun
:compileJava
:processResources UP-TO-DATE
:classes
> Building > :jettyRun > Running at http://localhost:8080/todo-webapp-jetty
```

```groovy
jettyRun {
	httpPort = 9090
	contextPath = 'todo'
}
```

#### Gradle wrapper

Picking the wrong version of the build tool distribution or the runtime environment may have a detrimental effect on the outcome of the build.

Gradle provides a very convenient and practical solution to this problem: the Gra- dle wrapper, which enables a machine to run a Gradle build script without having to install the runtime. It also ensures that the build script is run with a specific version of Gradle.

To set up the wrapper for your project, you’ll need to do two things: create a wrapper task and execute the task to generate the wrapper files.

![enter image description here](https://i.imgur.com/PMXMheb.png)

As a result, you’ll find the following wrapper files alongside your build script.

![enter image description here](https://i.imgur.com/2y7bEzs.png)

You’ll only need to run gradle wrapper on your project once. From that point on, you can use the wrapper’s script to execute your build. The downloaded wrapper files are supposed to be checked into version control.

To upgrade your wrapper version later you can change the `gradleVersion` and rerunning the wrapper task.

As part of the wrapper distribution, a command execution script is provided. For *nix systems, this is the shell script `gradlew`.

![enter image description here](https://i.imgur.com/JE3NoCM.png)

The distribution ZIP file is downloaded from a central server hosted by the Gradle project, stored on Mike’s local file system under $HOME_DIR/.gradle/wrapper/dists.

A build script executed by the Gradle wrapper provides exactly the same tasks, features, and behavior as it does when run with a local Gradle installation.

You can change the default properties to target an enterprise server hosting the runtime distribution. And while you’re at it, you’ll also change the local storage directory.

```groovy
task wrapper(type: Wrapper) {
	gradleVersion = '1.2'
	distributionUrl = 'http://myenterprise.com/gradle/dists'
	// Path where wrapper will be unzipped relative to Gradle home directory
	distributionPath = 'gradle-dists'
}
```

http://gradle.org/docs/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html

Not only does using wrapper allow you to run the project on a machine that doesn’t have Gradle installed, it also prevents version compatibility issues.

http://www.gradle.org/docs/current/userguide/standard_plugins.html



