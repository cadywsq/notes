<hr>
<p>title: Introducing Gradle<br>
date: 2016-01-08</p>
<hr>
<h2 id="introduction-to-project-automation">Introduction to project automation</h2>
<h3 id="anatomy-of-a-build-tool">Anatomy of a build tool</h3>
<h4 id="build-file">BUILD FILE</h4>
<ul>
<li>contains the configuration needed for the build</li>
<li>defines external dependencies such as third-party libraries</li>
<li>contains instructions to achieve a specific goal in the form of tasks and their interdependencies</li>
</ul>
<p><img src="https://i.imgur.com/zjyZJrV.png" alt="enter image description here"></p>
<p>Oftentimes, a scripting language is used to express the build logic. That’s why a build file is also referred to as a build script.</p>
<h4 id="build-inputs-and-outputs">BUILD INPUTS AND OUTPUTS</h4>
<p>Complex task dependency graphs may use the output of a dependent task as input.</p>
<p><img src="https://i.imgur.com/H2uDnAG.png" alt="enter image description here"></p>
<h4 id="build-engine">BUILD ENGINE</h4>
<p>Processes the build file at run- time, resolves dependencies between tasks, and sets up the entire configuration needed to command the execution.</p>
<p>Once the internal model is built, the engine will execute the series of tasks in the correct order.</p>
<p><img src="https://i.imgur.com/YUVMYN5.png" alt="enter image description here"></p>
<h4 id="dependency-manager">DEPENDENCY MANAGER</h4>
<p><img src="https://i.imgur.com/FdNXxQl.png" alt="enter image description here"></p>
<p>The dependency manager is used to process declarative dependency definitions for your build file, resolve them from an artifact repository (for example, the local file sys- tem, an FTP, or an HTTP server), and make them available to your project.</p>
<p>Many libraries depend on other libraries, called transitive dependencies. The depen- dency manager can use metadata stored in the repository to automatically resolve transitive dependencies as well.</p>
<h3 id="apache-maven">Apache Maven</h3>
<p>The Maven team realized the need for a standardized proj- ect layout and unified build lifecycle.</p>
<p>Maven picks up on the idea of convention over configuration, meaning that it provides sensible default values for your project config- uration and its behavior.</p>
<p>Maven’s core functionality can be extended by custom logic developed as plugins.</p>
<h4 id="standard-directory-layout">STANDARD DIRECTORY LAYOUT</h4>
<p>By introducing a default project layout, Maven ensures that every developer with the knowledge of one Maven project will immediately know where to expect specific file types.</p>
<p><img src="https://i.imgur.com/OgZIxpA.png" alt="enter image description here"></p>
<h4 id="dependency-management">DEPENDENCY MANAGEMENT</h4>
<p><img src="https://i.imgur.com/PvZd9fl.png" alt="enter image description here"></p>
<p>At runtime, the declared libraries and their transitive dependencies are downloaded by Maven’s dependency manager, stored in the local cache for later reuse, and made available to your build.</p>
<p><img src="https://i.imgur.com/aEHaJUp.png" alt="enter image description here"></p>
<p>Dependency management in Maven isn’t limited to external libraries. You can also declare a dependency on other Maven projects. This need arises if you decompose software into modules, which are smaller components based on associated functionality.</p>
<p><img src="https://i.imgur.com/evaYfWA.png" alt="enter image description here"></p>
<h4 id="sample-build-script">SAMPLE BUILD SCRIPT</h4>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>project</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://maven.apache.org/POM/4.0.0<span class="token punctuation">"</span></span>
<span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span>
<span class="token attr-name"><span class="token namespace">xsi:</span>schemaLocation</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://maven.apache.org/POM/4.0.0 ➥ http://maven.apache.org/xsd/maven-4.0.0.xsd<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>modelVersion</span><span class="token punctuation">&gt;</span></span>4.0.0<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>modelVersion</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>groupId</span><span class="token punctuation">&gt;</span></span>com.mycompany.app<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>groupId</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>artifactId</span><span class="token punctuation">&gt;</span></span>my-app<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>artifactId</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>packaging</span><span class="token punctuation">&gt;</span></span>jar<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>packaging</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>version</span><span class="token punctuation">&gt;</span></span>1.0<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>version</span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependencies</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependency</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>groupId</span><span class="token punctuation">&gt;</span></span>org.apache.commons<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>groupId</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>artifactId</span><span class="token punctuation">&gt;</span></span>commons-lang3<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>artifactId</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>version</span><span class="token punctuation">&gt;</span></span>3.1<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>version</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>scope</span><span class="token punctuation">&gt;</span></span>compile<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>scope</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependency</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependencies</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>project</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<p><strong><code>&lt;packaging&gt;jar&lt;/packageing&gt;</code> defines the type of artifact produced by project</strong></p>
<h4 id="shortcomings">SHORTCOMINGS</h4>
<ul>
<li>proposes a default structure and lifecycle, often is too restrictive<br>
and may not fit your project’s needs</li>
<li>writing custom extensions for Maven is overly cumbersome</li>
<li>earlier versions of Maven (&lt; 2.0.9) automatically try to update their own core plugins; for example, support for unit tests to the latest version. may cause brittle and unstable builds</li>
</ul>
<h3 id="requirements-for-a-next-generation-build-tool">Requirements for a next-generation build tool</h3>
<ul>
<li>expressive, declarative, and maintainable build language.</li>
<li>standardized project layout and lifecycle, but full flexibility and the option to fully configure the defaults.</li>
<li>easy-to-use and flexible ways to implement custom logic</li>
<li>support for project structures that consist of more than one project to build deliverable</li>
<li>support for dependency management</li>
<li>good integration and migration of existing build infrastructure, including the ability to import existing Ant build scripts and tools to translate existing Ant/ Maven logic into its own rule set</li>
<li>emphasis on scalable and high-performance builds</li>
</ul>
<h2 id="next-generation-builds-with-gradle">Next-generation builds with Gradle</h2>
<h3 id="why-gradle">Why Gradle?</h3>
<p><img src="https://i.imgur.com/PG1Mnkf.png" alt="enter image description here"></p>
<p>Gradle combines the best features from other build tools.</p>
<p>Compare with maven.</p>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>project</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://maven.apache.org/POM/4.0.0<span class="token punctuation">"</span></span>
<span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span>
<span class="token attr-name"><span class="token namespace">xsi:</span>schemaLocation</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>modelVersion</span><span class="token punctuation">&gt;</span></span>4.0.0<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>modelVersion</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>groupId</span><span class="token punctuation">&gt;</span></span>com.mycompany.app<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>groupId</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>artifactId</span><span class="token punctuation">&gt;</span></span>my-app<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>artifactId</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>packaging</span><span class="token punctuation">&gt;</span></span>jar<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>packaging</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>version</span><span class="token punctuation">&gt;</span></span>1.0-SNAPSHOT<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>version</span><span class="token punctuation">&gt;</span></span>

