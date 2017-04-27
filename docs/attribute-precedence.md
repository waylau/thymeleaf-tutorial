# 属性优先级

当在同一个标签中写入多个`th:*`属性时，会发生什么？ 对于下面的例子：

```html
<ul>
  <li th:each="item : ${items}" th:text="${item.description}">Item description here...</li>
</ul>
```

我们期望`th:each`属性在`th:text`之前执行，从而可以得到了我们想要的结果，但是鉴于 HTML/XML标准并未对标签中的属性的顺序给出任何的定义，所以必须在属性中建立_优先级（precedence）_机制
以确保这将按预期工作。

所以，所有的Thymeleaf属性定义一个数字优先级，它建立了它们在标签中执行的顺序。 这个是列表：

<div class="table-scroller">
<table style="width:90%;">
<colgroup>
<col style="width: 11%">
<col style="width: 48%">
<col style="width: 30%">
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">Order</th>
<th style="text-align: left;">Feature</th>
<th style="text-align: left;">Attributes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">1</td>
<td style="text-align: left;">Fragment inclusion</td>
<td style="text-align: left;"><code>th:insert</code><br>
<code>th:replace</code></td>
</tr>
<tr class="even">
<td style="text-align: left;">2</td>
<td style="text-align: left;">Fragment iteration</td>
<td style="text-align: left;"><code>th:each</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;">3</td>
<td style="text-align: left;">Conditional evaluation</td>
<td style="text-align: left;"><code>th:if</code><br>
<code>th:unless</code><br>
<code>th:switch</code><br>
<code>th:case</code></td>
</tr>
<tr class="even">
<td style="text-align: left;">4</td>
<td style="text-align: left;">Local variable definition</td>
<td style="text-align: left;"><code>th:object</code><br>
<code>th:with</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;">5</td>
<td style="text-align: left;">General attribute modification</td>
<td style="text-align: left;"><code>th:attr</code><br>
<code>th:attrprepend</code><br>
<code>th:attrappend</code></td>
</tr>
<tr class="even">
<td style="text-align: left;">6</td>
<td style="text-align: left;">Specific attribute modification</td>
<td style="text-align: left;"><code>th:value</code><br>
<code>th:href</code><br>
<code>th:src</code><br>
<code>...</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;">7</td>
<td style="text-align: left;">Text (tag body modification)</td>
<td style="text-align: left;"><code>th:text</code><br>
<code>th:utext</code></td>
</tr>
<tr class="even">
<td style="text-align: left;">8</td>
<td style="text-align: left;">Fragment specification</td>
<td style="text-align: left;"><code>th:fragment</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;">9</td>
<td style="text-align: left;">Fragment removal</td>
<td style="text-align: left;"><code>th:remove</code></td>
</tr>
</tbody>
</table>
</div>


这个优先机制意味着如果属性位置被反转，上述迭代片段将给出完全相同的结果（尽管它的可读性稍差）：

```html
<ul>
  <li th:text="${item.description}" th:each="item : ${items}">Item description here...</li>
</ul>
```
