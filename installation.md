# 安装

Log4j API 使用 Apache Software License 授权，该授权是经开源促进协会认证的、完整的开源协议。

最新版本的 Log4j，连同其代码、类文件和文档可通过 [http://logging.apache.org/log4j/](http://logging.apache.org/log4j/) 获取。

从上述地址下载 apache-Log4j-x.x.x.tar.gz 文件，按照下面的步骤安装 Log4j。

## 第一步

将所下载文件解压至 `/usr/local/` 目录：

```
$ gunzip apache-log4j-1.2.15.tar.gz
$ tar -xvf apache-log4j-1.2.15.tar
apache-log4j-1.2.15/tests/input/
apache-log4j-1.2.15/tests/input/xml/
apache-log4j-1.2.15/tests/src/
apache-log4j-1.2.15/tests/src/java/
apache-log4j-1.2.15/tests/src/java/org/
.......................................
```

解压时，会生成一个名为 `apache-log4j-x.x.x` 的文件夹，目录结构如下所示：

```
-rw-r--r--  1 root root   3565 2007-08-25 00:09 BUILD-INFO.txt
-rw-r--r--  1 root root   2607 2007-08-25 00:09 build.properties.sample
-rw-r--r--  1 root root  32619 2007-08-25 00:09 build.xml
drwxr-xr-x 14 root root   4096 2010-02-04 14:09 contribs
drwxr-xr-x  5 root root   4096 2010-02-04 14:09 examples
-rw-r--r--  1 root root   2752 2007-08-25 00:09 INSTALL
-rw-r--r--  1 root root   4787 2007-08-25 00:09 KEYS
-rw-r--r--  1 root root  11366 2007-08-25 00:09 LICENSE
-rw-r--r--  1 root root 391834 2007-08-25 00:29 log4j-1.2.15.jar
-rw-r--r--  1 root root    160 2007-08-25 00:09 NOTICE
-rwxr-xr-x  1 root root  10240 2007-08-25 00:27 NTEventLogAppender.dll
-rw-r--r--  1 root root  17780 2007-08-25 00:09 pom.xml
drwxr-xr-x  7 root root   4096 2007-08-25 00:13 site
drwxr-xr-x  8 root root   4096 2010-02-04 14:08 src
drwxr-xr-x  6 root root   4096 2010-02-04 14:09 tests
```

## 第二步

该步根据要使用 log4j 的功能是可选的。如果您已经在机器上安装了如下软件包，就不用担心了，否则您需要安装它们以使 Log4j 工作正常。

- **JavaMail API**：Log4j 中基于 e-mail 的功能需要 Java Mail API（mail.jar），请从 [glassfish.dev](https://glassfish.dev.java.net/javaee5/mail/) 下载安装。
- **JavaBeans Activation Framework**：Java Mail API 还需要安装 JavaBeans Activation Framework（activation.jar），请从 [http://java.sun.com/products/javabeans/jaf/index.jsp](http://java.sun.com/products/javabeans/jaf/index.jsp) 下载安装。
- **Java Message Service**：Log4j 中和 JMS 匹配的功能需要安装 JMS 和 Java Naming and Directory Interface JNDI，请从 [http://java.sun.com/products/jms](http://java.sun.com/products/jms) 下载安装。
- **XML Parser**：使用 Log4j，需要一个和JAXP兼容的 XML 解析器，请确保从 [http://xerces.apache.org/xerces-j/install.html](http://xerces.apache.org/xerces-j/install.html) 下载安装了 Xerces.jar。

## 第三步

现在需要正确地设置 **CLASSPATH** 和 **PATH**。这里我们只如何设置 Log4j.x.x.x.jar。

```
$ pwd
/usr/local/apache-log4j-1.2.15
$ export CLASSPATH=$CLASSPATH:/usr/local/apache-log4j-1.2.15/log4j-1.2.15.jar
$ export PATH=$PATH:/usr/local/apache-log4j-1.2.15/
```
