# 模板布局

## 包含模板片段

### 定义和引用片段

在我们的模板中，我们经常需要从其他模板中添加html页面片段，如页脚、标题、菜单...

为了做到这一点，Thymeleaf 需要我们来定义这些“片段”，可以使用`th：fragment`属性来完成。我们定义了`/WEB-INF/templates/footer.html`页面 

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <body>
  
    <div th:fragment="copy">
      &copy; 2017 <a href="https://waylau.com">waylau.com</a>
    </div>
  
  </body>
  
</html>
```

如果，我们想引用这个 `copy` 代码片段，我们可以用 `th:insert` 或 `th:replace` 属性 (`th:include` 也是可以，但自 Thymeleaf 3.0 以来就不再推荐):


```html
<body>

  ...

  <div th:insert="~{footer :: copy}"></div>
  
</body>
```

注意`th：insert`需要一个*片段表达式*（`〜{...}`）。 在上面的例子中，非复杂*片段表达式*，（`〜{`，`}`）包围是完全可选，所以上面的代码将等效于：


```html
<body>

  ...

  <div th:insert="footer :: copy"></div>
  
</body>
```

### 片段规范语法

 * `"~{templatename::selector}"` 名为`templatename`的模板上的指定标记选择器。 `selector`可以只是一个片段名。
 * `"~{templatename}"` ： 包含完整的模版 `templatename`
 * `~{::selector}"` or `"~{this::selector}"` 相同模版中的代码片段
   
### 不使用 `th:fragment`

 不使用 `th:fragment`也可以引用HTML片段，比如：
 

```html
...
<div id="copy-section">
  &copy; 2017 <a href="https://waylau.com">waylau.com</a>
</div>
...
```

通过 id 也可以引用到页面片段:

```html
<body>

  ...

  <div th:insert="~{footer :: #copy-section}"></div>
  
</body>
```

###  `th:insert`、 `th:replace` 、`th:include`三者区别

* `th：insert`是最简单的：它将简单地插入指定的片段作为正文 的主标签。
* `th：replace`用指定实际片段来替换其主标签。
* `th：include`类似于`th：insert`，但不是插入片段它只插入此片段的*内容*。

所以


```html
<footer th:fragment="copy">
  &copy; 2017 <a href="https://waylau.com">waylau.com</a>
</footer>
```

三种方式引用该片段

```html
<body>

  ...

  <div th:insert="footer :: copy"></div>

  <div th:replace="footer :: copy"></div>

  <div th:include="footer :: copy"></div>
  
</body>
```

结果为：

```html
<body>

  ...

  <div>
    <footer>
      &copy; 2017 <a href="https://waylau.com">waylau.com</a>
    </footer>
  </div>

  <footer>
    &copy; 2017 <a href="https://waylau.com">waylau.com</a>
  </footer>

  <div>
    &copy; 2017 <a href="https://waylau.com">waylau.com</a>
  </div>
  
</body>
```


