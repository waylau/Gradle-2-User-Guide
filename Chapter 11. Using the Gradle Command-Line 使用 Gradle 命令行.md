Chapter 11. Using the Gradle Command-Line 使用 Gradle 命令行
===================

本章介绍了 Gradle 命令行的基本知识。正如在前面的章节里你所见到的， 调用 gradle 命令来执行构建。

##11.1. Executing multiple tasks 执行多 task

同个构建可以执行多个 task ,通过再命令行 列出每个 task。举例，命令
 `gradle compile test` 将会执行 compile 和 test 两个 task。
Gradle 将会按顺序执行 命令行每个列出的 task，并且执行每个 task 的依赖。每个任务仅执行一次，不管它是如何被包含在构建中：无论是在命令行中指定，或作为另一个 task 的依赖，或两者都是。让我们看一个例子。

下面定义了4个 task。 dist 和 test 都依赖于 compile 。执行 ` gradle dist test` ，compile 将会仅仅被执行一次。

Figure 11.1. Task dependencies

![](http://99btgc01.info/uploads/2014/12/commandLineTutorialTasks.png)


Example 11.1. Executing multiple tasks

build.gradle

	task compile << {
	    println 'compiling source'
	}
	
	task compileTest(dependsOn: compile) << {
	    println 'compiling unit tests'
	}
	
	task test(dependsOn: [compile, compileTest]) << {
	    println 'running unit tests'
	}
	
	task dist(dependsOn: [compile, test]) << {
	    println 'building the distribution'
	}

执行 gradle dist test 输出 
	
	> gradle dist test
	:compile
	compiling source
	:compileTest
	compiling unit tests
	:test
	running unit tests
	:dist
	building the distribution
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

每个 task 只执行一次，所以 `gradle test test` 跟 `gradle test` 执行结果一样。

##11.2. Excluding tasks 排除 task

可以通过 -x 命令行来排除 task 被执行。如下：

Example 11.2. Excluding tasks

执行 gradle dist -x test 输出

	> gradle dist -x test
	:compile
	compiling source
	:dist
	building the distribution
	
	BUILD SUCCESSFUL

Total time: 1 secs

可以看到 ，test 并未执行，即使它是 dist 的依赖。同时注意到， test 的依赖，如 compileTest  也未执行。这些 test 的依赖如果是被其他 task 所需要的话，如 compile 仍会执行。

##11.3. Continuing the build when a failure occurs 发生故障时继续构建

默认情况下，只要任何 task 失败，Gradle 将中止执行。这使得构建更快地完成，但隐藏了其他可能发生的故障。为了发现在一个单一的构建中多个可能发生故障的地方，你可以使用 --continue 选项。

通过执行  --continue ，Gralde 会执行每一个 task ，当那个 task  所有的依赖都无故障的执行完成, 而不是一旦出现错误就会中断执行。所有故障信息都会在最后报告出来。

一旦某个 task 执行失败,那么所有依赖于该 task 的后面的 task 都不会被执行，因为这样做不安全。例如，在测试时，当编译代码失败则测试不会执行。因为 测试 task 将取决于 编译 task（不管是直接或间接）。

##11.4. Task name abbreviation 任务名称缩写

当你试图执行某个 task 的时候,无需输入 task 的全名.只需提供足够的可以唯一区分出该 task 的字符即可。例如,上面的例子你也可以这么写, 用 `gradle di` 来直接调用 dist 。

Example 11.3. Abbreviated task name

执行 gradle di

	> gradle di
	:compile
	compiling source
	:compileTest
	compiling unit tests
	:test
	running unit tests
	:dist
	building the distribution
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

同时也可以应用在驼峰的 task 名称。如，执行 compileTest 时，运行 
gradle compTest 或者 gradle cT 都可以。

Example 11.4. Abbreviated camel case task name

执行 gradle cT
	
	> gradle cT
	:compile
	compiling source
	:compileTest
	compiling unit tests
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

同时，也可以应用在 -x 命令选项。

##11.5. Selecting which build to execute 选择要执行的构建

调用 gradle 命令时,默认情况下总是会在当前目录下寻找构建文件（译者注：首先会寻找当前目录下的 build.gradle 文件,以及根据settings.gradle 中的配置寻找子项目的 build.gradle ）。 可以使用 -b 参数选择其他的构建文件,并且当你使用此参数时 settings.gradle 将不会被使用,看下面的例子:

Example 11.5. Selecting the project using a build file

subdir/myproject.gradle

	task hello << {
	    println "using build file '$buildFile.name' in '$buildFile.parentFile.name'."
	}

执行 gradle -q -b subdir/myproject.gradle hello 输出

	> gradle -q -b subdir/myproject.gradle hello
	using build file 'myproject.gradle' in 'subdir'.

或者，您可以使用 -p 选项来指定要使用的项目目录。多 project 的构建时应使用 -p 选项来代替 -b 选项。

Example 11.6. Selecting the project using project directory

执行 gradle -q -p subdir hello 输出

	> gradle -q -p subdir hello
	using build file 'build.gradle' in 'subdir'.

##11.6. Obtaining information about your build 获取构建信息

Gradle 提供了许多内置 task 来收集构建信息。这些内置 task 对于了解依赖结构以及解决问题都是很有帮助的.

了解更多,可以参阅[Chapter 41. The Project Report Plugin 关于 Project Report 插件](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2041.%20The%20Project%20Report%20Plugin%20%E5%85%B3%E4%BA%8E%20Project%20Report%20%E6%8F%92%E4%BB%B6.md),可以为你的项目添加构建报告


###11.6.1. Listing projects  项目列表 

执行 `gradle projects` 会为你列出选中项目的子项目列表.如下例.

Example 11.7. Obtaining information about projects

执行 gradle -q projects 输出

	> gradle -q projects
	------------------------------------------------------------
	Root project
	------------------------------------------------------------
	
	Root project 'projectReports'
	+--- Project ':api' - The shared API for the application
	\--- Project ':webapp' - The Web application implementation
	
	To see a list of the tasks of a project, run gradle <project-path>:tasks
	For example, try running gradle :api:tasks

这份报告展示了每个项目的描述信息。当然你可以在项目中用 description属性来指定这些描述信息。

Example 11.8. Providing a description for a project

build.gradle

	description = 'The shared API for the application'

###11.6.2. Listing tasks 任务列表 

执行 `gradle tasks` 会列出项目中所有 task。这份报告显示项目中所有的默认 task 以及每个 task 的描述。如下

Example 11.9. Obtaining information about tasks

执行 gradle -q tasks 输出
	
	> gradle -q tasks
	------------------------------------------------------------
	All tasks runnable from root project
	------------------------------------------------------------
	
	Default tasks: dists
	
	Build tasks
	-----------
	clean - Deletes the build directory (build)
	dists - Builds the distribution
	libs - Builds the JAR
	
	Build Setup tasks
	-----------------
	init - Initializes a new Gradle build. [incubating]
	wrapper - Generates Gradle wrapper files. [incubating]
	
	Help tasks
	----------
	components - Displays the components produced by root project 'projectReports'. [incubating]
	dependencies - Displays all dependencies declared in root project 'projectReports'.
	dependencyInsight - Displays the insight into a specific dependency in root project 'projectReports'.
	help - Displays a help message.
	projects - Displays the sub-projects of root project 'projectReports'.
	properties - Displays the properties of root project 'projectReports'.
	tasks - Displays the tasks runnable from root project 'projectReports' (some of the displayed tasks may belong to subprojects).
	
	To see all tasks and more detail, run with --all.

默认情况下,这只会显示那些被分组的 task.你可以通过为 task 设置group 属性和 description 来把这些信息展示到报告中

Example 11.10. Changing the content of the task report

build.gradle
	
	dists {
	    description = 'Builds the distribution'
	    group = 'build'
	}

当然你也可以用 --all 参数来收集更多 task 信息。这报告列出项目中所有被主 task 的分组的 task 以及 task 之间的依赖关系。下面是示例


Example 11.11. Obtaining more information about tasks

执行 gradle -q tasks --all 输出

	> gradle -q tasks --all
	------------------------------------------------------------
	All tasks runnable from root project
	------------------------------------------------------------
	
	Default tasks: dists
	
	Build tasks
	-----------
	clean - Deletes the build directory (build)
	api:clean - Deletes the build directory (build)
	webapp:clean - Deletes the build directory (build)
	dists - Builds the distribution [api:libs, webapp:libs]
	    docs - Builds the documentation
	api:libs - Builds the JAR
	    api:compile - Compiles the source files
	webapp:libs - Builds the JAR [api:libs]
	    webapp:compile - Compiles the source files
	
	Build Setup tasks
	-----------------
	init - Initializes a new Gradle build. [incubating]
	wrapper - Generates Gradle wrapper files. [incubating]
	
	Help tasks
	----------
	components - Displays the components produced by root project 'projectReports'. [incubating]
	api:components - Displays the components produced by project ':api'. [incubating]
	webapp:components - Displays the components produced by project ':webapp'. [incubating]
	dependencies - Displays all dependencies declared in root project 'projectReports'.
	api:dependencies - Displays all dependencies declared in project ':api'.
	webapp:dependencies - Displays all dependencies declared in project ':webapp'.
	dependencyInsight - Displays the insight into a specific dependency in root project 'projectReports'.
	api:dependencyInsight - Displays the insight into a specific dependency in project ':api'.
	webapp:dependencyInsight - Displays the insight into a specific dependency in project ':webapp'.
	help - Displays a help message.
	api:help - Displays a help message.
	webapp:help - Displays a help message.
	projects - Displays the sub-projects of root project 'projectReports'.
	api:projects - Displays the sub-projects of project ':api'.
	webapp:projects - Displays the sub-projects of project ':webapp'.
	properties - Displays the properties of root project 'projectReports'.
	api:properties - Displays the properties of project ':api'.
	webapp:properties - Displays the properties of project ':webapp'.
	tasks - Displays the tasks runnable from root project 'projectReports' (some of the displayed tasks may belong to subprojects).
	api:tasks - Displays the tasks runnable from project ':api'.
	webapp:tasks - Displays the tasks runnable from project ':webapp'.

###11.6.3. Show task usage details 显示 task 使用细节

执行 gradle help --task someTask 可以获取到 task 的详细信息， 或者多项目构建中相同 task 名称的所有 task 的信息,如下

Example 11.12. Obtaining detailed help for tasks

执行 gradle -q help --task libs 输出

	> gradle -q help --task libs
	Detailed task information for libs
	
	Paths
	     :api:libs
	     :webapp:libs
	
	Type
	     Task (org.gradle.api.Task)
	
	Description
	     Builds the JAR

这些结果包含了完整的 task 的路径、类型、可能的命令行选项以及描述信息等.

###11.6.4. Listing project dependencies 依赖列表 

执行 gradle dependencies 会列出项目的依赖列表,所有依赖会根据任务区分,以树型结构展示出来。如下


Example 11.13. Obtaining information about dependencies

执行 gradle -q dependencies api:dependencies webapp:dependencies 输出
	
	> gradle -q dependencies api:dependencies webapp:dependencies
	------------------------------------------------------------
	Root project
	------------------------------------------------------------
	
	No configurations
	
	------------------------------------------------------------
	Project :api - The shared API for the application
	------------------------------------------------------------
	
	compile
	\--- org.codehaus.groovy:groovy-all:2.3.6
	
	testCompile
	\--- junit:junit:4.11
	     \--- org.hamcrest:hamcrest-core:1.3
	
	------------------------------------------------------------
	Project :webapp - The Web application implementation
	------------------------------------------------------------
	
	compile
	+--- project :api
	|    \--- org.codehaus.groovy:groovy-all:2.3.6
	\--- commons-io:commons-io:1.2
	
	testCompile
	No dependencies

由于依赖的报告可以变得较大，可以使用特定的配置来限制到一个有用的报告。可以通过 --configuration 可选参数来实现。

Example 11.14. Filtering dependency report by configuration

执行  gradle -q api:dependencies --configuration testCompile 输出为

	> gradle -q api:dependencies --configuration testCompile
	------------------------------------------------------------
	Project :api - The shared API for the application
	------------------------------------------------------------
	
	testCompile
	\--- junit:junit:4.11
	     \--- org.hamcrest:hamcrest-core:1.3

###11.6.5. Getting the insight into a particular dependency 查看特定依赖 

执行 gradle dependencyInsight 可以查看指定的依赖情况，如下

Example 11.15. Getting the insight into a particular dependency

执行 gradle -q webapp:dependencyInsight --dependency groovy --configuration compile 输出

	> gradle -q webapp:dependencyInsight --dependency groovy --configuration compile
	org.codehaus.groovy:groovy-all:2.3.6
	\--- project :api
	     \--- compile

这对于分辨依赖、了解依赖关系、了解为何选择此版本作为依赖十分有用。了解更多请参阅 [DependencyInsightReportTask](http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.diagnostics.DependencyInsightReportTask.html) 类的 API

内建的 dependencyInsight 是'Help' task 分组中的一个。这项 task 需要进行依赖和配置文件的配置才可以。该报告寻找那些与定依赖规范指定的配置匹配的的依赖。如果应用了 Java 相关的插件，该dependencyinsight task 是预先经过 'compile' 配置，因为它通常依赖我们感兴趣的编译。你应该指定您感兴趣的依赖,通过命令行 '--dependency'选项。如果你不喜欢默认的，你可以选择通过 '--configuration' 选项来配置。更多信息见[DependencyInsightReportTask](http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.diagnostics.DependencyInsightReportTask.html) 类的API文档。


###11.6.6. Listing project properties 项目属性列表

执行 gradle properties 可以获取项目所有属性列表，如下

Example 11.16. Information about properties

执行 gradle -q api:properties 输出
	
	> gradle -q api:properties
	------------------------------------------------------------
	Project :api - The shared API for the application
	------------------------------------------------------------
	
	allprojects: [project ':api']
	ant: org.gradle.api.internal.project.DefaultAntBuilder@12345
	antBuilderFactory: org.gradle.api.internal.project.DefaultAntBuilderFactory@12345
	artifacts: org.gradle.api.internal.artifacts.dsl.DefaultArtifactHandler_Decorated@12345
	asDynamicObject: org.gradle.api.internal.ExtensibleDynamicObject@12345
	baseClassLoaderScope: org.gradle.api.internal.initialization.DefaultClassLoaderScope@12345
	buildDir: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build
	buildFile: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build.gradle
	

###11.6.7. Profiling a build

--profile 命令选项可以记录一些构建期间的信息并保存到 build/reports/profile 目录下并且以构建时间命名这些文件

该报告列出总时间和在配置和 task 的执行 阶段的细节。并以时间大小倒序排列，并且记录了任务的执行情况

如果采用了 buildSrc 构建,那么在 buildSrc/build 下同时也会给 buildSrc 生成一份日志记录

![](http://99btgc01.info/uploads/2014/12/profile.png)

##11.7. Dry Run 干跑

有时可能你只想知道某个 task 在一个 task 集中按顺序执行的结果,但并不想实际执行这些 task 。那么你可以用 -m 选项。例如 执行 `gradle -m clean compile` 将会看到所有的作为 clean 和 compile 一部分的 task 会被执行。这与 task 可以形成互补,让你知道哪些 task 可以用于执行。

##11.8. Summary 总结

本章看到了命令行的一部分。更多详见 [Appendix D. Gradle Command Line 命令行](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Appendix%20D.%20Gradle%20Command%20Line%20%E5%91%BD%E4%BB%A4%E8%A1%8C.md)
