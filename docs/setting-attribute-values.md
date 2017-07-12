# 设置属性值

本章将介绍我们在标记中设置（或修改）属性值的方式。

## 设置任意属性值

`th:attr` 用于设置属性：
 
```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

`th:attr`会将表达式的结果，设置到相应的属性中去。上面模板结果如下：

```html
<form action="/gtvg/subscribe">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="¡Suscríbe!"/>
  </fieldset>
</form>
```

我们也能同时设置多个属性值：

```html
<img src="../../images/gtvglogo.png" 
     th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

输出如下:

```html
<img src="/gtgv/images/gtvglogo.png" title="Logo de Good Thymes" alt="Logo de Good Thymes" />
```

## 设置值到指定的属性


现在，你可能会想到像：

```html
<input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
```

上面可以指定一个属性的值，但看上去有点丑陋，且并不是最优雅的方式。 通常，你将使用其他任务的`th：*`属性
设置特定的标签属性（而不仅仅是像“th：attr”这样的任意属性）。

例如，要设置`value`属性，使用`th：value`：



```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
```

要设置 `action` 属性，使用`th:action`：
 

```html
<form action="subscribe.html" th:action="@{/subscribe}">
```

Thymeleaf 提供了很多属性，每个都针对特定的HTML5属性：

<div class="table-scroller">
<table>
<tbody>
<tr class="odd">
<td style="text-align: left;"><code>th:abbr</code></td>
<td style="text-align: left;"><code>th:accept</code></td>
<td style="text-align: left;"><code>th:accept-charset</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:accesskey</code></td>
<td style="text-align: left;"><code>th:action</code></td>
<td style="text-align: left;"><code>th:align</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:alt</code></td>
<td style="text-align: left;"><code>th:archive</code></td>
<td style="text-align: left;"><code>th:audio</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:autocomplete</code></td>
<td style="text-align: left;"><code>th:axis</code></td>
<td style="text-align: left;"><code>th:background</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:bgcolor</code></td>
<td style="text-align: left;"><code>th:border</code></td>
<td style="text-align: left;"><code>th:cellpadding</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:cellspacing</code></td>
<td style="text-align: left;"><code>th:challenge</code></td>
<td style="text-align: left;"><code>th:charset</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:cite</code></td>
<td style="text-align: left;"><code>th:class</code></td>
<td style="text-align: left;"><code>th:classid</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:codebase</code></td>
<td style="text-align: left;"><code>th:codetype</code></td>
<td style="text-align: left;"><code>th:cols</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:colspan</code></td>
<td style="text-align: left;"><code>th:compact</code></td>
<td style="text-align: left;"><code>th:content</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:contenteditable</code></td>
<td style="text-align: left;"><code>th:contextmenu</code></td>
<td style="text-align: left;"><code>th:data</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:datetime</code></td>
<td style="text-align: left;"><code>th:dir</code></td>
<td style="text-align: left;"><code>th:draggable</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:dropzone</code></td>
<td style="text-align: left;"><code>th:enctype</code></td>
<td style="text-align: left;"><code>th:for</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:form</code></td>
<td style="text-align: left;"><code>th:formaction</code></td>
<td style="text-align: left;"><code>th:formenctype</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:formmethod</code></td>
<td style="text-align: left;"><code>th:formtarget</code></td>
<td style="text-align: left;"><code>th:fragment</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:frame</code></td>
<td style="text-align: left;"><code>th:frameborder</code></td>
<td style="text-align: left;"><code>th:headers</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:height</code></td>
<td style="text-align: left;"><code>th:high</code></td>
<td style="text-align: left;"><code>th:href</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:hreflang</code></td>
<td style="text-align: left;"><code>th:hspace</code></td>
<td style="text-align: left;"><code>th:http-equiv</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:icon</code></td>
<td style="text-align: left;"><code>th:id</code></td>
<td style="text-align: left;"><code>th:inline</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:keytype</code></td>
<td style="text-align: left;"><code>th:kind</code></td>
<td style="text-align: left;"><code>th:label</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:lang</code></td>
<td style="text-align: left;"><code>th:list</code></td>
<td style="text-align: left;"><code>th:longdesc</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:low</code></td>
<td style="text-align: left;"><code>th:manifest</code></td>
<td style="text-align: left;"><code>th:marginheight</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:marginwidth</code></td>
<td style="text-align: left;"><code>th:max</code></td>
<td style="text-align: left;"><code>th:maxlength</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:media</code></td>
<td style="text-align: left;"><code>th:method</code></td>
<td style="text-align: left;"><code>th:min</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:name</code></td>
<td style="text-align: left;"><code>th:onabort</code></td>
<td style="text-align: left;"><code>th:onafterprint</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onbeforeprint</code></td>
<td style="text-align: left;"><code>th:onbeforeunload</code></td>
<td style="text-align: left;"><code>th:onblur</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:oncanplay</code></td>
<td style="text-align: left;"><code>th:oncanplaythrough</code></td>
<td style="text-align: left;"><code>th:onchange</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onclick</code></td>
<td style="text-align: left;"><code>th:oncontextmenu</code></td>
<td style="text-align: left;"><code>th:ondblclick</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:ondrag</code></td>
<td style="text-align: left;"><code>th:ondragend</code></td>
<td style="text-align: left;"><code>th:ondragenter</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:ondragleave</code></td>
<td style="text-align: left;"><code>th:ondragover</code></td>
<td style="text-align: left;"><code>th:ondragstart</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:ondrop</code></td>
<td style="text-align: left;"><code>th:ondurationchange</code></td>
<td style="text-align: left;"><code>th:onemptied</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onended</code></td>
<td style="text-align: left;"><code>th:onerror</code></td>
<td style="text-align: left;"><code>th:onfocus</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onformchange</code></td>
<td style="text-align: left;"><code>th:onforminput</code></td>
<td style="text-align: left;"><code>th:onhashchange</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:oninput</code></td>
<td style="text-align: left;"><code>th:oninvalid</code></td>
<td style="text-align: left;"><code>th:onkeydown</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onkeypress</code></td>
<td style="text-align: left;"><code>th:onkeyup</code></td>
<td style="text-align: left;"><code>th:onload</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onloadeddata</code></td>
<td style="text-align: left;"><code>th:onloadedmetadata</code></td>
<td style="text-align: left;"><code>th:onloadstart</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onmessage</code></td>
<td style="text-align: left;"><code>th:onmousedown</code></td>
<td style="text-align: left;"><code>th:onmousemove</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onmouseout</code></td>
<td style="text-align: left;"><code>th:onmouseover</code></td>
<td style="text-align: left;"><code>th:onmouseup</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onmousewheel</code></td>
<td style="text-align: left;"><code>th:onoffline</code></td>
<td style="text-align: left;"><code>th:ononline</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onpause</code></td>
<td style="text-align: left;"><code>th:onplay</code></td>
<td style="text-align: left;"><code>th:onplaying</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onpopstate</code></td>
<td style="text-align: left;"><code>th:onprogress</code></td>
<td style="text-align: left;"><code>th:onratechange</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onreadystatechange</code></td>
<td style="text-align: left;"><code>th:onredo</code></td>
<td style="text-align: left;"><code>th:onreset</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onresize</code></td>
<td style="text-align: left;"><code>th:onscroll</code></td>
<td style="text-align: left;"><code>th:onseeked</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onseeking</code></td>
<td style="text-align: left;"><code>th:onselect</code></td>
<td style="text-align: left;"><code>th:onshow</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onstalled</code></td>
<td style="text-align: left;"><code>th:onstorage</code></td>
<td style="text-align: left;"><code>th:onsubmit</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:onsuspend</code></td>
<td style="text-align: left;"><code>th:ontimeupdate</code></td>
<td style="text-align: left;"><code>th:onundo</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:onunload</code></td>
<td style="text-align: left;"><code>th:onvolumechange</code></td>
<td style="text-align: left;"><code>th:onwaiting</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:optimum</code></td>
<td style="text-align: left;"><code>th:pattern</code></td>
<td style="text-align: left;"><code>th:placeholder</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:poster</code></td>
<td style="text-align: left;"><code>th:preload</code></td>
<td style="text-align: left;"><code>th:radiogroup</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:rel</code></td>
<td style="text-align: left;"><code>th:rev</code></td>
<td style="text-align: left;"><code>th:rows</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:rowspan</code></td>
<td style="text-align: left;"><code>th:rules</code></td>
<td style="text-align: left;"><code>th:sandbox</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:scheme</code></td>
<td style="text-align: left;"><code>th:scope</code></td>
<td style="text-align: left;"><code>th:scrolling</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:size</code></td>
<td style="text-align: left;"><code>th:sizes</code></td>
<td style="text-align: left;"><code>th:span</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:spellcheck</code></td>
<td style="text-align: left;"><code>th:src</code></td>
<td style="text-align: left;"><code>th:srclang</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:standby</code></td>
<td style="text-align: left;"><code>th:start</code></td>
<td style="text-align: left;"><code>th:step</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:style</code></td>
<td style="text-align: left;"><code>th:summary</code></td>
<td style="text-align: left;"><code>th:tabindex</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:target</code></td>
<td style="text-align: left;"><code>th:title</code></td>
<td style="text-align: left;"><code>th:type</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:usemap</code></td>
<td style="text-align: left;"><code>th:value</code></td>
<td style="text-align: left;"><code>th:valuetype</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:vspace</code></td>
<td style="text-align: left;"><code>th:width</code></td>
<td style="text-align: left;"><code>th:wrap</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:xmlbase</code></td>
<td style="text-align: left;"><code>th:xmllang</code></td>
<td style="text-align: left;"><code>th:xmlspace</code></td>
</tr>
</tbody>
</table>
</div>

