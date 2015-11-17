Chapter 1. Introduction 介绍
===================

Gradle 为Java（JVM）世界提供快速构建的工具。提供如下功能：

* 一个非常灵活的通用构建工具,如 Ant
* 方便从 Maven 中切换过来。但我们从不强制
* 对多项目构建具有强有力的支持
* 很强的依赖性管理（基于 Apache Ivy）
* 对你现有的 Maven 或者 Ivy 库全力支持
* 支持传递依赖管理，不需要远程仓库或者 pom.xml 和 ivy.xml 文件
* Ant 的任务和构建是一等公民
* Groovy 为构建使用脚本
* 一个丰富的域模型描述你的构建

在[Chapter 2. Overview 总览](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2002.%20Overview%20%E6%80%BB%E8%A7%88.md) 可以看到更多 Gradle 的概况。后面也有更多[教程](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2003.%20Tutorials%20%E6%95%99%E7%A8%8B.md)再等着你。

## 1.1. About this user guide 关于本用户指南
本指南如同 Gradle 一样不断的再更新，如有勘误或者改正之处，请给本指南更多建议，可以访问[Gradle web site](http://www.gradle.org/contribute)

*译者注：对于本翻译如有勘误或者改正之处，[点此](https://github.com/waylau/Gradle-2-User-Guide/issues)提问。*

在用户指南，你会发现一些图表示 Gradle 任务之间的依赖关系。这些使用一些类似 UML 依赖符号，使箭从一个任务到任务（第一个任务所依赖的）。
