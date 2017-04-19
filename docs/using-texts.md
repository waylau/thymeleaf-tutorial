# 使用文本

先看一个页面示例：


```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all" 
          href="../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>
  
    <p th:text="#{home.welcome}">Welcome to our grocery store!</p>
  
  </body>

</html>
```

首先，注意到的这个文件是HTML5的，可以由任何浏览器正确显示，因为它不包含任何非HTML标签（浏览器会忽略他们所不能理解的属性，如 `th:text`）。

但是您也可能会注意到，这个模板并不是一个真正有效的HTML5文档，因为HTML5规范不允许在`th:*`形式中使用这些非标准属性。 事实上，我们甚至在我们的 `<html>` 标签中添加了一个`xmlns:th` 属性，这也不属于 HTML5 语言：

```html
<html xmlns:th="http://www.thymeleaf.org">
```

它在模板处理中根本没有任何影响，但我们的 IDE 会提示诸如“缺少`th:*`属性命名空间定义”等字样的告警。

如果我们想让这些模板对于 HTML5 验证是有效的，需要做简单地修改，将属性语法，改为`data-`前缀，(`:`)改为 (`-`) 即可：

```html
<!DOCTYPE html>

<html>

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all" 
          href="../../css/gtvg.css" data-th-href="@{/css/gtvg.css}" />
  </head>

  <body>
  
    <p data-th-text="#{home.welcome}">Welcome to our grocery store!</p>
  
  </body>

</html>
```

这样，`data-`前缀就符合 HTML5 中的规范了。

*注：在实际开发过程中，由于`th:*`的辨识度往往必`data-th-*`这种方式更高，所以很多人仍然是选择使用`th:*`，这也是可以理解的。本书的示例大多也是采用`th:*`方式。*


## 使用 th:text 和外部化文本

外部化文本是从模板文件中提取模板代码的片段，以便它们可以保存在单独的文件（通常为 `.properties` 文件）中，并且可以轻松地替换为使用其他语言编写的等效文本（称为国际化或简单的i18n） 。文本的外部化片段通常称为“消息（messages）”。

消息总是具有标识它们的 key，而Thymeleaf允许您指定文本应与 `#{...}`语法对应的特定消息：


```html
<p th:text="#{home.welcome}">Welcome to our grocery store!</p>
```

我们在这里可以看到的其实是Thymeleaf标准方言的两个不同功能：

* `th:text`属性，它评估其值表达式并将结果设置为主机标签的主体，有效地替换了代码中我们看到的“Welcome to our grocery store!”文本。
* `#{home.welcome}`表达式指示 `th:text` 属性使用的文本应该是key 为`home.welcome` 所对应于的消息。


Thymeleaf中外部化文本的位置是完全可配置的，它将取决于正在使用的具体的 `org.thymeleaf.messageresolver.IMessageResolver`实现。通常，将使用基于 `.properties`文件的实现，但是如果我们想要（例如）从数据库获取消息，我们可以创建自己的实现。

但是，我们在初始化期间尚未为模板引擎指定消息解析器，这意味着我们的应用程序正在使用由`org.thymeleaf.messageresolver.StandardMessageResolver`实现的标准消息解析器。

标准消息解析器期望在 `/WEB-INF/templates/home.html`中找到与该模板相同的文件夹中的属性文件的消息，例如：

* `/WEB-INF/templates/home_en.properties` 为英文文本
* `/WEB-INF/templates/home_es.properties` 西班牙语文本
* `/WEB-INF/templates/home_pt_BR.properties`葡萄牙语（巴西）语言
* `/WEB-INF/templates/home.properties` 为默认文本（如果语言环境不匹配）
   
 
我们来看看我们的 `home_es.properties` 文件:

```
home.welcome=¡Bienvenido a nuestra tienda de comestibles!
```

## 上下文

为了执行模版，我们创建了`HomeController`类，它实现了 `IGTVGController` 接口:

```java
public class HomeController implements IGTVGController {

    public void process(
            final HttpServletRequest request, final HttpServletResponse response,
            final ServletContext servletContext, final ITemplateEngine templateEngine)
            throws Exception {
        
        WebContext ctx = 
                new WebContext(request, response, servletContext, request.getLocale());
        
        templateEngine.process("home", ctx, response.getWriter());
        
    }

}
```

首先是 *上下文（context）*的创建。 Thymeleaf 上下文对象实现了 `org.thymeleaf.context.IContext`接口。上下文包含了执行模版引擎的所有数据变量 map，同时也引用了外部消息的区域设置。

