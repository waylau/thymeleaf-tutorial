# 注释及注释块

## 标准 HTML/XML 注释

标准 HTML/XML 注释`<!-- ... -->` 可以在模版中使用：


```html
<!-- User info follows -->
<div th:text="${...}">
  ...
</div>
```

## Thymeleaf 解析器级注释块

解析器级注释块是当 Thymeleaf 解析它的模板时，这些代码将被简单地从中删除。 他们看起来像这样：
```html
<!--/* This code will be removed at Thymeleaf parsing time! */-->
``` 

Thymeleaf将删除`<!--/*` 和 `*/-->`之间的所有内容，所以这些注释块也可以用于在模板静态打开时显示代码，当Thymeleaf处理它会被删除：

```html
<!--/*--> 
  <div>
     you can see me only before Thymeleaf processes me!
  </div>
<!--*/-->
```

这在原型中设计中有很多 `<tr>`时，非常适用：

```html
<table>
   <tr th:each="x : ${xs}">
     ...
   </tr>
   <!--/*-->
   <tr>
     ...
   </tr>
   <tr>
     ...
   </tr>
   <!--*/-->
</table>
```

## 原型注释块

原型注释块是指，当模版静态打开时（比如原型设计），原型注释块所注释的代码将被注释，而在模版执行时，这些注释的代码，就能被显示出来。

```html
<span>hello!</span>
<!--/*/
  <div th:text="${...}">
    ...
  </div>
/*/-->
<span>goodbye!</span>
```

Thymeleaf 的解析系统将简单地删除`<!--/*/` 和 `/*/-->` 标记，但保留所注释的内容。 那么什么时候执行模板，Thymeleaf 实际上会看到：


```html
<span>hello!</span>
 
  <div th:text="${...}">
    ...
  </div>
 
<span>goodbye!</span>
```

与解析器级注释块一样，此功能与方言无关。

## 合成`th:block`标签
 
标准方言中包含的唯一的元素处理器（不是属性）是`th:block`。

`th：block`只是一个属性容器，允许模板开发人员指定他们想要的任何属性。 Thymeleaf 将执行这些属性，然后简单地制作块，而不是让其内容消失。

因此，例如，当为每个元素创建需要多个`<tr>` 的迭代时，这可能是有用的：

```html
<table>
  <th:block th:each="user : ${users}">
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
  </th:block>
</table>
```

当与原型注释块组合使用时尤其有用：

```html
<table>
    <!--/*/ <th:block th:each="user : ${users}"> /*/-->
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
    <!--/*/ </th:block> /*/-->
</table>
```
