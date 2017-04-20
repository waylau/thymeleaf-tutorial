# 局部变量

在迭代器中，我们可以使用局部变量`prod`：

```html
<tr th:each="prod : ${prods}">
    ...
</tr>
```


Thymeleaf 为您提供了一种在不使用迭代的情况下声明局部变量的方法`th：with`属性，其语法与属性值类似：

```html
<div th:with="firstPer=${persons[0]}">
  <p>
    The name of the first person is <span th:text="${firstPer.name}">Julius Caesar</span>.
  </p>
</div>
```

当`th：with`被处理时，`firstPer`变量创建为局部变量，并添加到来自上下文的变量 map 中，以便它是可用于评估以及在上下文中声明的任何其他变量，但只能在包含`<div>`标签的范围内。

可以同时定义多个变量，赋值语法为：

```html
<div th:with="firstPer=${persons[0]},secondPer=${persons[1]}">
  <p>
    The name of the first person is <span th:text="${firstPer.name}">Julius Caesar</span>.
  </p>
  <p>
    But the name of the second person is 
    <span th:text="${secondPer.name}">Marcus Antonius</span>.
  </p>
</div>
```

`th：with`属性允许重用在同一属性中定义的变量：

```html
<div th:with="company=${user.company + ' Co.'},account=${accounts[company]}">...</div>
```