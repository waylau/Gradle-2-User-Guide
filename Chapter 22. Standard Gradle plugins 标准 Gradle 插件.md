Chapter 22. Standard Gradle plugins 标准 Gradle 插件
===================

Gradle 发布包包含了很多插件，如下：

##22.1. Language plugins 语言类插件

这些插件添加各种语言可以被编译为在JVM中执行的支持。

Table 22.1. Language plugins

<table id="N11A0C"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="java_plugin.html">
<code class="literal">java</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>-</td>
<td>
<p>Adds Java compilation, testing and bundling capabilities to a project. It serves
as the basis for many of the other Gradle plugins. See also <a class="xref" href="tutorial_java_projects.html">Chapter&nbsp;7, <i>Java Quickstart</i></a>.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="groovy_plugin.html">
<code class="literal">groovy</code>
</a>
</td>
<td><code class="literal">java</code>,
<code class="literal">groovy-base</code>
</td>
<td>-</td>
<td>
<p>Adds support for building Groovy projects. See also <a class="xref" href="tutorial_groovy_projects.html">Chapter&nbsp;9, <i>Groovy Quickstart</i></a>.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="scala_plugin.html">
<code class="literal">scala</code>
</a>
</td>
<td><code class="literal">java</code>,
<code class="literal">scala-base</code>
</td>
<td>-</td>
<td>
<p>Adds support for building Scala projects.</p>
</td>
</tr><tr>
<td>
<a class="link" href="antlr_plugin.html">
<code class="literal">antlr</code>
</a>
</td>
<td>
<code class="literal">java</code>
</td>
<td>-</td>
<td>
<p>Adds support for generating parsers using <a class="ulink" href="http://www.antlr.org/" target="_top">Antlr</a>.
</p>
</td>
</tr></tbody></table>


##22.2. Incubating language plugins 孵化中的语言插件

Table 22.2. Language plugins

<table id="N11A9D"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">assembler</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds native assembly language capabilities to a project.</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">c</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds C source compilation capabilities to a project.</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">cpp</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds C++ source compilation capabilities to a project.</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">objective-c</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds Objective-C source compilation capabilities to a project.</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">objective-cpp</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds Objective-C++ source compilation capabilities to a project.</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">windows-resources</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds support for including Windows resources in native binaries.</p>
</td>
</tr></tbody></table>

##22.3. Integration plugins 集成插件

这些插件提供一些集成各种运行技术。

Table 22.3. Integration plugins

<table id="N11B44"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="application_plugin.html">
<code class="literal">application</code>
</a>
</td>
<td>
<code class="literal">java</code>
</td>
<td>-</td>
<td>
<p>Adds tasks for running and bundling a Java project as a command-line application.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="ear_plugin.html">
<code class="literal">ear</code>
</a>
</td>
<td>-</td>
<td>
<code class="literal">java</code>
</td>
<td>
<p>Adds support for building J2EE applications.</p>
</td>
</tr><tr>
<td>
<a class="link" href="jetty_plugin.html">
<code class="literal">jetty</code>
</a>
</td>
<td>
<code class="literal">war</code>
</td>
<td>-</td>
<td>
<p>Deploys your web application to a Jetty web container embedded in the build. See also <a class="xref" href="web_project_tutorial.html">Chapter&nbsp;10, <i>Web Application Quickstart</i></a>.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="maven_plugin.html">
<code class="literal">maven</code>
</a>
</td>
<td>-</td>
<td><code class="literal">java</code>,
<code class="literal">war</code>
</td>
<td>
<p>Adds support for publishing artifacts to Maven repositories.</p>
</td>
</tr><tr>
<td>
<a class="link" href="osgi_plugin.html">
<code class="literal">osgi</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>
<code class="literal">java</code>
</td>
<td>
<p>Adds support for building OSGi bundles.</p>
</td>
</tr><tr>
<td>
<a class="link" href="war_plugin.html">
<code class="literal">war</code>
</a>
</td>
<td>
<code class="literal">java</code>
</td>
<td>-</td>
<td>
<p>Adds support for assembling web application WAR files. See also <a class="xref" href="web_project_tutorial.html">Chapter&nbsp;10, <i>Web Application Quickstart</i></a>.
</p>
</td>
</tr></tbody></table>

##22.4. Incubating integration plugins 孵化中的集成插件

这些插件提供一些集成各种运行技术。

Table 22.4. Incubating integration plugins

<table id="N11C08"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="distribution_plugin.html">
<code class="literal">distribution</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds support for building ZIP and TAR distributions.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="javaLibraryDistribution_plugin.html">
<code class="literal">java-library-distribution</code>
</a>
</td>
<td><code class="literal">java</code>, <code class="literal">distribution</code></td>
<td>-</td>
<td>
<p>Adds support for building ZIP and TAR distributions for a Java library.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="publishing_ivy.html">
<code class="literal">ivy-publish</code>
</a>
</td>
<td>-</td>
<td><code class="literal">java</code>, <code class="literal">war</code></td>
<td>
<p>This plugin provides a new DSL to support publishing artifacts to Ivy repositories, which improves on the existing DSL.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="publishing_maven.html">
<code class="literal">maven-publish</code>
</a>
</td>
<td>-</td>
<td><code class="literal">java</code>, <code class="literal">war</code></td>
<td>
<p>This plugin provides a new DSL to support publishing artifacts to Maven repositories, which improves on the existing DSL.
</p>
</td>
</tr></tbody></table>


