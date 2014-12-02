Chapter 6. Build Script Basics 构建脚本的基本
===================

在 Gradle 中两个顶级概念：project（项目）和 task 任务）

所有 Gradle 都有一个或多个 project 构成。project 的展现取决于 Gradle 所做的工作。举例。 project 可以是一个 JAR 库 或者是 web 应用。它可以是由项目生产 JAR 组成发布的 ZIP。一个 project 不一定
代表一个东西要构建。它可能是一件要做的事，如将应用程序部署到工作台
或生产环境。如果这看起来有点模糊，现在不要担心。Gradle 基于约定的构建支持增加一个 更具体的定义的 project。

每个项目都是由一个或多个 task。一个 task 代表了一个构建生成的原子的作品。这可能是编写一些类，创建一个 JAR ，生成 Javadoc，或发布一些库。

现在，我们将看看在构建一个 project 时定义一些简单的 task 。后面的章节将介绍多个 project 和更多的 task 。

##6.2. Hello world

项目的根目录下有个 build.gradle 的文件，这个就是构建的脚本，或者严格说是构建的配置脚本。他定义了project（项目）和 task 任务）。

尝试输出，创建一个 `build.gradle` 命名的文件：

Example 6.1. Your first build script

build.gradle

	task hello {
		doLast {
			println 'Hello world!'
		}
	}