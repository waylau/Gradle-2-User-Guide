Chapter 7. Java Quickstart 快速开始 Java
===================

##7.1. The Java plugin 关于 Java 插件

Gradle 是一个通用的构建工具，它能构建任何基于你的构建脚本的东西。开箱即用，当然除非你添加代码到你的构建脚本里，不然它不会构建任何东西。

很多 Java 项目都有类似的基本流程：编译 Java 源文件，运行单元测试，创建 JAR 文件。如果你不是把代码从头写到尾，那还能接受。现在有了 Gradle 就不用忍受这些。解决问题的方法就是 插件。插件是 Gradle 配置的扩展，通常是添加配置前的 task。Gradle 装载很多插件，这样可以方便共享。其中，Java 插件 就是添加 task 到 project ，会编译、单元测试你的 Java 代码，并构建进一个 JAR 文件。

Java 插件 是基于约定的。这意味着，该插件定义了 项目 许多方面的的默认值，如 Java 源文件所在的位置。如果你跟随你的项目的约定，你一般不需要在你的构建脚本做太多。Gradle 允许您自定义您的项目，如果你不想或不遵循某种公约。事实上，因为 Java 项目的支持作为一个插件来实现的，你不需要使用所有的插件来构建一个Java项目，如果你不想。

后续章节，我们有许多案例关于 Java 插件、依赖管理、多 project。在这一章中，我们想给你一个初始的想法关于如何使用 Java 插件来构建一个 Java 项目。

##7.2. A basic Java project 基本的 Java 项目

为了使用 Java  插件，添加下面代码到构建文件：

Example 7.1. Using the Java plugin

build.gradle

	apply plugin: 'java'

这个就是 定义一个 Java 项目的全部。它会将 Java 插件应用到项目中，并且添加很多 task。

Gradle 会在 src/main/java 目录下寻找产品代码，在 src/test/java 寻找测试代码 。 另外在 src/main/resources 包含了资源的 JAR 文件,  src/test/resources 包含了运行测试。所有的输出都在 build  目录下，JAR 在  build/libs 目录下

###7.2.1. Building the project 构建项目

在 Java 插件增添了相当多的 task 在 project 中。然而，只有少数的task 是需要在 构建 project 时需要的。最常用的任务是  build task，这就能构建一个完整的 project 。当你运行 gradle build，Gradle 将编译和测试您的代码，并创建一个包含您的主要类和资源的 JAR 文件。

Example 7.2. Building a Java project

执行 gradle build 输出

	> gradle build
	:compileJava
	:processResources
	:classes
	:jar
	:assemble
	:compileTestJava
	:processTestResources
	:testClasses
	:test
	:check
	:build
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs


其他有用的 task 有:

**clean**

删除 build 目录，移除所有构建的文件

**assemble**

编译打包代码,但不运行单元测试。其他插件带给这个 task 更多特性，比如如果你使用 War 插件，task 将给 project 构建 WAR 文件

**check**

编译测试你的代码。其他插件带给这个 task 提供更多检查类型。比如，你使用 checkstyle 插件, 这个 task 建辉在你的代码中 执行 Checkstyle 
 
###7.2.2. External dependencies 外部依赖

Java 项目经常会有一些外部 JAR 的依赖。为了引用这些 JAR 文件，需要在 Gradle　里面配置。在　Gradle，类似与　JAR 文件将会放在 repository 中。一个 repository 可以被依赖的项目获取到，或者提交项目的拷贝到 repository 中，或者两者都可。比如，我们使用  Maven repository :

Example 7.3. Adding Maven repository

build.gradle

	repositories {
	    mavenCentral()
	}

我们添加一些依赖，声明了 编译时 需要的依赖和测试时需要的依赖

Example 7.4. Adding dependencies

build.gradle

	dependencies {
	    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
	    testCompile group: 'junit', name: 'junit', version: '4.+'
	}


详见[Chapter 8. Dependency Management Basics 依赖管理的基础知识](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%208.%20Dependency%20Management%20Basics%20%E4%BE%9D%E8%B5%96%E7%AE%A1%E7%90%86%E7%9A%84%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.md) 

###7.2.3. Customizing the project 自定义 项目

在 Java 插件添加属性到您的项目。这些属性通常足在启动时使用默认值。如果他们不适合你，你很容易改他们。让我们看一看我们的示例。在这里，我们将说明我们的 Java 项目的版本号，包括 Java 的版本号。我们也添加一些属性的 JAR 文件清单。

Example 7.5. Customization of MANIFEST.MF

build.gradle

	sourceCompatibility = 1.5
	version = '1.0'
	jar {
	    manifest {
	        attributes 'Implementation-Title': 'Gradle Quickstart',
	                   'Implementation-Version': version
	    }
	}
