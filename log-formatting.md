# 日志格式

Apache log4j 提供了多个 `Layout` 对象，每个根据布局的不同都可格式化日志数据。还可以创建一个 `Layout` 对象，以应用特有的方式格式化日志。

所有 `Layout` 对象从 `Appender` 对象那里接收一个 `LoggingEvent ` 对象，然后从 `LoggingEvent ` 对象那里获取信息，并使用恰当的 `ObjectRenderer` 对象获取该信息的字符串形式。

## Layout 类型

位于继承关系顶层的是抽象类 `org.apache.log4j.Layout`，这是所有 log4j API 中 `Layout` 类的基类。

`Layout` 类是个抽象类，在应用中我们从不直接使用该类，而是使用它的子类，如下所示：

- DateLayout
- [HTMLLayout](log4j-htmllayout.md)
- [PatternLayout](log4j-patternlayout.md)
- SimpleLayout
- XMLLayout

## Layout 方法

该类对于所有其他 `Layout` 对象的通用操作提供了框架性的实现，声明了两个抽象方法：

<table class="table table-bordered">
<tbody><tr>
<th style="width:5%">序号</th>
<th>方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td><b>public abstract boolean ignoresThrowable()</b>
<p>该方法指明日志信息是否处理由日志事件传来的 java.lang.Throwable 对象，如果 Layout 对象能处理 Throwable 对象，则 Layout 对象不会忽略它，并且返回 false。</p>
</td>
</tr>
<tr>
<td>2</td>
<td>
<b>public abstract String format(LoggingEvent event)</b>
<p>各 Layout 子类实现该方法，以定义各自的格式。</p>
</td>
</tr>
</tbody></table>

除了这些抽象方法，`Layout` 类还提供了如下的具体方法：

<table class="table table-bordered">
<tbody><tr>
<th style="width:5%">序号</th>
<th>方法 &amp; 描述</th>
</tr>
<tr>
<td>1</td>
<td>
<b>public String getContentType()</b>
<p>返回 Layout 对象使用的内容类型。基类返回 text/plain 作为默认内容类型。</p></td>
</tr>
<tr>
<td>2</td>
<td>
<b>public String getFooter()</b>
<p>返回日志信息的脚注。</p></td>
</tr>
<tr>
<td>3</td>
<td>
<b>public String getHeader()</b>
<p>返回日志信息的日志头。</p></td>
</tr>
</tbody></table>

每个子类均可覆盖这些方法的实现，返回各自特有的信息。