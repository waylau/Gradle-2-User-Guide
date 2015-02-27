Appendix E. Existing IDE Support and how to cope without it 支持的 IDE 以及如何应对没有它
===================

##E.1. IntelliJ

Idea IntelliJ 可以很好的开发 Gradle ，提供了不错的插件。这个 IDE 同样支持 Gradle 的构建脚本。IntelliJ 允许您定义任何文件形式来被解释为一个 Groovy 脚本。在 Gradle ，您可以定义 build.gradle 和settings.gradle 这种模式。这就已经很有用。现在缺少的是路径的 Gradle 的二进制文件的 Gradle 类提供内容辅助。你可以添加 Gradle jar （你可以在你的发布包找到）到你的项目的类路径。它真的不属于那里，但如果你这样做，你将可以有一个出色的 IDE 支持开发 Gradle 脚本。当然，如果你使用其他库的构建脚本会进一步污染你的项目的类路径。

我们希望在未来的 *.gradle 文件在 IntelliJ 中能得到特殊待遇，你能为它们定义一个特定的路径。

##E.2. Eclipse

在 Eclipse 有 Groovy 的插件。我们不知道它是什么样的状态，以及它是如何支持 Gradle。在本用户指南的下一版本我们希望能写更多关于这个。

##E.3. Using Gradle without IDE support 无需IDE

我们能为您做些什么是让你输入东西就像抛出 new  org.gradle.api.tasks.StopExecutionException() 那样，而仅仅需要输入抛出 new StopExecutionException() 来代替 。我们是通过添加 
Gradle 脚本自动实现的。

