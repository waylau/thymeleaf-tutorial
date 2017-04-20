# 内联

## 表达式内联

虽然标准方言使我们能够使用标签属性来做几乎所有的事情，但是有些情况下我们更喜欢将表达式直接写入我们的HTML文本。 例如，我们可以喜欢写这个：

```html
<p>Hello, [[${session.user.name}]]!</p>
```

来代替:

```html
<p>Hello, <span th:text="${session.user.name}">Sebastian</span>!</p>
```

`[[...]]` 或 `[(...)]` 被称为 **内联表达式（inlined expressions）**，分别对应于 `th:text` 和 `th:utext` 属性。

```html
<p>The message is "[(${msg})]"</p>
```

这个的结果显示， `<b>` 标签不会被转义:

```html
<p>The message is "This is <b>great!</b>"</p>
```

如果是这样:

```html
<p>The message is "[[${msg}]]"</p>
```

则结果会被转义:

```html
<p>The message is "This is &lt;b&gt;great!&lt;/b&gt;"</p>
```

### 内联与自然模板

虽然内联看上去比自然模板更加简洁，但它并不总是适用，比如当您静态打开HTML文件时，会逐字显示，因为内联无法将它们用作设计原型了！

### 禁用内联

有时，我们想禁用这种机制，比如，想输出 `[[...]]` 或  `[(...)]` 文本内容，而不是将其视为表达式。 为此，我们将使用 `th:inline="none"`：

```html
<p th:inline="none">A double array looks like this: [[1, 2, 3], [4, 5]]!</p>
```

结果为：

```html
<p>A double array looks like this: [[1, 2, 3], [4, 5]]!</p>
```

## 文本内联

*文本内联*与我们拥有的*表达式内联*功能非常相似，但实际上拥有了更多的能力。 为了启用它我们使用`th:inline="text"`。

文本内联不仅允许我们使用相同的*内联表达式*，而且实际上可以处理在“TEXT”模板模式下的标签体。

我们将在下一章中看到关于[文本模板模式](textual-template-modes.md)的更多信息。

## JavaScript 内联

```html
<script th:inline="javascript">
    ...
    var username = [[${session.user.name}]];
    ...
</script>
```

结果为:

```html
<script th:inline="javascript">
    ...
    var username = "Sebastian \"Fruity\" Applejuice";
    ...
</script>
```

### JavaScript 自然模版

我们可以在JavaScript中包装（转义）内联的表达式注释如下：


```html
<script th:inline="javascript">
    ...
    var username = /*[[${session.user.name}]]*/ "Gertrud Kiwifruit";
    ...
</script>
```

Thymeleaf 将会忽略所有注释之后分号之前的内容（就是例子中的` 'Gertrud Kiwifruit'`）。所以结果输出如下:

```html
<script th:inline="javascript">
    ...
    var username = "Sebastian \"Fruity\" Applejuice";
    ...
</script>
```

### 高级内联评估和JavaScript序列化

关于JavaScript内联的一个重要的事情是这个表达式评估是非常智能的，并不仅限于字符串的处理。Thymeleaf 会正确写入 JavaScript 语法中的以下几种对象：

 * String
 * Number
 * Boolean
 * Array
 * Collection
 * Map
 * Bean (有 _getter_ and _setter_ 方法的对象)

例如，如果我们有以下代码：

```html
<script th:inline="javascript">
    ...
    var user = /*[[${session.user}]]*/ null;
    ...
</script>
```

那个 `${session.user}` 表达式将评估为一个`User`对象，Thymeleaf 将正确转换为 JavaScript 语法：

```html
<script th:inline="javascript">
    ...
    var user = {"age":null,"firstName":"John","lastName":"Apricot",
                "name":"John Apricot","nationality":"Antarctica"};
    ...
</script>
```

这个 JavaScript 序列化的方式是通过实现`org.thymeleaf.standard.serializer.IStandardJavaScriptSerializer`
接口来实现的，可以在  `StandardDialect`的实例配置被用于模板引擎。

此JS序列化机制的默认实现将会查找在类路径中[Jackson 库]（https://github.com/FasterXML/jackson），如果存在，将会使用它。如果没有，它将应用一个内置的序列化机制。这涵盖了大多数场景的需求，并产生了类似的结果（但是
相比较 Jackson 而言不够灵活）。


## CSS 内联

Thymeleaf 允许使用 CSS `<style>`标签，比如:

```html
<style th:inline="css">
  ...
</style>
```

比如，我们想设置两个变量到不同的`String`值:

```
classname = 'main elems'
align = 'center'
```

我们可以这么做:

```html
<style th:inline="css">
    .[[${classname}]] {
      text-align: [[${align}]];
    }
</style>
```

结果为:

```html
<style th:inline="css">
    .main\ elems {
      text-align: center;
    }
</style>
```


请注意，CSS 内联中还包含一些*智能*处理，就像JavaScript 内联一样。具体来说，通过*转义*表达式来输出表达式，如 `[[${classname}]]`
将被转义为**CSS 标识符**。 这就是为什么我们的 `classname = 'main elems'`在上面的代码片段中变成了`main\ elems`。

### CSS 自然模版

与JavaScript 类似，也有 CSS 自然模版，用法如下：

```html
<style th:inline="css">
    .main\ elems {
      text-align: /*[[${align}]]*/ left;
    }
</style>
```


