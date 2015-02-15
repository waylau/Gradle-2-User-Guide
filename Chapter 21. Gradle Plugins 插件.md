Chapter 21. Gradle Plugins 插件
===================

Gradle 在它的核心中有意地提供了一些小但有用的功能，用于在真实世界中的自动化。所有有用的功能，例如以能够编译 Java 代码为例，都是通过插件进行添加的。插件添加了新任务 （例如 [JavaCompile](http://gradle.org/docs/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html)），域对象 （例如 [SourceSet](http://gradle.org/docs/current/dsl/org.gradle.api.tasks.SourceSet.html) ），约定（例如主要的 Java 源代码是位于 src/main/java ），以及扩展的核心对象和其他插件的对象。

在这一章中，我们将讨论如何使用插件以及术语和插件相关的概念。


##21.1. Types of plugins 插件类型

Gradle 一般有两种类型的插件 :script 插件和 binary 插件。script 插件是额外的构建脚本，用于进一步配置构建，以及实现一种声明性方法操纵构建。他们通常用于构建虽然他们可以外部化和从远程位置访问。binary插件的类实现 [Plugin](http://gradle.org/docs/current/javadoc/org/gradle/api/Plugin.html) 接口,采用的编程方法操纵构建。binary 插件通过项目层次结构或外部插件 jar 驻留在构建脚本中。

##21.2. Applying plugins 应用插件

插件是可以被应用的，通过 [Project.apply()](http://gradle.org/docs/current/dsl/org.gradle.api.Project.html#org.gradle.api.Project:apply(java.util.Map)) 方法来完成。

###21.2.1. Script plugins

Example 21.1. Applying a script plugin

build.gradle

	apply from: 'other.gradle'

script 插件可以从本地文件系统或在远程位置的脚本应用。文件位置是相对于项目目录，而远程脚本的位置是一个 HTTP URL 指定。多个脚本插件（或形式）可以应用到一个给定的建立。

###21.2.2. Binary plugins

Example 21.2. Applying a binary plugin

build.gradle

	apply plugin: 'java'

核心插件注册一个简短的名字。在上面的例子中，我们使用短名称“java”来应用 [JavaPlugin](http://gradle.org/docs/current/javadoc/org/gradle/api/plugins/JavaPlugin.html)。插件也有插件ID，以一个完全合格的形式如 com.github.foo.bar，虽然一些遗留的插件还可以利用短期的，不合格的形式。

该方法还可以接受一个类识别插件：

Example 21.3. Applying a binary plugin by type

build.gradle

	apply plugin: JavaPlugin

在上述样本JavaPlugin  符号就是指 [JavaPlugin](http://gradle.org/docs/current/javadoc/org/gradle/api/plugins/JavaPlugin.html)。这类不需要严格引入 org.gradle.api.plugins 包在所有构建脚本会自动导入（见[Appendix E. Existing IDE Support and how to cope without it 支持的 IDE 以及如何应对没有它](Appendix E. Existing IDE Support and how to cope without it 支持的 IDE 以及如何应对没有它.md)）。此外，不需要追加  .class 来确认这个类是在 Groovy 还是在 Java。

插件的应用是幂等。就是说一个插件，可多次应用。如果插件已被应用，任何进一步的应用将没有任何效果。

####21.2.2.1. Locations of binary plugins 关于 binary 插件的位置

一个插件是任意类实现  [Plugin](http://gradle.org/docs/current/javadoc/org/gradle/api/Plugin.html) 接口。Gradle 提供核心插件为其分配部分简单地应用插件来提供所有你需要做的。然而，非核心 binary 插件需要可用于构建类路径才可以应用。这可以通过很多途径实现，包括：

* 定义插件为内联类的声明在一个构建脚本。
* 定义插件作为一个源在项目目录下的文件 buildsrc。
* 包括从外部罐定义为 buildscript 依赖插件（见[60.5节，“外部依赖关系的构建脚本”](http://gradle.org/docs/current/userguide/organizing_build_logic.html#sec:external_dependencies)）。
* 包括插件从插件门户使用插件的DSL（见[21.3节，“应用插件与插件DSL”](http://gradle.org/docs/current/userguide/plugins.html#sec:plugins_block)）。

更多关于定义你自己的插件，见[Chapter 59. Writing Custom Plugins 编写自定义插件](Chapter 59. Writing Custom Plugins 编写自定义插件.md)。

##21.3. Applying plugins with the plugins DSL 使用DSL应用插件

插件DSL目前正在[酝酿中](http://gradle.org/docs/current/userguide/feature_lifecycle.html)。请注意,DSL和其他配置可能会在以后 Gradle版本改变。

新的插件提供了一个更简洁的 DSL 和方便的方式来声明插件的依赖关系。它的工作原理与新的 [Gradle 插件门户](http://plugins.gradle.org/) 提供方便地访问核心和社区插件。插件脚本块配置实例 [PluginDependenciesSpec](http://gradle.org/docs/current/dsl/org.gradle.plugin.use.PluginDependenciesSpec.html)。

申请一个核心插件，可以使用短名称：

Example 21.4. Applying a core plugin

build.gradle

	plugins {
	    id 'java'
	}

通过To apply a community plugin from the portal, the fully qualified plugin id must be used:

Example 21.5. Applying a community plugin

build.gradle

	plugins {
	    id "com.jfrog.bintray" version "0.4.1"
	}

不需要进一步配置。具体来说,不需要配置 buildscript 类路径。Gradle将在插件门户解决插件,找到它,使它可用于构建。

有关更多信息,请参见 [PluginDependenciesSpec](http://gradle.org/docs/current/dsl/org.gradle.plugin.use.PluginDependenciesSpec.html) 使用插件的 DSL。

##21.4. Finding community plugins 寻找社区插件

Gradle  至今有一个充满活力的社区插件开发人员贡献为各种功能的插件。Gradle [插件门户](http://plugins.gradle.org/)提供了一个接口用于搜索和探索社区插件。

##21.5. What plugins do 插件做啥

把插件应用到项目中可以让插件来扩展项目的功能。它可以做的事情如：

* 将任务添加到项目 （如编译、 测试）
* 使用有用的默认设置对已添加的任务进行预配置。
* 向项目中添加依赖配置 （见[第 8.3 节，“依赖配置”](http://gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html#configurations)）。
* 通过扩展对现有类型添加新的属性和方法。

举例

Example 21.6. Tasks added by a plugin

build.gradle

	apply plugin: 'java'
	
	task show << {
	    println relativePath(compileJava.destinationDir)
	    println relativePath(processResources.destinationDir)
	}

执行 gradle -q show

	> gradle -q show
	build/classes/main
	build/resources/main

这个 Java 插件增加了 compileJava 任务 processResources 任务到项目中，以及给这两个任务配置 destinationDir  属性。

##21.6. Conventions 约定

插件可以通过智能的方法对项目进行预配置以支持约定优于配置。Gradle 对此提供了机制和完善的支持，而它是强大-然而-简洁的构建脚本中的一个关键因素。

在上面的示例中我们看到，Java 插件添加了一个任务，名字为compileJava ，有一个名为 destinationDir 的属性（即配置编译的 Java 代码存放的地方）。Java 插件默认此属性指向项目目录中的build/classes/main。这是通过一个合理的默认的约定优于配置的例子。

我们可以简单地通过给它一个新的值来更改此属性。

Example 21.7. Changing plugin defaults

build.gradle

	apply plugin: 'java'
	
	compileJava.destinationDir = file("$buildDir/output/classes")
	
	task show << {
	    println relativePath(compileJava.destinationDir)
	}

执行  gradle -q show
	
	> gradle -q show
	build/output/classes

然而， compileJava 任务很可能不是唯一需要知道类文件在哪里的任务。

Java 插件添加了 source sets 的概念 （见[SourceSet](http://gradle.org/docs/current/dsl/org.gradle.api.tasks.SourceSet.html)） 来描述的源文件集的各个方面，其中一个方面是在编译的时候这些类文件应该被写到哪个地方。Java 插件将 compileJava 任务的 destinationDir 属性映射到源文件集的这一个方面。

我们可以通过这个源码集修改写入类文件的位置。

Example 21.8. Plugin convention object

build.gradle

	apply plugin: 'java'
	
	sourceSets.main.output.classesDir = file("$buildDir/output/classes")
	
	task show << {
	    println relativePath(compileJava.destinationDir)
	}

执行 gradle -q show

	> gradle -q show
	build/output/classes

在上面的示例中，我们应用 Java 插件，除其他外，还做了下列操作：

* 添加了一个新的域对象类型： [SourceSet](http://gradle.org/docs/current/dsl/org.gradle.api.tasks.SourceSet.html)
* 通过属性的默认（即常规）配置了 main 源码集
* 配置支持使用这些属性来执行工作的任务

所有这一切都发生在 `apply plugin: "java"`这一步过程中。在上面例子中，我们在约定配置被执行之后，修改了类文件所需的位置。在上面的示例中可以注意到，compileJava.destinationDir 的值也被修改了，以反映出配置的修改。

考虑一下另一种消费类文件的任务的情况。如果这个任务使用sourceSets.main.output.classesDir 的值来配置，那么修改了这个位置的值，无论它是什么时候被修改，将同时更新 compileJava 任务和这一个消费者任务。

这种配置对象的属性以在所有时间内（甚至当它更改的时候）反映另一个对象的任务的值的能力被称为“映射约定”。它可以令 Gradle 通过约定优于配置及合理的默认值来实现简洁的配置方式。而且，如果默认约定需要进行修改时，也不需要进行完全的重新配置。如果没有这一点，在上面的例子中，我们将不得不重新配置需要使用类文件的每个对象。

##21.7. More on plugins 更多插件

这一章旨在作为对插件和 Gradle 及他们扮演的角色的导言。关于插件的内部运作的详细信息，请参阅[Chapter 59. Writing Custom Plugins 编写自定义插件](Chapter 59. Writing Custom Plugins 编写自定义插件.md)。