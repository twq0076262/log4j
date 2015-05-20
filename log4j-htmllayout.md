# HTMLLayout

如果您希望以 HTML 格式输出日志文件，可使用 `org.apache.Log4j.HTMLLayout` 格式化日志信息。

`HTMLLayout` 继承自抽象类 `org.apache.Log4j.Layout`，覆盖了其 `format()` 方法来提供 HTML 格式。

它提供了如下信息以供显示：

- 从应用启动到日志产生之间经过的时间。
- 发起记录日志请求的线程名字。
- 和记录日志请求关联的级别。
- logger 的名字和日志信息。
- 程序文件的位置信息和触发日志的行号，该信息是可选的。

`HTMLLayout` 是一种非常简单的 `Layout` 对象，它提供了如下方法：

<table class="table table-bordered">
<tbody><tr>
<th style="width:5%">序号</th>
<th>方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td>
<b>setContentType(String)</b>
<p>设置 HTML 的内容类型，默认为 text/html。</p>
</td>
</tr>
<tr>
<td>2</td>
<td>
<b>setLocationInfo(String)</b>
<p>设置日志事件的地域信息。</p>
</td>
</tr>
<tr>
<td>3</td>
<td>
<b>setTitle(String)</b>
<p>设置 HTML 文件的标题，默认为 Log4j Log Messages。</p>
</td>
</tr>
</tbody></table>

## HTMLLayout 示例

下面是为 `HTMLLayout` 编写的一个简单配置文件：

```
# Define the root logger with appender file
log = /usr/home/Log4j
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.FileAppender
Log4j.appender.FILE.File=${log}/htmlLayout.html

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.HTMLLayout
Log4j.appender.FILE.layout.Title=HTML Layout Example
Log4j.appender.FILE.layout.LocationInfo=true
```
下面的 Java 程序会生成日志信息：

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

编译和运行上述程序，在目录 /usr/home/Log4j 下，会生成一个名为 htmlLayout.html 的文件，该文件包含如下日志信息：

Log session start time Mon Mar 22 13:30:24 AST 2010

<table cellspacing="0" cellpadding="4" border="1" bordercolor="#224466" width="100%" style="font-family: arial,sans-serif; font-size: x-small;">
<tbody><tr>
<th style="background: #336699; color: #FFFFFF; text-align: left;">Time</th>
<th style="background: #336699; color: #FFFFFF; text-align: left;">Thread</th>
<th style="background: #336699; color: #FFFFFF; text-align: left;">Level</th>
<th style="background: #336699; color: #FFFFFF; text-align: left;">Category</th>
<th style="background: #336699; color: #FFFFFF; text-align: left;">File:Line</th>
<th style="background: #336699; color: #FFFFFF; text-align: left;">Message</th>
</tr>
<tr>
<td>0</td>
<td title="main thread">main</td>
<td title="Level"><font color="#339933">DEBUG</font></td>
<td title="Log4jExample category">Log4jExample</td>
<td>Log4jExample.java:15</td>
<td title="Message">Hello this is an debug message</td>
</tr>
<tr>
<td>6</td>
<td title="main thread">main</td>
<td title="Level">INFO</td>
<td title="Log4jExample category">Log4jExample</td>
<td>Log4jExample.java:16</td>
<td title="Message">Hello this is an info message</td>
</tr>
</tbody></table>

您可以使用浏览器打开 htmlLayout.html 文件。需要注意的是末尾缺失了 `</html>` 和 `</body>` 标签。

将日志格式化为 HTML 的一个优势在于可将其发布成一个 Web 页面，便于远程浏览。