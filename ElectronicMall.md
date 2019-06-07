### MySQL

Create a new schema callled ***company***

```mysql
CREATE SCHEMA 'company' DEFAULT CHARACTER SET utf8 ;
```

<br>

Create Table called ***member***

```mysql
CREATE TABLE 'company'.'member' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'name' VARCHAR(45) NULL,
	'password' VARCHAR(45) NULL,
	
PRIMARY KEY('id'))

ENGIN = InnoDB

DEFAULT CHARACTER SET = utf8 ;
```

<br>

Create Table called ***porder***

```mysql
CREATE TABLE 'company'.'porder' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'name' VARCHAR(45) NULL,
	'product' VARCHAR(45) NULL,
	'amount' INT NULL,
	'sum' INT NULL,
	
PRIMARY KEY('id))

ENGIN = InnoDB

DEFAULT CHARACTER SET = utf8 ;
```

<br>

/src New Package called ***model***

/src/model New Class

`member.java`

```java
package model;

public class member {
	
	private Integer id;
	private String name;
	private String password;
	
	// 有寫參數的建構式的話，無參數的建構式就一定要自己寫出來。
	public member() { }
	
	public member(String name, String password) {
		
		this.name = name;
		this.password = password;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

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

`porder.java`

```java
package model;

public class porder {

	private Integer id;
	private String name;
	private String product;
	private Integer amount;
	private Integer sum;
	
	// 有寫參數的建構式的話，無參數的建構式就一定要自己寫出來。
	public porder() { }
	
	public porder(String name, String product, int amount) {
		
		this.name = name;
		this.product = product;
		this.amount = amount;
		
		if(this.product.equals("A")) sum = amount * 1000;
		if(this.product.equals("B")) sum = amount * 1200;
		if(this.product.equals("C")) sum = amount * 1500;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getProduct() {
		return product;
	}

	public void setProduct(String product) {
		this.product = product;
	}

	public Integer getAmount() {
		return amount;
	}

	public void setAmount(Integer amount) {
		this.amount = amount;
	}

	public Integer getSum() {
		return sum;
	}

	public void setSum(Integer sum) {
		this.sum = sum;
	}
}
```

<br>

將 member.java 和 porder.java 點右鍵 New Hibernate XML Mapping file (hbm.xml)

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/6/7 ?????? 11:16:30 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
    <class name="model.member" table="MEMBER" catalog="COMPANY">
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
    </class>
</hibernate-mapping>
```

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019/6/7 ?????? 11:16:44 by Hibernate Tools 3.4.0.CR1 -->
<hibernate-mapping>
    <class name="model.porder" table="PORDER" catalog="COMPANY">
        <id name="id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="native" />
        </id>
        <property name="name" type="java.lang.String">
            <column name="NAME" />
        </property>
        <property name="product" type="java.lang.String">
            <column name="PRODUCT" />
        </property>
        <property name="amount" type="java.lang.Integer">
            <column name="AMOUNT" />
        </property>
        <property name="sum" type="java.lang.Integer">
            <column name="SUM" />
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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
                                         "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
 <session-factory name="">
  <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
  <property name="hibernate.connection.password">11111111</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/company</property>
  <property name="hibernate.connection.username">root</property>
  <mapping resource="model/member.hbm.xml"/>
  <mapping resource="model/porder.hbm.xml"/>
 </session-factory
</hibernate-configuration>
```

注意：

產生出 `hibernate.cfg.xml` 要去 Add Mappings `member.hbm.xml` 和 `porder.hbm.xml`

<br>

/src New Package called ***dbconn***

/src/dbconn New Class

`DBConn.java`

```java
package dbconn;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class DBConn {
	
	public static Session getSession() {
		
		Configuration config = new Configuration().configure();
		
		SessionFactory sf = config.buildSessionFactory();
		
		Session se = sf.openSession();
		
		return se;
	}
	
	public static void main(String[] args) {
		
		System.out.println(DBConn.getSession());
	}
}
```

<br>

/src New Package called ***dao***

/src/dao New Class

`MemberDao.java`

> 新增帳號密碼

```java
package dao;

import java.util.List;

import org.hibernate.SQLQuery;
import org.hibernate.Session;
import org.hibernate.Transaction;

import dbconn.DBConn;
import model.member;

public class MemberDao {
	
	public static void add(member m) {
		
		Session se = DBConn.getSession();
		
		Transaction tx = null;
		
		try {
			
			tx = se.beginTransaction();
			
			se.save(m);
			
			tx.commit();
			
		} catch (Exception e) {
			
			if( tx != null )
				tx.rollback();
			
			e.printStackTrace();
			
		} finally {
			
			se.close();
		}
	}
	
