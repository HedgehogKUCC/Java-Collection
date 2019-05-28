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






