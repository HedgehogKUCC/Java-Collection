# Struts2 + Hibernate

### MySQL

`Create a Schema called "school"`

```mysql
CREATE SCHEMA 'school' DEFAULT CHARACTER SET utf8;
```

<br>

`Create a Table called "login"`

```mysql
CREATE TABLE 'school'.'login' (
	
	'id' INT NULL AUTO_INCREMENT,
	'name' VARCHAR(45) NULL,
	'password' VARCHAR(45) NULL,
	'role' TINYINT NULL,

PRIMARY KEY('id') )

ENGINE = InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

`Create a Table called "book"`

```mysql
CREATE TABLE 'school'.'book' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'ISBN' VARCHAR(45) NULL,
	'bookName' VARCHAR(45) NULL,
	'author' VARCHAR(45) NULL,
	'publisher' VARCHAR(45) NULL,
	'price' FLOAT NULL,
	'cnum' INT NULL,
	'snum' INT NULL,
	'summary' VARCHAR(45) NULL,

PRIMARY KEY('id') )

ENGINE = InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

`Create a Table called "student"`

```mysql
CREATE TABLE 'school'.'student' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'readerID' VARCHAR(45) NULL,
	'name' VARCHAR(45) NULL,
	'spec' VARCHAR(45) NULL,
	'sex' TINYINT NULL,
	'born' DATE NULL,
	'num' INT NULL,
	'snum' INT NULL,
	
PRIMARY KEY('id') )

ENGINE InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

`Create a Table called "len"`

```mysql
CREATE TABLE 'school'.'len' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'bookID' INT NULL,
	'readID' INT NULL,
	'bookName' VARCHAR(45) NULL,
	'publisher' VARCHAR(45) NULL,
	'price' FLOAT NULL,
	'ISBN' VARCHAR(45) NULL,
	'lTime' DATE NULL,

PRIMARY KEY('id') )

ENGINE = InooDB

DEFAULT CHARACTER SET = utf8;
```

<br>

New Dynamic Web Project called ***BookManage***

/src New Package called

***org.model*** , 需要的 class ( 與 資料庫中 Table 有關 )

***org.dao*** , 管理資料庫的 method

***org.action*** , 管理畫面的操作流程 (Control) 與 View 有關

***org.DBconn*** , 連線功能

***org.tool*** , 額外的功能 class

/src New Class called ***Login*** , ***Book*** , ***Student*** , ***Len***

```java
package org.model;

import java.io.Serializable;

public class Login implements Serializable {

	private Integer id;
	private String name;
	private String password;
	private boolean role;
	
	// Getters / Setters
```

```java
package org.model;

import java.io.Serializable;

public class Book implements Serializable {

	private Integer id;
	private String ISBN;
	private String bookName;
	private String author;
	private String publisher;
	private float price;
	private int cnum;
	private int snum;
	private String summary;
	
	// Getters / Setters
```

```java
package org.model;

import java.io.Serializable;
import java.util.Date;

public class Student implements Serializable {

	private Integer id;
	private String readerId;
	private String name;
	private String spec;
	private boolean sex;
	private Date born;
	private int num;
	private int snum;
	
	// Getters / Setters
```

```java
package org.model;

import java.io.Serializable;
import java.util.Date;

public class Lend implements Serializable {

	private Integer id;
	private String bookId;
	private String readerId;
	private String bookName;
	private String publisher;
	private float price;
	private String ISBN;
	private Date lTime;
	
