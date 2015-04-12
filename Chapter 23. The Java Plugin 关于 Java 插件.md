Chapter 23. The Java Plugin 关于 Java 插件
===================

Java 插件添加 Java 编译和测试、捆绑的能力到项目中。这是许多其他 Gradle 插件的基础。

## 23.1. 用法

使用 Java 插件，添加如下脚本

Example 23.1. Using the Java plugin

build.gradle

	apply plugin: 'java'

## 23.2. Source set

Java 插件引入了 source set 概念。 source set 就是一个源文件集合，用于编译和执行。这些源文件可能包含 Java 源文件和资源文件。其他插件添加这种包含 Groovy 和 Scala 源文件的能力。 source set 与编译 classpath 和运行时 classpath 关联。

source set 的一个用途是将源文件进行逻辑分组，这样可以描述他们的目的。例如，你可能使用 source set 来定义一个继承测试套件，或者你可能使用独立的 source set 来定义 API 和项目类的实现。

Java 插件定义了两个标准 source set ，称为 main 和 test。mian source set 包含您的生产源代码，并编译成一个 JAR 文件。test source set 包含您的测试源代码，这是使用 JUnit 和 TestNG 来编译和执行。这些可以是单元测试，集成测试，验收测试，或任何组合，对你非常有用。

## 23.3. Task（任务）

Java 插件添加了很多任务到你的项目中，如下：

Table 23.1. Java plugin - tasks

<table><thead><tr>
<td>Task 名称</td>
<td>依赖于</td>
<td>类型</td>
<td>描述</td>
</tr></thead><tbody><tr>
<td>
<code class="literal">compileJava</code>
</td>
<td>所有任务产生编译 classpath。这包含了 jar 任务给项目依赖，包含在编译配置中
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html" target="_top"><code class="classname">JavaCompile</code></a></td>
<td>Compiles production Java source files 使用 javac 产生编译 Java 源文件</td>
</tr><tr>
<td>
<code class="literal">processResources</code>
</td>
<td>-</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.Copy.html" target="_top"><code class="classname">Copy</code></a></td>
<td>拷贝生产资源到生产类目下</td>
</tr><tr>
<td>
<code class="literal">classes</code>
</td>
<td>
<code class="literal">compileJava</code> 任务和<code class="literal">processResources</code> 任务。一些插件添加了额外的编译任务
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.Task.html" target="_top"><code class="classname">Task</code></a></td>
<td>组装生产类目录</td>
</tr><tr>
<td>
<code class="literal">compileTestJava</code>
</td>
<td>
<code class="literal">compile</code>, 加上所有的任务产生测试编译 classpath
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html" target="_top"><code class="classname">JavaCompile</code></a></td>
<td>使用 javac 来编译测试 Java 源文件</td>
</tr><tr>
<td>
<code class="literal">processTestResources</code>
</td>
<td>-</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.Copy.html" target="_top"><code class="classname">Copy</code></a></td>
<td>拷贝测试源文件到测试类目录</td>
</tr><tr>
<td>
<code class="literal">testClasses</code>
</td>
<td>
<code class="literal">compileTestJava</code> 任务和 <code class="literal">processTestResources</code> 任务。一些插件添加了额外的测试编译任务
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.Task.html" target="_top"><code class="classname">Task</code></a></td>
<td>组装生产类目录</td>
</tr><tr>
<td>
<code class="literal">jar</code>
</td>
<td>
<code class="literal">compile</code>
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.bundling.Jar.html" target="_top"><code class="classname">Jar</code></a></td>
<td>组装 JAR 文件</td>
</tr><tr>
<td>
<code class="literal">javadoc</code>
</td>
<td><code class="literal">compile</code></td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.javadoc.Javadoc.html" target="_top"><code class="classname">Javadoc</code></a></td>
<td>使用 Javadoc 产生 API 文档给生产的 Java 源文件 </td>
</tr><tr>
<td>
<code class="literal">test</code>
</td>
<td>
<code class="literal">compile</code>,
<code class="literal">compileTest</code>,
加上所有的任务产生的测试运行时的classpath
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.testing.Test.html" target="_top"><code class="classname">Test</code></a></td>
<td>使用 JUnit 或 TestNG 执行单元测试</td>
</tr><tr>
<td>
<code class="literal">uploadArchives</code>
</td>
<td>
任务产生在 <code class="literal">archives</code> 配置中的的构件,包含了<code class="literal">jar</code>.
</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.Upload.html" target="_top"><code class="classname">Upload</code></a></td>
<td>上传<code class="literal">archives</code> 配置的构件, 包含了 JAR 文件</td>
</tr><tr>
<td>
<code class="literal">clean</code>
</td>
<td>-</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.Delete.html" target="_top"><code class="classname">Delete</code></a></td>
<td>删除项目构建目录</td>
</tr><tr>
<td>
<code class="literal">clean<em class="replaceable"><code>TaskName</code></em></code>
</td>
<td>-</td>
<td><a class="ulink" href="http://gradle.org/docs/current/dsl/org.gradle.api.tasks.Delete.html" target="_top"><code class="classname">Delete</code></a></td>
<td>删除特定任务的文件。<code class="literal">cleanJar</code>
将会删除 <code class="literal">jar</code>任务生产的 JAR 文件,
<code class="literal">cleanTest</code> 将会删除<code class="literal">test</code> 任务测试产生的结果
</td>
</tr></tbody></table>


For each source set you add to the project, the Java plugin adds the following compilation tasks: