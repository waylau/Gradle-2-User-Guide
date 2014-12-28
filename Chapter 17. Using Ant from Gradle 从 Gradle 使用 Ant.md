Chapter 17. Using Ant from Gradle 从 Gradle 使用 Ant
===================

Gradle 提供了对 Ant 的优秀集成。可以在你的 Gradle 构建中，使用单独的 Ant 任务或整个 Ant 构建。事实上，你会发现在 Gradle 中使用 Ant 任务比使用 Ant 的 XML 格式更容易也更强大。你甚至可以只把 Gradle 当作一个强大的 Ant 任务脚本的工具。

Ant 可以分为两层。第一层是 Ant 的语言。它提供了用于 build.xml，处理的目标，特殊的构造方法比如宏，还有其他等等的语法。换句话说，除了 Ant 任务和类型之外全部都有。Gradle 理解这种语言，并允许您直接导入你的 Ant build.xml 到 Gradle 项目中。然后你可以使用你的 Ant 构建中的 target，就好像它们是 Gradle task 一样。

Ant 的第二层是其丰富的 Ant 任务和类型，如 javac、copy或jar。这一层 Gradle 只靠 Groovy 和非常棒的 AntBuilder，对其提供了集成。

最后，由于构建脚本是 Groovy 脚本，所以您始终可以作为一个外部进程来执行 Ant 构建。你的构建脚本可能包含有类似这样的语句：`"ant clean compile".execute()` (在 Groovy 中，你可以执行字符串。了解更多关于执行外部进程，请查看 9.3.2 的'Groovy in Action'或者   Groovy wiki)

你可以把 Gradle 的 Ant 集成当成一个路径，将你的构建从 Ant 迁移至 Gradle 。例如，你可以通过导入您现有的 Ant 构建来开始。然后，可以将您的依赖声明从 Ant 脚本移到您的构建文件。最后，您可以将整个任务移动到您的构建文件，或者把它们替换为一些 Gradle 插件。这个过程可以随着时间一点点完成，并且在这整个过程当中你的 Gradle 构建都可以使用用。

##17.1. Using Ant tasks and types in your build 在你的构建中使用 Ant 的任务和类型

