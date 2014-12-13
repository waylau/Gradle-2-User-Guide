Chapter 14. Tutorial - 'This and That' 教程-这个那个
===================

##14.1. Directory creation 创建目录

多个 task 依赖于现存的目录，这是常见的情况。当然，你可以在 task 前添加 `mkdir ` 但这不是好办法，因为你只需要一次，却要不断重复代码序列（看看  DRY principle ）。好的做法是在 task 间使用 dependsOn 来重用 task 创建目录

Example 14.1. Directory creation with mkdir

build.gradle

	def classesDir = new File('build/classes')
	
	task resources << {
	    classesDir.mkdirs()
	    // do something
	}
	task compile(dependsOn: 'resources') << {
	    if (classesDir.isDirectory()) {
	        println 'The class directory exists. I can operate'
	    }
	    // do something
	}

执行 gradle -q compile 输出

	> gradle -q compile
	The class directory exists. I can operate

##14.2. Gradle properties and system properties 属性

Gradle 提供了许多方式将属性添加到您的构建中。 从 Gradle 启动的 JVM，你可以使用 -D 命令行选项向它传入一个系统属性。 Gradle 命令的 -D 选项和 java 命令的 -D 选项有着同样的效果。

此外，您也可以通过属性文件直接向您的 project 对象添加属性。您可以把一个 gradle.properties 文件放在 Gradle 的用户主目录 （默认如果不是 USER_HOME /.gradle 设置的话，就由 “GRADLE_USER_HOME”  环境变量定义 ） ，或您的项目目录中。对于多项目构建，您可以将 gradle.properties 文件放在任何子项目的目录中。通过 project 对象，可以访问到 gradle.properties 里的属性。用户的主目录中的属性文件比项目目录中的属性文件更先被访问到。 

你还可以通过使用 -P 命令行选项来直接向您的 project 对象添加属性。

也可以通过 特别命名的系统属性 或者环境属性把属性设置 project 属性。这个特性非常有用，通常出于安全原因，当你在持续集成的服务器没有管理员权限时，对于设置属性值你是不可见的。在这种情况下，你就不能使用 -P 选项，也不能修改系统级别的配置文件。正确的策略是，要改变你的持续集成配置建设工作，增加一个环境变量设置符合预期的模式。这不会对系统正常的用户可见的。（ Jenkins, Teamcity, 或者 Bamboo 是这些 CI （Continuous Integration  持续集成）服务商，提供这些功能）

如果环境变量名称类似与 ORG_GRADLE_PROJECT_prop=somevalue, Gradle 将会设置 prop 属性到你的 project 对象，值就是 somevalue 。Gradle 也提供了系统属性的支持，但有着不同命名方式，类似的  org.gradle.project.prop

也可在 gradle.properties 设置系统属性。如果此类文件中的属性有一个 `systemProp.` 的前缀，像`systemProp.propName`,该属性和它的值会被添加到系统属性，且不带此前缀。在多 project 构建中，除了在根项目之外的任何项目里的`systemProp.` 属性集都将被忽略。也就是，只有根 project 的 gradle.properties 文件里的 `systemProp.`前缀的 属性会被作为系统属性。 

Example 14.2. Setting properties with a gradle.properties file

gradle.properties

	gradlePropertiesProp=gradlePropertiesValue
	sysProp=shouldBeOverWrittenBySysProp
	envProjectProp=shouldBeOverWrittenByEnvProp
	systemProp.system=systemValue

build.gradle

	task printProps << {
	    println commandLineProjectProp
	    println gradlePropertiesProp
	    println systemProjectProp
	    println envProjectProp
	    println System.properties['system']
	}

执行 gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps 输出

	> gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps
	commandLineProjectPropValue
	gradlePropertiesValue
	systemPropertyValue
	envPropertyValue
	systemValue

###14.2.1. Checking for project properties 检查 project 属性

当你要使用一个变量时，你可以仅通过其名称在构建脚本中访问一个项目的属性。如果此属性不存在，则会引发异常，并且构建失败。如果您的构建脚本依赖于一些可选属性，而这些属性用户可能在比如 gradle.properties 文件中设置，您就需要在访问它们之前先检查它们是否存在。你可以通过使用方法 hasProperty('propertyName') 来进行检查，它返回 true 或 false。

##14.3. Configuring the project using an external build script 使用外部构建脚本配置项目

你可以使用一个外部构建脚本配置当前 project 。所有的 Gradle 构建语言都可用在外部脚本。。您甚至可以在外部脚本中应用其他脚本。

Example 14.3. Configuring the project using an external build script

build.gradle
	
	apply from: 'other.gradle'
	other.gradle
	
	println "configuring $project"
	task hello << {
	    println 'hello from other script'
	}

执行 gradle -q hello 输出

	> gradle -q hello
	configuring root project 'configureProjectUsingScript'
	hello from other script

##14.4. Configuring arbitrary objects 配置任意对象

您可以用以下非常易理解的方式配置任意对象。 

Example 14.4. Configuring arbitrary objects

build.gradle

	task configure << {
	    def pos = configure(new java.text.FieldPosition(10)) {
	        beginIndex = 1
	        endIndex = 5
	    }
	    println pos.beginIndex
	    println pos.endIndex
	}

执行 gradle -q configure 输出

	> gradle -q configure
	1
	5

##14.5. Configuring arbitrary objects using an external script 使用外部脚本配置任意对象

Example 14.5. Configuring arbitrary objects using a script

build.gradle

	task configure << {
	    def pos = new java.text.FieldPosition(10)
	    // Apply the script
	    apply from: 'other.gradle', to: pos
	    println pos.beginIndex
	    println pos.endIndex
	}

other.gradle

执行 gradle -q configure 输出

	> gradle -q configure
	1
	5

##14.6. Caching 缓存

为了提高响应速度，默认情况下 Gradle 会缓存所有已编译的脚本。这包括所有构建脚本，初始化脚本和其他脚本。你第一次运行一个项目构建时， Gradle 会创建 .gradle 目录，用于存放已编译的脚本。下次你运行此构建时， 如果该脚本自它编译后没有被修改，Gradle 会使用这个已编译的脚本。否则该脚本会重新编译，并把最新版本存在缓存中。如果您通过 --recompile-scripts 选项运行 Gradle ，会丢弃缓存的脚本，然后重新编译此脚本并将其存在缓存中。通过这种方式，您可以强制 Gradle 重新生成缓存
