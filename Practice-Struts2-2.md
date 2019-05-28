New Dynamic Web Project called ***struts2***

<h3>

掛好 Server Tomcat 8.5 後

請先在 /WEB-INF New JSP File 來做測試

看能不能 Run on Server 成功

</h3>

<br>

掛進 struts2.jar 至 /WebContent/WEB-INF/lib 裡面後

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

再 Run on Server 看看能不能 成功打開網頁

<br>

/src New Package called ***org.action***

/src/org/action New Class called ***LoginAction***

```java
package org.action;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class LoginAction extends ActionSupport{

	private String username;
	private String password;
	
	public String execute() throws Exception {
		
		if(getUsername().equals("aaa")
			&& getPassword().equals("111")) 
		{
				// 將 getUsername() 的資料 放入 session 取名為 user
				ActionContext.getContext().getSession()
					.put("user", getUsername());
				
				// 將 getPassword() 的資料 放入 session 取名為 pass
				ActionContext.getContext().getSession()
					.put("pass", getPassword());
				
				return SUCCESS;
		}
		else
		{
			return ERROR;
		}
	}
	
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}
```

<br>

/src New XML File called ***struts*** (一定要命名為 struts)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
	"http://struts.apache.org/dtds/struts-2.5.dtd">
	
<struts>

	<package name="default" namespace="/" extends="struts-default">
	
		<action name="login" class="org.action.LoginAction">
		
			<result name="success">/welcome.jsp</result>
			<result name="error">/error.jsp</result>
		
		</action>
		
	</package>

</struts>
```

<br>

/WebContent New JSP File called ***loginForm*** , ***welcome*** , ***error***

`loginForm.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>LoginForm</title>
</head>
<body>

<!-- 樣式一 -->

LoginForm
<s:form action="login">
	<s:textfield name="username" key="輸入帳號" />
	<s:textfield name="password" key="輸入密碼" /> <!-- s:textfield 輸入時會顯示出來 -->
	<!-- <s:password name="password" key="輸入密碼" /> 改成 s:password 輸入時就變成 ****** -->
	<s:submit value="提交" />
</s:form>

<!-- 樣式二 -->

<h1 align="center">LoginForm</h1>

<s:form action="login" theme="simple">
	<table width="300" align="center" border="1">
		<tr align="center">
			<td>帳號<s:textfield name="username"  />
		<tr align="center">
			<td>密碼<s:password name="password"  />
		<tr align="center">
			<td>
				<s:submit value="提交" />
				<s:reset value="重填" />
			</td>
	</table>
</s:form>

</body>
</html>
```

<br>

`welcome.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Welcome</title>
</head>
<body>

恭喜成功登錄<br>
帳號: ${sessionScope.user } <br>  <!-- sessionScope.user 對應到 LoginAction 中 put("user", getUsername())  -->
密碼: ${sessionScope.pass } <br>  <!-- sessionScope.pass 對應到 LoginAction 中 put("pass", getPassword())  -->
<a href="loginForm.jsp">首頁</a>

</body>
</html>
```

<br>

`error.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Error</title>
</head>
<body>

帳號密碼錯誤<br>
<a href="loginForm.jsp">首頁</a>

</body>
</html>
```