##22.5. Software development plugins 软件开发插件

Table 22.5. Software development plugins

<table id="N11C8B"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="announce_plugin.html">
<code class="literal">announce</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Publish messages to your favourite platforms, such as Twitter or Growl.</p>
</td>
</tr><tr>
<td>
<a class="link" href="build_announcements_plugin.html">
<code class="literal">build-announcements</code>
</a>
</td>
<td>announce</td>
<td>-</td>
<td>
<p>Sends local announcements to your desktop about interesting events in the build lifecycle.</p>
</td>
</tr><tr>
<td>
<a class="link" href="checkstyle_plugin.html">
<code class="literal">checkstyle</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>-</td>
<td>
<p>Performs quality checks on your project's Java source files using
<a class="ulink" href="http://checkstyle.sourceforge.net/index.html" target="_top">Checkstyle</a>
and generates reports from these checks.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="codenarc_plugin.html">
<code class="literal">codenarc</code>
</a>
</td>
<td>
<code class="literal">groovy-base</code>
</td>
<td>-</td>
<td>
<p>Performs quality checks on your project's Groovy source files using
<a class="ulink" href="http://codenarc.sourceforge.net/index.html" target="_top">CodeNarc</a>
and generates reports from these checks.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="eclipse_plugin.html">
<code class="literal">eclipse</code>
</a>
</td>
<td>-</td>
<td><code class="literal">java</code>,<code class="literal">groovy</code>,
<code class="literal">scala</code>
</td>
<td>
<p>Generates files that are used by <a class="ulink" href="http://eclipse.org" target="_top">Eclipse IDE</a>, thus making
it possible to import the project into Eclipse. See also <a class="xref" href="tutorial_java_projects.html">Chapter&nbsp;7, <i>Java Quickstart</i></a>.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="eclipse_plugin.html">
<code class="literal">eclipse-wtp</code>
</a>
</td>
<td>-</td>
<td><code class="literal">ear</code>,
<code class="literal">war</code>
</td>
<td>
<p>Does the same as the eclipse plugin plus generates eclipse WTP (Web Tools Platform) configuration files.
After importing to eclipse your war/ear projects should be configured to work with WTP.
See also <a class="xref" href="tutorial_java_projects.html">Chapter&nbsp;7, <i>Java Quickstart</i></a>.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="findbugs_plugin.html">
<code class="literal">findbugs</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>-</td>
<td>
<p>Performs quality checks on your project's Java source files using
<a class="ulink" href="http://findbugs.sourceforge.net" target="_top">FindBugs</a>
and generates reports from these checks.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="idea_plugin.html">
<code class="literal">idea</code>
</a>
</td>
<td>-</td>
<td>
<code class="literal">java</code>
</td>
<td>
<p>Generates files that are used by <a class="ulink" href="http://www.jetbrains.com/idea/index.html" target="_top">Intellij IDEA IDE</a>,
thus making it possible to import the project into IDEA.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="jdepend_plugin.html">
<code class="literal">jdepend</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>-</td>
<td>
<p>Performs quality checks on your project's source files using
<a class="ulink" href="http://clarkware.com/software/JDepend.html" target="_top">JDepend</a>
and generates reports from these checks.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="pmd_plugin.html">
<code class="literal">pmd</code>
</a>
</td>
<td>
<code class="literal">java-base</code>
</td>
<td>-</td>
<td>
<p>Performs quality checks on your project's Java source files using
<a class="ulink" href="http://pmd.sourceforge.net" target="_top">PMD</a>
and generates reports from these checks.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="project_reports_plugin.html">
<code class="literal">project-report</code>
</a>
</td>
<td>
<code class="literal">reporting-base</code>
</td>
<td>-</td>
<td>
<p>Generates reports containing useful information about your Gradle build.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="signing_plugin.html">
<code class="literal">signing</code>
</a>
</td>
<td>base</td>
<td>-</td>
<td>
<p>Adds the ability to digitally sign built files and artifacts.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="sonar_plugin.html">
<code class="literal">sonar</code>
</a>
</td>
<td>-</td>
<td>java-base, java, jacoco</td>
<td>
<p>Provides integration with the
<a class="ulink" href="http://www.sonarsource.org" target="_top">Sonar</a>
code quality platform. Superceeded by the <a class="link" href="sonar_runner_plugin.html"><code class="literal">sonar-runner</code></a> plugin.
</p>
</td>
</tr></tbody></table>

##22.6. Incubating software development plugins 孵化中的软件开发插件

Table 22.6. Software development plugins

