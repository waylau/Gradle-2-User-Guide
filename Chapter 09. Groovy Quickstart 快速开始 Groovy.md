Chapter 9. Groovy Quickstart 快速开始 Groovy
===================

使用  Groovy 插件来构建  Groovy 项目。这个插件继承自  Java 插件，使你的应用具备了编译能力。你的项目可以包含 Groovy 源码，Java 源码，或者两者都包含。在其他各方面，Groovy 项目与我们在[Chapter 07. Java Quickstart 快速开始 Java](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2007.%20Java%20Quickstart%20%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B%20Java.md) 中所看到的 Java 项目几乎相同 。

##9.1. A basic Groovy project一个基本的Groovy 项目

让我们来看一个例子。要使用 Groovy 插件，你需要在构建脚本文件当中添加以下内容

Example 9.1. Groovy plugin

build.gradle

	apply plugin: 'groovy'

*注意，完整的项目源码见[https://github.com/waylau/Gradle-2-User-Guide-Demos](https://github.com/waylau/Gradle-2-User-Guide-Demos) 中 groovy/quickstart*

同时会将 Java 插件应用到项目中，如果还没有应用的话。Groovy 插件 继承自  compile task 在 src/main/groovy 目录中查找源文件；且继承了 compileTest task，在 src/test/groovy 目录中查找测试的源文件。这些编译 task 对这些目录使用了联合编译，这意味着它们可以同时包含  Java 和 Groovy  源文件。

Example 9.2. Dependency on Groovy

build.gradle

	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile 'org.codehaus.groovy:groovy-all:2.3.6'
	}

下面是完整的构建文件：

Example 9.3. Groovy example - complete build file

build.gradle

	apply plugin: 'eclipse'
	apply plugin: 'groovy'
	
	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile 'org.codehaus.groovy:groovy-all:2.3.6'
	    testCompile 'junit:junit:4.11'
	}

运行 `gradle build` 将会对你的项目进行编译，测试和打成 JAR 包。

##9.2. Summary 总结

这一章描述了一个很简单的 Groovy 项目。通常情况下，一个真实的项目所需要的不止于此。因为一个 Groovy 项目也是一个 Java 项目，因此你能用 Java 做的事情 Groovy 也能做。

你可以参阅 [Chapter 24. The Groovy Plugin ](Chapter 24. The Groovy Plugin ) 去了解更多关于 Groovy 插件的内容，或在 [https://github.com/waylau/Gradle-2-User-Guide-Demos](https://github.com/waylau/Gradle-2-User-Guide-Demos) 中 groovy 目录中找到更多的 Groovy 项目示例。