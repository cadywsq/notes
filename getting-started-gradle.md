---
title: Getting Started with Gradle
date: 2016-01-10
---

# Installing Gradle

You can download the distribution directly from the Gradle homepage at http://gradle.org/downloads.

To reference your Gradle runtime in the shell, you’ll need to create the environment variable GRADLE_HOME and add the binaries to your shell’s execution path.

```bash
export GRADLE_HOME=/opt/gradle
export PATH=$PATH:$GRADLE_HOME/bin
```

```
$ gradle –v

------------------------------------------------------------
Gradle 1.7
------------------------------------------------------------

Build time: 2013-08-06 11:19:56 UTC
Build number: none
Revision: 9a7199efaf72c620b33f9767874f0ebced135d83

Groovy: 1.8.6
Ant: Apache Ant(TM) version 1.8.4 compiled on May 22 2012
Ivy: 2.2.0
JVM: 1.6.0_51 (Apple Inc. 20.51-b01-457)
OS: Mac OS X 10.8.4 x86_64
```

Now that you’re all set, you’ll implement a simple build script with Gradle. Even though most of the popular IDEs provide a Gradle plugin, all you need now is your favorite editor.

# Getting started with Gradle

Every Gradle build starts with a script. The default naming convention for a Gradle build script is build.gradle.

Within that script, define a single atomic piece of work. In Gradle’s vocabulary, this is called a **task**.

``` groovy
task helloWorld {
    doLast {
        println 'Hello world!'
	}
}
```

The method println is Groovy’s shorter equivalent to Java’s System.out.println.

```
$ gradle –q helloWorld
Hello world!
```

By defining the optional command-line option quiet with –q, you tell Gradle to only output the task’s output.

Tasks and actions are impor- tant elements of the language. An action named doLast is almost self-expressive. It’s the last action that’s executed for a task.

The left shift operator << is a shortcut for the action doLast.

```groovy
task helloWorld << {
    println 'Hello world!'
}
```

#### Dynamic task definition and task chaining

```groovy
task startSession << {
    chant()
}
def chant() {
    // implicit ant task usage
    ant.echo(message: 'Repeat after me...')
}
3.times {
    // dynamic task difinition
    task "yayGradle$it" << {
        println 'Gradle rocks'
    }
}
// task dependencies
yayGradle0.dependsOn startSession
yayGradle2.dependsOn yayGradle1, yayGradle0
task groupTherapy(dependsOn: yayGradle2)
```

`dependsOn` to indicate dependencies between tasks.

Depended-on task will always be executed before the task that defines the dependency. Under the hood, **dependsOn** is actually a method of a task.

Every script is equipped with a property called ant that grants direct access to Ant tasks. In this example, you print out the message “Repeat after me” using the Ant task echo to start the therapy session.

A nifty feature Gradle provides is the definition of **dynamic tasks**, which specify their name at runtime. Your script creates three new tasks within a loop using Groovy’s times method extension on `java.lang.Number`. Groovy automatically exposes an implicit variable named `it` to indicate the loop iteration index.

```
$ gradle groupTherapy
:startSession
[ant:echo] Repeat after me...
:yayGradle0 Gradle rocks
:yayGradle1 Gradle rocks
:yayGradle2 Gradle rocks
:groupTherapy
```

![enter image description here](https://i.imgur.com/BjExzR3.png)

# Using the Command line

#### Listing available tasks of a project

Gradle provides a helper task named tasks to introspect your build script and display each available task, including a descriptive message of its purpose.

![enter image description here](https://i.imgur.com/bKHHOPn.png)

Gradle provides the concept of a task group, which can be seen as a cluster of tasks assigned to that group.

Out of the box, each build script exposes the task group Help tasks without any additional work from the developer.

If a task doesn’t belong to a task group, it’s displayed under Other tasks.

The --all option is a great way to determine the execution order of a task graph before actually executing it.

![enter image description here](https://i.imgur.com/WJ2Qx0I.png)

#### Task execution

You can also exe- cute multiple tasks in a single build run by defining them as command-line parame- ters. Running gradle `yayGradle0` `groupTherapy` would execute the task `yayGradle0` first and the task `groupTherapy` second.

Tasks are always executed just once, no matter whether they’re specified on the command line or act as a dependency for another task.

![enter image description here](https://i.imgur.com/LLfrZbg.png)

You see the same output as if you’d just run gradle groupTherapy. The correct order was preserved and each of the tasks was only executed once.

#### Gradle daemon

When using Gradle on a day-to-day basis, you’ll find yourself having to run your build repetitively. This is especially true if you’re working on a web application. You change a class, rebuild the web application archive, bring up the server, and reload the URL in the browser to see your changes being reflected.

Many developers prefer test-driven development to implement their application. For continuous feedback on their code quality, they run their unit tests over and over again to find code defects early on.

In both cases, you’ll notice a significant productivity hit. Each time you initiate a build, the JVM has to be started, Gradle’s dependencies have to be loaded into the class loader, and the project object model has to be constructed. This procedure usually takes a couple of seconds.

The daemon runs Gradle as a background process. Once started, the gradle com- mand will reuse the forked daemon process for subsequent builds, avoiding the startup costs altogether.

It’s easy to start the Gradle daemon on the command line: simply add the option --daemon to your gradle command. In a shell run the command ps | grep gradle to list the pro- cesses that contain the name gradle.

To stop the daemon process, man- ually run `gradle --stop`.

http://gradle.org/docs/current/userguide/gradle_daemon.html





