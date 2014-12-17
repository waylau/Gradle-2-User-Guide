Chapter 15. More about Tasks 更多关于任务
===================

在入门教程中（[Chapter 06. Build Script Basics 构建脚本的基础识](Chapter 06. Build Script Basics 构建脚本的基础识.md)），已经学到了如何创建简单 task。之后您还学习了如何将其他行为添加到这些 task 中，同时你已经学会了如何创建 task 之间的依赖。这都是简单的 task 。但 Gradle 让 task 的概念更深远。Gradle 支持增强的task，也就是，有自己的属性和方法的 task 。这是真正的与你所使用的 Ant target（目标）的不同之处。这种增强的任务可以由你提供，或由 Gradle 构建。

 
##15.1. Defining tasks 定义任务

在（[Chapter 06. Build Script Basics 构建脚本的基础识](Chapter 06. Build Script Basics 构建脚本的基础识.md)）中我们已经看到如何通过关键字这种风格来定义 task 。在某些情况中，你可能需要使用这种关键字风格的几种不同的变式。例如，在表达式中不能用这种关键字风格。

Example 15.1. Defining tasks

build.gradle

	task(hello) << {
	    println "hello"
	}
	
	task(copy, type: Copy) {
	    from(file('srcDir'))
	    into(buildDir)
	}

也可以使用字符串作为 task 名称

Example 15.2. Defining tasks - using strings for task names

build.gradle

	task('hello') <<
	{
	    println "hello"
	}
	
	task('copy', type: Copy) {
	    from(file('srcDir'))
	    into(buildDir)
	}

你可能更愿意使用另外一种替代的语法来定义任务：

Example 15.3. Defining tasks with alternative syntax

build.gradle
	
	tasks.create(name: 'hello') << {
	    println "hello"
	}
	
	tasks.create(name: 'copy', type: Copy) {
	    from(file('srcDir'))
	    into(buildDir)
	}

##15.2. Locating tasks 定位任务

你经常需要在构建文件中查找你所定义的 task，例如，为了去配置或是使用它们作为依赖。对这样的情况，有很多种方法。首先，每个 task 都可作为 project 的一个属性，并且使用 task 名称作为这个属性名称：

Example 15.4. Accessing tasks as properties

build.gradle

	task hello
	
	println hello.name
	println project.hello.name

task 也可以通过 task 集合来访问

Example 15.5. Accessing tasks via tasks collection

build.gradle

	task hello
	
	println tasks.hello.name
	println tasks['hello'].name

您可以从任何 project 中，使用 tasks.getByPath() 方法获取 task 路径并且通过这个路径来访问 task。你可以用 task 名称，相对路径或者是绝对路径作为参数调用 getByPath() 方法。

Example 15.6. Accessing tasks by path

build.gradle

	project(':projectA') {
	    task hello
	}
	
	task hello
	
	println tasks.getByPath('hello').path
	println tasks.getByPath(':hello').path
	println tasks.getByPath('projectA:hello').path
	println tasks.getByPath(':projectA:hello').path

执行 gradle -q hello

	> gradle -q hello
	:hello
	:hello
	:projectA:hello
	:projectA:hello

详见 [TaskContainer](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/TaskContainer.html) 关于更多定位 task 的选项

##15.3. Configuring tasks 配置任务

作为一个例子，让我们看看由 Gradle 提供的 Copy task。若要创建Copy task ，您可以在构建脚本中声明：

Example 15.7. Creating a copy task

build.gradle

	task myCopy(type: Copy)