## 同时设置多个值


`th:alt-title` 和 `th:lang-xmllang` 是两个特殊的属性，可以同时设置同一个值到两个属性：

 * `th:alt-title` 用于设置 `alt` 和 `title`
 * `th:lang-xmllang` 用于设置 `lang` 和 `xml:lang`


```html
<img src="../../images/gtvglogo.png" 
     th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```


等价于:

```html
<img src="../../images/gtvglogo.png" 
     th:src="@{/images/gtvglogo.png}" th:alt-title="#{logo}" />
```

最终结果都是：

```html
<img src="../../images/gtvglogo.png" 
     th:src="@{/images/gtvglogo.png}" th:title="#{logo}" th:alt="#{logo}" />
```


## 附加和添加前缀


`th:attrappend` 和 `th:attrprepend` 用于附加和添加前缀属性。例如



```html
<input type="button" value="Do it!" class="btn" th:attrappend="class=${' ' + cssStyle}" />
```

执行模版， `cssStyle` 变量设置为 `"warning"`时，输出如下：

```html
<input type="button" value="Do it!" class="btn warning" />
```

同时，有 `th:classappend` 和 `th:styleappend` 用于设置CSS 的 class 和 style。例如：

```html
<tr th:each="prod : ${prods}" class="row" th:classappend="${prodStat.odd}? 'odd'">
```



