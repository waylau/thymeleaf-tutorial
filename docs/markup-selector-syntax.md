# 标记选择器语法

Thymeleaf 的标记选择器语法可直接查看Thymeleaf 的解析库[AttoParser](http://attoparser.org)。

该选择器的语法与XPath、CSS和jQuery 的选择器的语法相似，这使得它们易于被大多数用户所理解。你可以有看看完整的语法参考
[AttoParser文档]（http://www.attoparser.org/apidocs/attoparser/2.0.0.RELEASE/org/attoparser/select/package-summary.html）。

例如，以下选择器将选择每个  class 是 `content`的`<div>`：



```html
<div th:insert="mytemplate :: //div[@class='content']">...</div>
```

基本语法包括：

 * `/x` 表示名为x的当前节点的直接子节点

 * `//x` 表示当前节点名为x的任何深度的子节点

 * `x[@z="v"]` 表示名称为x的元素，名称为z的属性为值“v”

 * `x[@z1="v1" and @z2="v2"]`表示名称为x和属性z1的元素，z2分别为值“v1”和“v2”

 * `x[i]` 是指名称为x的元素位于第i个兄弟之间

 * `x[@z="v"][i]` 表示名称为x，属性z值为“v”的元素位于第i位的兄弟姐妹中也符合这一条件


但是也可以使用更简洁的语法：

 * `x` 完全等同于 `//x` 

 * 只要包含参数的规范，允许使用没有元素名称/引用的选择器。所以`[@class='oneclass']`是一个有效的选择器，用于查找具有值`"oneclass"`的类属性的任何元素（标签）。

高级属性选择功能：

 * 除了`=`（等于），其他比较运算符也是有效的： `!=`（不等于），`^=`（开始）和`$=` （以结尾）。例如：`x[@class^='section']``表示名称为“x”的元素，以“section”开头的属性“class”的值。

 * 属性可以以“@”（XPath样式）开头和不以之开头（jQuery样式）指定。所以x[z='v']` 等同于 `x[@z='v']`。
 
 * 多属性修饰符可以使用`and`（XPath样式）以及链接多个修饰符（jQuery样式）。所以`x[@z1='v1' and @z2='v2']` 实际上等同于 `x[@z1='v1'][@z2='v2']` （还有`x[z1='v1'][z2='v2']`）。

直接_jQuery样式_选择器：:

 * `x.oneclass`等价于`x[class='oneclass']`

 * `.oneclass` 等价于`[class='oneclass']`

 * `x#oneid` 等价于`x[id='oneid']`

 * `#oneid` 等价于 `[id='oneid']`

 * `x%oneref` 表示 `<x>`标签有一个`th:ref="oneref"` 或 `th:fragment="oneref"`属性

 * `%oneref` 表示任何有`th:ref="oneref"` 或 `th:fragment="oneref"属性的`标签。这实际上等同于简单的`oneref`，因为可以使用引用而不是元素名称。

 * 直接选择器和属性选择器可以混用: `a.external[@href^='https']`.

所以上面的 Markup Selector表达式:

```html
<div th:insert="mytemplate :: //div[@class='content']">...</div>
```

可以写为:

```html
<div th:insert="mytemplate :: div.content">...</div>
```

检查一个不同的例子，这个:

```html
<div th:replace="mytemplate :: myfrag">...</div>
```
将寻找一个`th:fragment="myfrag"`片段签名（或`th:ref`引用）。 但是，如果它们存在（它们不是在HTML中）也会查找名称为`myfrag`的标签。 注意区别：

```html
<div th:replace="mytemplate :: .myfrag">...</div>
```

它将实际上寻找任何具有`class="myfrag"`的元素，而不用关心`th:fragment`签名（或 `th:ref` 引用）。
 


### 多值类匹配

标记选择器将类属性理解为**多值的（multivalued）**，因此即使该元素具有多个类值，也允许在该属性上应用选择器。

例如，`div.two`将匹配 `<div class="one two three" />`