Figure E.1. gradle-imports

	import org.gradle.*
	import org.gradle.api.*
	import org.gradle.api.artifacts.*
	import org.gradle.api.artifacts.cache.*
	import org.gradle.api.artifacts.component.*
	import org.gradle.api.artifacts.dsl.*
	import org.gradle.api.artifacts.ivy.*
	import org.gradle.api.artifacts.maven.*
	import org.gradle.api.artifacts.query.*
	import org.gradle.api.artifacts.repositories.*
	import org.gradle.api.artifacts.result.*
	import org.gradle.api.component.*
	import org.gradle.api.distribution.*
	import org.gradle.api.distribution.plugins.*
	import org.gradle.api.dsl.*
	import org.gradle.api.execution.*
	import org.gradle.api.file.*
	import org.gradle.api.initialization.*
	import org.gradle.api.initialization.dsl.*
	import org.gradle.api.invocation.*
	import org.gradle.api.java.archives.*
	import org.gradle.api.logging.*
	import org.gradle.api.plugins.*
	import org.gradle.api.plugins.announce.*
	import org.gradle.api.plugins.antlr.*
	import org.gradle.api.plugins.buildcomparison.gradle.*
	import org.gradle.api.plugins.jetty.*
	import org.gradle.api.plugins.osgi.*
	import org.gradle.api.plugins.quality.*
	import org.gradle.api.plugins.scala.*
	import org.gradle.api.plugins.sonar.*
	import org.gradle.api.plugins.sonar.model.*
	import org.gradle.api.publish.*
	import org.gradle.api.publish.ivy.*
	import org.gradle.api.publish.ivy.plugins.*
	import org.gradle.api.publish.ivy.tasks.*
	import org.gradle.api.publish.maven.*
	import org.gradle.api.publish.maven.plugins.*
	import org.gradle.api.publish.maven.tasks.*
	import org.gradle.api.publish.plugins.*
	import org.gradle.api.reporting.*
	import org.gradle.api.reporting.components.*
	import org.gradle.api.reporting.dependencies.*
	import org.gradle.api.reporting.plugins.*
	import org.gradle.api.resources.*
	import org.gradle.api.specs.*
	import org.gradle.api.tasks.*
	import org.gradle.api.tasks.ant.*
	import org.gradle.api.tasks.application.*
	import org.gradle.api.tasks.bundling.*
	import org.gradle.api.tasks.compile.*
	import org.gradle.api.tasks.diagnostics.*
	import org.gradle.api.tasks.incremental.*
	import org.gradle.api.tasks.javadoc.*
	import org.gradle.api.tasks.scala.*
	import org.gradle.api.tasks.testing.*
	import org.gradle.api.tasks.testing.junit.*
	import org.gradle.api.tasks.testing.testng.*
	import org.gradle.api.tasks.util.*
	import org.gradle.api.tasks.wrapper.*
	import org.gradle.buildinit.plugins.*
	import org.gradle.buildinit.tasks.*
	import org.gradle.external.javadoc.*
	import org.gradle.ide.cdt.*
	import org.gradle.ide.cdt.tasks.*
	import org.gradle.ide.visualstudio.*
	import org.gradle.ide.visualstudio.plugins.*
	import org.gradle.ide.visualstudio.tasks.*
	import org.gradle.ivy.*
	import org.gradle.jvm.*
	import org.gradle.jvm.platform.*
	import org.gradle.jvm.plugins.*
	import org.gradle.jvm.tasks.*
	import org.gradle.jvm.toolchain.*
	import org.gradle.language.*
	import org.gradle.language.assembler.*
	import org.gradle.language.assembler.plugins.*
	import org.gradle.language.assembler.tasks.*
	import org.gradle.language.base.*
	import org.gradle.language.base.artifact.*
	import org.gradle.language.base.plugins.*
	import org.gradle.language.c.*
	import org.gradle.language.c.plugins.*
	import org.gradle.language.c.tasks.*
	import org.gradle.language.coffeescript.*
	import org.gradle.language.cpp.*
	import org.gradle.language.cpp.plugins.*
	import org.gradle.language.cpp.tasks.*
	import org.gradle.language.java.*
	import org.gradle.language.java.artifact.*
	import org.gradle.language.java.plugins.*
	import org.gradle.language.java.tasks.*
	import org.gradle.language.javascript.*
	import org.gradle.language.jvm.*
	import org.gradle.language.jvm.plugins.*
	import org.gradle.language.jvm.tasks.*
	import org.gradle.language.nativeplatform.*
	import org.gradle.language.nativeplatform.tasks.*
	import org.gradle.language.objectivec.*
	import org.gradle.language.objectivec.plugins.*
	import org.gradle.language.objectivec.tasks.*
	import org.gradle.language.objectivecpp.*
	import org.gradle.language.objectivecpp.plugins.*
	import org.gradle.language.objectivecpp.tasks.*
	import org.gradle.language.rc.*
	import org.gradle.language.rc.plugins.*
	import org.gradle.language.rc.tasks.*
	import org.gradle.language.scala.*
	import org.gradle.language.scala.plugins.*
	import org.gradle.language.scala.tasks.*
	import org.gradle.language.scala.toolchain.*
	import org.gradle.maven.*
	import org.gradle.model.*
	import org.gradle.nativeplatform.*
	import org.gradle.nativeplatform.platform.*
	import org.gradle.nativeplatform.plugins.*
	import org.gradle.nativeplatform.tasks.*
	import org.gradle.nativeplatform.test.*
	import org.gradle.nativeplatform.test.cunit.*
	import org.gradle.nativeplatform.test.cunit.plugins.*
	import org.gradle.nativeplatform.test.cunit.tasks.*
	import org.gradle.nativeplatform.test.plugins.*
	import org.gradle.nativeplatform.test.tasks.*
	import org.gradle.nativeplatform.toolchain.*
	import org.gradle.nativeplatform.toolchain.plugins.*
	import org.gradle.platform.base.*
	import org.gradle.platform.base.binary.*
	import org.gradle.platform.base.component.*
	import org.gradle.platform.base.test.*
	import org.gradle.play.*
	import org.gradle.play.platform.*
	import org.gradle.play.plugins.*
	import org.gradle.play.tasks.*
	import org.gradle.play.toolchain.*
	import org.gradle.plugin.use.*
	import org.gradle.plugins.ear.*
	import org.gradle.plugins.ear.descriptor.*
	import org.gradle.plugins.ide.api.*
	import org.gradle.plugins.ide.eclipse.*
	import org.gradle.plugins.ide.idea.*
	import org.gradle.plugins.javascript.base.*
	import org.gradle.plugins.javascript.coffeescript.*
	import org.gradle.plugins.javascript.envjs.*
	import org.gradle.plugins.javascript.envjs.browser.*
	import org.gradle.plugins.javascript.envjs.http.*
	import org.gradle.plugins.javascript.envjs.http.simple.*
	import org.gradle.plugins.javascript.jshint.*
	import org.gradle.plugins.javascript.rhino.*
	import org.gradle.plugins.javascript.rhino.worker.*
	import org.gradle.plugins.signing.*
	import org.gradle.plugins.signing.signatory.*
	import org.gradle.plugins.signing.signatory.pgp.*
	import org.gradle.plugins.signing.type.*
	import org.gradle.plugins.signing.type.pgp.*
	import org.gradle.process.*
	import org.gradle.sonar.runner.*
	import org.gradle.sonar.runner.plugins.*
	import org.gradle.sonar.runner.tasks.*
	import org.gradle.testing.jacoco.plugins.*
	import org.gradle.testing.jacoco.tasks.*
	import org.gradle.util.*

