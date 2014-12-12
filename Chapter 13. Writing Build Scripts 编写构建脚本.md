Chapter 13. Writing Build Scripts 编写构建脚本
===================

本章着眼于一些编写构建脚本的详细信息。

##13.1. The Gradle build language 构建语言

Gradle 提供一种 domain specific language （领域特定语言）或者说是 DSL，来描述构建。这种构建语言基于 Groovy 中，并进行了一些补充，使其易于描述构建。

构建脚本可以包含任何 Groovy 语言的元素（除了声明标签任何语言元素）。Gradle 假定每个构建脚本使用 UTF-8 编码。

##13.2. The Project API 项目 API 

在[Chapter 07. Java Quickstart 快速开始 Java](https://github.com/waylau/Gradle-2-User-Guide/blob/master/Chapter%2007.%20Java%20Quickstart%20%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B%20Java.md)的教程中，我们使用了 apply() 方法。这方法从何而来？我们之前说在 Gradle 中构建脚本定义了一个 project 。在构建的每一个 project，Gradle 创建了一个 [Project](http://www.gradle.org/docs/current/dsl/org.gradle.api.Project.html) 类型的实例，并在构建脚本中关联此 Project 对象。当构建脚本执行时，它会配置此Project 对象：

* 在构建脚本中，你所调用的任何一个方法，如果在构建脚本中未定义，它将被委托给 Project 对象。
* 在构建脚本中，你所访问的任何一个属性，如果在构建脚本里未定义，它也会被委托给 Project 对象。

下面我们来试试这个，试试访问 Project 对象的 name 属性。

Example 13.1. Accessing property of the Project object

build.gradle

	println name
	println project.name

执行 gradle -q check

	> gradle -q check
	projectApi
	projectApi

这两个 println 语句打印出相同的属性。在生成脚本中未定义的属性，第一次使用时自动委托到P roject 对象。其他语句使用了在任何构建脚本中可以访问的 project 属性，则返回关联的 Project 对象。只有当您定义的属性或方法 Project 对象的一个成员相同名字时，你才需要使用project 属性。

*获取有关编写构建脚本帮助*

*不要忘记您的构建脚本是简单的 Groovy 代码，并驱动着 Gradle API。并且 Project 接口是您在 Gradle API 中访问一切 的入点。所以，如果你想知道什么 '标签（tag）' 在构建脚本中可用，您可以去看项目接口的文档。*


###13.2.1. Standard project properties 标准 project 属性

Project 对象提供了一些在构建脚本中可用的标准的属性。下表列出了常用的几个属性。

<table id="N10A62"><thead><tr>
<td>名称</td>
<td>类型</td>
<td>默认值</td>
    </tr></thead><tbody><tr>
    <td><code class="literal">project</code></td>
    <td><a class="ulink" href="../dsl/org.gradle.api.Project.html" target="_top"><code class="classname">Project</code></a></td>
    <td>The <code class="classname">Project实例</td>
</tr><tr>
    <td><code class="literal">name</code></td>
    <td><code class="classname">String</code></td>
    <td>项目目录的名称</td>
</tr><tr>
    <td><code class="literal">path</code></td>
    <td><code class="classname">String</code></td>
    <td>项目的绝对路径</td>
</tr><tr>
    <td><code class="literal">description</code></td>
    <td><code class="classname">String</code></td>
    <td>项目的描述</td>
</tr><tr>
    <td><code class="literal">projectDir</code></td>
    <td><code class="classname">File</code></td>
    <td>包含生成脚本的目录</td>
</tr><tr>
    <td><code class="literal">buildDir</code></td>
    <td><code class="classname">File</code></td>
    <td><code class="filename"><em class="replaceable"><code>projectDir</code></em>/build</code></td>
</tr><tr>
    <td><code class="literal">group</code></td>
    <td><code class="classname">Object</code></td>
    <td><code class="literal">未指定</code></td>
</tr><tr>
    <td><code class="literal">version</code></td>
    <td><code class="classname">Object</code></td>
    <td><code class="literal">未指定</code></td>
</tr><tr>
    <td><code class="literal">ant</code></td>
    <td><a class="ulink" href="../javadoc/org/gradle/api/AntBuilder.html" target="_top"><code class="classname">AntBuilder</code></a></td>
    <td>An <code class="classname">AntBuilder实例</td>
</tr></tbody></table>

##13.3. The Script API 脚本 API

当 Gradle 执行一个脚本时，它将脚本编译为一个实现了 Script 接口的类。这意味着所有由该 Script 接口声明的属性和方法在您的脚本中是可用的。

##13.4. Declaring variables 声明变量

有两类可以在生成脚本中声明的变量： 局部变量和额外属性。

###13.4.1. Local variables  局部变量

局部变量是用 def 关键字声明的。它们只在定义它们的范围内可以被访问。局部变量是 Groovy 语言底层的一个特征。

Example 13.2. Using local variables

build.gradle

	def dest = "dest"
	
	task copy(type: Copy) {
	    from "source"
	    into dest
	}

###13.4.2. Extra properties 额外属性

Gradle 的域模型中，所有增强的对象都可以容纳用户定义的额外的属性。这包括但并不限于 project、task 和源码集。额外的属性可以通过所属对象的 ext 属性进行添加，读取和设置。或者，可以使用 ext块同时添加多个属性。

Example 13.3. Using extra properties

build.gradle

	apply plugin: "java"
	
	ext {
	    springVersion = "3.1.0.RELEASE"
	    emailNotification = "build@master.org"
	}
	
	sourceSets.all { ext.purpose = null }
	
	sourceSets {
	    main {
	        purpose = "production"
	    }
	    test {
	        purpose = "test"
	    }
	    plugin {
	        purpose = "production"
	    }
	}
	
	task printProperties << {
	    println springVersion
	    println emailNotification
	    sourceSets.matching { it.purpose == "production" }.each { println it.name }
	}

执行 gradle -q printProperties

	> gradle -q printProperties
	3.1.0.RELEASE
	build@master.org
	main
	plugin

在此示例中， 一个 ext 代码块将两个额外属性添加到 project 对象中。此外，通过将 ext.purpose设置为 null（null是一个允许的值），一个名为 purpose 的属性被添加到每个源码集。一旦属性被添加，他们就可以像预定的属性一样被读取和设置。

通过添加属性所要求特殊的语法，Gradle 可以在你试图设置 （预定义的或额外的） 的属性，但该属性拼写错误或不存在时马上失败。额外属性在任何能够访问它们所属的对象的地方都可以被访问，这使它们有着比局部变量更广泛的作用域。父项目上的额外属性，在子项目中也可以访问。

有关额外属性和它们的 API 的详细信息，请参阅[ExtraPropertiesExtension](http://www.gradle.org/docs/current/dsl/org.gradle.api.plugins.ExtraPropertiesExtension.html) 类的 API。

##13.5. Some Groovy basics 一些 Groovy 的基础

Groovy 提供了用于创建 DSL 的大量特点，并且 Gradle 构建语言利用了这些特点。了解构建语言是如何工作的，将有助于你编写构建脚本，特别是当你开始写自定义插件和 task 的时候。

###13.5.1. Groovy JDK

Groovy 对 Java 的标准类增加了很多有用的方法。例如， Iterable 新增的each方法，会对Iterable  的元素进行遍历：

Example 13.4. Groovy JDK methods

build.gradle

	// Iterable gets an each() method
	configurations.runtime.each { File f -> println f }

在 [http://groovy.codehaus.org/groovy-jdk/](http://groovy.codehaus.org/groovy-jdk/) 查看详情

###13.5.2. Property accessors属性访问器

Groovy 会自动地把一个属性的引用转换为对适当的 getter 或 setter 方法的调用。

Example 13.5. Property accessors

build.gradle

	// Using a getter method
	println project.buildDir
	println getProject().getBuildDir()
	
	// Using a setter method
	project.buildDir = 'target'
	getProject().setBuildDir('target')

###13.5.3. Optional parentheses on method calls 括号可选的方法调用

调用方法时括号是可选的。

Example 13.6. Method call without parentheses

build.gradle

	test.systemProperty 'some.prop', 'value'
	test.systemProperty('some.prop', 'value')

###13.5.4. List and map literals 

Groovy 提供了一些定义 List 和 Map 实例的快捷写法。两种类型都是简单的 literal，但 map literal 有一些有趣的曲折。

例如，“apply” 方法（如你通常应用插件）需要 map 参数。然而，当你有一个像  “apply plugin:'java'”，你实际上并没有使用  map literal ，你实际上使用“命名的参数”，这几乎是和  map literal 相同的语法（不包包装器）。命名参数列表将被转换成一个 map 当方法被调用时，但它没有在开始作为一个 map。

Example 13.7. List and map literals

build.gradle

	// List literal
	test.includes = ['org/gradle/api/**', 'org/gradle/internal/**']
	
	List<String> list = new ArrayList<String>()
	list.add('org/gradle/api/**')
	list.add('org/gradle/internal/**')
	test.includes = list
	
	// Map literal.
	Map<String, String> map = [key1:'value1', key2: 'value2']
	
	// Groovy will coerce named arguments
	// into a single map argument
	apply plugin: 'java'

###13.5.5. Closures as the last parameter in a method 作为方法最后一个参数的闭包

Gradle DSL 在很多地方使用闭包。你可以在这里查看更多有关闭包的资料。当方法的最后一个参数是一个闭包时，你可以把闭包放在方法调用之后：

Example 13.8. Closure as method parameter

build.gradle

	repositories {
	    println "in a closure"
	}
	repositories() { println "in a closure" }
	repositories({ println "in a closure" })

###13.5.6. Closure delegate  闭包委托

每个闭包都有一个委托对象，Groovy 使用它来查找变量和方法的引用，而不是作为闭包的局部变量或参数。Gradle 在配置闭包中使用到它，把委托对象设置为被配置的对象。

Example 13.9. Closure delegates

build.gradle

	dependencies {
	    assert delegate == project.dependencies
	    testCompile('junit:junit:4.11')
	    delegate.testCompile('junit:junit:4.11')
	}
