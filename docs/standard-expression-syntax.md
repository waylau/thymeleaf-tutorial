# 标准表达式语法

Thymeleaf 标准表达式（ Standard Expression）语法，是 Thymeleaf 标准方言（Standard Dialect）的最重要组成部分之一。

## 标准表达式总结

Thymeleaf 提供了多种标准表达式包括：

* 简单表达式：
	* Variable expressions（变量表达式）`${...}` 
	* Selection expressions（选择表达式）`*{...}`
	* Message (i18n) expressions（消息表达式） `#{...}` 
	* Link (URL) expressions（链接表达式）`@{...}`
	* Fragment expressions（分段表达式）`~{...}`
* 字面量:
	* 文本：`'one text'`、`'Another one!'`等；
	* 数值：0、34、3.0、12.3等；
	* 布尔：true、false
	* Null：null
	* Literal token(字面标记): one、sometext、 main等；
* 文本操作:
	* 字符串拼接：`+`
	* 文本替换：`|The name is ${name}|`
* 算术操作:
	* 二元运算符：`+`、`-`、 `*`、`/`、`%`
	* 减号（单目运算符）：`-`
* 布尔操作：
	* 二元运算符：`and`、`or`
	* 布尔否定（一元运算符）：`!`、`not`
* 比较和等价：
	* 比较：`>`、`<`、`>=`、`<=`（`gt`、`lt`、`ge`、`le`）
	* 等价:`==`、`!=`（`eq`、`ne`）
* 条件运算符：
	* If-then：`(if) ? (then)`
	* If-then-else：`(if) ? (then) : (else)`
	* Default：`(value) ?: (defaultvalue)`
* 特殊标记：
	* No-Operation（无操作）：`_`
	
	
下面的这个示例，涵盖了上述大部分表达式：

```
'User is of type ' + (${user.isAdmin()} ? 'Administrator' : (${user.type} ?: 'Unknown'))
```
	
## Message (i18n) expressions（消息表达式）

消息表达式（通常称为文本外化、国际化或i18n）允许我们从外部源（`.properties`文件）检索特定于语言环境的消息，通过 key 引用它们（可选）应用一组参数。

在Spring应用程序中，这将自动与Spring的MessageSource机制集成。

```
#{main.title}

#{message.entrycreated(${entryId})}
```

在模版中的应用如下：

```
<table>
  ...
  <th th:text="#{header.address.city}">...</th>
  <th th:text="#{header.address.country}">...</th>
  ...
</table>
```

请注意，如果希望消息 key 由上下文变量的值确定，或者要将变量指定为参数，则可以在消息表达式中使用变量表达式：

```
#{${config.adminWelcomeKey}(${session.user.name})}
```

## Variable expressions（变量表达式）

变量表达式可以是OGNL表达式或者是 Spring EL，如果集成了Spring的话，可以在上下文变量（context variables ）中执行。