在构建脚本中，Gradle 提供了一个名为 ant 的属性。它指向一个 [AntBuilder](http://gradle.org/docs/current/javadoc/org/gradle/api/AntBuilder.html) 实例。AntBuilder 用于从你的构建脚本中访问 Ant 任务、 类型和属性。从 Ant 的 build.xml 格式到 Groovy 之间有一个非常简单的映射，下面解释。

通过调用 AntBuilder 实例上的一个方法，可以执行一个 Ant 任务。你可以把任务名称当作方法名称使用。例如，你可以通过调用 ant.echo() 方法执行 Ant 的 echo 任务。Ant 任务的属性会作为 Map 参数传给该方法。下面是执行 echo 任务的例子。请注意我们还可以混合使用 Groovy 代码和 Ant 任务标记。这将会非常强大。

Example 17.1. Using an Ant task

build.gradle

	task hello << {
	    String greeting = 'hello from Ant'
	    ant.echo(message: greeting)
	}

执行 gradle hello

	> gradle hello
	:hello
	[ant:echo] hello from Ant
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

你可以把一个嵌套文本，通过作为任务方法调用的参数，把它传给一个 Ant 任务。在此示例中，我们将把作为嵌套文本的消息传给 echo 任务：

Example 17.2. Passing nested text to an Ant task

build.gradle

	task hello << {
	    ant.echo('hello from Ant')
	}

执行 gradle hello

	> gradle hello
	:hello
	[ant:echo] hello from Ant
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

你可以在一个闭包里把嵌套的元素传给一个 Ant 任务。嵌套元素的定义方式与任务相同，通过调用与我们要定义的元素一样的名字的方法

Example 17.3. Passing nested elements to an Ant task

build.gradle

	task zip << {
	    ant.zip(destfile: 'archive.zip') {
	        fileset(dir: 'src') {
	            include(name: '**.xml')
	            exclude(name: '**.java')
	        }
	    }
	}

您可以用访问任务同样的方法，把类型名字作为方法名称，访问 Ant 类型。方法调用返回 Ant 数据类型，然后可以在构建脚本中直接使用。在以下示例中，我们创建一个 Ant 的 path 对象，然后循环访问它的内容。

Example 17.4. Using an Ant type

build.gradle

	task list << {
	    def path = ant.path {
	        fileset(dir: 'libs', includes: '*.jar')
	    }
	    path.list().each {
	        println it
	    }
	}

更多有关 AntBuilder 见 8.4 的'Groovy in Action' 或者 [Groovy Wiki](http://groovy.codehaus.org/Using+Ant+from+Groovy)

###17.1.1. Using custom Ant tasks in your build 在构建中使用自定义的 Ant 任务

要使自定义任务在您的构建中可用，你可以使用 Ant 任务 taskdef（通常更容易） 或 typedef，就像在 build.xml 文件中一样。然后，您可以像引用内置的 Ant 任务一样引用自定义 Ant 任务。

Example 17.5. Using a custom Ant task

build.gradle

	task check << {
	    ant.taskdef(resource: 'checkstyletask.properties') {
	        classpath {
	            fileset(dir: 'libs', includes: '*.jar')
	        }
	    }
	    ant.checkstyle(config: 'checkstyle.xml') {
	        fileset(dir: 'src')
	    }
	}

你可以使用 Gradle 的依赖管理组合类路径，以用于自定义任务。要做到这一点，你需要定义一个自定义配置的类路径中，然后将一些依赖项添加到配置中。这在 50.4章节，“如何声明你的依赖”有更详细的描述。

Example 17.6. Declaring the classpath for a custom Ant task

build.gradle

	configurations {
	    pmd
	}
	
	dependencies {
	    pmd group: 'pmd', name: 'pmd', version: '4.2.5'
	}

若要使用类路径配置，请使用自定义配置里的 asPath 属性。

Example 17.7. Using a custom Ant task and dependency management together

build.gradle

task check << {
    ant.taskdef(name: 'pmd',
                classname: 'net.sourceforge.pmd.ant.PMDTask',
                classpath: configurations.pmd.asPath)
    ant.pmd(shortFilenames: 'true',
            failonruleviolation: 'true',
            rulesetfiles: file('pmd-rules.xml').toURI().toString()) {
        formatter(type: 'text', toConsole: 'true')
        fileset(dir: 'src')
    }
}

##17.2. Importing an Ant build 导入 Ant 的构建

你可以使用 ant.importBuild() 方法来向 Gradle 项目导入一个 Ant 构建。当您导入一个 Ant 构建时，每个 Ant 目标被视为一个 Gradle 任务。这意味着你可以用与 Gradle 任务完全相机的方式操纵和执行 Ant 目标。

Example 17.8. Importing an Ant build

build.gradle

	ant.importBuild 'build.xml'

build.xml
	
	<project>
	    <target name="hello">
	        <echo>Hello, from Ant</echo>
	    </target>
	</project>

执行 gradle hello

	> gradle hello
	:hello
	[ant:echo] Hello, from Ant
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

您可以添加一个依赖于 Ant 目标的任务：

build.gradle
	
	ant.importBuild 'build.xml'
	
	task intro(dependsOn: hello) << {
	    println 'Hello, from Gradle'
	}

执行 gradle intro

	> gradle intro
	:hello
	[ant:echo] Hello, from Ant
	:intro
	Hello, from Gradle
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

或者，您可以将行为添加到 Ant 目标中：

Example 17.10. Adding behaviour to an Ant target

build.gradle

	ant.importBuild 'build.xml'

	hello << {
	    println 'Hello, from Gradle'
	}

执行 gradle hello

	> gradle hello
	:hello
	[ant:echo] Hello, from Ant
	Hello, from Gradle
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

它也可以用于一个依赖于 Gradle 任务的 Ant 目标：

Example 17.11. Ant target that depends on Gradle task

build.gradle

	ant.importBuild 'build.xml'
	
	task intro << {
	    println 'Hello, from Gradle'
	}

build.xml

	<project>
	    <target name="hello" depends="intro">
	        <echo>Hello, from Ant</echo>
	    </target>
	</project>

执行 gradle hello

	> gradle hello
	:intro
	Hello, from Gradle
	:hello
	[ant:echo] Hello, from Ant
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs‘

有时可能需要“改名”来避免 Ant target  生成的 task 与现有的 Gradle task 在命名上冲突。使用 [AntBuilder.importBuild()](http://gradle.org/docs/current/javadoc/org/gradle/api/AntBuilder.html#importBuild(java.lang.Object, org.gradle.api.Transformer)) 方法

Example 17.12. Renaming imported Ant targets

build.gradle

	ant.importBuild('build.xml') { antTargetName ->
	    'a-' + antTargetName
	}

build.xml

	<project>
	    <target name="hello">
	        <echo>Hello, from Ant</echo>
	    </target>
	</project>

执行 gradle a-hello

	> gradle a-hello
	:a-hello
	[ant:echo] Hello, from Ant
	
	BUILD SUCCESSFUL
	
	Total time: 1 secs

请注意，虽然这个方法的第二个参数应该是一个 [Transformer](http://gradle.org/docs/current/javadoc/org/gradle/api/Transformer.html)，在 Groovy 编程时可以使用简单的闭包而不是一个匿名内部类（或类似的），因为 [Groovy 的支持自动强制关闭 single-abstract-method（单一的抽象方法）的类型](http://mrhaki.blogspot.ie/2013/11/groovy-goodness-implicit-closure.html)。

##17.3. Ant properties and references 关于 Ant 的属性和引用

有几种方法来设置 Ant 属性，以便使该属性被 Ant 任务使用。你可以直接在 AntBuilder 实例上设置属性。Ant 属性也可以从一个你可以修改的 Map 中获得。您还可以使用 Ant property 任务。下面是一些如何做到这一点的例子。

Example 17.13. Setting an Ant property

build.gradle

	ant.buildDir = buildDir
	ant.properties.buildDir = buildDir
	ant.properties['buildDir'] = buildDir
	ant.property(name: 'buildDir', location: buildDir)

build.xml

	<echo>buildDir = ${buildDir}</echo>

有几种方法可以设置 Ant 引用：

Example 17.14. Getting an Ant property

build.xml

	<property name="antProp" value="a property defined in an Ant build"/>

build.gradle

	println ant.antProp
	println ant.properties.antProp
	println ant.properties['antProp']

有几种方法可以获取 Ant 引用：

Example 17.15. Setting an Ant reference

build.gradle

	ant.path(id: 'classpath', location: 'libs')
	ant.references.classpath = ant.path(location: 'libs')
	ant.references['classpath'] = ant.path(location: 'libs')

build.xml

	<path refid="classpath"/>

有几种方法可以获取 Ant 引用：

Example 17.16. Getting an Ant reference

build.xml

	<path id="antPath" location="libs"/>

build.gradle

	println ant.references.antPath
	println ant.references['antPath']

##17.4. API

[AntBuilder](http://gradle.org/docs/current/javadoc/org/gradle/api/AntBuilder.html) 提供 Ant 的集成