	public static void main(String[] args) {
		
		member m = new member();
		
		m.setName("KUCC");
		m.setPassword("777");
		
		MemberDao.add(m);
	}
}
```

<br>

> 確認帳號密碼有沒有正確

```java
	public static boolean checkID(String name, String password) {
		
		Session se = DBConn.getSession();
		
		String sql = "select * from member where name='"+name+"' and password= '"+password+"'";
		
		SQLQuery q = se.createSQLQuery(sql);
		
		q.addEntity("m", member.class);
		
		List li = q.list();		// java.util.List;
		
		boolean b;
		
		if(li.size() != 0) 
		{
			b = true;
		}
		else 
		{
			b = false;
		}
		
		return b;
	}
	
	public static void main(String[] args) {
		
		boolean b = MemberDao.checkID("KUCC", "777");
		
		System.out.println(b);
	}
```

<br>

> 註冊時確認此帳號是否已被使用

```java
	public static boolean checkID(String name) {
		
		Session se = DBConn.getSession();
		
		String sql = "select * from member where name='"+name+"'";
		
		SQLQuery q = se.createSQLQuery(sql);
		
		q.addEntity("m", member.class);
		
		List li = q.list();
		
		boolean b;
		
		if( li.size() != 0 )
		{
			b = true;
		}
		else
		{
			b = false;
		}
		
		return b;
	}
	
	public static void main(String[] args) {
		
		boolean b = MemberDao.checkID("gjun");
		
		System.out.println(b);
	}
```

注意：

如果執行出現 Exception，請先去檢查 String sql = ""; 看是否有打錯字或少打什麼符號！！！

<br>

/src/dao New Class

`PorderDao.java`

> 新增

```java
package dao;

import org.hibernate.Session;
import org.hibernate.Transaction;

import dbconn.DBConn;
import model.porder;

public class PorderDao {
	
	public static void add(porder p) {
		
		Session se = DBConn.getSession();
		
		Transaction tx = null;
		
		try {
			
			tx = se.beginTransaction();
			
			se.save(p);
			
			tx.commit();
			
		} catch (Exception e) {
			
			if( tx != null )
				tx.rollback();
			
			e.printStackTrace();
			
		} finally {
			
			se.close();
		}
	}
	
	public static void main(String[] args) {
		
		// 訂購人姓名、產品名稱、數量
		porder p = new porder("KUCC", "A", 7);
		
		PorderDao.add(p);
	}
}
```

<br>

/WebContent New Folder called ***Content***

/WebContent/Content New Folder ***css*** 、 ***img*** 、 ***memeber*** 、 ***order***

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
<link rel="stylesheet" type="text/css" href="Content/css/st.css">
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center>
				<tr>
					<td align=center><h3>欲購買商品請登入</h3>
				<tr>
					<td align=center><a href="Content/member/login.jsp">登入</a>
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="end.jsp" />
	
	</table>
</body>
</html>
```

<br>

`title.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
<h1>GJUN電子商城</h1>
</body>
</html>
```

<br>

`end.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
<h2>台北市公園路30號3樓</h2>
</body>
</html>
```

<br>

/WebContent/Content/css New CSS File

`st.css`

```css
@charset "UTF-8";

.title
{
	background-color: #547AA5;
	color: #F87431;
}

.content
{
	background-color: #50D8D7;
}

.border
{
	border: 2px dotted black;
}

.end
{
	background-color: #547AA5;
	color: #F87431;
}
```

<br>

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

<br>

/WebContent/Content/member New JSP File

`login.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>登入頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css">
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center border=1 class="border">
				<tr>
					<td align=center>會員登入
					
					<s:form method="post" action="login" theme="simple">
					
						<table width=200 align=center>
							<tr>
								<td align=center>帳號<s:textfield name="name" />
							<tr>
								<td align=center>密碼<s:password name="password" />
							<tr>
								<td>
									<s:submit value="確認" />
									<s:reset value="重填" />
									<a href="<s:url value="add.jsp" />">註冊</a>
								</td>
						</table>
					
					</s:form>
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
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
<title>註冊頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css">
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center border=1 class="border">
				<tr>
					<td align=center>會員註冊
					
					<s:form method="post" action="addMem" theme="simple">
					
						<table width=200 align=center>
							<tr>
								<td align=center>帳號<s:textfield name="name" />
							<tr>
								<td align=center>密碼<s:password name="password" />
							<tr>
								<td>
									<s:submit value="確認" />
									<s:reset value="重填" />
								</td>
						</table>
					
					</s:form>
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

/WebContent/Content/order New JSP File

`addOrder.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>訂購頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css">
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center border=1 class="border">
				<tr>
					<td align=center>
					
					<s:form method="post" action="addOrder" theme="simple">
					
						<table width=200 align=center>
							<tr>
								<td colspan=2 align=center>新增訂單
							<tr>
								<td align=center>產品
								<td>
									<select name="product">
										<option value="A">A
										<option value="B">B
										<option value="C">C
									</select>
							<tr>
								<td align=center>數量
								<td>
									<input type="text" name="amount" value="0" />
							<tr>
								<td>
									<s:submit value="送出" />
								<td>
									<s:reset value="重填" />
						</table>
					
