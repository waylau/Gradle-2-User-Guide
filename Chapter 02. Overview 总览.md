Chapter 2. Overview 总览
===================
##2.1. Features 特性

下面列出了一些 Gradle 的特性：

###Declarative builds and build-by-convention声明式构建，符合公约

gradle 的核心是在 基于 Groovy 对 Domain Specific Language (DSL)语言进行一个丰富的扩展。根据喜好，Gradle 将陈述建立下一级提供声明性语言元素。这些元素也提供支持 Java，Groovy，OSGi，Web和Scala 项目。甚至更多，这说明语言是可扩展的。添加您自己的新语言元素或加强现有的，从而提供了简洁，易于维护和易于理解的构建
 

###Language for dependency based programming 依赖型编程语言

声明式语言位于一个通用的任务图，你可以充分利用你的构建。它提供了适应您的独特需求的最大灵活性的工具。

###Structure your build 良好的结构

工具的柔软性和丰富性允许您用一般性设计原则来构建项目。你可以创建一个结构良好，易于维护，易于理解的建立。
 
###Deep API 深层次的API

工具允许您监视和自定义配置和执行行为

###Gradle scales 可伸缩

Gradle　伸缩性能非常好。它会增加你的生产力，从简单的单项目到建立庞大的企业多项目建设。　

###Multi-project builds　多项目构建

Gradle 的多项目构建支持是非常优秀的。项目依赖性是一等公民。我们允许你在多项目构建的项目的关系中建模，即使他们是问题域。Gradle 会遵从你的布局而不是相反。

Gradle 提供部分的基础之上。如果你建立一个子项目 Gradle 负责构建所有子项目依赖于子项目。您也可以选择重建依赖于特定的子项目的子项目。再加上增量构建，对于较大的构建这很节省很多时间，。

## Many ways to manage your dependencies 多途径管理依赖

不同的团队喜欢不同的方式来管理他们的外部依赖。Gradle 提供任何便捷的策略支持。从远程 Maven 和 Ivy 库的依赖管理与到本地文件系统上 jars 或目录。

###Gradle is the first build integration tool　Gradle是第一个构建集成工具

Ant　任务是一等公民。更有趣的是，Ant　的项目也都是一等公民。Gradle 提供深入的引用给 Ant 项目，在运行时，可以转换 Ant 目标到 原生的Gradle 任务。你可以依靠他们的工具，可以提高他们的工具，你甚至可以在build.xml 宣布对 Gradle 任务的依赖。相同的集成提供了性能，路径，等…

Gradle 支持现有的 Maven 或 Ivy 仓库依赖关系。工具还提供了一个转换器将 Maven pom.xml 转成 Gradle 脚本。Maven 项目运行的进口就快来了。

###Ease of migration 易迁移

Gradle 可以适应任何已有的结构。我们通常建议写测试，确保与生产环境类似。这样的迁移是更少的破坏性和尽可能的可靠。这是继重构应用小步骤的最佳实践。

###Groovy 语言

工具的构建脚本是用 Groovy，不是XML。但是，不像其他的方法，这不是简单地将动态语言的原始脚本进行能力的扩展。这只会导致一个保持非常困难的构建。工具的总体设计是面向的是将 Gradle 作为一种语言，而不是一个严格的框架。工具提供了一些标准的故事，但他们不做任何形式的限制。这是我们的一个主要特点。

###The Gradle wrapper 关于Gradle的包装

该Gradle包装允许你机器上没有安装Gradle工具也能执行 Gradle 的构建

###Free and open source 免费开源

遵守[ASL](http://www.gradle.org/license)开源协议
 
##2.2. Why Groovy? 为啥用 Groovy

我们认为，当使用构建脚本作为内部 DSL （基于动态语言）比 XML 有更大的优势。有很多动态语言，但为啥是 Groovy？答案是在于上下文工具的操作。虽然 Gradle 是一个通用的构建工具，这是它的核心，但它的主要焦点还是是 Java 项目。在这样的项目中，团队成员更加熟悉 Java。我们考虑的是编译应该都所有成员来说是尽可能的透明。

你可能会说，为什么不使用 Java 作为构建脚本。这里有一个问题，就是对于团队的最高的透明度和最低的学习曲线，但是 由于 Java 语言的限制，作为构建语言效果并不理想（参考 <http://www.defmacro.org/ramblings/lisp.html> )可以看到 Ant, XML, Java 和 Lisp 的对比，有趣的是，Java 的语法实际上是 Groovy 的语法。）。其他语言， Python, Groovy 或者 Ruby 都更能胜任这个工作。我们选择 Groovy 是因为对于 Java 使用者来说有更高的透明度。它的基本语法与 Java 类似，包括 本文系统，包结构和其他方面。Groovy 提供了最重要内容但都是符合 Java 基础功能的。

对于对 Python 或 Ruby 知识拥有强烈的学习欲望的 Java 开发者来说，上述论点不适用。该工具的设计非常适合于创建另一个建立在 JRuby 和Jython 脚本引擎。对于我们来说暂时它只是不具有最高优先级。我们高兴地支持任何社区的努力来创建额外的构建脚本引擎。