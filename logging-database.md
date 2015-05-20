# 使用数据库记录日志

Log4j API 提供了 `org.apache.Log4j.jdbc.JDBCAppender` 对象，该对象可将日志信息记录到特定的数据库之中。

## JDBCAppender 配置

<table class="table table-bordered">
<tbody><tr>
<th style="width:25%">属性</th>
<th>描述</th>
</tr>
<tr>
<td>bufferSize</td>
<td>设置缓冲区大小，缺省为 1。</td>
</tr>
<tr>
<td>driver</td>
<td>以字符串形式设置驱动类，如果未设置，缺省为 <b>sun.jdbc.odbc.JdbcOdbcDriver</b>。</td>
</tr>
<tr>
<td>layout</td>
<td>设置 layout，缺省为 <b>org.apache.Log4j.PatternLayout</b>。</td>
</tr>
<tr>
<td>password</td>
<td>设置数据库密码。</td>
</tr>
<tr>
<td>sql</td>
<td>设置每次日志事件触发时需要执行的 SQL 语句，该语句可以是 INSERT、UPDATE 或 DELETE。</td>
</tr>
<tr>
<td>URL</td>
<td>设置 JDBC URL.</td>
</tr>
<tr>
<td>user</td>
<td>设置数据库用户名。</td>
</tr>
</tbody></table>

## 日志表的配置

在使用基于 JDBC 的日志之前，先要创建一张表以保存所有日志信息，下面是用来创建 LOGS 表的 SQL 语句：

```
CREATE TABLE LOGS
   (USER_ID VARCHAR(20)    NOT NULL,
    DATED   DATE           NOT NULL,
    LOGGER  VARCHAR(50)    NOT NULL,
    LEVEL   VARCHAR(10)    NOT NULL,
    MESSAGE VARCHAR(1000)  NOT NULL
   );
```

## 示例配置文件

下面是一个为 `JDBCAppender` 编写的 **Log4j.properties** 的示例配置文件，使用该对象将日志信息记录到 LOGS 表中。

```
# Define the root logger with appender file
Log4j.rootLogger = DEBUG, DB

# Define the DB appender
Log4j.appender.DB=org.apache.Log4j.jdbc.JDBCAppender

# Set JDBC URL
Log4j.appender.DB.URL=jdbc:mysql://localhost/DBNAME

# Set Database Driver
Log4j.appender.DB.driver=com.mysql.jdbc.Driver

# Set database user name and password
Log4j.appender.DB.user=user_name
Log4j.appender.DB.password=password

# Set the SQL statement to be executed.
Log4j.appender.DB.sql=INSERT INTO LOGS VALUES('%x','%d','%C','%p','%m')

# Define the layout for file appender
Log4j.appender.DB.layout=org.apache.Log4j.PatternLayout
```

如果使用 MySQL 数据库，需要使用真实的 DBNAME、用户名和密码，就是刚才用来创建 LOGS 表的那些属性。SQL 语句执行 INSERT 语句，为 LOGS 表插入具体数值。

`JDBCAppender` 不需要显示定义 layout，传入的 SQL 语句会使用 `PatternLayout`。

如果您需要和上述 **Log4j.properties** 文件等价的 XML 配置文件，如下所示：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE Log4j:configuration SYSTEM "Log4j.dtd">
<Log4j:configuration>

<appender name="DB" class="org.apache.Log4j.jdbc.JDBCAppender">
   <param name="url" value="jdbc:mysql://localhost/DBNAME"/>
   <param name="driver" value="com.mysql.jdbc.Driver"/>
   <param name="user" value="user_id"/>
   <param name="password" value="password"/>
   <param name="sql" value="INSERT INTO LOGS VALUES('%x','%d','%C','%p','%m')"/>
   
   <layout class="org.apache.Log4j.PatternLayout">
   </layout>
</appender>

<logger name="Log4j.rootLogger" additivity="false">
   <level value="DEBUG"/>
   <appender-ref ref="DB"/>
</logger>

</Log4j:configuration>
```

## 示例程序

下述 Java 类是一个非常简单的例子，该类在 Java 应用中初始化并使用了 Log4j 类库。

```
import org.apache.Log4j.Logger;
import java.sql.*;
import java.io.*;
import java.util.*;

public class Log4jExample{
   /* Get actual class name to be printed on */
   static Logger log = Logger.getLogger(Log4jExample.class.getName());
   
   public static void main(String[] args)throws IOException,SQLException{
      log.debug("Debug");
      log.info("Info");
   }
}
```

编译和运行上述程序的步骤如下。在继续编译和运行程序之前，确保正确设置了 **PATH** 和 **CLASSPATH**。

所有的类库都需要包含在 **CLASSPATH** 中，*Log4j.properties* 文件需要包含在 **PATH** 中，步骤如下：

- 创建如上所示的 Log4j.properties 文件。
- 创建如上所示的 Log4jExample.java 文件并编译。
- 运行 Log4jExample。

现在检查 DBNAME 数据库中的 LOGS 表，会发现如下条目：

```
mysql >  select * from LOGS;
+---------+------------+--------------+-------+---------+
| USER_ID | DATED      | LOGGER       | LEVEL | MESSAGE |
+---------+------------+--------------+-------+---------+
|         | 2010-05-13 | Log4jExample | DEBUG | Debug   |
|         | 2010-05-13 | Log4jExample | INFO  | Info    |
+---------+------------+--------------+-------+---------+
2 rows in set (0.00 sec)
```
**注意**——这里 x 用来输出和生成日志事件线程相关联的嵌套诊断上下文（NDC），我们使用 NDC 在处理多个客户端的服务器端来区分客户端，具体请查阅 Log4j 手册。