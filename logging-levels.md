# Logging 级别

`org.apache.log4j.Level` 类定义了日志级别，您可通过继承 `Level` 类定制自己的级别。

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">级别</th>
<th>描述</th>
</tr>
<tr>
<td>ALL</td>
<td>所有级别，包括定制级别。</td>
</tr>
<tr>
<td>DEBUG</td>
<td>指明细致的事件信息，对调试应用最有用。</td>
</tr>
<tr>
<td>ERROR</td>
<td>指明错误事件，但应用可能还能继续运行。</td>
</tr>
<tr>
<td>FATAL</td>
<td>指明非常严重的错误事件，可能会导致应用终止执行。</td>
</tr>
<tr>
<td>INFO</td>
<td>指明描述信息，从粗粒度上描述了应用运行过程。</td>
</tr>
<tr>
<td>OFF</td>
<td>最高级别，用于关闭日志。</td>
</tr>
<tr>
<td>TRACE</td>
<td>比 DEBUG 级别的粒度更细。</td>
</tr>
<tr>
<td>WARN</td>
<td>指明潜在的有害状况。</td>
</tr>
</tbody></table>

## 级别是如何工作的？

在一个级别为 `q` 的 logger 对象中，一个级别为 `p` 的日志请求在 p >= q 的情况下是**开启**的。该规则是 log4j 的核心，它假设级别是有序的。对于标准级别，其顺序为：ALL < DEBUG < INFO < WARN < ERROR < FATAL < OFF。

下面的例子展示了如何过滤 DEBUG 和 INFO 级别的日志。改程序使用 logger 对象的 `setLevel(Level.X)` 方法设置期望的日志级别：

该例子会打印出除过 DEBUG 和 INFO 级别外的所有信息：

```
import org.apache.log4j.*;

public class LogClass {
   private static org.apache.log4j.Logger log = Logger.getLogger(LogClass.class);
   
   public static void main(String[] args) {
      log.setLevel(Level.WARN);

      log.trace("Trace Message!");
      log.debug("Debug Message!");
      log.info("Info Message!");
      log.warn("Warn Message!");
      log.error("Error Message!");
      log.fatal("Fatal Message!");
   }
}
```

编译并运行 `LogClass`，会产生如下输出：

```
Warn Message!
Error Message!
Fatal Message!
```

## 使用配置文件设置日志级别

log4j 提供了基于配置文件设置日志级别的功能，当您需要改变调试级别时，不用再去修改代码了。

下面这个例子和上面那个例子功能一样，不过不用使用 `setLevel(Level.WARN)` 方法，只需修改配置文件：

```
# Define the root logger with appender file
log = /usr/home/log4j
log4j.rootLogger = WARN, FILE

# Define the file appender
log4j.appender.FILE=org.apache.log4j.FileAppender
log4j.appender.FILE.File=${log}/log.out

# Define the layout for file appender
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.conversionPattern=%m%n
```

现在使用如下程序：

```
import org.apache.log4j.*;

public class LogClass {

   private static org.apache.log4j.Logger log = Logger.getLogger(LogClass.class);
   
   public static void main(String[] args) {
   
      log.trace("Trace Message!");
      log.debug("Debug Message!");
      log.info("Info Message!");
      log.warn("Warn Message!");
      log.error("Error Message!");
      log.fatal("Fatal Message!");
   }
}
```

编译并运行如下程序，会在 `/usr/home/log4j/log.out` 文件内生成如下内容：

```
Warn Message!
Error Message!
Fatal Message!
```