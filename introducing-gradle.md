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
<h2 id="next-generation-builds-with-gradle">Next-generation builds with Gradle</h2>
<h2 id="building-a-gradle-project-by-example">Building a Gradle project by example</h2>
<h1 id="mastering-the-fundamentals">Mastering the fundamentals</h1>
<h2 id="build-script-essentials">Build script essentials</h2>
<h2 id="dependency-management">Dependency management</h2>
<h2 id="multiproject-builds">Multiproject builds</h2>
<h2 id="testing-with-gradle">Testing with Gradle</h2>
<h2 id="extending-gradle">Extending Gradle</h2>
<h2 id="integration-and-migration">Integration and migration</h2>
<h1 id="from-build-to-deployment">From build to deployment</h1>
<h2 id="ide-support-and-tooling">IDE support and tooling</h2>
<h2 id="building-polyglot-projects">Building polyglot projects</h2>
<h2 id="code-quality-management-and-monitoring">Code quality management and monitoring</h2>
<h2 id="continuous-integration">Continuous integration</h2>
<h2 id="artifact-assembly-and-publishing">Artifact assembly and publishing</h2>
<h2 id="infrastructure-provisioning-and-deployment">Infrastructure provisioning and deployment</h2>