	// Getters / Setters
```

<br>

### Hibernate XML Mapping file (hbm.xml)

`修改 主索引 assigned 變成 native`

`加上 屬性 catalog="SCHOOL" 對應到資料庫的 Schema`

<br>

Login.hbm.xml

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/5/12 ?????? 11:28:56 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
	<class name="org.model.Login" table="LOGIN" catalog="SCHOOL">
		<id name="id" type="java.lang.Integer">
			<column name="ID" />
			<generator class="native" />
		</id>
		<property name="name" type="java.lang.String">
			<column name="NAME" />
		</property>
		<property name="password" type="java.lang.String">
			<column name="PASSWORD" />
		</property>
		<property name="role" type="boolean">
			<column name="ROLE" />
		</property>
	</class>
</hibernate-mapping>
```

<br>

Book.hbm.xml

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/5/12 ?????? 11:25:41 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
	<class name="org.model.Book" table="BOOK" catalog="SCHOOL">
		<id name="id" type="java.lang.Integer">
			<column name="ID" />
			<generator class="native" />
		</id>
		<property name="ISBN" type="java.lang.String">
			<column name="ISBN" />
		</property>
		<property name="bookName" type="java.lang.String">
			<column name="BOOKNAME" />
		</property>
		<property name="author" type="java.lang.String">
			<column name="AUTHOR" />
		</property>
		<property name="publisher" type="java.lang.String">
			<column name="PUBLISHER" />
		</property>
		<property name="price" type="float">
			<column name="PRICE" />
		</property>
		<property name="cnum" type="int">
			<column name="CNUM" />
		</property>
		<property name="snum" type="int">
			<column name="SNUM" />
		</property>
		<property name="summary" type="java.lang.String">
			<column name="SUMMARY" />
		</property>
	</class>
</hibernate-mapping>
```

<br>

Student.hbm.xml

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/5/12 ?????? 11:29:31 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
	<class name="org.model.Student" table="STUDENT" catalog="SCHOOL">
		<id name="id" type="java.lang.Integer">
			<column name="ID" />
			<generator class="native" />
		</id>
		<property name="readerId" type="java.lang.String">
			<column name="READERID" />
		</property>
		<property name="name" type="java.lang.String">
			<column name="NAME" />
		</property>
		<property name="spec" type="java.lang.String">
			<column name="SPEC" />
		</property>
		<property name="sex" type="boolean">
			<column name="SEX" />
		</property>
		<property name="born" type="java.util.Date">
			<column name="BORN" />
		</property>
		<property name="num" type="int">
			<column name="NUM" />
		</property>
		<property name="snum" type="int">
			<column name="SNUM" />
		</property>
	</class>
</hibernate-mapping>
```

<br>

Lend.hbm.xml

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/5/12 ?????? 11:28:27 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
	<class name="org.model.Lend" table="LEND" catalog="SCHOOL">
		<id name="id" type="java.lang.Integer">
			<column name="ID" />
			<generator class="native" />
		</id>
		<property name="bookId" type="java.lang.String">
			<column name="BOOKID" />
		</property>
		<property name="readerId" type="java.lang.String">
			<column name="READERID" />
		</property>
		<property name="bookName" type="java.lang.String">
			<column name="BOOKNAME" />
		</property>
		<property name="publisher" type="java.lang.String">
			<column name="PUBLISHER" />
		</property>
		<property name="price" type="float">
			<column name="PRICE" />
		</property>
		<property name="ISBN" type="java.lang.String">
			<column name="ISBN" />
		</property>
		<property name="lTime" type="java.util.Date">
			<column name="LTIME" />
		</property>
	</class>
</hibernate-mapping>
```

<br>

### Generate /src/hibernate.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory name="">
		<property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
		<property name="hibernate.connection.password">11111111</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/school</property>
		<property name="hibernate.connection.username">root</property>
		<mapping resource="org/model/Login.hbm.xml" />
		<mapping resource="org/model/Book.hbm.xml" />
		<mapping resource="org/model/Student.hbm.xml" />
		<mapping resource="org/model/Lend.hbm.xml" />
	</session-factory>
</hibernate-configuration>
```

<br>

DBCONN.java

```java
package org.DBconn;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class DBCONN {

	public static Session Conn() {
	
		Configuration conn = new Configuration().configure();
		
		SessionFactory sf = conn.buildSessionFactory();
		
		Session se = sf.openSession();
		
		return se;
	}
	
	// determine if connection
	public static void main(String[] args) {
	
		System.out.println(DBCONN.Conn());
	}
}
```

<br>

bookDao.java

```java
package org.dao;

import org.DBconn.DBCONN;
import org.hibernate.Session;
import org.hibernate.Transaction;
import org.model.Book;

public class bookDao {

	public static void add(Book b) {
	
		Session se = DBCONN.Conn();
		
		Transaction tx = se.beginTransaction();
		
		se.save(b);
		tx.commit();
	}
	
	public static Book load(int id) {
	
		Session se = DBCONN.Conn();
		
		Book b = (Book) se.get(Book.class , new Integer(id) );
		
		return b;
	}
	
	public static void Update(int id, String bookname) {
	
		Session se = DBCONN.Conn();
		
		Book b = (Book) se.get(Book.class , new Integer(id) );
		
		b.setBookName(bookname);
		
		Transaction tx = se.beginTransaction();
		
		se.update(b);
		
		tx.commit();
	}
	
	public static void Delete(int id) {
	
		Session se = DBCONN.Conn();
		
		Book b = (Book) se.get(Book.class , new Integer(id) );
		
		Transaction tx = se.beginTransaction();
		
		se.delete(b);
		
		tx.commit();
	}
}
	
```

<br>

### Generate Deployment Descriptor Stub

`web.xml`

```xml
<?xml version="1.0". encoding="UTF-8"?>
<web-app 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
	<display-name>BookManage</display-name>
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
		<url-pattern>/*<url-pattern>
	</filter-mapping>
</web-app>
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
	
		<action name="add" class="org.action.BookAction">
		
			<result name="success">/success.jsp</result>
		
		</action>
		
	</package>
	
</struts>
```

<br>

`BookAction.java`

```java
package org.action;

import org.dao.bookDao;
import org.model.Book;

import com.opensymphony.xwork2.ActionSupport;

public class BookAction extends ActionSupport {

	private String ISBN;
	private String bookName;
	private String author;
	private String publisher;
	private float price;
	private int cnum;
	private int snum;
	private String summary;
	
	public String execute() throws Exception {
	
		Book b = new Book();
		
		b.setISBN(ISBN);
		b.setBookName(bookName);
		b.setAuthor(author);
		b.setPublisher(publisher);
		b.setPrice(price);
		b.setCnum(cnum);
		b.setSnum(snum);
		b.setSummary(summary);
		
		bookDao.add(b);
		
		return SUCCESS;
	}
	
	// Getters / Setters
}
```

<br>

`index.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>圖書館-管理系統</title>
</head>
<body>
	<table width="300" align="center" border="1">
		<tr>
			<td align="center"><a href="add.jsp">新增</a></td>
		</tr>
	</table>
</body>
</html>
```

<br>

`add.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8" %>
<%@taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>新增</title>
</head>
<body bgcolor="#6698FF">
<s:form action="add" method="post" theme="simple">
	<table align="center">
		<caption>新增頁面</caption>
		<tr>
			<td>ISBN <s:textfield name="ISBN" size="20" /></td>
		</tr>
		<tr>
			<td>書名 <s:textfield name="bookName" size="20" /></td>
		</tr>
		<tr>
			<td>作者 <s:textfield name="author" size="20" /></td>
		</tr>
		<tr>
			<td>出版社 <s:textfield name="publisher" size="20" /></td>
		</tr>
		<tr>
			<td>價格 <s:textfield name="price" size="20" /></td>
		</tr>
		<tr>
			<td>數量 <s:textfield name="cnum" size="20" /></td>
		</tr>
		<tr>
			<td>庫存 <s:textfield name="snum" size="20" /></td>
		</tr>
		<tr>
			<td>備註 <s:textarea name="summary" /></td>
		</tr>
		<tr>
			<td>
				<s:submit value="送出" />
				<s:reset value="重設" />
			</td>
		</tr>
	</table>
</s:form>
</body>
</html>
```

<br>

`success.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>SUCCESS</title>
</head>
<body>
<h1>新增至資料庫成功</h1>
</body>
</html>
```

<br>

練習用 SQL語法 結合 Hibernate 查詢 資料庫內容

/src/org/dao

`bookDao.java`

```java
	public static List sq1() {
		
		String sql = "select * from book";
		
		Session se = DBCONN.Conn();
		
		SQLQuery sl = se.createSQLQuery(sql);
		
		sl.addEntity("B", Book.class);
		
		List li = sl.list();		// java.util.List
		
		return li;
	}
	
	public static void miain(String[] args) {
	
		List li = bookDao.sql1();
		
		for( Object o : li ) {
		
			Book b = (Book) o;
			
			System.out.println
			(
				b.getId()+"\t"+
				b.getISBN()+"\t"+
   				b.getBookName()+"\t"+
   				b.getPrice()
   			);
   		}
	}
```

<br>

```java
	public static List sql2(int price) {
	
		String sql = "select * from book where price >= " + price;
		
		Session se = DBCONN.Conn();
		
		SQLQuery sl = se.createSQLQuery(sql);
		
		sl.addEntity("B", Book.class);
		
		List li = sl.list();
		
		return li;
	}
	
	public static void main(String[] args) {
	
		List li = bookDao.sql2(777);
		
		for( Object o : li ) {
		
			Book b = (Book) o;
			
			System.out.println
			(
				b.getId()+"\t"+
				b.getISBN()+"\t"+
   				b.getBookName()+"\t"+
   				b.getPrice()
   			);
   		}
   	}
```

<br>

```java
	public static List sql3(int start, int end) {
	
		String sql = "select * from book where price between " + start + " and " + end;
		
		Session se = DBCONN.Conn();
		
		SQLQuery sl = se.createSQLQuery(sql);
		
		sl.addEntity();
		
		List li = sl.list();
		
		return li;
	}
	
	public static void main(String[] args) {
	
		List li = bookDao.sql3(333, 7777777);
		
		for( Object o : li ) {
		
			Book b = (Book) o;
			
			System.out.println
			(
				b.getId()+"\t"+
				b.getISBN()+"\t"+
   				b.getBookName()+"\t"+
   				b.getPrice()
   			);
   		}
   	}
```
