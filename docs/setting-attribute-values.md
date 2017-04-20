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
---------------------- ---------------------- ----------------------
`th:abbr`              `th:accept`            `th:accept-charset`    
`th:accesskey`         `th:action`            `th:align`             
`th:alt`               `th:archive`           `th:audio`             
`th:autocomplete`      `th:axis`              `th:background`        
`th:bgcolor`           `th:border`            `th:cellpadding`       
`th:cellspacing`       `th:challenge`         `th:charset`           
`th:cite`              `th:class`             `th:classid`           
`th:codebase`          `th:codetype`          `th:cols`              
`th:colspan`           `th:compact`           `th:content`           
`th:contenteditable`   `th:contextmenu`       `th:data`              
`th:datetime`          `th:dir`               `th:draggable`         
`th:dropzone`          `th:enctype`           `th:for`               
`th:form`              `th:formaction`        `th:formenctype`       
`th:formmethod`        `th:formtarget`        `th:fragment`          
`th:frame`             `th:frameborder`       `th:headers`           
`th:height`            `th:high`              `th:href`              
`th:hreflang`          `th:hspace`            `th:http-equiv`        
`th:icon`              `th:id`                `th:inline`            
`th:keytype`           `th:kind`              `th:label`             
`th:lang`              `th:list`              `th:longdesc`          
`th:low`               `th:manifest`          `th:marginheight`      
`th:marginwidth`       `th:max`               `th:maxlength`         
`th:media`             `th:method`            `th:min`               
`th:name`              `th:onabort`           `th:onafterprint`      
`th:onbeforeprint`     `th:onbeforeunload`    `th:onblur`            
`th:oncanplay`         `th:oncanplaythrough`  `th:onchange`          
`th:onclick`           `th:oncontextmenu`     `th:ondblclick`        
`th:ondrag`            `th:ondragend`         `th:ondragenter`       
`th:ondragleave`       `th:ondragover`        `th:ondragstart`       
`th:ondrop`            `th:ondurationchange`  `th:onemptied`         
`th:onended`           `th:onerror`           `th:onfocus`           
`th:onformchange`      `th:onforminput`       `th:onhashchange`      
`th:oninput`           `th:oninvalid`         `th:onkeydown`         
`th:onkeypress`        `th:onkeyup`           `th:onload`            
`th:onloadeddata`      `th:onloadedmetadata`  `th:onloadstart`       
`th:onmessage`         `th:onmousedown`       `th:onmousemove`       
`th:onmouseout`        `th:onmouseover`       `th:onmouseup`         
`th:onmousewheel`      `th:onoffline`         `th:ononline`          
`th:onpause`           `th:onplay`            `th:onplaying`         
`th:onpopstate`        `th:onprogress`        `th:onratechange`      
`th:onreadystatechange``th:onredo`            `th:onreset`           
`th:onresize`          `th:onscroll`          `th:onseeked`          
`th:onseeking`         `th:onselect`          `th:onshow`            
`th:onstalled`         `th:onstorage`         `th:onsubmit`          
`th:onsuspend`         `th:ontimeupdate`      `th:onundo`            
`th:onunload`          `th:onvolumechange`    `th:onwaiting`         
`th:optimum`           `th:pattern`           `th:placeholder`       
`th:poster`            `th:preload`           `th:radiogroup`        
`th:rel`               `th:rev`               `th:rows`              
`th:rowspan`           `th:rules`             `th:sandbox`           
`th:scheme`            `th:scope`             `th:scrolling`         
`th:size`              `th:sizes`             `th:span`              
`th:spellcheck`        `th:src`               `th:srclang`           
`th:standby`           `th:start`             `th:step`              
`th:style`             `th:summary`           `th:tabindex`          
`th:target`            `th:title`             `th:type`              
`th:usemap`            `th:value`             `th:valuetype`         
`th:vspace`            `th:width`             `th:wrap`              
`th:xmlbase`           `th:xmllang`           `th:xmlspace`          
---------------------- ---------------------- ----------------------
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
------------------- ------------------ ------------------
`th:async`          `th:autofocus`     `th:autoplay`      
`th:checked`        `th:controls`      `th:declare`       
`th:default`        `th:defer`         `th:disabled`      
`th:formnovalidate` `th:hidden`        `th:ismap`         
`th:loop`           `th:multiple`      `th:novalidate`    
`th:nowrap`         `th:open`          `th:pubdate`       
`th:readonly`       `th:required`      `th:reversed`      
`th:scoped`         `th:seamless`      `th:selected`      
------------------- ------------------ ------------------
</div>


## 默认属性处理器

提供 默认属性处理器（default attribute processor t），当标准方言没有提供的属性时，也可以设置其属性。比如：

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