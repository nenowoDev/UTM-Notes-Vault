- server side Java template engine
	- process 
		- HTML
		- XML
		- JS
		- CSS
		- PLAIN TEXT
- generate content dynamically

Recap
Spring MVC
- Browser
- Handler Mapping
- Controller 
- View resolver
- View engine(Thymeleaf/JSP)



| JSP                     | Thymeleaf             |
| ----------------------- | --------------------- |
| Hard for frontend dev   | Easy for frontend dev |
| Specific file extension | HTML file format      |


SETUP

pom.xml (dependencies)
- format
```xml
<!-- Thymeleaf dependency -->
<dependency>
	<groupId>  org.thymeleaf </groupId>
	<artifactId> thymeleaf   </artifactId>
	<version> 3.1.1.RELEASE   </version>
</dependency>

<!-- Thymeleaf spring integration -->
<dependency>
	<groupId>  org.thymeleaf </groupId>
	<artifactId> thymeleaf-spring5   </artifactId>
	<version> 3.1.1.RELEASE   </version>
</dependency>
```


servlet.xml (spring config)
- format
```xml
<bean
id="templateResolver"
class="org.thymeleaf.spring5.SpringResourceTemplateResolver">
	<property name="prefix" value="WEB-INF/templates/" >
	<property name="suffix" value=".html" >
	<property name="templateMode" value="HTML" >
	<property name="characterEncoding" value="UTF-8" >
</bean>


<bean
id="templateEngine"
class="org.thymeleaf.spring5.SpringTemplateEngine">
	<property name="templateResolver" ref="templateResolver">
</bean>


<bean
class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
	<property name="templateEngine"  ref="templateEngine">
	<property name="characterEncoding" value="UTF-8">
</bean>
```


web.xml(dispatcher)
- format
```xml
<web-app
xmlns               = "http://java.sun.com/xml/ns/javaee"
xmnls:xsi           = "http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation  = "http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
version             = "3.0"
>


<!-- DispatcherServlet Config-->
<servlet>
	<servlet-name   > mySpring </servlet-name>
	<servlet-class  > org.springframework.web.servlet.DispatcherServlet </servlet-class>
	<load-on-startup> 1 <load-on-startup>
</servlet>

<!-- URL Pattern mapping -->
<servlet-mapping>
	<servlet-name>mySpring</servlet-name>
	<url-pattern>  /  </url-pattern>
</servlet-mapping>

```



Thymeleaf standard Expression
```js

// btw only ${} can be use in content ,, other expressions are tag-only

${..} = variable expression
// Basically a variable
// Example
<p> ${name} </p>
<p> ${person.name}  </p>
<p> ${name=='Amir'?'Amir' : NoName }</p>
// OUTPUT
Amir
Amir
Amir







*{..} = selection expression
// NEED TO SELECT Object first, else error
// can only be use as tag property,, it will replace the content of the tag
// Example 
<p th:object="${person}">
	<span th:text=*{name}> Default name</span>
</p>
// It is the same as ${person.name}
//OUTPUT
Amir







#{..} = message expression
// go into a .properties file and grab appropriate msg
// example in menu.properties
// menu.welcome = "Hi"
// can only be use as tag property,, it will replace the content of the tag
<span th:text=#{menu.welcome}> Default name</span>
//OUTPUT
Hi








@{..} = URL expression
//Root Relative,, from root
<a th:href="@{/orders}"></a>
//OUTPUT
<a href="locahost/orders}"></a>

//Page Relative
<a th:href="@{../orders}"></a>
<a th:href="@{orders/list}"></a>
//OUTPUT
<a href="../orders"></a>
<a href="orders/list"></a>

//Protocol Relative & Absolute
<a th:href="@{//google.com}"></a>
<a th:href="@{https://google.com}"></a>
//OUTPUT
<a href="//google.com"></a>
<a href="https://google.com"></a>


//PARAMETER google.com/orders?a=x&b=y
@{google.com/orders(a=${x},b=${y})}
@{google.com/orders?a=${x}..}
@{'google.com/orders?a='+${x}+'..'}

//Path var google.com/orders/list
@{google.com/{a}/{b}(a=orders,b=list)}
..
..

//just rmmbr important the first example for parametr/path var





~{..} = fragment expression
// replace content of div , to the content of menu.html
<div th:insert=~{/menu.html}>dont really matter</div>
// replace div tag with menu.html
<div th:replace=~{/menu.html}>dont really matter</div>
// replace content of div to fragment named sidebar of menu.html
<div th:insert"~menu.html :: sidebar> wakawaka </div>



```


Thymeleaf utility object
```js

${#dates.format(date,'dd/mm/yyyy')}

${#numbers.formatDecimal(123.223,2)}

${#strings.capitalize('adf')}

	${#lists.size(list)}

```


Thymeleaf Form
```html

<!-- USING @RequestParam -->

<form th:action="@{/submit}" th:method="post">
	<input type="number" name="id"  />
	<input type="number"  name="name" />

	<input type="submit" />
</form>


<!-- USING @ModelAttribute OBJECT BINDING-->

<form th:action="@{/submit}" th:method="post">
	<input type="number" th:field="*{id}"  />
	<input type="number"  th:field="*{name}" />

	<input type="submit" />
</form>





```

Thymeleaf flow control
```html

<ul th:each="person: ${personlist}" th:object="${person}">
	<li th:text="*{name}"></li>
	<li th:text="*{age}"></li>
	<li th:if="*{mark>=9}">Ok</li>
	<!--UNLESS IS BASICALLY NOT IF,, aka opposite of if , theres no else and else if in thymeleaf, use case  instead-->
	<li th:unless="*{mark>=9}">Not ok</li>
</ul>

```


```html
<!--USE ${},, dont use *{} OK??!!-->
<ul  th:switch="${name}">
	<li th:case="'Amir'">Amir</li>
	<li th:case="'Amar'">Amar</li>
	<li th:case="*">match all</li>
</ul>

```