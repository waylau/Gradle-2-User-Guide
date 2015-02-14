Chapter 20. The Build Environment 构建环境
===================

##20.1. 通过 gradle.propert 配置构建环境

Gradle 提供几个选项,使它容易配置将用于执行构建的 Java 进程。同时可以通过 GRADLE_OPTS 或 JAVA_OPTS 配置这些在你本地环境,包含的设置包括比如 JVM 内存设置,Java home,守护进程开/关,它们可以和你的项目在你的版本控制系统中被版本化的话，将会更有用，这样整个团队就可以使用一致的环境了。在你的构建当中，建立一致的环境，就和把这些配置放进 gradle.properties 文件一样简单。这些配置将会按以下顺序被应用（以防在多个地方都有配置时只有最后一个 生效） 

* 从 gradle.properties 在项目构建 dir。
* 从 gradle.properties 在 gradle user home.
* 从系统属性,例如当 -Dsome.property 在命令行上设置。

可以使用以下属性来配置 Gradle 构建环境:

org.gradle.daemon

当设置为true 时，Gradle 守护进程会运行构建。对于本地开发者的构建而言，这是我们最喜欢的属性。开发人员的环境在速度和反馈上会优化，所以我们几乎总是使用守护进程运行 Gradle 作业。由于 CI 环境在一致性和可靠性上的优化，我们不通过守护进程运行 CI 构建（即长时间运行进程）

org.gradle.java.home

为 Gradle 构建进程指定 java home 目录。这个值可以设置为 jdk 或jre 的位置，不过，根据你的构建所做的，选择 jdk 会更安全。如果该设置未指定，将使用合理的默认值。

org.gradle.jvmargs

指定用于该守护进程的 jvmargs。该设置对调整内存设置特别有用。目前的内存上的默认设置很大方。

org.gradle.configureondemand

启用新的孵化模式，可以在配置项目时使得 Gradle 具有选择性。只适用于相关的项目被配置为在大型多项目中更快地构建。请参阅 [Section 57.1.1.1, “Configuration on demand”.](http://gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand)

org.gradle.parallel

如果配置了这一个，Gradle 将在孵化的并行模式下运行。
