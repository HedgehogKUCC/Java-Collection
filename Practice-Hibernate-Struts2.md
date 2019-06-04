### MySQL

Create a new schema called ***bookstore***

```mysql
CREATE SCHEMA 'bookstore' DEFAULT CHARACTER SET utf8 ;
```

<br>

Create Table called ***users***

```mysql
CREATE TABLE 'bookstore'.'users' (
	
	'id' INT NOT NULL AUTO_INCREMENT,
	'loginName' VARCHAR(45) NULL,
	'loginPwd' VARCHAR(45) NULL,
	
PRIMARY KEY('id'))

ENGIN = InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

/src New Package called ***model***

/src/model New Class called ***Users***

```java
package model;

import java.io.Serializable;

public class Users implements Serializable {

	private Integer id;
	private String loginName;
	private String loginPwd;
	
	public Users() {}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getLoginName() {
		return loginName;
	}

	public void setLoginName(String loginName) {
		this.loginName = loginName;
	}

	public String getLoginPwd() {
		return loginPwd;
	}

	public void setLoginPwd(String loginPwd) {
		this.loginPwd = loginPwd;
	}
}
```

<br>

將 Users.java 點右鍵 New Hibernate XML Mapping file (hbm.xml)

產生出 `Users.hbm.xml`

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/6/3 ?????? 09:10:17 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
    <class name="model.Users" table="USERS" catalog="BOOKSTORE">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="native" />
        </id>
        <property name="loginName" type="java.lang.String">
            <column name="LOGINNAME" />
        </property>
        <property name="loginPwd" type="java.lang.String">
            <column name="LOGINPWD" />
        </property>
    </class>
</hibernate-mapping>
```

注意：

`catalog = "Mysql schema name"`

`<generator class = "assigned" 或 "native" />`

native : 資料表主鍵欄位為***自動增加***型態

assigned : 主鍵欄位***沒有***設定為自動增加

<br>

/src New Hibernate Configuration File (cfg.xml)

產生出 `hibernate.cfg.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
                                         "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
 <session-factory name="">
  <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
  <property name="hibernate.connection.password">11111111</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/bookstore</property>
  <property name="hibernate.connection.username">root</property>
  <mapping resource="model/Users.hbm.xml"/>
 </session-factory>
</hibernate-configuration>
```

<br>

/src New Package called ***DBCONN***

/src/DBCONN New Class called ***DBConn***

```java
package DBCONN;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class DBConn {
	
	public static Session getSession() {
		
		// 初始化，讀取組態檔 hibernate.cfg.xml
		Configuration config = new Configuration().configure();
		
		// 讀取並解析映射檔案 (Users.hbm.xml)，建立 SessionFactory
		SessionFactory sf = config.buildSessionFactory();
		
		// 開啟 Session
		Session se = sf.openSession();
		
		return se;
	}
	
	public static void main(String[] args) {
		
		System.out.print(DBConn.getSession());
	}
}
```

<br>

/src New Package called ***dao***

/src/dao New Class called ***UsersDAO***

<br>

> 新增

```java
package dao;

import org.hibernate.Session;
import org.hibernate.Transaction;

import DBCONN.DBConn;
import model.Users;

public class UsersDAO {
	
	public static void addUser(String name, String pwd) {
	
		// 建立持久化物件
		Users user = new Users();
		user.setLoginName(name);
		user.setLoginPwd(pwd);
	
		Session se = DBConn.getSession();
	
		Transaction tx = null;
	
		try {
			
			// 開始一個交易
			tx = se.beginTransaction();	
			
			// 持久化動作
			se.save(user);
			
			// 傳送交易
			tx.commit();
		
		} catch (Exception e) {
			
			if( tx != null) {
				
				// 交易返回
				tx.rollback();
			}
			e.printStackTrace();
			
		} finally {
			
			// 關閉 Session
			se.close();
		}
	}
	
	public static void main(String[] args) {
		UsersDAO.addUser("KUCC", "777");
	}
}
```

<br>

> 查詢

```java
	public static Users loadUser(int x) {
		
		Session se = DBConn.getSession();
		
		Users user = (Users)se.get(Users.class, new Integer(x));
		
		System.out.println(user.getLoginName()+"\t"+user.getLoginPwd());
		
		return user;
	}
	
	public static void main(String[] args) {
		UsersDAO.loadUser(1);
	}
```

<br>

> 更新

```java
	public static void updateUser(int x, String name) {
		
		Session se = DBConn.getSession();
		
		Users user = (Users)se.get(Users.class, new Integer(x));
		
		user.setLoginName(name);
		
		Transaction tx = null;
		
		try {
			
			tx = se.beginTransaction();
			
			se.update(user);
			
			tx.commit();
			
		} catch (Exception e) {
			
			if(tx != null) {
				
				tx.rollback();
				
			}
			
			e.printStackTrace();
			
		} finally {
			
			se.close();
		}
		
	}
	
	public static void main(String[] args) {
		UsersDAO.updateUser(1, "Hiber");
	}
```