					</s:form>
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

`comfirmOrder.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>訂單確認頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css">
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center border=1 class="border">
			
				<tr>
					<td colspan=2 align=center>這是您的訂單
				<tr>
					<td>姓名
					<td>${P.getName() }
				<tr>
					<td>產品
					<td>${P.getProduct() }
				<tr>
					<td>數量
					<td>${P.getAmount() }
				<tr>
					<td>總價
					<td>${P.getSum() }
				<tr>
					<td colspan=2>
					<a href="finish.jsp">OK</a>
					
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

`finish.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="model.porder, dao.PorderDao"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>訂購完成頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css">
</head>
<%
	porder p = (porder) request.getSession().getAttribute("P");
	
	PorderDao.add(p);
	
	// 加這行才能避免 重整頁面時 資料一直增加
	session.removeAttribute("P");
%>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center border=1 class="border">
			
				<tr>
					<td align=center>訂單完成
				<tr>
					<td align=center>
					<a href="../../index.jsp">首頁</a>
					
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

/src New Package called ***action***

/src/action New Class

`loginAction.java`

```java
package action;

import java.util.Map;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

import dao.MemberDao;
import model.member;

public class loginAction extends ActionSupport {
	
	private String name;
	private String password;
	
	public String execute() throws Exception {
		
		boolean b = MemberDao.checkID(name, password);
		
		member m;
		
		if( b == true )
		{
			m = new member(name, password);
			
			Map session = ActionContext.getContext().getSession();
			
			session.put("M", m);
			
			return SUCCESS;
		}
		else
		{
			return ERROR;
		}
	}
	
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

`addOrder.java`

```java
package action;

import javax.servlet.http.HttpServletRequest;

import org.apache.struts2.ServletActionContext;

import com.opensymphony.xwork2.ActionSupport;

import model.member;
import model.porder;

public class addOrder extends ActionSupport {
	
	private String product;
	private int amount;
	
	public String execute() throws Exception {
		
		Map session = ActionContext.getContext().getSession();
		
		member m = (member)session.get("M");
		
		/*
		HttpServletRequest request = ServletActionContext.getRequest();
		
		member m = (member)request.getSession().getAttribute("M");
		*/
		
		porder p = new porder(m.getName(), product, amount);
		
		ActionContext.getContext().getSession().put("P", p);
		
		return SUCCESS;
	}
	
	public String getProduct() {
		return product;
	}
	public void setProduct(String product) {
		this.product = product;
	}
	public int getAmount() {
		return amount;
	}
	public void setAmount(int amount) {
		this.amount = amount;
	}
}
```

<br>

`addMemberAction.java`

```java
package action;

import com.opensymphony.xwork2.ActionSupport;

import dao.MemberDao;
import model.member;

public class addMemberAction extends ActionSupport{

	private String name;
	private String password;
	
	public String execute() throws Exception {
		
		boolean b = MemberDao.checkID(name);
		
		member m;
		
		if( b == false )
		{
			m = new member(name, password);
			
			MemberDao.add(m);
			
			return SUCCESS;
		}
		else
		{
			return ERROR;
		}
	}
	
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

/src New XML File called ***struts***	&nbsp; &nbsp; &nbsp; (一定要命名為 struts)

`struts.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
	"http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>

	<package name="default" extends="struts-default">
	
		<action name="login" class="action.loginAction">
		
			<result name="success">/Content/order/addOrder.jsp</result>
			<result name="error">/Content/member/add.jsp</result>
		
		</action>
		
		<action name="addOrder" class="action.addOrder">
		
			<result name="success" type="redirect">/Content/order/comfirmOrder.jsp</result>
		
		</action>
		
		<action name="addMem" class="action.addMemberAction">
		
			<result name="success" type="redirect">/Content/member/login.jsp</result>
			<result name="error">/Content/member/add.jsp</result>
		
		</action>
	
	</package>

</struts>
```

<br>

在這個練習中遇到的問題

`addOrder.jsp` 轉頁至 `comfirmOrder.jsp` 時

會發生 css樣式不見了連帶著 轉頁至 finish.jsp 也會發生 404

這個問題也發生在

註冊頁面 `add.jsp` 成功註冊好要轉頁回 `login.jsp` 會發生 css樣式不見

多次嘗試後確認是因為經過struts2框架上轉頁才會發生這個問題

上網查詢到 [解決方法](https://blog.csdn.net/u011847748/article/details/39394203)

<br>

展示頁面

![index](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/index.jpg)

![login](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/login.jpg)

![addOrder](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/addOrder.jpg)

![comfirmOrder](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/comfirmOrder.jpg)

![finish](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/finish.jpg)

![add](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/add.jpg)

