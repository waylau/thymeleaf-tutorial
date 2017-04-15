# Thymeleaf 简介

## 什么是 Thymeleaf

Thymeleaf 是面向 Web 和独立环境的现代服务器端 Java 模板引擎，能够处理 HTML、XML、JavaScript、CSS 甚至纯文本。

Thymeleaf 的主要目标是提供一个优雅和高度可维护的创建模板的方式。 为了实现这一点，它建立在自然模板（ Natural Templates）的概念上，将其逻辑注入到模板文件中，不会影响模板被用作设计原型。 这改善了设计的沟通，弥合了设计和开发团队之间的差距。

Thymeleaf 的设计从一开始就遵从 Web 标准，特别是 HTML5，这样就能创建完全符合验证的模板。

## 处理模版

Thymeleaf 能处理以下 6 种类型的模版，我们称之为**模版模式（Template Mode）**:

 * HTML
 * XML
 * TEXT
 * JAVASCRIPT
 * CSS
 * RAW
 
 其中包含有两种标记模板模式（HTML和XML），三种文本模板模式（TEXT、JAVASCRIPT和CSS）和一个无操作模板模式（RAW）。
 
HTML 模板模式将允许任何类型的 HTML输入，包括HTML5、HTML4和XHTML。将不执行验证或对格式进行严格检查，这样就能尽可能的将模板代码/结构进行输出。

XML模板模式将允许XML输入。在这种情况下，代码预期格式是良好的——没有未关闭的标签，没有引用属性等，如果找到为符合格式要求，解析器将抛出异常。请注意，不会执行验证（针对DTD或XML架构）。

TEXT模板模式将允许对非标记性质的模板使用特殊语法。此类模板的示例可能是文本电子邮件或模板文档。请注意，HTML或XML模板也可以作为TEXT处理，在这种情况下，它们将不会被解析为标记，并且每个标签DOCTYPE、注释等将被视为纯文本。

JAVASCRIPT模板模式将允许在Thymeleaf应用程序中处理JavaScript文件。这意味着能够使用JavaScript文件中的模型数据与HTML文件中可以完成的方式相同，但可以使用特定于JavaScript的集成，例如专门的转义或自然脚本（natural scripting）。 JAVASCRIPT模板模式被认为是文本模式，因此使用与TEXT模板模式相同的特殊语法。

CSS模板模式将允许处理涉及Thymeleaf应用程序的CSS文件。与JAVASCRIPT模式类似，CSS模板模式也是文本模式，并使用TEXT模板模式下的特殊处理语法。

RAW模板模式根本不会处理模板。它用于将未经修改的资源（文件、URL响应等）插入正在处理的模板中。例如，HTML格式的外部不受控制的资源可以包含在应用程序模板中，安全地知道这些资源可能包含的任何Thymeleaf代码将不会被执行。

## 标准方言

Thymeleaf是一个非常可扩展的模板引擎，实际上它更像是一个模板引擎框架（template engine framework）），允许您定义和自定义您的模板。

将一些逻辑应用于标记工件（例如标签、某些文本、注释或只有占位符）的一个对象被称为处理器（processor）。方言（dialect）通常包括这些处理器的集合以及一些额外的工件。Thymeleaf 的核心库提供了一种称为标准方言（Standard Dialect）的方言，提供给用户开箱即用的功能。

当然，如果用户希望在利用库的高级功能的同时定义自己的处理逻辑，用户也可以创建自己的方言（甚至扩展标准的方言）。也可以将Thymeleaf配置为同时使用几种方言。

> 官方的 thymeleaf-spring3 和 thymeleaf-spring4 集成包都定义了一种称为“SpringStandard Dialect”的方言，与标准方言大致相同，但是对于 Spring 框架中的某些功能则更加友好，例如， 想通过使用Spring Expression Language 或 SpringEL 而不是 OGNL。所以如果你是一个Spring MVC用户，这里的所有东西都能够在你的Spring应用程序中使用。

标准方言的大多数处理器是属性处理器。这样，即使在处理之前，浏览器也可以正确地显示HTML模板文件，因为它们将简单地忽略其他属性。对比JSP，在浏览器中会直接显示的代码片断：

```html
<form:inputText name="userName" value="${user.name}" />
```

Thymeleaf 标准方言将允许我们实现与以下功能相同的功能：:

```html
<input type="text" name="userName" value="James Carrot" th:value="${user.name}" />
```
 
浏览器不仅可以正确显示这些信息，而且还可以（可选地）在浏览器中静态打开原型时显示的值（可选地）指定一个值属性（在这种情况下为“James Carrot”），将在模板处理期间由`${user.name}`的评估得到的值代替。

这有助于您的设计师和开发人员处理相同的模板文件，并减少将静态原型转换为工作模板文件所需的工作量。这样的功能是称为 _自然模板(Natural Templating)_的功能.