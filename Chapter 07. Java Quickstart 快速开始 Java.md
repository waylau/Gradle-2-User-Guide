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

*注意，完整的项目源码见[https://github.com/waylau/Gradle-2-User-Guide-Demos](https://github.com/waylau/Gradle-2-User-Guide-Demos) 中 java/quickstart*

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


详见[Chapter 8. Dependency Management Basics 依赖管理的基础知识](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2008.%20Dependency%20Management%20Basics%20%E4%BE%9D%E8%B5%96%E7%AE%A1%E7%90%86%E7%9A%84%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.md) 

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

Java 插件添加的 task 和 平常的 task 完全一样，在构建文件中声明。这意味着你可以使用任何在前面的章节中自定义这些 task 的机制。例如，您可以设置 task 的性能，添加行为的一个任务，更改 task 的依赖，或替换完全的 task 。在我们的示例，我们将配置测试 task ，这是类型 [Test](http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.testing.Test.html) ，增加一个系统属性，当执行测试时：

Example 7.6. Adding a test system property

build.gradle

	test {
	    systemProperties 'property': 'value'
	}


*有哪些属性存在？*

*执行 gradle properties  可以列出 project 的属性,你可以看到 Java 插件添加的属性和他们的默认值*

###7.2.4. Publishing the JAR file 发布 JAR 文件

需要告诉 Gradle 要发布 JAR 的位置。在 Gradle 中， 产物 比如 JAR 文件等是发布到库中的。我们的例子中是发布到了本地路径。你也可以发布到 远程位置或者多个位置。

Example 7.7. Publishing the JAR file

build.gradle

	uploadArchives {
	    repositories {
	       flatDir {
	           dirs 'repos'
	       }
	    }
	}

执行 gradle uploadArchives 来发布
	
###7.2.5. Creating an Eclipse project 创建一个 Eclipse project

创建 Eclipse 特点的描述文件，比如 .project，需要添加插件

Example 7.8. Eclipse plugin

build.gradle

	apply plugin: 'eclipse'

执行 gradle eclipse 来生产 Eclipse project 文件。更多  eclipse task 相关内容详见 [Chapter 38. The Eclipse Plugin 关于 Eclipse 插件](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2038.%20The%20Eclipse%20Plugin%20%E5%85%B3%E4%BA%8E%20Eclipse%20%E6%8F%92%E4%BB%B6.md)

###7.2.6. Summary 总结

下面是完整的示例 build 文件

Example 7.9. Java example - complete build file

build.gradle

	apply plugin: 'java'
	apply plugin: 'eclipse'
	
	sourceCompatibility = 1.5
	version = '1.0'
	jar {
	    manifest {
	        attributes 'Implementation-Title': 'Gradle Quickstart',
	                   'Implementation-Version': version
	    }
	}
	
	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
	    testCompile group: 'junit', name: 'junit', version: '4.+'
	}
	
	test {
	    systemProperties 'property': 'value'
	}
	
	uploadArchives {
	    repositories {
	       flatDir {
	           dirs 'repos'
	       }
	    }
	}

##7.3. Multi-project Java build 多 project 的 Java 构建

下面是一个 多 project 构建的 项目结构：

Example 7.10. Multi-project build - hierarchical layout
 
	multiproject/
	  api/
	  services/webservice/
	  shared/
	  services/shared/


*注意，完整的项目源码见[https://github.com/waylau/Gradle-2-User-Guide-Demos](https://github.com/waylau/Gradle-2-User-Guide-Demos) 中 java/multiproject*

里面包含 4 个 project。 `api` 是产生出 JAR 文件 给客户端加载提供给 Java 客户端需要的 XML webservice。 `webservice` 是一个 web 应用返回 XML 。`shared` 包含了 `api`、`webservice` 使用的代码。项目 `services/shared` 包含了 依赖 `shared` 的代码。 

###7.3.1. Defining a multi-project build 定义 build 文件

配置文件的名字叫 settings.gradle，如下

Example 7.11. Multi-project build - settings.gradle file

settings.gradle

	include "shared", "api", "services:webservice", "services:shared"


详见 [Chapter 57. Multi-project Builds 多项目构建](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2057.%20Multi-project%20Builds%20%E5%A4%9A%E9%A1%B9%E7%9B%AE%E6%9E%84%E5%BB%BA.md)

##7.3.2. Common configuration 常见配置

有很多常见的配置。我们的示例中使用了  configuration injection （配置注入）。在这里，根项目就像一个容器，subprojects 方法遍历容器中的元素（实例中的 project ），并将指定的配置。这样我们可以很容易地定义所有档案的 manifest 的内容，和一些常见的依赖关系：

Example 7.12. Multi-project build - common configuration

build.gradle
	
	subprojects {
	    apply plugin: 'java'
	    apply plugin: 'eclipse-wtp'
	
	    repositories {
	       mavenCentral()
	    }
	
	    dependencies {
	        testCompile 'junit:junit:4.11'
	    }
	
	    version = '1.0'
	
	    jar {
	        manifest.attributes provider: 'gradle'
	    }
	}

注意，示例中 在 所有 子 project 中应用了 Java 插件。意思是 task 和配置属性将会出现在虽偶有  子 project 中。所以，你可以 在根 project 目录中，运行 gradle build 来编译、测试、将所有 project 打包成 JAR 。

注意，插件只应用在 subprojects 包含的区域，其他根级别的将不适用。

###7.3.3. Dependencies between projects 项目间的依赖

在相同的构建里，您可以添加项目之间的依存关系，这样，例如，一个项目的 JAR 文件可以用来编译另外一个项目。在`api` 构建文件中我们将添加对`shared `项目的依赖。由于这种依赖，Gradle 将确保 `shared `在`api` 之前获得构建。

Example 7.13. Multi-project build - dependencies between projects

api/build.gradle

	dependencies {
	    compile project(':shared')
	}

详见 [Chapter 57. Multi-project Builds 多项目构建](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2057.%20Multi-project%20Builds%20%E5%A4%9A%E9%A1%B9%E7%9B%AE%E6%9E%84%E5%BB%BA.md) 中  Section 57.7.1, “Disabling the build of dependency projects” 如何禁用这个功能

###7.3.4. Creating a distribution 创建发布包

添加发布包，提供给客户端装载

Example 7.14. Multi-project build - distribution file

api/build.gradle

	task dist(type: Zip) {
	    dependsOn spiJar
	    from 'src/dist'
	    into('libs') {
	        from spiJar.archivePath
	        from configurations.runtime
	    }
	}
	
	artifacts {
	   archives dist
	}
	
	
##7.4. Where to next? 下步工作

你可以查看更多关于  Java 插件 ，见[Chapter 23. The Java Plugin 关于 Java 插件](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2023.%20The%20Java%20Plugin%20%E5%85%B3%E4%BA%8E%20Java%20%E6%8F%92%E4%BB%B6.md)。也可以在 [https://github.com/waylau/Gradle-2-User-Guide-Demos](https://github.com/waylau/Gradle-2-User-Guide-Demos) 中 java 目录下，看到更多 Java 的示例

继续 [Chapter 8. Dependency Management Basics 依赖管理的基础知识](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2008.%20Dependency%20Management%20Basics%20%E4%BE%9D%E8%B5%96%E7%AE%A1%E7%90%86%E7%9A%84%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.md)