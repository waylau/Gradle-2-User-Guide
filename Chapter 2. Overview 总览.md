Chapter 2. Overview 总览
===================
#2.1. Features 特性
下面列出了一些 Gradle 的特性：

##Declarative builds and build-by-convention声明式构建，符合公约
gradle 的核心是在 基于 Groovy 对Domain Specific Language (DSL)语言进行一个丰富的扩展。根据喜好，Gradle将陈述建立下一级提供声明性语言元素。这些元素也提供支持Java，Groovy，OSGi，Web和Scala项目。甚至更多，这说明语言是可扩展的。添加您自己的新语言元素或加强现有的，从而提供了简洁，易于维护和易于理解的构建
 

##Language for dependency based programming 依赖型编程语言
声明式语言位于一个通用的任务图，你可以充分利用你的建立。它提供了适应您的独特需求的最大灵活性的工具。

##Structure your build 良好的结构
工具的柔软性和丰富性允许您用一般性设计原则来构建项目。你可以创建一个结构良好，易于维护，易于理解的建立。
 
##Deep API 深层次的API
工具允许您监视和自定义配置和执行行为

##Gradle scales 可伸缩
Gradle　伸缩性能非常好。它会增加你的生产力，从简单的单项目到建立庞大的企业多项目建设。　

##Multi-project builds　多项目构建
Gradle支持多项目建设非常突出。项目依赖是一等公民。

##Gradle is the first build integration tool　Gradle是第一个建立的集成工具
Ant　任务是一等公民。更有趣的是，Ant　的项目也都是一等公民。Gradle 提供深入的引用给 Ant 项目，在运行时，可以转换 Ant 目标到 原生的Gradle 任务。你可以依靠他们的工具，可以提高他们的工具，你甚至可以在build.xml 宣布对Gradle任务的依赖。相同的集成提供了性能，路径，等…

Gradle 支持现有的Maven或Ivy 仓库依赖关系。工具还提供了一个转换器将Maven pom.xml转成 Gradle 脚本。Maven项目运行的进口就快来了。

##Ease of migration 易迁移
Gradle可以适应任何已有的结构。我们通常建议写测试，确保与生产环境类似。这样的迁移是更少的破坏性和尽可能的可靠。这是继重构应用小步骤的最佳实践。

##Groovy 语言
工具的构建脚本是用Groovy，不是XML。但是，不像其他的方法，这不是简单地将动态语言的原始脚本进行能力的扩展。这只会导致一个保持非常困难的构建。工具的总体设计是面向的是将 Gradle 作为一种语言，而不是一个严格的框架。工具提供了一些标准的故事，但他们不做任何形式的限制。这是我们的一个主要特点。

##The Gradle wrapper 关于Gradle的包装
该Gradle包装允许你机器上没有安装Gradle工具也能执行 Gradle 的构建

##Free and open source 免费开源
遵守[ASL](http://www.gradle.org/license)开源协议
 
2.2. Why Groovy? 为啥用Groovy

我们认为，内部DSL的优点（基于动态语言）的XML是以巨大的构建脚本。有一对夫妇的动态语言有。为什么常规？答案在于上下文工具的操作。虽然工具是一个通用的构建工具在它的核心，它的主要焦点是Java项目。在这样的项目团队成员知道Java明显。我们认为，建立应该尽可能对所有的团队成员。
你可能会说，为什么不使用Java作为语言的构建脚本。我们认为这是一个有效的问题。我们选择了Groovy它迄今为止最大的透明度提供了Java的人。它的基础是Java的语法，以及它的类型系统一样，它的封装结构和其他的东西。常规的基础上，很多。常规的基础上，很多。但在一个Java的共同点。对于Java团队共享也Python或Ruby知识或是快乐的学习，上述论点不适用。对于Java团队共享也Python或Ruby知识或是快乐的学习，上述论点不适用。
该工具的设计非常适合于创建另一个建立在JRuby和Jython脚本引擎。它只是不具有最高优先级，此时我们。它只是不具有最高优先级，此时我们。我们高兴地支持任何社区的努力来创建额外的构建脚本引擎。