```java
public interface IContext {

    public Locale getLocale();
    public boolean containsVariable(final String name);
    public Set<String> getVariableNames();
    public Object getVariable(final String name);
    
}
```

这个接口有一个专门的扩展，`org.thymeleaf.context.IWebContext`，用于基于ServletAPI的Web应用程序（如SpringMVC）中。



```java
public interface IWebContext extends IContext {
    
    public HttpServletRequest getRequest();
    public HttpServletResponse getResponse();
    public HttpSession getSession();
    public ServletContext getServletContext();
    
}
```

Thymeleaf 核心库提供了这些接口的每个实现:

* `org.thymeleaf.context.Context` 实现了 `IContext`
* `org.thymeleaf.context.WebContext` 实现了 `IWebContext`
 
而在控制器代码中可以看到，`WebContext`是我们使用的。 实际上我们必须，因为使用一个`ServletContextTemplateResolver`要求我们使用实现  `IWebContext` 的上下文。

```java
WebContext ctx = new WebContext(request, response, servletContext, request.getLocale());
```

只需要这四个构造函数参数中的三个，因为如果没有指定，那么将使用系统的默认语言环境（尽管不应该在实际应用程序中发生）。

有一些专门的表达式，能够从我们的模板中的 `WebContext` 获取请求参数和请求、会话和应用程序属性。 例如：

* `${x}` 将返回存储在Thymeleaf上下文中的变量 `x` 或作为 *请求属性（request attribute）*
* `${param.x}` 将返回一个名为 `x`的*请求参数（request parameter）*（可能是多值的）
* `${session.x}` 将返回一个名为 `x`的 *会话属性（session attribute）*
* `${application.x}` 将返回一个名为`x`的*servlet上下文属性 （servlet context attribute）* 

## 执行模版引擎

随着我们的上下文对象准备就绪，现在我们可以告诉模板引擎使用上下文来处理模板（通过它的名字），并传递一个响应写入器，以便可以将响应写入它：

```java
templateEngine.process("home", ctx, response.getWriter());
```

在西班牙环境下，输出如下 :

```html
<!DOCTYPE html>

<html>

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta content="text/html; charset=UTF-8" http-equiv="Content-Type"/>
    <link rel="stylesheet" type="text/css" media="all" href="/gtvg/css/gtvg.css" />
  </head>

  <body>
  
    <p>¡Bienvenido a nuestra tienda de comestibles!</p>

  </body>

</html>
```

## 非转义文本

如果我们的消息中包含特殊字符，比如html标签，如下


```java
home.welcome=Welcome to our <b>fantastic</b> grocery store!
```

那么，执行模版，将会得到如下输出:

```html
<p>Welcome to our &lt;b&gt;fantastic&lt;/b&gt; grocery store!</p>
```

这是`th:text` 属性的默认行为。 但我们希望 Thymeleaf 能如实输出HTML标签，我们可以使用`th:utext`，即“unescaped text（非转义文本)”：


```html
<p th:utext="#{home.welcome}">Welcome to our grocery store!</p>
```

这样就能输出符合我们预期的文本了:

```html
<p>Welcome to our <b>fantastic</b> grocery store!</p>
```

## 使用和显示变量

我们控制器如下：

```java
public void process(
            final HttpServletRequest request, final HttpServletResponse response,
            final ServletContext servletContext, final ITemplateEngine templateEngine)
            throws Exception {
        
    SimpleDateFormat dateFormat = new SimpleDateFormat("dd MMMM yyyy");
    Calendar cal = Calendar.getInstance();
        
    WebContext ctx = 
            new WebContext(request, response, servletContext, request.getLocale());
    ctx.setVariable("today", dateFormat.format(cal.getTime()));
        
    templateEngine.process("home", ctx, response.getWriter());
        
}
```

添加了一个名为  `today` 的`String` 变量到我们的上下文中，我们在模版中显示:

```html
<body>

  <p th:utext="#{home.welcome}">Welcome to our grocery store!</p>

  <p>Today is: <span th:text="${today}">13 February 2011</span></p>
  
</body>
```


正如你所看到的，我们仍然使用`th:text`属性，但是这个时候语法有点不同，不是  `#{...}`，我们使用 `${...}`。 这是一个变量表达式，它包含一个名为OGNL（Object-Graph Navigation Language）的表达式。

 `${today}` 表达式只是意味着“获取名为 today 的变量”，但是这些表达式可能可以更复杂（例如 `${user.name}` 表示变量为  user的变量，调用其 `getName()` 方法。

