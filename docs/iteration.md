# 迭代器


## 基本的迭代


`th:each`将循环 array 或 list 中的元素并重复打印一组标签，语法相当于 Java foreach 表达式：

```
<li th:each="book : ${books}" th:text="${book.title}">En las Orillas del Sar</li>
```

可以使用`th：each`属性进行遍历的对象包括：

* 任何实现`java.util.Iterable`的对象
* 任何实现`java.util.Enumeration`的对象
* 任何实现`java.util.Iterator`的对象，其值将被迭代器返回，而不需要在内存中缓存所有的值
* 任何实现`java.util.Map`的对象。 迭代映射时，迭代变量 将是`java.util.Map.Entry`类
* 任何数组
* 任何其他对象将被视为包含对象本身的单值列表

## 状态变量

Thymeleaf 提供 _状态变量（status variable）_ 来跟踪迭代器的状态。

`th:each`属性中，定义了如下状态变量：

 * `index` 属性是当前 _迭代器索引（iteration index）_，从0开始
 * `count` 属性是当前 _迭代器索引（iteration index）_，从1开始
 * `size` 属性是迭代器元素的总数
 * `current` 是当前 _迭代变量（iter variable）_
 * `even/odd` 判断当前迭代器是否是 even 或 odd
 * `first` 判断当前迭代器是否是第一个
 * `last` 判断当前迭代器是否是最后
 

看下面的例子：

```html
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
  </tr>
  <tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
  </tr>
</table>
```

状态变量（在本示例中为“iterStat”）在`th：each`中定义了。 

我们来看看模板的处理后的结果：

```html
<!DOCTYPE html>

<html>

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta content="text/html; charset=UTF-8" http-equiv="Content-Type"/>
    <link rel="stylesheet" type="text/css" media="all" href="/gtvg/css/gtvg.css" />
  </head>

  <body>

    <h1>Product list</h1>
  
    <table>
      <tr>
        <th>NAME</th>
        <th>PRICE</th>
        <th>IN STOCK</th>
      </tr>
      <tr class="odd">
        <td>Fresh Sweet Basil</td>
        <td>4.99</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>Italian Tomato</td>
        <td>1.25</td>
        <td>no</td>
      </tr>
      <tr class="odd">
        <td>Yellow Bell Pepper</td>
        <td>2.50</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>Old Cheddar</td>
        <td>18.75</td>
        <td>yes</td>
      </tr>
    </table>
  
    <p>
      <a href="/gtvg/" shape="rect">Return to home</a>
    </p>

  </body>
  
</html>
```

请注意，我们的迭代状态变量已经运行良好，建立只有奇数行具有 “odd” CSS 类。

如果您没有明确设置状态变量，则 Thymeleaf 将始终创建一个状态变量，可以通过后缀“Stat”获取到迭代变量的名称：

```html
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
  </tr>
  <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
  </tr>
</table>
```

