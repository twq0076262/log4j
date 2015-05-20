# PatternLayout

如果您希望基于某种模式生成特定格式的日志信息，可使用 `org.apache.Log4j.PatternLayout` 格式化您的日志信息。

`PatternLayout` 继承自抽象类 `org.apache.Log4j.Layout`，覆盖了其 `format()` 方法，通过提供的模式，来格式化日志信息。

`PatternLayout` 是一个简单的 `Layout` 对象，提供了如下属性，该属性可通过配置文件更改：

<table class="table table-bordered">
<tbody><tr>
<th style="width:5%">序号</th>
<th>属性 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td>
<b>conversionPattern</b>
<p>设置转换模式，默认为 %r [%t] %p %c %x - %m%n。</p></td>
</tr>
</tbody></table>

## 模式转换字符

下面的表格解释了上面模式中用到的字符，以及所有定制模式时能用到的字符：

<table class="table table-bordered">
<tbody><tr>
<th style="width:10%">转换字符</th>
<th>含义</th>
</tr>
<tr>
<td>c</td>
<td>使用它为输出的日志事件分类，比如对于分类 "a.b.c"，模式 %c{2} 会输出 "b.c" 。</td>
</tr>
<tr>
<td>C</td>
<td>使用它输出发起记录日志请求的类的全名。比如对于类 "org.apache.xyz.SomeClass"，模式 %C{1} 会输出 "SomeClass"。</td>
</tr>
<tr>
<td>d</td>
<td>使用它输出记录日志的日期，比如 %d{HH:mm:ss,SSS} 或 %d{dd MMM yyyy HH:mm:ss,SSS}。</td>
</tr>
<tr>
<td>F</td>
<td>在记录日志时，使用它输出文件名。</td>
</tr>
<tr>
<td>l</td>
<td>用它输出生成日志的调用者的地域信息。</td>
</tr>
<tr>
<td>L</td>
<td>使用它输出发起日志请求的行号。</td>
</tr>
<tr>
<td>m</td>
<td>使用它输出和日志事件关联的，由应用提供的信息。</td>
</tr>
<tr>
<td>M</td>
<td>使用它输出发起日志请求的方法名。</td></tr>
<tr>
<td>n</td>
<td>输出平台相关的换行符。</td>
</tr>
<tr>
<td>p</td>
<td>输出日志事件的优先级。</td>
</tr>
<tr>
<td>r</td>
<td>使用它输出从构建布局到生成日志事件所花费的时间，以毫秒为单位。</td>
</tr>
<tr>
<td>t</td>
<td>输出生成日志事件的线程名。</td>
</tr>
<tr>
<td>x</td>
<td>输出和生成日志事件线程相关的 NDC (嵌套诊断上下文)。</td>
</tr>
<tr>
<td>X</td>
<td>该字符后跟 MDC 键，比如 X{clientIP} 会输出保存在 MDC 中键 clientIP 对应的值。</td>
</tr>
<tr>
<td>%</td>
<td>百分号， %% 会输出一个 %。</td>
</tr>
</tbody></table>

## 格式修饰符

缺省情况下，信息保持原样输出。但是借助格式修饰符的帮助，就可调整最小列宽、最大列宽以及对齐。

下面的表格涵盖了各种修饰符：

<table class="table table-bordered" cellpadding="5" cellspacing="5" border="1">
<tbody><tr>
<th>格式修饰符</th>
<th>左对齐</th>
<th>最小宽度</th>
<th>最大宽度</th>
<th>注释</th>
</tr>
<tr>
<td align="center">%20c</td>
<td align="center">否</td>
<td align="center">20</td>
<td align="center">无</td>
<td>如果列名少于 20 个字符，左边使用空格补齐。</td>
</tr>
<tr>
<td align="center">%-20c</td> 
<td align="center">是</td>
<td align="center">20</td>
<td align="center">无</td>
<td>如果列名少于 20 个字符，右边使用空格补齐。</td>
</tr>
<tr>
<td align="center">%.30c</td>
<td align="center">不适用</td>
<td align="center">无</td>
<td align="center">30</td>
<td>如果列名长于 30 个字符，从开头剪除。</td>
</tr>
<tr>
<td align="center">%20.30c</td>
<td align="center">否</td>
<td align="center">20</td>
<td align="center">30</td>
<td>如果列名少于 20 个字符，左边使用空格补齐，如果列名长于 30 个字符，从开头剪除。</td>
</tr>
<tr>
<td align="center">%-20.30c</td>
<td align="center">是</td>
<td align="center">20</td>
<td align="center">30</td>
<td>如果列名少于 20 个字符，右边使用空格补齐，如果列名长于 30 个字符，从开头剪除。</td>
</tr>
</tbody></table>

## PatternLayout 示例

下面是为 `PatternLayout` 编写的一个简单配置：

```
# Define the root logger with appender file
log = /usr/home/Log4j
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.FileAppender
Log4j.appender.FILE.File=${log}/log.out

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.PatternLayout
Log4j.appender.FILE.layout.ConversionPattern=%d{yyyy-MM-dd}-%t-%x-%-5p-%-10c:%m%n
```

下面是生成日志信息的 Java 程序：

```
import org.apache.Log4j.Logger;

import java.io.*;
import java.sql.SQLException;
import java.util.*;

public class Log4jExample{
   /* Get actual class name to be printed on */
   static Logger log = Logger.getLogger(Log4jExample.class.getName());
   
   public static void main(String[] args)throws IOException,SQLException{
      log.debug("Hello this is an debug message");
      log.info("Hello this is an info message");
   }
}
```

编译并运行上述程序，会在目录 `/usr/home/Log4j` 下生成一个名为 `log.out` 的文件，该文件包含如下日志信息：

```
2010-03-23-main--DEBUG-Log4jExample:Hello this is an debug message
2010-03-23-main--INFO -Log4jExample:Hello this is an info message
```