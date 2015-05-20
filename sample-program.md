# 示例程序

我们已经学会了如何创建配置文件，本章讲述如何生成调试信息，并将其写入一个简单的文本文件。

下面是为我们的例子创建的一个简单配置文件，让我们再来复习一遍：

- 定义根日志级别为 **DEBUG**，并将名为 **FILE** 的 appender 添加其上。
- appender **FILE** 定义为 **org.apache.Log4j.FileAppender**，它将日志写入 **log** 目录下一个名为 **log.out** 的文件中。
- layout 被定义为 *%m%n*，即打印出来的日志信息末尾加入换行。

**Log4j.properties** 文件的内容如下：

```
# Define the root logger with appender file
Log4j.rootLogger = DEBUG, FILE

# Define the file appender
Log4j.appender.FILE=org.apache.Log4j.FileAppender
Log4j.appender.FILE.File=${log}/log.out

# Define the layout for file appender
Log4j.appender.FILE.layout=org.apache.Log4j.PatternLayout
Log4j.appender.FILE.layout.conversionPattern=%m%n
```

## 在 Java 程序中使用 Log4j

下面的 Java 类是一个非常简单的例子，它在 Java 应用中初始化，然后使用了 Log4j 类库。

```
import org.apache.Log4j.Logger;

import java.io.*;
import java.sql.SQLException;
import java.util.*;

public class Log4jExample{

   /* Get actual class name to be printed on */
   static Logger log = Logger.getLogger(Log4jExample.class.getName());
   
   public static void main(String[] args)throws IOException,SQLException{
      log.debug("Hello this is a debug message");
      log.info("Hello this is an info message");
   }
}
```

## 编译和运行

下面是编译和运行上述程序的步骤。在编译和运行前，首先确保正确地设置了 **CLASSPATH** 和 **PATH**。

所有的类库都必需包含在 **CLASSPATH** 里，**Log4j.properties** 文件必需包含在 **PATH** 里。按照下面的步骤操作：

- 创建如上所述的 Log4j.properties 文件。
- 创建如上所述的 Log4jExample.java 文件并编译。
- 运行 Log4jExample。

在 `/usr/home/Log4j/log.out` 文件中会生成如下内容：

```
Hello this is a debug message
Hello this is an info message
```