<br>

> 刪除

```java
	public static void deleteUser(int x) {
		
		Session se = DBConn.getSession();
		
		Users user = (Users)se.get(Users.class, new Integer(x));
		
		Transaction tx = null;
		
		try {
			
			tx = se.beginTransaction();
			
			se.delete(user);
			
			tx.commit();
			
		} catch (Exception e) {
			
			if( tx != null) {
				
				tx.rollback();
				
			}
			e.printStackTrace();
			
		} finally {
			
			se.close();
		}
	}
	
	public static void main(String[] args) {
		UsersDAO.deleteUser(1);
	}
```

<br>

/WebContent New JSP File

`index.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>首頁</title>
</head>
<body>

<table width="300" align="center" border="1">

	<tr>
		<td align="center">
			<a href="add.jsp">新增</a> <br>
			<a href="query.jsp">查詢</a> <br>
			<a href="update.jsp">更新</a> <br>
			<a href="delete.jsp">刪除</a>
		</td>
	</tr>
	
</table>

</body>
</html>
```

<br>

`add.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>新增頁面</title>
</head>
<body>

<s:form method="post" action="add">

	<table width="500" align="center" border="1">
	
		<tr>
			<td colspan="2" align="center">新增資料
		<tr>
			<s:textfield name="loginName" label="帳號" />
		<tr>
			<s:password name="loginPwd" label="密碼" />
		<tr>
			<s:submit value="送出" />
	
	</table>

</s:form>

</body>
</html>
```

<br>

`welcome.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>WELCOME</title>
<%
	String name = request.getParameter("loginName");
%>
</head>
<body>
用戶 <%=name %>, Welcome <br>
<a href="index.jsp">首頁</a>
</body>
</html>
```

<br>

Generator Deployment Descriptor

會在 /WebContent/WEB-INF 裡面產生 

`web.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
  <display-name>PraHibStru</display-name>
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

將 `index.jsp` Run On Server，測試看看是否能成功。

<br>

/src New Package called ***action***

/src/action New Class

`add.java`

```java
package action;

import com.opensymphony.xwork2.ActionSupport;

import dao.UsersDAO;

public class add extends ActionSupport {
	
	private String loginName;
	private String loginPwd;
	
	public String execute() throws Exception {
		
		UsersDAO.addUser(loginName, loginPwd);
		
		return SUCCESS;
	}
	
	public String getLoginName() {
		return loginName;
	}
	public void setLoginName(String loginName) {
		this.loginName = loginName;
	}
	public String getLoginPwd() {
		return loginPwd;
	}
	public void setLoginPwd(String loginPwd) {
		this.loginPwd = loginPwd;
	}
}
```

<br>

/src New XML File called ***struts***	&nbsp; &nbsp; &nbsp; (一定要命名為 struts)

`struts.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
	"http://struts.apache.org/dtds/struts-2.5.dtd">
	
<struts>

	<package name="default" extends="struts-default">
	
		<action name="add" class="action.add">
		
			<result name="success">welcome.jsp</result>
		
		</action>
	
	</package>

</struts>
```

<br>

重新將 `index.jsp` Run On Server

測試 `新增` 功能是否正常執行至 `welcome.jsp`頁面

<br>

/WebContet New JSP File

`query.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢頁面</title>
</head>
<body>

<s:form method="post" action="query">

	<s:textfield name="id" label="ID" />
	<s:submit value ="確認" />

</s:form>

</body>
</html>
```

<br>

`queryWelcome.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>WELCOME</title>
</head>
<body>

用戶帳號：${user.getLoginName()} <br>
用戶密碼：${user.getLoginPwd()} <br>
<a href="index.jsp">首頁</a>

</body>
</html>
```

<br>

/src/action New Class

`query.java`

```java
package action;

import java.util.Map;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

import dao.UsersDAO;
import model.Users;

public class query extends ActionSupport {
	
	private Integer id;
	
	public String execute() throws Exception {
		
		Users user = UsersDAO.loadUser(id);
		
		/*
		Map session = ActionContext.getContext().getSession();
		
		session.put("user", user);
		*/
		
		ActionContext.getContext().getSession().put("user", user);
		
		return SUCCESS;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}
}
```

<br>

`struts.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
	"http://struts.apache.org/dtds/struts-2.5.dtd">
	
<struts>

	<package name="default" extends="struts-default">
	
		<action name="add" class="action.add">
		
			<result name="success">welcome.jsp</result>
		
		</action>
		
		<action name="query" class="action.query">
		
			<result name="success">queryWelcome.jsp</result>
		
		</action>
	
	</package>

</struts>
```

<br>

