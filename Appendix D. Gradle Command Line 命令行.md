Appendix D. Gradle Command Line 命令行
===================

**gradle** 命令行用法如下:

	gradle [option...] [task...]

现存的命令行选项如下:

**-?, -h, --help**：
用于显示帮助信息
	

**-a, --no-rebuild**：不要重新编译依赖

**--all**：在任务列表中显示附加的详细信息，见 11.6.2, “Listing tasks”.

**-b, --build-file**：指定构建文件，见 Section 11.5, “Selecting which build to execute”.

**-c, --settings-file**：指定配置文件

**--console**：指定控制台输出产生哪种类型,plain 是生成普通的文本，该选项禁止所有颜色和富文本输出； auto (默认)当构建程序与控制台相关联时启动 颜色和富文本输出，或者不关联时生成普通文本；rich 启动颜色和富文本输出，忽略构建程序是否关联了控制台,如果没有关联构建输出将输出 ANSI 控制字符来生产富文本输出

**--continue**：任务失败时继续执行任务

**--configure-on-demand (incubating)**：只有相关的项目都配置在此构建中运行。这意味着为大型多项目拥有更快的构建，见 Section 57.1.1.1, “Configuration on demand”.

**-D, --system-prop**：设置 JVM 的系统属性, 如 -Dmyprop=myvalue. See Section 14.2, “Gradle properties and system properties”.

**-d, --debug**：记录在 debug 模式(包含平常的 stacktrace). See Chapter 18, Logging.

**-g, --gradle-user-home**：指定 Gradle 的用户目录，默认是在home 目录

**--gui**：启动 Gradle GUI. 见 Chapter 12, Using the Gradle Graphical User Interface.

**-I, --init-script**：指定初始化的脚本，见 Chapter 61, Initialization Scripts.

**-i, --info**：设置日志的信息级别，见Chapter 18, Logging.

**-m, --dry-run**：运行构建，所有任务的动作都禁止，见 Section 11.7, “Dry Run”.

**--offline**：指定构建操作不访问网络资源，见 Section 51.9.2, “Command line options to override caching”.

**-P, --project-prop**：设置 根项目的项目属性，如 -Pmyprop=myvalue，见 Section 14.2, “Gradle properties and system properties”.

**-p, --project-dir**：指定 Gradle 的启动目录，默认是当前目录，见 Section 11.5, “Selecting which build to execute”.

**--parallel (incubating)**：并行构建项目， Gradle 尝试确定用来使用的执行程序线程的最佳数量。只在解耦项目中使用此选项(见 Section 57.9, “Decoupled Projects”).

**--parallel-threads (incubating)**：并行构建项目，执行线程数量，如 --parallel-threads=3。只在解耦项目中使用此选项(见 Section 57.9, “Decoupled Projects”).

**--profile**：
描述执行时间并生成报告在 buildDir/reports/profile 目录，见 Section 11.6.7, “Profiling a build”.

**--project-cache-dir**：执行 project-specific 的缓冲目录，默认是在 .gradle 所在根项目目录，见 Section 14.6, “Caching”.

**-q, --quiet**：只记录错误，见 Chapter 18, Logging.

**--recompile-scripts**：
指定被缓存的构建脚本跳过，被迫重新编译。见 Section 14.6, “Caching”.

**--refresh-dependencies**：更新依赖的状态，见 Section 51.9.2, “Command line options to override caching”.

**--rerun-tasks**：指定忽略任意任务优化
Specifies that any task optimization is ignored.

**-S, --full-stacktrace**：打印全部异常（非常详细） stacktrace 消息，见 Chapter 18, Logging.

**-s, --stacktrace**：也打印出用户异常 (e.g. 编译错误). 见 Chapter 18, Logging.

**-u, --no-search-upwards**：不要再父目录寻找 settings.gradle 文件

**-v, --version**：打印版本信息

**-x, --exclude-task**：指定要排除的执行的任务，见 Section 11.2, “Excluding tasks”.

当你执行 gradle -h 上述信息将会打印到控制台

###D.1. 过时的命令行选项

**--no-color**：不要使用颜色。这个命令替换为 --console plain 选项

###D.2. DAEMON 命令行选项

Chapter 19, Gradle Daemon 包含了很多信息关于 daemon，来避免使用 --daemon 

**--daemon**：
使用 Gradle daemon 来构建，当 daemon 未运行或者现存的daemon太繁忙时，可以启动 daemon ，详见 Chapter 19

**--foreground**：
在前景中启动 Gradle daemon ，在debug 或 troubleshooting时有用，可以轻松监控构建执行

**--no-daemon**：
不使用 Gradle daemon 

**--stop**：
停止运行的 Gradle daemon 

###D.3. 系统属性

以下系统属性可用于 gradle 命令。请注意，命令行选项优先于系统属性。

**gradle.user.home**：执行 Gradle user用户目录

将 Section 20.1, “Configuring the build environment via gradle.properties” 

###D.4. 环境变量
 
**GRADLE_OPTS**：
指定用于启动 JVM 命令行参数。这可以是用于设置系统属性。例如，你可以设置  GRADLE_OPTS="-Dorg.gradle.daemon=true" 使用 Gradle daemon 而无需每次运行  Gradle daemon 时都要使用--daemon 选项。

**GRADLE_USER_HOME**：
指定 Gradle 用户 home 目录 (默认是 “USER_HOME/.gradle” )

**JAVA_HOME**：
指定 JDK 安装目录