<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependencies</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>dependency</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>groupId</span><span class="token punctuation">&gt;</span></span>junit<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>groupId</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>artifactId</span><span class="token punctuation">&gt;</span></span>junit<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>artifactId</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>version</span><span class="token punctuation">&gt;</span></span>4.11<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>version</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>scope</span><span class="token punctuation">&gt;</span></span>test<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>scope</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependency</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>dependencies</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>project</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<pre class=" language-groovy"><code class="prism  language-groovy">apply plugin<span class="token punctuation">:</span> <span class="token string">'java'</span>
group <span class="token operator">=</span> <span class="token string">'com.mycompany.app'</span>
archivesBaseName <span class="token operator">=</span> <span class="token string">'my-app'</span>
version <span class="token operator">=</span> <span class="token string">'1.0-SNAPSHOT'</span>

repositories <span class="token punctuation">{</span>
	<span class="token function">mavenCentral<span class="token punctuation">(</span></span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

dependencies <span class="token punctuation">{</span>
	testCompile <span class="token string">'junit:junit:4.11'</span>
<span class="token punctuation">}</span>
</code></pre>
<p>Compelling feature set.</p>
<p><img src="https://i.imgur.com/ZusXesK.png" alt="enter image description here"></p>
<p>The key to unlocking Gradle’s power features within your build script lies in discover- ing and applying its domain model.</p>
<p><img src="https://i.imgur.com/mvrs7f6.png" alt="enter image description here"></p>
<p>A build script directly maps to an instance of type Project in Gradle’s API. The dependencies configuration block in the build script invokes the method dependencies() of the project instance.</p>
<p>Available as HTML Javadoc documentation on Gradle’s website at <a href="http://www.gradle.org/docs/current/javadoc/index.html">http://www.gradle.org/docs/current/javadoc/index.html</a>.</p>
<p>Each element in a Gradle script has a one-to-one representation with a Java class.</p>
<p>By exposing <strong>hooks</strong> into lifecycle phases, Gradle allows for monitoring and configuring your build script’s execution behavior. Let’s assume you have the very unique requirement of send- ing out an email to the development team whenever a unit test failure occurs. By writing a custom <strong>test listener</strong> that’s notified after the test execution lifecycle event, you can easily incorporate this feature for your build.</p>
<p>Gradle establishes a vocabulary for its model by exposing a <strong>DSL implemented in Groovy</strong>. When dealing with a complex problem domain, in this case the task of build- ing software, being able to use a common language to express your logic can be a pow- erful tool.</p>
<p>Most common to builds is the notation of a unit of work that you want to get executed. Gradle describes this unit of work as a <strong>task</strong>. Part of Gradle’s standard DSL is the ability to define tasks very specific to compiling and packaging Java source code.</p>
<p>A good place to start is the Gradle Build Language Reference Guide at <a href="http://www.gradle.org/docs/current/dsl/index.html">http://www.gradle.org/docs/current/dsl/index.html</a>.</p>
<h4 id="gradle-is-groovy">Gradle is Groovy</h4>
<p>Prominent build tools like Ant and Maven define their build logic through XML. As we all know, XML is easy to read and write, but can become a maintenance nightmare if used in large quantities. XML isn’t very expressive. It makes it hard to define com- plex custom logic.</p>
<p>Gradle takes a different approach. Under the hood, Gradle’s DSL is written with Groovy providing syntactic sugar on top of Java.</p>
<p>Battle- scarred Groovy veterans will assure you that using Groovy instead of Java will boost your productivity by orders of magnitude.</p>
<h4 id="flexible-conventions">Flexible conventions</h4>
<p>Default tasks are provided that make sense in the context of a Java project. For example, you can compile your Java production source code, run tests, and assemble a JAR file. Every Java project starts with a standard directory layout. It defines where to find production source code, resource files, and test code. <strong>Convention properties</strong> are used to change the defaults.</p>
<p>The same concept applies to other project archetypes like Scala, Groovy, web proj- ects, and many more. Gradle calls this concept <strong>build by convention.</strong></p>
<p>You can concentrate on what needs to be configured.</p>
<p><strong>Maven</strong> is very opinionated; it proposes that a project only contains one Java source directory and only produces one single JAR file. This is not necessarily reality for many enterprise projects.</p>
<p>On the opposite side of the spectrum, <strong>Ant</strong> never gave you a lot of guidance on how to structure your build script, allowing for a maximum level of flexibility.</p>
<p>Gradle takes the middle ground by offering conventions combined with the ability to easily change them.</p>
<blockquote>
<p>Gradle is an opinionated framework on top of an unopinionated toolkit.</p>
</blockquote>
<p><img src="https://i.imgur.com/v0msyOV.png" alt="enter image description here"></p>
<h4 id="integration-with-other-build-tools">Integration with other build tools</h4>
<p>Maven POMs and plugins will be treated as Gradle natives. Maven and Ivy repositories have become an important part of today’s build infrastructure. Retrieving dependencies from a repository is only one part of the story; publishing to them is just as important. With a little configuration, Gradle can upload your project’s artifact for companywide or public consumption.</p>
<h3 id="the-bigger-picture-continuous-delivery">The bigger picture: continuous delivery</h3>
<p>Being able to build your source code is only one aspect of the software delivery pro- cess. More importantly, you want to release your product to a production environment to <strong>deliver</strong> business value.</p>
<p>Along the way, you want to run tests, build the distribution, analyze the code for quality-control purposes, potentially provision a target environ- ment, and deploy to it.</p>
<p>Some organizations like Etsy and Flickr even ship code to production several times a day!</p>
<p>Practices like automated testing, CI, and deployment feed into the general concept of con- tinuous delivery.</p>
<p><em>Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation by Jez Humble and David Farley (Addison Wesley, 2010).</em></p>
<h4 id="automating-your-project-from-build-to-deployment">Automating your project from build to deployment</h4>
<p>Continuous delivery introduces the concept of a deployment pipeline, also referred to as the build pipeline.</p>
<p>A deployment pipeline represents the technical implementation of the process for getting software <strong>from version control into your production environment</strong>.</p>
<p><img src="https://i.imgur.com/YGSSbbb.png" alt="enter image description here"></p>
<ul>
<li><strong>Commit stage</strong>: Reports on the technical health level of your project. The main stakeholder of this phase is the development team as it provides feedback about broken code and finds “code smells.” The job of this stage is to <strong>compile the code, run tests, perform code analysis, and prepare the distribution</strong>.</li>
<li><strong>Automated acceptance test stage</strong>: Asserts that <strong>functional and nonfunctional requirements</strong> are met by running automated tests.</li>
<li><strong>Manual test stage</strong>: Verifies that the system is actually usable in a test environment. Usually, this stage involves QA personnel to verify requirements on the level of user stories or use cases.</li>
<li><strong>Release stage</strong>: Either delivers the software to the end user as a packaged distribution or <strong>deploys it to the production environment</strong>.</li>
</ul>
<p>Gradle can be used in the commit and automated acceptance test stages.</p>
<ul>
<li>Compiling the code</li>
<li>Running unit and integration tests</li>
<li>Performing static code analysis and generating test coverage</li>
<li>Creating the distribution</li>
<li>Provisioning the target environment</li>
<li>Deploying the deliverable</li>
<li>Performing smoke and automated functional tests</li>
</ul>
<p><img src="https://i.imgur.com/rRTDQQi.png" alt="enter image description here"></p>
<p>Asgard, a web-based cloud management and deploy- ment tool built and used by Netflix (<a href="https://github.com/Netflix/asgard">https://github.com/Netflix/asgard</a>).</p>
