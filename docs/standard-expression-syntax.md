# Thymeleaf 标准表达式语法

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