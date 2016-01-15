---
title: Introducing Gradle
date: 2016-01-08
---

## Introduction to project automation
### Anatomy of a build tool

#### BUILD FILE

* contains the configuration needed for the build
* defines external dependencies such as third-party libraries
* contains instructions to achieve a specific goal in the form of tasks and their interdependencies

![enter image description here](https://i.imgur.com/zjyZJrV.png)

Oftentimes, a scripting language is used to express the build logic. That’s why a build file is also referred to as a build script.

#### BUILD INPUTS AND OUTPUTS

Complex task dependency graphs may use the output of a dependent task as input.

![enter image description here](https://i.imgur.com/H2uDnAG.png)

#### BUILD ENGINE

Processes the build file at run- time, resolves dependencies between tasks, and sets up the entire configuration needed to command the execution.

Once the internal model is built, the engine will execute the series of tasks in the correct order.

![enter image description here](https://i.imgur.com/YUVMYN5.png)

#### DEPENDENCY MANAGER

![enter image description here](https://i.imgur.com/FdNXxQl.png)

The dependency manager is used to process declarative dependency definitions for your build file, resolve them from an artifact repository (for example, the local file sys- tem, an FTP, or an HTTP server), and make them available to your project.

Many libraries depend on other libraries, called transitive dependencies. The depen- dency manager can use metadata stored in the repository to automatically resolve transitive dependencies as well.

### Apache Maven

The Maven team realized the need for a standardized proj- ect layout and unified build lifecycle.

Maven picks up on the idea of convention over configuration, meaning that it provides sensible default values for your project config- uration and its behavior.

Maven’s core functionality can be extended by custom logic developed as plugins.

#### STANDARD DIRECTORY LAYOUT

By introducing a default project layout, Maven ensures that every developer with the knowledge of one Maven project will immediately know where to expect specific file types.

![enter image description here](https://i.imgur.com/OgZIxpA.png)

#### DEPENDENCY MANAGEMENT

![enter image description here](https://i.imgur.com/PvZd9fl.png)

At runtime, the declared libraries and their transitive dependencies are downloaded by Maven’s dependency manager, stored in the local cache for later reuse, and made available to your build.

![enter image description here](https://i.imgur.com/aEHaJUp.png)

Dependency management in Maven isn’t limited to external libraries. You can also declare a dependency on other Maven projects. This need arises if you decompose software into modules, which are smaller components based on associated functionality.

![enter image description here](https://i.imgur.com/evaYfWA.png)

#### SAMPLE BUILD SCRIPT

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 ➥ http://maven.apache.org/xsd/maven-4.0.0.xsd">

<modelVersion>4.0.0</modelVersion>
<groupId>com.mycompany.app</groupId>
<artifactId>my-app</artifactId>
<packaging>jar</packaging>
<version>1.0</version>

<dependencies>
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-lang3</artifactId>
	<version>3.1</version>
	<scope>compile</scope>
	</dependency>
</dependencies>
</project>
```
**`<packaging>jar</packageing>` defines the type of artifact produced by project**

#### SHORTCOMINGS

 * proposes a default structure and lifecycle, often is too restrictive
   and may not fit your project’s needs
 * writing custom extensions for Maven is overly cumbersome
* earlier versions of Maven (< 2.0.9) automatically try to update their own core plugins; for example, support for unit tests to the latest version. may cause brittle and unstable builds

### Requirements for a next-generation build tool

* expressive, declarative, and maintainable build language.
* standardized project layout and lifecycle, but full flexibility and the option to fully configure the defaults.
* easy-to-use and flexible ways to implement custom logic
* support for project structures that consist of more than one project to build deliverable
* support for dependency management
* good integration and migration of existing build infrastructure, including the ability to import existing Ant build scripts and tools to translate existing Ant/ Maven logic into its own rule set
* emphasis on scalable and high-performance builds

## Next-generation builds with Gradle

### Why Gradle?

![enter image description here](https://i.imgur.com/PG1Mnkf.png)

Gradle combines the best features from other build tools.

Compare with maven.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

<modelVersion>4.0.0</modelVersion>
<groupId>com.mycompany.app</groupId>
<artifactId>my-app</artifactId>
<packaging>jar</packaging>
<version>1.0-SNAPSHOT</version>

<dependencies>
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.11</version>
		<scope>test</scope>
	</dependency>
</dependencies>
</project>
```

```groovy
apply plugin: 'java'
group = 'com.mycompany.app'
archivesBaseName = 'my-app'
version = '1.0-SNAPSHOT'

repositories {
	mavenCentral()
}

dependencies {
	testCompile 'junit:junit:4.11'
}
```
Compelling feature set.

![enter image description here](https://i.imgur.com/ZusXesK.png)

The key to unlocking Gradle’s power features within your build script lies in discover- ing and applying its domain model.

![enter image description here](https://i.imgur.com/mvrs7f6.png)

A build script directly maps to an instance of type Project in Gradle’s API. The dependencies configuration block in the build script invokes the method dependencies() of the project instance.

Available as HTML Javadoc documentation on Gradle’s website at http://www.gradle.org/docs/current/javadoc/index.html.

Each element in a Gradle script has a one-to-one representation with a Java class.

By exposing **hooks** into lifecycle phases, Gradle allows for monitoring and configuring your build script’s execution behavior. Let’s assume you have the very unique requirement of send- ing out an email to the development team whenever a unit test failure occurs. By writing a custom **test listener** that’s notified after the test execution lifecycle event, you can easily incorporate this feature for your build.

Gradle establishes a vocabulary for its model by exposing a **DSL implemented in Groovy**. When dealing with a complex problem domain, in this case the task of build- ing software, being able to use a common language to express your logic can be a pow- erful tool. 

Most common to builds is the notation of a unit of work that you want to get executed. Gradle describes this unit of work as a **task**. Part of Gradle’s standard DSL is the ability to define tasks very specific to compiling and packaging Java source code.

A good place to start is the Gradle Build Language Reference Guide at http://www.gradle.org/docs/current/dsl/index.html.

#### Gradle is Groovy

Prominent build tools like Ant and Maven define their build logic through XML. As we all know, XML is easy to read and write, but can become a maintenance nightmare if used in large quantities. XML isn’t very expressive. It makes it hard to define com- plex custom logic.

Gradle takes a different approach. Under the hood, Gradle’s DSL is written with Groovy providing syntactic sugar on top of Java.

Battle- scarred Groovy veterans will assure you that using Groovy instead of Java will boost your productivity by orders of magnitude.

#### Flexible conventions

Default tasks are provided that make sense in the context of a Java project. For example, you can compile your Java production source code, run tests, and assemble a JAR file. Every Java project starts with a standard directory layout. It defines where to find production source code, resource files, and test code. **Convention properties** are used to change the defaults.

The same concept applies to other project archetypes like Scala, Groovy, web proj- ects, and many more. Gradle calls this concept **build by convention.**

You can concentrate on what needs to be configured.

**Maven** is very opinionated; it proposes that a project only contains one Java source directory and only produces one single JAR file. This is not necessarily reality for many enterprise projects.

On the opposite side of the spectrum, **Ant** never gave you a lot of guidance on how to structure your build script, allowing for a maximum level of flexibility.

Gradle takes the middle ground by offering conventions combined with the ability to easily change them.

> Gradle is an opinionated framework on top of an unopinionated toolkit.

![enter image description here](https://i.imgur.com/v0msyOV.png)

#### Integration with other build tools

Maven POMs and plugins will be treated as Gradle natives. Maven and Ivy repositories have become an important part of today’s build infrastructure. Retrieving dependencies from a repository is only one part of the story; publishing to them is just as important. With a little configuration, Gradle can upload your project’s artifact for companywide or public consumption.

### The bigger picture: continuous delivery

Being able to build your source code is only one aspect of the software delivery pro- cess. More importantly, you want to release your product to a production environment to **deliver** business value.

Along the way, you want to run tests, build the distribution, analyze the code for quality-control purposes, potentially provision a target environ- ment, and deploy to it.

Some organizations like Etsy and Flickr even ship code to production several times a day!

Practices like automated testing, CI, and deployment feed into the general concept of con- tinuous delivery.

*Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation by Jez Humble and David Farley (Addison Wesley, 2010).*

#### Automating your project from build to deployment

Continuous delivery introduces the concept of a deployment pipeline, also referred to as the build pipeline.

A deployment pipeline represents the technical implementation of the process for getting software **from version control into your production environment**.

![enter image description here](https://i.imgur.com/YGSSbbb.png)

* **Commit stage**: Reports on the technical health level of your project. The main stakeholder of this phase is the development team as it provides feedback about broken code and finds “code smells.” The job of this stage is to **compile the code, run tests, perform code analysis, and prepare the distribution**.
* **Automated acceptance test stage**: Asserts that **functional and nonfunctional requirements** are met by running automated tests.
* **Manual test stage**: Verifies that the system is actually usable in a test environment. Usually, this stage involves QA personnel to verify requirements on the level of user stories or use cases.
* **Release stage**: Either delivers the software to the end user as a packaged distribution or **deploys it to the production environment**.

Gradle can be used in the commit and automated acceptance test stages.

* Compiling the code
* Running unit and integration tests
* Performing static code analysis and generating test coverage
* Creating the distribution
* Provisioning the target environment
* Deploying the deliverable
* Performing smoke and automated functional tests

![enter image description here](https://i.imgur.com/rRTDQQi.png)

Asgard, a web-based cloud management and deploy- ment tool built and used by Netflix (https://github.com/Netflix/asgard).



