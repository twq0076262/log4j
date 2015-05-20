# 使用文件记录日志

使用 `org.apache.Log4j.FileAppender` 将日志记录到文件。

## FileAppender 配置

FileAppender 拥有如下配置参数：

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">属性</th>
<th>描述</th>
</tr>
<tr>
<td>immediateFlush</td>
<td>该标志位默认为 true，意味着每次日志追加操作都将输出流刷新至文件。</td>
</tr>
<tr>
<td>encoding</td>
<td>可以使用任何编码，默认情况下使用平台相关的编码。</td>
</tr>
<tr>
<td>threshold </td>
<td>appender 对象的阀值。</td>
</tr>
<tr>
<td>Filename</td>
<td>日志文件名。</td>
</tr>
<tr>
<td>fileAppend</td>
<td>该值默认为 true，其含义是让日志追加至文件末尾。</td>
</tr>
<tr>
<td>bufferedIO</td>
<td>该标志位表示是否打开缓冲区写，缺省为 false。</td>
</tr>
<tr>
<td>bufferSize</td>
<td>如果开启缓冲区 I/O，该属性指示缓冲区大小，缺省为 8 kb。</td>
</tr>
</tbody></table>

下面是一个使用 FileAppender 的示例配置文件 **Log4j.properties**：

```
# Define the root logger with appender file
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.FileAppender

# Set the name of the file
Log4j.appender.FILE.File=${log}/log.out

# Set the immediate flush to true (default)
Log4j.appender.FILE.ImmediateFlush=true

# Set the threshold to debug mode
Log4j.appender.FILE.Threshold=debug

# Set the append to false, overwrite
Log4j.appender.FILE.Append=false

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.PatternLayout
Log4j.appender.FILE.layout.conversionPattern=%m%n
```

如果您需要一个和 **Log4j.properties** 文件功能类似的 XML 配置文件，如下所示：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE Log4j:configuration SYSTEM "Log4j.dtd">
<Log4j:configuration>

<appender name="FILE" class="org.apache.Log4j.FileAppender">

   <param name="file" value="${log}/log.out"/>
   <param name="immediateFlush" value="true"/>
   <param name="threshold" value="debug"/>
   <param name="append" value="false"/>
   
   <layout class="org.apache.Log4j.PatternLayout">
      <param name="conversionPattern" value="%m%n"/>
   </layout>
</appender>

<logger name="Log4j.rootLogger" additivity="false">
   <level value="DEBUG"/>
   <appender-ref ref="FILE"/>
</logger>

</Log4j:configuration>
```

您可以使用上述配置尝试[示例程序](sample-program.md)一章中的例子。

## 使用多个文件记录日志

您可能因为某些原因，需要将日志写入多个文件，比如当文件大小达到一定阀值时。

为了将日志写入多个文件，您需要使用 `org.apache.Log4j.RollingFileAppender`，该类继承了 `FileAppender` 类，继承了它的所有属性。

除了上述提到的 `FileAppender` 类的属性，该类还包括如下可配置的属性：

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">属性</th>
<th>描述</th>
</tr>
<tr>
<td>maxFileSize</td>
<td>这是文件大小的关键值，大于该值时，文件会回滚。该值默认为 10 MB。</td>
</tr>
<tr>
<td>maxBackupIndex</td>
<td>该值表示备份文件的个数，默认为 1。</td>
</tr>
</tbody></table>

下面是为 `RollingFileAppender` 定义的示例配置文件 **Log4j.properties**：

```
# Define the root logger with appender file
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.RollingFileAppender

# Set the name of the file
Log4j.appender.FILE.File=${log}/log.out

# Set the immediate flush to true (default)
Log4j.appender.FILE.ImmediateFlush=true

# Set the threshold to debug mode
Log4j.appender.FILE.Threshold=debug

# Set the append to false, should not overwrite
Log4j.appender.FILE.Append=true

# Set the maximum file size before rollover
Log4j.appender.FILE.MaxFileSize=5KB

# Set the the backup index
Log4j.appender.FILE.MaxBackupIndex=2

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.PatternLayout
Log4j.appender.FILE.layout.conversionPattern=%m%n
```

如果您想使用 XML 配置文件，可以像上一节一样生成 XML 配置文件，只需添加和 `RollingFileAppender` 相关的配置即可。

该示例配置文件展示了每个日志文件最大为 5 MB，如果超过该最大值，则会生成一个新的日志文件。由于 `maxBackupIndex` 的值为 2，当第二个文件的大小超过最大值时，会擦去第一个日志文件的内容，所有的日志都回滚至第一个日志文件。

您可以使用上述配置尝试[示例程序](sample-program.md)一章中的例子。

## 逐日生成日志文件

您也许需要逐日生成日志文件，以此更加整洁的记录日志信息。

为了逐日生成日志文件，需要使用 `org.apache.Log4j.DailyRollingFileAppender`，，该类继承了 `FileAppender` 类，继承了它的所有属性。

除了上述提到的 `FileAppender` 类的属性，该类多包含了一个重要属性：

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">属性</th>
<th>描述</th>
</tr>
<tr>
<td>DatePattern</td>
<td>该属性表明什么时间回滚文件，以及文件的命名约定。缺省情况下，在每天午夜回滚文件。</td>
</tr>
</tbody></table>

`DatePattern` 使用如下规则控制回滚计划：

<table class="table table-bordered">
<tbody><tr>
<th style="width:35%">DatePattern</th>
<th>描述</th>
</tr>
<tr>
<td>'.' yyyy-MM</td>
<td>在本月末，下月初回滚文件。</td>
</tr>
<tr>
<td>'.' yyyy-MM-dd</td>
<td>在每天午夜回滚文件，这是缺省值。</td>
</tr>
<tr>
<td>'.' yyyy-MM-dd-a</td>
<td>在每天中午和午夜回滚文件。</td>
</tr>
<tr>
<td>'.' yyyy-MM-dd-HH</td>
<td>在每个整点回滚文件。</td>
</tr>
<tr>
<td>'.' yyyy-MM-dd-HH-mm</td>
<td>每分钟回滚文件。</td>
</tr>
<tr>
<td>'.' yyyy-ww</td>
<td>根据地域，在每周的第一天回滚。</td>
</tr>
</tbody></table>

下述 **Log4j.properties** 示例文件产生的日志文件在每天中午和午夜回滚：

```
# Define the root logger with appender file
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.DailyRollingFileAppender

# Set the name of the file
Log4j.appender.FILE.File=${log}/log.out

# Set the immediate flush to true (default)
Log4j.appender.FILE.ImmediateFlush=true

# Set the threshold to debug mode
Log4j.appender.FILE.Threshold=debug

# Set the append to false, should not overwrite
Log4j.appender.FILE.Append=true

# Set the DatePattern
Log4j.appender.FILE.DatePattern='.' yyyy-MM-dd-a

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.PatternLayout
Log4j.appender.FILE.layout.conversionPattern=%m%n
``` 

如果您想使用 XML 配置文件，可以像上一节一样生成 XML 配置文件，只需添加和 `DailyRollingFileAppender` 相关的配置即可。

您可以使用上述配置尝试[示例程序](sample-program.md)一章中的例子。