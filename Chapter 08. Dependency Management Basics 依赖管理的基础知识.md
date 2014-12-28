Chapter 8. Dependency Management Basics 依赖管理的基础
===================

本章介绍一些 Gradle 依赖管理的基础

##8.1. What is dependency management? 什么是依赖管理

大致上，依赖管理是由2块组成。首先，Gradle 需要知道项目构建或者运行的需要是东西。我们把引进的文件称之为 项目的依赖。其次，Gradle 需要构建和上传项目的产物。我们把向外输出的文件称之为项目的发布。现在看下细节：

很多项目不能完全自我包含。他们需要其他项目的产物。比如， 使用 Hibernate ，JDBC driver 或者 Ehcache jars，需要将他们放在我们项目的 classpath， 来实现需要的功能。

这些引进的项目依赖的文件， Gradle  允许你告诉你的项目所需要的依赖，这样项目才能找到他们，在构建的时候使用他们。这些依赖可能要从远程的 Maven 或者 Ivy  下载，放在你的本地的目录，或者需要被其他项目构建（在相同的 多 project 构建中）。我们称之为 dependency resolution （依赖性解析）。

请注意，此功能提供了 Ant 的一个主要优势。与 Ant 相比，你只需要指定需要加载的绝对或相对路径的特定的 jars。在 Gradle，你只是声明依赖的的“名字”， 和其他布局的确定的位置。你可以通过增加 Apache Ivy到 Ant 得到类似的行为，但 Gradle 做得更好。

通常，一个项目的依赖会包含自己的依赖。例如，Hibernate 的核心需要几个其他包在类路径中存在才能运行。所以，当 Gradle 运行你的项目的测试，它也需要找到这些依赖关系，使他们存在。我们称这些 transitive dependencies （过渡依赖）。

大多数项目的主要目的是构建一些文件是在项目中使用。例如，如果你的项目生成 Java 库，你需要建立一个 jar，也许一个源 jar 和一些文档，并将其发布的某个地方。

这些输出文件以发布包的形式。Gradle 还负责这个重要的工作给你。你声明你的项目的发布，Gradle 照顾构建和发布他们。究竟发布什么取决于你想做什么。你可能想将文件复制到本地目录，或将它们上传到一个远程 Maven 或 Ivy 库。或者你可能使用在相同的多 project 的另一个项目文件的构建。我们称这个过程为 publication（发布） 。

##8.2. Declaring your dependencies 声明依赖

下面是基本的脚本

Example 8.1. Declaring dependencies

build.gradle

	apply plugin: 'java'
	
	repositories {
	    mavenCentral()
	}
	
	dependencies {
	    compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
	    testCompile group: 'junit', name: 'junit', version: '4.+'
	}

这个脚本说明了几个事。首先项目需要 Hibernate core 3.6.7.Final  版本来编译。其中隐含的意思是，Hibernate core 和 他的依赖在运行时是需要的。其次，需要  junit >= 4.0 版本在测试时需要编译。同时 告诉 Gradle 依赖在 Maven central 库 中找。下面详述

##8.3. Dependency configurations 项目配置

一个配置是一个简单的命名依赖的集合。我们称它为依赖配置。你可以用它们来声明项目的外部依赖。正如我们将看到的，他们还用声明项目的 发布。

Java 配置定义了一些标准的配置，这些配置在Java 插件使用的 classpath 中，下面是一些列表。详见[Chapter 23. The Java Plugin 关于 Java 插件](Chapter 23. The Java Plugin 关于 Java 插件.md) 中  Table 23.5, “Java plugin - dependency configurations“

**compile**

编译项目的生产源所需的依赖。

**runtime**

生产类在运行时所需的依赖。默认情况下，还包括编译时的依赖。 

**testCompile**

编译项目的测试源所需的依赖。默认情况下，还包括产品编译类和编译时的依赖。

**testRuntime**

运行测试所需的依赖。默认情况下，还包括 编译，运行时和测试编译的依赖。

各种插件添加进一步的标准配置。您也可以定义自己的自定义配置，使用你的建构建。请参见[Chapter 51. Dependency Manageme](Chapter 51. Dependency Management 依赖管理.md) Section 51.3, “Dependency configurations” 关于更多自定义依赖配置。