<table id="N11E27"><thead><tr>
<td>Plugin Id</td>
<td>Automatically applies</td>
<td>Works with</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>
<a class="link" href="buildDashboard_plugin.html">
<code class="literal">build-dashboard</code>
</a>
</td>
<td>reporting-base</td>
<td>-</td>
<td>
<p>Generates build dashboard report.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="build_init_plugin.html">
<code class="literal">build-init</code>
</a>
</td>
<td>wrapper</td>
<td>-</td>
<td>
<p>Adds support for initializing a new Gradle build. Handles converting a Maven build to a Gradle build.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">cunit</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds support for running <a class="ulink" href="http://cunit.sourceforge.net" target="_top">CUnit</a> tests.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="jacoco_plugin.html">
<code class="literal">jacoco</code>
</a>
</td>
<td>reporting-base</td>
<td>java</td>
<td>
<p>Provides integration with the
<a class="ulink" href="http://www.eclemma.org/jacoco/" target="_top">JaCoCo</a>
code coverage library for Java.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="sonar_runner_plugin.html">
<code class="literal">sonar-runner</code>
</a>
</td>
<td>-</td>
<td>java-base, java, jacoco</td>
<td>
<p>Provides integration with the
<a class="ulink" href="http://www.sonarsource.org" target="_top">Sonar</a>
code quality platform. Supersedes the <a class="link" href="sonar_plugin.html"><code class="literal">sonar</code></a> plugin.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="nativeBinaries.html">
<code class="literal">visual-studio</code>
</a>
</td>
<td>-</td>
<td>native language plugins</td>
<td>
<p>Adds integration with Visual Studio.</p>
</td>
</tr><tr>
<td>
<a class="link" href="wrapper_plugin.html">
<code class="literal">wrapper</code>
</a>
</td>
<td>-</td>
<td>-</td>
<td>
<p>Adds a <a class="ulink" href="../dsl/org.gradle.api.tasks.wrapper.Wrapper.html" target="_top"><code class="classname">Wrapper</code></a> task for generating Gradle wrapper files.
</p>
</td>
</tr><tr>
<td>
<a class="link" href="javaGradle_plugin.html">
<code class="literal">java-gradle-plugin</code>
</a>
</td>
<td>java</td>
<td></td>
<td>
<p>Assists with development of Gradle plugins by providing standard plugin build configuration and validation.</p>
</td>
</tr></tbody></table>

##22.7. Base plugins 基本插件

这些插件形成基本构建块，用来提供给其他插件组装。它们可以被使用在你的构建文件，并列出的完整性。然而，要注意他们还没有考虑 Gradle 的公共API的一部分。因此，这些插件都不能列在用户指南文件中。你可以参考他们的API文档来了解他们。

Table 22.7. Base plugins

<table id="N11F13"><thead><tr>
<td>Plugin Id</td>
<td>Description</td>
</tr></thead><tbody><tr>
<td>base</td>
<td>
<p>Adds the standard lifecycle tasks and configures reasonable defaults for the archive tasks:
</p><div class="itemizedlist"><ul class="itemizedlist"><li class="listitem">adds build
<em class="replaceable"><code>ConfigurationName</code></em>
tasks.
Those tasks assemble the artifacts belonging to the specified configuration.
</li><li class="listitem">adds upload
<em class="replaceable"><code>ConfigurationName</code></em>
tasks.
Those tasks assemble and upload the artifacts belonging to the specified configuration.
</li><li class="listitem">configures reasonable default values for all archive tasks (e.g. tasks that inherit from
<a class="ulink" href="../dsl/org.gradle.api.tasks.bundling.AbstractArchiveTask.html" target="_top"><code class="classname">AbstractArchiveTask</code></a>).
For example, the archive tasks are tasks of types: <a class="ulink" href="../dsl/org.gradle.api.tasks.bundling.Jar.html" target="_top"><code class="classname">Jar</code></a>,
<a class="ulink" href="../dsl/org.gradle.api.tasks.bundling.Tar.html" target="_top"><code class="classname">Tar</code></a>, <a class="ulink" href="../dsl/org.gradle.api.tasks.bundling.Zip.html" target="_top"><code class="classname">Zip</code></a>.
Specifically, <code class="literal">destinationDir</code>,
<code class="literal">baseName</code>
and
<code class="literal">version</code>
properties of the archive tasks are preconfigured with defaults.
This is extremely useful because it drives consistency across projects;
the consistency regarding naming conventions of archives and their location after the build completed.
</li></ul></div><p>
</p>
</td>
</tr><tr>
<td>java-base</td>
<td>
<p>Adds the source sets concept to the project. Does not add any particular source sets.</p>
</td>
</tr><tr>
<td>groovy-base</td>
<td>
<p>Adds the Groovy source sets concept to the project.</p>
</td>
</tr><tr>
<td>scala-base</td>
<td>
<p>Adds the Scala source sets concept to the project.</p>
</td>
</tr><tr>
<td>reporting-base</td>
<td>
<p>Adds some shared convention properties to the project, relating to report generation.</p>
</td>
</tr></tbody></table>


##22.8. Third party plugins 第三方插件

可以从[这里](http://plugins.gradle.org/)看到外部的插件