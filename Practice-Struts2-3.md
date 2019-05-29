### MySQL

Create a new schema called ***gjun***

```mysql
CREATE SCHEMA 'gjun' DEFAULT CHARACTER utf8;
```

<br>

Create Table called ***login***

```mysql
CREATE TABLE 'gjun'.'login' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'name' VARCHAR(45) NULL,
	'password' VARCHAR(45) NULL,
	
PRIMARY KEY('id'))

ENGIN = InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

自己先填入`帳號` `密碼`

```mysql
INSERT INTO 'gjun'.'login'('name','password') VALUES('abc','1234');
INSERT INTO 'gjun'.'login'('name','password') VALUES('aaa','1111');
INSERT INTO 'gjun'.'login'('name','password') VALUES('qqq','2222');
INSERT INTO 'gjun','login'('name','password') VALUES('eee','3333');
```

<br>

New Dynamic Web Project called ***struts3***

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

/src New Package called ***org.model***

/src/org/model New Class called ***Login***

```java
package org.model;

public class Login {

	private String name;
	private String password;
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
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

/src New Package called ***org.DBCONN***

/src/org/DBCONN New Class called ***DBconn***

先測試有沒有抓到 Driver

```java
package org.DBCONN;

import java.sql.Connection;

public class DBconn {

	public static Connection getConn() {
		
		try {
			Class.forName("com.mysql.cj.jdbc.Driver"); // 較新的版本才要加 cj
			System.out.println("SUCCESS");
		} catch (ClassNotFoundException e) {
			System.out.println("No Driver");
		}
		
		return null;
	}
	
	public static void main(String[] args) 
	{
		DBconn.getConn();
	}
}
```

`Run As` `Java Application` 顯示出 ***No Driver***

請把 `mysql-connector-java-8.0.13.jar` 掛進 /WebContent/WEB-INF/lib 裡面

重新 `Run As` `Java Application` 顯示出 ***SUCCESS***

<br>

測試有沒有連上資料庫

```java
package org.DBCONN;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBconn {
	
	public static Connection conn;

	public static Connection getConn() {
		
		String url = "jdbc:mysql://localhost:3306/gjun";
		String user = "root";
		String password = "11111111";
		
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			conn = DriverManager.getConnection(url, user, password);
			System.out.println("SUCCESS");
			return conn;
			
		} catch (ClassNotFoundException e) {
			System.out.println("No Driver");
			return null;
			
		} catch (SQLException e1) {
			System.out.println("No Connection");
			return null;
		}	
	}
	
	public static void CloseConn() {
		
		try {
			conn.close();
		} catch (SQLException e) {
			System.out.println("No Close");
		}
	}
	
	public static void main(String[] args) {
	
		DBconn.getConn();
	}
}
```

<br>

/src New Package called ***org.DAO***

/src/org/DAO New Class called ***LoginDAO***

```java
package org.DAO;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.DBCONN.DBconn;
import org.model.Login;

public class LoginDAO {

	Connection conn;
	
	public Login checkLogin(String name, String password) {
		
		try {
			conn = DBconn.getConn();
			PreparedStatement pstmt = conn.prepareStatement("select * from login where name=? and password=?");
			pstmt.setString(1, name);
			pstmt.setString(2, password);
			ResultSet rs = pstmt.executeQuery();
			
			if(rs.next()) {
				
				Login login = new Login();
				login.setName(rs.getString("name"));           // 可以將 2 替代 name
				login.setPassword(rs.getString("password"));   // 可以將 3 替代 password
				System.out.println("name : " + login.getName() + "\tpassword : " + login.getPassword());
				return login;
			}
			else
			{
				System.out.println("No Data");
				return null;
			}
		} catch (SQLException e) {
			e.printStackTrace();
			return null;
		}
		finally
		{
			DBconn.CloseConn();
		}
	}
	
	public static void main(String[] args) {
		
		// 測試有沒有這筆資料，如果有就會在Console印出來，沒有就會印出 No Data。
		LoginDAO l = new LoginDAO();
		l.checkLogin("aaa", "111");
	}
}
```

<br>

/src New Package called ***org.Action***

/src/org/Action New Class called ***LoginAction***

```java
package org.Action;

import org.DAO.LoginDAO;
import org.model.Login;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class LoginAction extends ActionSupport {

	private Login login;
	
	public String execute() throws Exception {
		
		LoginDAO logindao = new LoginDAO();
		
		Login l = logindao.checkLogin(login.getName(), login.getPassword());
		
		if(l != null)
		{
			ActionContext.getContext().getSession().put("Login", l);
			return SUCCESS;
		}
		else
		{
			return ERROR;
		}
	}

	public Login getLogin() {
		return login;
	}

	public void setLogin(Login login) {
		this.login = login;
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

	<package name="default" extends="struts-default">
	
		<action name="login" class="org.Action.LoginAction>
		
			<result name="success">main.jsp</result>
			<result name="error">error.jsp</result>
		
		</action>
	
	</package>

</struts>
```

<br>

/WebContent New JSP File called ***login*** , ***main*** , ***error***

`login.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>會員管理系統</title>
</head>
<body bgcolor="#5CB3FF">

<s:form action="login" method="post" theme="simple">

	<table align="center">
		<caption>會員登入</caption>
		<tr>
			<td>帳號<s:textfield name="login.name" size="20" />
			<!-- login.name 對應到 LoginAction 中 private Login login; -->
		<tr>
			<td>密碼<s:password name="login.password" size="20" />
			<!-- login.name 對應到 LoginAction 中 private Login login; -->
		<tr>
			<td>
				<s:submit value="登入" />
				<s:reset value="重填" />
			</td>
	</table>

</s:form>

</body>
</html>
```

<br>

`main.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>歡迎</title>
</head>
<body>

	<s:set var="login" value="#session['Login']" />
	<s:property value="#login.name" />，您好！恭喜登入成功
	
	<br>
	
	${Login.getName() } ，您好！恭喜登入成功	

</body>
</html>
```

<br>

`error.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Error</title>
</head>
<body>
無此帳號，請點<a href="login.jsp">這裡</a>回首頁
</body>
</html>
```
