# 配置

上一章解释了 log4j 的核心组件。本章讲述如何使用配置文件来配置核心组件。log4j 的配置包括在配置文件中指定 Level、定义 Appender 和指明 Layout。

**log4j.properties** 文件是 log4j 的配置文件，属性以键值对的形式定义。默认情况下，LogManager 会在 **CLASSPATH** 中寻找 **log4j.properties** 文件。

- 根日志的级别定义为 **DEBUG**，并将名为 X 的 appender 添加其上。
- 将名为 X 的 appender 设置为合法的 appender。
- 设置 appender X 的 layout。

## log4j.properties 的语法

为 appender X 定义的 *log4j.properties* 的语法如下：

```
# Define the root logger with appender X
log4j.rootLogger = DEBUG, X

# Set the appender named X to be a File appender
log4j.appender.X=org.apache.log4j.FileAppender

# Define the layout for X appender
log4j.appender.X.layout=org.apache.log4j.PatternLayout
log4j.appender.X.layout.conversionPattern=%m%n
```

## log4j.properties 示例

使用上述语法，我们在 **log4j.properties** 定义如下属性：

- 定义根日志级别为 **DEBUG**，并将名为 **FILE** 的 appender 添加其上。
- appender **FILE** 定义为 **org.apache.log4j.FileAppender**，它将日志写入 **log** 目录下一个名为 **log.out** 的文件中。
- layout 被定义为 *%m%n*，即打印出来的日志信息末尾加入换行。

```
# Define the root logger with appender file
log4j.rootLogger = DEBUG, FILE

# Define the file appender
log4j.appender.FILE=org.apache.log4j.FileAppender
log4j.appender.FILE.File=${log}/log.out

# Define the layout for file appender
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.conversionPattern=%m%n
```

值得注意的是，log4j 支持 Unix 风格的变量替换，比如 ${variableName}。

## DEBUG 级别

两个 appender 都使用了 DEBUG 级别，所有可能的选项如下：

- TRACE
- DEBUG
- INFO
- WARN
- ERROR
- FATAL
- ALL

这些级别会在本教程的后续部分讲解。

## Appender

Apache log4j 提供的 Appender 对象主要负责将日志信息打印至不同目的地，比如控制台、文件、网络套接字、NT 事件日志等。

每个 Appender 对象都有不同的属性，这些属性决定了该对象的行为。

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">属性</th>
<th>描述</th>
</tr>
<tr>
<td>layout</td>
<td>Appender 使用 Layout 对象和与之关联的模式来格式化日志信息。</td>
</tr>
<tr>
<td>target</td>
<td>目的地可以是控制台、文件，或依赖于 appender 的对象。</td>
</tr>
<tr>
<td>level</td>
<td>级别用来控制过滤日志信息。</td>
</tr>
<tr>
<td>threshold</td>
<td>Appender 可脱离于日志级别定义一个阀值级别，Appender 对象会忽略所有级别低于阀值级别的日志。</td>
</tr>
<tr>
<td>filter</td>
<td>Filter 对象可在级别基础之上分析日志信息，来决定 Appender 对象是否处理或忽略一条日志记录。</td>
</tr>
</tbody></table>

通过在配置文件中使用如下方法，将 Appender 对象添加至 Logger 对象：

```
log4j.logger.[logger-name]=level, appender1,appender..n
```

也可以在 XML 中定义同样的配置：

```
<logger name="com.apress.logging.log4j" additivity="false">
   <appender-ref ref="appender1"/>
   <appender-ref ref="appender2"/>
</logger>
```

如果想在程序中添加 Appender 对象，可使用如下方法：

```
public void addAppender(Appender appender);
```
`addAppender()` 方法为 Logger 对象增加一个 Appender。如示例配置展示的那样，可以通过逗号分隔的列表，为 logger 添加多个 Appender，将日志信息打印到不同的目的地。

在上面的例子中，我们只用到了 *FileAppender*，所有可用的 appender 包括：

- AppenderSkeleton
- AsyncAppender
- ConsoleAppender
- DailyRollingFileAppender
- ExternallyRolledFileAppender
- FileAppender
- JDBCAppender
- JMSAppender
- LF5Appender
- NTEventLogAppender
- NullAppender
- RollingFileAppender
- SMTPAppender
- SocketAppender
- SocketHubAppender
- SyslogAppender
- TelnetAppender
- WriterAppender 

我们将在[使用文件记录日志](logging-files.md)一章讲解 FileAppender，在[使用数据库记录日志](logging-database.md)一章讲解 JDBC Appender。

## Layout

我们已经在 appender 中使用了 PatternLayout，所有选项还包括：

- DateLayout
- HTMLLayout
- PatternLayout
- SimpleLayout
- XMLLayout

使用 HTMLLayout 和 XMLLayout，可以生成 HTML 和 XML 格式的日志。

## 日志格式化

您将在[日志格式](log-formatting.md)一章学习如何格式化日志信息。 