上面的代码创建了一个什么都没做的复制 task 。可以使用它的 API 来配置这个任务 （见[Copy](http://www.gradle.org/docs/current/dsl/org.gradle.api.tasks.Copy.html)）。下面的示例演示了几种不同的方式来实现相同的配置。

要明白，意识到这项任务的名称是 “myCopy”，但它的类型是“Copy”。你可以有多个同一类型的 task ，但具有不同的名称。你会发现这给你一个很大的权力来实现横切关注点在一个特定类型的所有 task。

Example 15.8. Configuring a task - various ways

build.gradle

	Copy myCopy = task(myCopy, type: Copy)
	myCopy.from 'resources'
	myCopy.into 'target'
	myCopy.include('**/*.txt', '**/*.xml', '**/*.properties')

这类似于我们通常在 Java 中配置对象的方式。您必须在每一次的配置语句重复上下文 （myCopy）。这显得很冗余并且很不好读。

还有另一种配置任务的方式。它也保留了上下文，且可以说是可读性最强的。它是我们通常最喜欢的方式。

Example 15.9. Configuring a task - with closure

build.gradle

	task myCopy(type: Copy)
	
	myCopy {
	   from 'resources'
	   into 'target'
	   include('**/*.txt', '**/*.xml', '**/*.properties')
	}

这种方式适用于任何任务。该例子的第 3 行只是 tasks.getByName() 方法的简洁写法。特别要注意的是，如果您向 getByName() 方法传入一个闭包，这个闭包的应用是在配置这个任务的时候，而不是任务执行的时候。

您也可以在定义一个任务的时候使用一个配置闭包。

Example 15.10. Defining a task with closure

build.gradle
	
	task copy(type: Copy) {
	   from 'resources'
	   into 'target'
	   include('**/*.txt', '**/*.xml', '**/*.properties')
	}

##15.4. Adding dependencies to a task 给任务添加依赖

定义任务的依赖关系有几种方法。在第 6.5 章节，"任务依赖"中，已经向你介绍了使用任务名称来定义依赖。任务的名称可以指向同一个项目中的任务，或者其他项目中的任务。要引用另一个项目中的任务，你需要把它所属的项目的路径作为前缀加到它的名字中。下面是一个示例，添加了从projectA:taskX 到 projectB:taskY 的依赖关系：

Example 15.11. Adding dependency on task from another project

build.gradle
	
	project('projectA') {
	    task taskX(dependsOn: ':projectB:taskY') << {
	        println 'taskX'
	    }
	}
	
	project('projectB') {
	    task taskY << {
	        println 'taskY'
	    }
	}

执行 gradle -q taskX

	> gradle -q taskX
	taskY
	taskX

您可以使用一个 Task 对象而不是任务名称来定义依赖，如下：

Example 15.12. Adding dependency using task object

build.gradle

	task taskX << {
	    println 'taskX'
	}
	
	task taskY << {
	    println 'taskY'
	}

	taskX.dependsOn taskY

执行 gradle -q taskX

	> gradle -q taskX
	taskY
	taskX

对于更高级的用法，您可以使用闭包来定义 task 依赖。在计算依赖时，闭包会被传入正在计算依赖的任务。这个闭包应该返回一个 Task 对象或是Task 对象的集合，返回值会被作为这个 task 的依赖项。下面的示例是从taskX 加入了 project 中所有名称以 lib 开头的 task 的依赖

Example 15.13. Adding dependency using closure

build.gradle

	task taskX << {
	    println 'taskX'
	}
	
	taskX.dependsOn {
	    tasks.findAll { task -> task.name.startsWith('lib') }
	}
	
	task lib1 << {
	    println 'lib1'
	}
	
	task lib2 << {
	    println 'lib2'
	}
	
	task notALib << {
	    println 'notALib'
	}

执行 gradle -q taskX

	> gradle -q taskX
	lib1
	lib2
	taskX

更多关于 task 依赖 ，见  [Task](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html) API

##15.5. Ordering tasks 排序任务

任务排序还是一个[孵化中](Appendix C. The Feature Lifecycle 特性的生命周期.md)的功能。请注意此功能在以后的 Gradle 版本中可能会改变。

在某些情况下，控制两个任务的执行的顺序，而不引入这些任务之间的显式依赖，是很有用的。任务排序和任务依赖之间的主要区别是，排序规则不会影响那些任务的执行，而仅将执行的顺序。

任务排序在许多情况下可能很有用：

* 强制任务顺序执行： 如，'build' 永远不会在 'clean' 前面执行。
* 在构建中尽早进行构建验证：如，验证在开始发布的工作前有一个正确的证书。
* 通过在长久验证前运行快速验证以得到更快的反馈：如，单元测试应在集成测试之前运行。
* 一个任务聚合了某一特定类型的所有任务的结果：如，测试报告任务结合了所有执行的测试任务的输出。

有两种排序规则是可用的："必须在之后运行"和"应该在之后运行"。

通过使用 “必须在之后运行”的排序规则，您可以指定 taskB 必须总是运行在 taskA 之后，无论 taskA 和 taskB 这两个任务在什么时候被调度执行。这被表示为 taskB.mustRunAfter(taskA) 。“应该在之后运行”的排序规则与其类似，但没有那么严格，因为它在两种情况下会被忽略。首先是如果使用这一规则引入了一个排序循环。其次，当使用并行执行，并且一个任务的所有依赖项除了任务应该在之后运行之外所有条件已满足，那么这个任务将会运行，不管它的“应该在之后运行”的依赖项是否已经运行了。当倾向于更快的反馈时，会使用“应该在之后运行”的规则，因为这种排序很有帮助但要求不严格。

目前使用这些规则仍有可能出现 taskA 执行而 taskB 没有执行，或者taskB 执行而 taskA 没有执行。

Example 15.14. Adding a 'must run after' task ordering
	
build.gradle

	task taskX << {
	    println 'taskX'
	}
	task taskY << {
	    println 'taskY'
	}
	taskY.mustRunAfter taskX

执行 gradle -q taskY taskX

	> gradle -q taskY taskX
	taskX
	taskY

Example 15.15. Adding a 'should run after' task ordering

build.gradle

	task taskX << {
	    println 'taskX'
	}
	task taskY << {
	    println 'taskY'
	}
	taskY.shouldRunAfter taskX

执行 gradle -q taskY taskX

	> gradle -q taskY taskX
	taskX
	taskY

在上面的例子中，它仍有可能执行 taskY 而不会导致 taskX 也运行：

Example 15.16. Task ordering does not imply task execution

执行 gradle -q taskY

	> gradle -q taskY
	taskY

如果想指定两个任务之间的“必须在之后运行”和“应该在之后运行”排序，可以使用 [Task.mustRunAfter()](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html#org.gradle.api.Task:mustRunAfter(java.lang.Object[])) 和 [Task.shouldRunAfter()](http://www.gradle.org/docs/current/javadoc/org/gradle/api/Task.html#shouldRunAfter(java.lang.Object[])) 方法。这些方法接受一个任务实例、 任务名称或 [Task.dependsOn()](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html#org.gradle.api.Task:dependsOn(java.lang.Object[])) 所接受的任何其他输入作为参数。

请注意"B.mustRunAfter(A)"或"B.shouldRunAfter(A)"并不意味着这些任务之间的任何执行上的依赖关系：

它是可以独立地执行任务 A 和 B 的。

* 排序规则仅在这两项任务计划执行时起作用。
* 当--continue参数运行时，可能会是 A 执行失败后 B 执行了。

如之前所述，如果“应该在之后运行”的排序规则引入了排序循环，那么它将会被忽略。

Example 15.17. A 'should run after' task ordering is ignored if it introduces an ordering cycle

build.gradle

	task taskX << {
	    println 'taskX'
	}
	task taskY << {
	    println 'taskY'
	}
	task taskZ << {
	    println 'taskZ'
	}
	taskX.dependsOn taskY
	taskY.dependsOn taskZ
	taskZ.shouldRunAfter taskX

执行 gradle -q taskX

	> gradle -q taskX
	taskZ
	taskY
	taskX

##15.6. Adding a description to a task 给任务添加描述

可以给任务添加描述，这个描述将会在 task 执行时显示。

Example 15.18. Adding a description to a task

build.gradle

	task copy(type: Copy) {
	   description 'Copies the resource directory to the target directory.'
	   from 'resources'
	   into 'target'
	   include('**/*.txt', '**/*.xml', '**/*.properties')
	}

##15.7. Replacing tasks 替换任务

有时您想要替换一个任务。例如，您想要把通过 Java 插件添加的一个任务与不同类型的一个自定义任务进行交换。你可以这样实现：

Example 15.19. Overwriting a task

build.gradle

	task copy(type: Copy)
	
	task copy(overwrite: true) << {
	    println('I am the new one.')
	}

执行 gradle -q copy

	> gradle -q copy
	I am the new one.

在这里我们用一个简单的任务替换 Copy 类型的任务。当创建这个简单的任务时，您必须将 overwrite 属性设置为 true。否则 Gradle 将抛出异常，说这种名称的任务已经存在。

##15.8. Skipping tasks 跳过任务

Gradle 提供多种方式来跳过任务的执行。

###15.8.1. Using a predicate 使用断言

你可以使用 onlyIf() 方法将断言附加到一项任务中。如果断言结果为 true，才会执行任务的操作。你可以用一个闭包来实现断言。闭包会作为一个参数传给任务，并且任务应该执行时返回 true，或任务应该跳过时返回false。断言只在任务要执行前才计算。

Example 15.20. Skipping a task using a predicate

build.gradle

	task hello << {
	    println 'hello world'
	}
	
	hello.onlyIf { !project.hasProperty('skipHello') }

执行 gradle hello -PskipHello

	> gradle hello -PskipHello
	:hello SKIPPED
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

###15.8.2. Using StopExecutionException

如果跳过任务的规则不能与断言同时表达，您可以使用[StopExecutionException](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/StopExecutionException.html)。如果一个操作（action）抛出了此异常，那么这个操作（action）接下来的行为和这个任务的其他 操作（action）都会被跳过。构建会继续执行下一个任务。

Example 15.21. Skipping tasks with StopExecutionException

build.gradle
	
	task compile << {
	    println 'We are doing the compile.'
	}
	
	compile.doFirst {
	    // Here you would put arbitrary conditions in real life.
	    // But this is used in an integration test so we want defined behavior.
	    if (true) { throw new StopExecutionException() }
	}
	task myTask(dependsOn: 'compile') << {
	   println 'I am not affected'
	}

Output of gradle -q myTask

	> gradle -q myTask
	I am not affected

如果您使用由 Gradle 提供的任务，那么此功能将非常有用。它允许您向一个任务的内置操作中添加执行条件。(你可能会想，为什么既不导入StopExecutionException 也没有通过其完全限定名来访问它。原因是，Gradle 会向您的脚本添加默认的一些导入。这些导入是可自定义的 （见[Appendix E. Existing IDE Support and how to cope without it 支持的 IDE 以及如何应对没有它](Appendix E. Existing IDE Support and how to cope without it 支持的 IDE 以及如何应对没有它.md)）。)

###15.8.3. Enabling and disabling tasks 启用和禁用任务

每一项任务有一个默认值为 true 的 enabled 标记。将它设置为 false，可以不让这个任务的任何操作执行。 

Example 15.22. Enabling and disabling tasks

build.gradle

	task disableMe << {
	    println 'This should not be printed if the task is disabled.'
	}
	disableMe.enabled = false

执行 gradle disableMe

	> gradle disableMe
	:disableMe SKIPPED
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

##15.9. Skipping tasks that are up-to-date 跳过处于最新状态的任务

如果您使用 Gradle 自带的任务，如 Java 插件所添加的任务的话，你可能已经注意到 Gradle 将跳过处于最新状态的任务。这种行在您自己定义的任务上也有效，而不仅仅是内置任务。

###15.9.1. Declaring a task's inputs and outputs 声明一个任务的输入和输出

让我们来看一个例子。在这里我们的任务从一个 XML 源文件生成多个输出文件。让我们运行它几次。

Example 15.23. A generator task

build.gradle

	task transform {
	    ext.srcFile = file('mountains.xml')
	    ext.destDir = new File(buildDir, 'generated')
	    doLast {
	        println "Transforming source file."
	        destDir.mkdirs()
	        def mountains = new XmlParser().parse(srcFile)
	        mountains.mountain.each { mountain ->
	            def name = mountain.name[0].text()
	            def height = mountain.height[0].text()
	            def destFile = new File(destDir, "${name}.txt")
	            destFile.text = "$name -> ${height}\n"
	        }
	    }
	}

执行 gradle transform

	> gradle transform
	:transform
	Transforming source file.

执行 gradle transform

	> gradle transform
	:transform
	Transforming source file.

请注意 Gradle 第二次执行执行这项任务时，即使什么都未作改变，也没有跳过该任务。我们的示例任务被用一个操作（action）闭包来定义。Gradle 不知道这个闭包做了什么，也无法自动判断这个任务是否为最新状态。若要使用 Gradle 的最新状态（up-to-date）检查，您需要声明这个任务的输入和输出。

每个任务都有一个 inputs 和 outputs 的属性，用来声明任务的输入和输出。下面，我们修改了我们的示例，声明它将 XML 源文件作为输入，并产生输出到一个目标目录。让我们运行它几次。

Example 15.24. Declaring the inputs and outputs of a task

build.gradle

	task transform {
	    ext.srcFile = file('mountains.xml')
	    ext.destDir = new File(buildDir, 'generated')
	    inputs.file srcFile
	    outputs.dir destDir
	    doLast {
	        println "Transforming source file."
	        destDir.mkdirs()
	        def mountains = new XmlParser().parse(srcFile)
	        mountains.mountain.each { mountain ->
	            def name = mountain.name[0].text()
	            def height = mountain.height[0].text()
	            def destFile = new File(destDir, "${name}.txt")
	            destFile.text = "$name -> ${height}\n"
	        }
	    }
	}

执行 gradle transform

	> gradle transform
	:transform
	Transforming source file.

执行 gradle transform

	> gradle transform
	:transform UP-TO-DATE

现在，Gradle 知道哪些文件要检查以确定任务是否为最新状态。

任务的 inputs 属性是 [TaskInputs](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/TaskInputs.html)类型。任务的 outputs 属性是 [TaskOutputs](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/TaskOutputs.html) 类型。

一个没有定义输出的任务将永远不会被当作是最新的。对于任务的输出并不是文件的场景，或者是更复杂的场景， [TaskOutputs.upToDateWhen() ](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/TaskOutputs.html#upToDateWhen(groovy.lang.Closure))方法允许您以编程方式计算任务的输出是否应该被判断为最新状态。

一个只定义了输出的任务，如果自上一次构建以来它的输出没有改变，那么它会被判定为最新状态。

###15.9.2. How does it work 它是怎么实现的？

在第一次执行任务之前，Gradle 对输入进行一次快照。这个快照包含了输入文件集和每个文件的内容的哈希值。然后 Gradle 执行该任务。如果任务成功完成，Gradle 将对输出进行一次快照。该快照包含输出文件集和每个文件的内容的哈希值。Gradle 会保存这两个快照，直到任务的下一次执行。

之后每一次，在执行任务之前，Gradle 会对输入和输出进行一次新的快照。如果新的快照和前一次的快照一样，Gradle 会假定这些输出是最新状态的并跳过该任务。如果它们不一则， Gradle 则会执行该任务。Gradle 会保存这两个快照，直到任务的下一次执行。

请注意，如果一个任务有一个指定的输出目录，在它上一次执行之后添加到该目录的所有文件都将被忽略，并且不会使这个任务成为过时状态。这是不相关的任务可以在不互相干扰的情况下共用一个输出目录。如果你因为一些理由而不想这样，请考虑使用 [TaskOutputs.upToDateWhen](http://www.gradle.org/docs/current/javadoc/org/gradle/api/tasks/TaskOutputs.html#upToDateWhen(groovy.lang.Closure))

##15.10. Task rules 任务规则

有时你想要有这样一项任务，它的行为依赖于参数数值范围的一个大数或是无限的数字。任务规则是提供此类任务的一个很好的表达方式：

Example 15.25. Task rule

build.gradle

	tasks.addRule("Pattern: ping<ID>") { String taskName ->
	    if (taskName.startsWith("ping")) {
	        task(taskName) << {
	            println "Pinging: " + (taskName - 'ping')
	        }
	    }
	}

执行 gradle -q pingServer1

	> gradle -q pingServer1
	Pinging: Server1

这个字符串参数被用作这条规则的描述。当对这个例子运行 gradle tasks 的时候，这个描述会被显示。

规则不只是从命令行调用任务才起作用。你也可以对基于规则的任务创建依赖关系

Example 15.26. Dependency on rule based tasks

build.gradle
	
	tasks.addRule("Pattern: ping<ID>") { String taskName ->
	    if (taskName.startsWith("ping")) {
	        task(taskName) << {
	            println "Pinging: " + (taskName - 'ping')
	        }
	    }
	}
	
	task groupPing {
	    dependsOn pingServer1, pingServer2
	}

执行 gradle -q groupPing

	> gradle -q groupPing
	Pinging: Server1
	Pinging: Server2

执行 gradle -q tasks 你找不到 “pingServer1” 或 “pingServer2”
任务，但脚本执行逻辑是基于请求执行这些任务的

##15.11. Finalizer tasks 析构器任务

析构器任务是一个 孵化中 的功能 (请参阅[Appendix C. The Feature Lifecycle 特性的生命周期](Appendix C. The Feature Lifecycle 特性的生命周期.md) C.1.2 章节， “Incubating”)。

当最终的任务准备运行时，析构器任务会自动地添加到任务图中。

Example 15.27. Adding a task finalizer

build.gradle

	task taskX << {
	    println 'taskX'
	}
	task taskY << {
	    println 'taskY'
	}

	taskX.finalizedBy taskY

执行 gradle -q taskX

	> gradle -q taskX
	taskX
	taskY

即使最终的任务执行失败，析构器任务也会被执行。

Example 15.28. Task finalizer for a failing task

build.gradle

	task taskX << {
	    println 'taskX'
	    throw new RuntimeException()
	}
	task taskY << {
	    println 'taskY'
	}

	taskX.finalizedBy taskY

执行 gradle -q taskX

	> gradle -q taskX
	taskX
	taskY

另一方面，如果最终的任务什么都不做的话，比如由于失败的任务依赖项或如果它被认为是最新的状态，析构任务不会执行。

在不管构建成功或是失败，都必须清理创建的资源的情况下，析构认为是很有用的。这样的资源的一个例子是，一个 web 容器会在集成测试任务前开始，并且在之后关闭，即使有些测试失败。

你可以使用 [Task.finalizedBy()](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html#org.gradle.api.Task:finalizedBy(java.lang.Object[])） 方法指定一个析构器任务。这个方法接受一个任务实例、 任务名称或[Task.dependsOn()](http://www.gradle.org/docs/current/dsl/org.gradle.api.Task.html#org.gradle.api.Task:dependsOn(java.lang.Object[]))所接受的任何其他输入作为参数。

##15.12. Summary 总结

如果你是从 Ant 转过来的，像 Copy 这种增强的 Gradle 任务，看起来就像是一个 Ant target（目标）和一个 Ant task (任务）之间的混合物。实际上确实是这样子。Gradle 没有像 Ant 那样对任务和目标进行分离。简单的 Gradle 任务就像 Ant 的目标，而增强的 Gradle 任务还包括 Ant 任务方面的内容。Gradle 的所有任务共享一个公共 API，您可以创建它们之间的依赖性。这样的一个任务可能会比一个 Ant 任务更好配置。它充分利用了类型系统，更具有表现力而且易于维护。