## 固定值布尔属性

HTML具有布尔属性的概念，没有值的属性意味着该值为“true”。 在XHTML中，这些属性只取一个值，即它本身。

例如，`checked`:

```html
<input type="checkbox" name="option2" checked /> <!-- HTML -->
<input type="checkbox" name="option1" checked="checked" /> <!-- XHTML -->
```


标准方言包括允许您通过评估条件来设置这些属性，如果评估为true，则该属性将被设置为其固定值，如果评估为false，则不会设置该属性：

```html
<input type="checkbox" name="active" th:checked="${user.active}" />
```

标准方言中存在以下固定值布尔属性：


<div class="table-scroller">
<table>
<tbody>
<tr class="odd">
<td style="text-align: left;"><code>th:async</code></td>
<td style="text-align: left;"><code>th:autofocus</code></td>
<td style="text-align: left;"><code>th:autoplay</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:checked</code></td>
<td style="text-align: left;"><code>th:controls</code></td>
<td style="text-align: left;"><code>th:declare</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:default</code></td>
<td style="text-align: left;"><code>th:defer</code></td>
<td style="text-align: left;"><code>th:disabled</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:formnovalidate</code></td>
<td style="text-align: left;"><code>th:hidden</code></td>
<td style="text-align: left;"><code>th:ismap</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:loop</code></td>
<td style="text-align: left;"><code>th:multiple</code></td>
<td style="text-align: left;"><code>th:novalidate</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:nowrap</code></td>
<td style="text-align: left;"><code>th:open</code></td>
<td style="text-align: left;"><code>th:pubdate</code></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><code>th:readonly</code></td>
<td style="text-align: left;"><code>th:required</code></td>
<td style="text-align: left;"><code>th:reversed</code></td>
</tr>
<tr class="even">
<td style="text-align: left;"><code>th:scoped</code></td>
<td style="text-align: left;"><code>th:seamless</code></td>
<td style="text-align: left;"><code>th:selected</code></td>
</tr>
</tbody>
</table>
</div>

## 默认属性处理器

提供 “默认属性处理器（default attribute processor）”，当标准方言没有提供的属性时，也可以设置其属性。比如：

```html
<span th:whatever="${user.name}">...</span>
```

`th:whatever` 不是标准方言中提供的属性，则最终输出如下:

```html
<span whatever="John Apricot">...</span>
```


## 支持对HTML5 友好的属性及元素名称

`data-{prefix}-{name}`语法在HTML5中编写自定义属性的标准方式，不需要开发人员使用任何名称空间的名字，如`th：*`。
Thymeleaf使这种语法自动提供给所有的方言（而不只是标准方言）。

```html	
<table>
    <tr data-th-each="user : ${users}">
        <td data-th-text="${user.login}">...</td>
        <td data-th-text="${user.name}">...</td>
    </tr>
</table>
```