> 有关OGNL语法和功能的详细信息，请阅读[OGNL语言指南](http://commons.apache.org/proper/commons-ognl/)
> 在 Spring MVC 启用的应用程序中，OGNL将被替换为**SpringEL**，但其语法与OGNL非常相似（实际上，在大多数常见情况下完全相同）。

在Spring术语中，变量表达式也称为模型属性（model attributes）。 他们看起来像这样：

```
${session.user.name}
```

他们作为属性值或作为属性的一部分：

```
<span th:text="${book.author.name}">
```

上面的表达式在在OGNL和SpringEL中等价于：

```
((Book)context.getVariable("book")).getAuthor().getName()
```

这些变量表达式不仅涉及输出，还包括更复杂的处理，如条件判断、迭代等：


```
<li th:each="book : ${books}">
```

这里`${books}`从上下文中选择名为`books`的变量，并将其评估为可在`th:each`循环中使用的迭代器（iterable）。

更多 OGNL 的功能有：

```java
/*
 * 使用点(.)来访问属性，等价于调用属性的  getter 
 */
${person.father.name}

/*
 * 访问属性也可以使用([])块
 */
${person['father']['name']}

/*
 * 如果对象是一个map，则点和块语法等价于调用其get(...)方法
 */
${countriesByCode.ES}
${personsByName['Stephen Zucchini'].age}

/*
 * 在块语法中，也可以通过索引来访问数组或者集合
 */
${personsArray[0].name}

/*
 * 可以调用方法，同时也支持参数
 */
${person.createCompleteName()}
${person.createCompleteNameWithSeparator('-')}
```

### 表达式基本对象（Expression Basic Objects）

当对上下文变量评估 OGNL 表达式时，某些对象可用于表达式以获得更高的灵活性。 这些对象将被引用（按照OGNL标准），从`＃`符号开始：

* `#ctx`：上下文对象。
* `#vars`：上下文变量。
* `#locale`：上下文区域设置。
* `#request`:HttpServletRequest 对象(仅在 Web 上下文中）。
* `#response`：HttpServletResponse 对象(仅在 Web 上下文中）。
* `#session`:HttpSession对象(仅在 Web 上下文中）。
* `#servletContext`:ServletContext对象(仅在 Web 上下文中）。

所以我们可以这样做：

```html
Established locale country: <span th:text="${#locale.country}">US</span>.
```

完整内容可以参考后文“[表达式基本对象](expression-basic-objects.md)”部分。


### 表达式工具对象（Expression Utility Objects）

除了上面这些基本的对象之外，Thymeleaf 将为我们提供一组工具对象，这些对象将帮助我们在表达式中执行常见任务：

 * `#execInfo`: 模版执行的信息
 * `#messages`: 在变量内获取外部消息的方法 表达式，与使用`＃{...}`语法获得的方式相同。.
 * `#uris`: 用于转义 URL/URI 部分的方法
 * `#conversions`: 执行已配置的 *conversion service*
 * `#dates`: `java.util.Date`对象的方法，比如格式化，组件提取等
 * `#calendars`:类似于`＃dates`，但是对应于`java.util.Calendar`对象
 * `#numbers`: 格式化数字对象的方法。
 * `#strings`: `String`对象的方法，包括 contains、startsWith、prepending/appending等
    等等
 * `#objects`: 对象通常的方法
 * `#bools`: 布尔判断的方法
 * `#arrays`: array 方法
 * `#lists`: list 方法
 * `#sets`: set 方法
 * `#maps`: map 方法
 * `#aggregates`:在数组或集合上创建聚合的方法
 * `#ids`: 用于处理可能重复的id属性的方法（例如，作为迭代的结果）。

下面是一个格式化日期的例子：

```html
<p>
  Today is: <span th:text="${#calendars.format(today,'dd MMMM yyyy')}">13 May 2011</span>
</p>
```

完整内容可以参考后文“[表达式工具对象](expression-utility-objects.md)”部分。


## Selection expressions（选择表达式）

选择表达式与变量表达式很像，区别在于它们是在当前选择的对象而不是整个上下文变量映射上执行。 他们看起来像这样：


```
*{customer.name}
```

它们所作用的对象由`th:object`属性指定：

```
<div th:object="${book}">
  ...
  <span th:text="*{title}">...</span>
  ...
</div>
```

这等价于：

```
{
  // th:object="${book}"
  final Book selection = (Book) context.getVariable("book");
  // th:text="*{title}"
  output(selection.getTitle());
}
```

## Link (URL) expressions（链接表达式）


链接表达式旨在构建URL并向其添加有用的上下文和会话信息（通常称为URL重写的过程）。

因此，对于部署在Web服务器的`/myapp`上下文中的Web应用程序，可以使用以下表达式：

```
<a th:href="@{/order/list}">...</a>
```

可以转成：

```
<a href="/myapp/order/list">...</a>
```

cookie没有启用下，如果我们需要保持会话，可以这样：

```
<a href="/myapp/order/list;jsessionid=23fa31abd41ea093">...</a>
```
URL 可以携带参数：

```
<a th:href="@{/order/details(id=${orderId},type=${orderType})}">...</a>
```

结果如下：

```
<!-- Note ampersands (&) should be HTML-escaped in tag attributes... -->
<a href="/myapp/order/details?id=23&amp;type=online">...</a>
```


链接表达式可以是相对的，在这种情况下，应用程序上下文将不会作为URL的前缀：

```
<a th:href="@{../documents/report}">...</a>
```

也 可以是服务器相对（同样，没有应用程序上下文前缀）：

```
<a th:href="@{~/contents/main}">...</a>
```

和协议相对（就像绝对URL，但浏览器将使用在显示的页面中使用的相同的HTTP或HTTPS协议）：

```
<a th:href="@{//static.mycompany.com/res/initial}">...</a>
```

当然，Link表达式可以是绝对的：

```
<a th:href="@{http://www.mycompany.com/main}">...</a>
```


> 在绝对（或协议相对）的URL等里面，Thymeleaf链接表达式添加的是什么值？
> 答案是，可能是由响应过滤器定义的URL重写。在基于Servlet的Web应用程序中，对于每个输出的URL（上下文相对、相对、绝对...）Thymeleaf将总是调用HttpServletResponse.encodeUrl(...) 机制 在显示URL之前。 这意味着过滤器可以通过包装HttpServletResponse对象（通常使用的机制）来为应用程序执行定制的URL重写。

## Fragment expressions（分段表达式）

分段表达式是 3.x 版本新增的内容。

分段段表达式是一种表示标记片段并将其移动到模板周围的简单方法。 正是由于这些表达式，片段可以被复制，或者作为参数传递给其他模板等等。

最常见的用法是使用`th:insert`或`th:replace:`插入片段：

```
<div th:insert="~{commons :: main}">...</div>
```

但是它们可以在任何地方使用，就像任何其他变量一样：

```
<div th:with="frag=~{footer :: #main/text()}">
  <p th:insert="${frag}">
</div>
```

分段表达式可以有参数。

## Literals（字面量）
 

### 本文

文本文字只是在单引号之间指定的字符串。 他们可以包含任何字符，但您应该避免其中的任何单引号使用`\'`。
 

```html
<p>
  Now you are looking at a <span th:text="'working web application'">template file</span>.
</p>
```

### 数字

数字文字就是数字。

```html
<p>The year is <span th:text="2013">1492</span>.</p>
<p>In two years, it will be <span th:text="2013 + 2">1494</span>.</p>
```


### 布尔

布尔文字为“true”和“false”。 例如：:

```html
<div th:if="${user.isAdmin()} == false"> ...
```

在这个例子中，`== false`写在大括号之外，Thymeleaf 会做处理。如果是写在大括号内，那就是由 OGNL/SpringEL 引擎负责处理：


```html
<div th:if="${user.isAdmin() == false}"> ...
```


### null

`null` 字面量使用如下：

```html
<div th:if="${variable.something} == null"> ...
```


### 字面量标记

数字、布尔和 null 字面实际上是_字面量标记(literal tokens)_的特殊情况。

这些标记允许在标准表达式中进行一点简化。 他们工作与文字文字（`'...'`）完全相同，但只允许使用字母（`A-Z`）和`a-z'），数字（0-9），括号（`[`和`]`），点（`.`），连字符（`-`）
和下划线（`_`）。 所以没有空白，没有逗号等

标记不需任何引号。 所以我们可以这样做：


```html
<div th:class="content">...</div>
```

用来代替:

```html
<div th:class="'content'">...</div>
```

## 附加文本

无论是文字，还是评估变量或消息表达式的结果，都可以使用　`+` 操作符轻松地附加文本：
　

```html
<span th:text="'The name of the user is ' + ${user.name}">
```

##　字面量替换

字面量替换允许容易地格式化包含变量值的字符串，而不需要使用 `'...' + '...'`附加文字。

这些替换必须被(`|`)包围，如：


```html
<span th:text="|Welcome to our application, ${user.name}!|">
```

其等价于：

```html
<span th:text="'Welcome to our application, ' + ${user.name} + '!'">
```

字面量替换可以与其他类型的表达式相结合：



```html
<span th:text="${onevar} + ' ' + |${twovar}, ${threevar}|">
```

>　 `|...|` 字面量替换只允许使用变量/消息表达式(`${...}`, `*{...}`, `#{...}`)，
> 其他字面量  (`'...'`)、布尔/数字标记、条件表达式等是不允许的 

## 算术运算

支持算术运算：`+`, `-`, `*`, `/` 和 `%`。


```html
<div th:with="isEven=(${prodStat.count} % 2 == 0)">
```

请注意，这些运算符也可以在OGNL变量表达式本身中应用（在这种情况下将由OGNL执行，而不是Thymeleaf标准表达式引擎）：


```html
<div th:with="isEven=${prodStat.count % 2 == 0}">
```

请注意，其中一些运算符存在文本别名：`div` (`/`)、 `mod` (`%`)。

## 比较与相等

表达式中的值可以与`>`, `<`, `>=` 和 `<=` 号进行比较，并且可以使用`==` 和 `!=` 运算符来检查是否相等。 请注意， `<` 和 `>`符号不应该在XML属性值中使用，因此它们应被替换为`&lt;` 和`&gt;`。

```html
<div th:if="${prodStat.count} &gt; 1">
<span th:text="'Execution mode is ' + ( (${execMode} == 'dev')? 'Development' : 'Production')">
```


一个更简单的替代方案可能是使用一些这些运算符存在的文本别名：`gt` (`>`), `lt` (`<`), `ge` (`>=`), `le` (`<=`), `not` (`!`). `eq` (`==`), `neq`/`ne` (`!=`)


## 条件表达式

条件表达式仅用于评估两个表达式中的一个，这取决于评估条件（本身就是另一个表达式）的结果。

我们来看一个示例 `th:class` 片段 ：

```html
<tr th:class="${row.even}? 'even' : 'odd'">
  ...
</tr>
```

条件表达式（condition，then和else）的所有三个部分都是自己的表达式，这意味着它们可以是变量(`${...}`, `*{...}`)，消息 (`#{...}`) ，(`@{...}`) 或字面量(`'...'`)。

条件表达式也可以使用括号嵌套：


```html
<tr th:class="${row.even}? (${row.first}? 'first' : 'even') : 'odd'">
  ...
</tr>
```

else表达式也可以省略，在这种情况下，如果条件为false，则返回null值：

```html
<tr th:class="${row.even}? 'alt'">
  ...
</tr>
```

## 默认表达式（Elvis operator）
 

_默认表达式(default expression)_是一种特殊的条件值，没有_then_
部分。它相当于某些语言中的_Elvis operator_存在，比如
Groovy。指定两个表达式，如果第一个不是 null，则使用第二个。

查看如下示例：

```html
<div th:object="${session.user}">
  ...
  <p>Age: <span th:text="*{age}?: '(no age specified)'">27</span>.</p>
</div>
```


这相当于：

```html
<p>Age: <span th:text="*{age != null}? *{age} : '(no age specified)'">27</span>.</p>
```


与条件表达式一样，它们之间可以包含嵌套表达式：


```html
<p>
  Name: 
  <span th:text="*{firstName}?: (*{admin}? 'Admin' : #{default.username})">Sebastian</span>
</p>
```


## 无操作标记

无操作标记由下划线符号（`_`）表示。表示什么也不做，这允许开发人员使用原型中的文本默认值。 例如，：

```html
<span th:text="${user.name} ?: 'no user authenticated'">...</span>
```

我们可以直接使用*'no user authenticated'* 作为原型文本，这样代码从设计的角度来看起来很简洁：

```html
<span th:text="${user.name} ?: _">no user authenticated</span>
```

## 数据转换及格式化

Thymeleaf 的双大括号为变量表达式（`$ {...}`）和选择表达式（`* {...}`）提供了数据*转换服务*：

它看上去是这样的：

```html
<td th:text="${{user.lastAccessDate}}">...</td>
```

注意到双括号吗？：`$ {{...}}`。这意味着Thymeleaf可以通过转换服务将结果转换为`String`。

假设`user.lastAccessDate`类型为`java.util.Calendar`，如果*转换服务*
（“IStandardConversionService”的实现）已经被注册并且包含有效的`Calendar - > String`的
转换，则它将被应用。

“IStandardConversionService”（“StandardConversionService”）的默认实现类）只需在转换为“String”的任何对象上执行`.toString()`。有关更多信息如何注册一个自定义*转换服务*实现，看看[更多配置]（＃more-on-configuration）部分。

## 表达式预处理

表达式预处理（expression preprocessing），它被定义在下划线`_`之间：

```
#{selection.__${sel.code}__}
```

我们看到的变量表达式`${sel.code}`将先被执行，假如结果是`"ALL"`，那么_之间的值`"ALL"`将被看做表达式的一部分被执行，在这里会变成`selection.ALL`。	

