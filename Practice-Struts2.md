New Dynamic Web Project celled ***struts1***

<h3>

掛好 Server Tomcat 8.5 後

請先在 /WEB-INF New JSP File 來做測試

看能不能 Run on Server 成功

</h3>

<br>

掛進 .jar 至 /WebContent/WEB-INF/lib 裡面後

Generate Deployment Descriptor Stub

就會在 /WEB-INF 裡面產生 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
  <display-name>struts1</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  <filter>
  	<filter-name>struts2</filter-name>
  	<filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
  </filter>
  
  <filter-mapping>
  	<filter-name>struts2</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
 
</web-app>
```

新版路徑 `org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter`

舊版路徑 `org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter`

想確定的話把 `struts2-core-2.5.14.jar` 打開

再 Run on Server 看看能不能 成功打開網頁

成功： 代表`<filter><filter-mapping>`其中內容沒有字打錯，web.xml 也有找到 `struts2-core-2.5.14.jar`

<br>

/WebContent New JSP File called ***index*** , ***hello***

`index.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Index</title>
</head>
<body>

<a href="hello" target=_blank>hello.jsp</a>
<!-- href="hello" 對應到 struts.xml 中 <action name="hello"> -->

</body>
</html>
```

<br>

`hello.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Hello</title>
</head>
<body>

<h1>Hello Struts2</h1>

</body>
</html>
```

<br>

/src New Package called ***com.helloworld.Action***

/src/com/helloworld/Action New Class called ***MyAction***

```java
package com.helloworld.Action;

import com.opensymphony.xwork2.ActionSupport;

public class MyAction extends ActionSupport {

	public String execute() throws Exception {
		
		return SUCCESS;
	}
}
```

<br>

/src New XML File called ***struts*** (一定要命名為 struts)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
	"http://struts.apache.org/dtds/struts-2.0.dtd">
	
<struts>

	<package name="default" namespace="/" extends="struts-default">
	
		<action name="hello">
		
			<result name="success">
			
				/hello.jsp
			
			</result>
		
		</action>
	
	</package>

</struts>
```

