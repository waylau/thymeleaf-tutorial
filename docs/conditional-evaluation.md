# 条件语句

## "if" 和 "unless"

`th:if` 属性用法如下：

```html
<a href="comments.html"
   th:href="@{/product/comments(prodId=${prod.id})}" 
   th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```


请注意，`th:if` 属性不仅是将评估布尔条件。 它的功能有点超出这一点，它将按照这些规则评估指定的表达式：

* 如果值不为 null：
	* 如果值为布尔值，则为true。
	* 如果值是数字，并且不为零
	* 如果值是一个字符且不为零
	* 如果value是String，而不是“false”，“off”或“no”
	* 如果值不是布尔值，数字，字符或字符串。
* 如果值为null，则th：if 将为 false。

另外，`th:if`有一个相反的属性`th:unless`，前面的例子改为：



```html
<a href="comments.html"
   th:href="@{/comments(prodId=${prod.id})}" 
   th:unless="${#lists.isEmpty(prod.comments)}">view</a>
```

## switch 语句

switch 语句使用`th:switch` / `th:case` 属性集合来实现：

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
</div>
```

请注意，只要一个`th：case`属性被评估为'true'，每个其他同一个 switch 语句中的`th：case`属性将被评估为`false`。

`th:case="*"`来设置默认选项：

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

