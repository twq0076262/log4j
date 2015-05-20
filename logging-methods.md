# Logging 方法

Logger 类提供了很多方法用来处理日志，Logger 类不允许初始化一个新的实例，但提供了两个静态方法用来获取 Logger 对象：

- `public static Logger getRootLogger();`
- `public static Logger getLogger(String name);`

第一个方法返回应用实例没有名字的根日志。

其他有名字的 Logger 对象通过传入日志的名字，调用第二个方法获得。日志的名字是传入的任何字符串，通常为类名或包名，如上一章和下面的例子所示：

```
static Logger log = Logger.getLogger(Log4jExample.class.getName());
```

## Logging 方法

一旦获取一个有名字的 logger 实例，就可以使用多个方法记录日志。Logger 类拥有如下方法用于打印日志信息。

<table class="table table-bordered">
<tbody><tr>
<th style="width:5%">#</th>
<th>方法和描述</th>
</tr>
<tr>
<td>1</td>
<td><b>public void debug(Object message)</b>
<p>使用 Level.DEBUG 级别打印日志。</p>
</td>
</tr>
<tr>
<td>2</td>
<td><b>public void error(Object message)</b>
<p>使用 Level.ERROR 级别打印日志。</p>
</td>
</tr>
<tr>
<td>3</td>
<td><b>public void fatal(Object message)</b>
<p>使用 Level.FATAL 级别打印日志。</p>
</td>
</tr>
<tr>
<td>4</td>
<td><b>public void info(Object message)</b>
<p>使用 Level.INFO 级别打印日志。</p>
</td>
</tr>
<tr>
<td>5</td>
<td><b>public void warn(Object message)</b>
<p>使用 Level.WARN 级别打印日志。</p>
</td>
</tr>
<tr>
<td>6</td>
<td><b>public void trace(Object message)</b>
<p>使用 Level.TRACE 级别打印日志。</p>
</td>
</tr>
</tbody></table>

所有级别均在 `org.apache.Log4j.Level` 类中定义，这些方法使用如下方式调用：

```
import org.apache.Log4j.Logger;

public class LogClass {
   private static org.apache.Log4j.Logger log = Logger.getLogger(LogClass.class);
   
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

编译并运行 `LogClass `，输出如下：

```
Debug Message!
Info Message!
Warn Message!
Error Message!
Fatal Message!
```

调试信息和级别联合使用才更有意义。我们将在下一章讲解日志级别，您会对如何联合使用这些方法和不同调试级别有一个更好的理解。