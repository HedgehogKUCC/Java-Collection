## 會員註冊 & 登入

<br>

### MySQL

Create a new schema called ***gjun***

```mysql
CREATE SCHEMA 'gjun' DEAFAULT CHARACTER utf8;
```

<br>

Create Table called ***member***

```mysql
CREATE TABLE 'gjun'.'member' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'name' VARCHAR(45) NULL,
	'user' VARCHAR(45) NULL,
	'password' VARCHAR(45) NULL,
	'address' VARCHAR(45) NULL,
	'tel' VARCHAR(45) NULL,
	'sex' VARCHAR(45) NULL,
	'interest' VARCHAR(45) NULL,
	'remark' VARCHAR(45) NULL,
	
PRIMARY KEY ('id'))

ENGIN = InnoDB

DEAFAULT CHARACTER SET = utf8;
```

<br>

New Dynamic Web Project called ***StMember***

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
  <display-name>StMember</display-name>
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

/src New Package called ***com.model***

/src/com/model New Class called ***member***

```java
package com.model;

public class member {
	
	private String name;
	private String user;
	private String password;
	private String address;
	private String tel;
	private String sex;
	private String[] interest;		// 對應 add.jsp 中 <s:checkboxlist /> 的 list 是陣列
	private String remark;
	
	public member() { }
	
	public member(String name, String user, String password, String address, String tel, String sex, String[] interest, String remark)
	{
		this.name = name;
		this.user = user;
		this.password = password;
		this.address = address;
		this.tel = tel;
		this.sex = sex;
		this.interest = interest;
		this.remark = remark;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getUser() {
		return user;
	}

	public void setUser(String user) {
		this.user = user;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public String getTel() {
		return tel;
	}

	public void setTel(String tel) {
		this.tel = tel;
	}

	public String getSex() {
		return sex;
	}

	public void setSex(String sex) {
		this.sex = sex;
	}

	public String[] getInterest() {
		return interest;
	}
	
	// 利用這個 method 把陣列資料用for迴圈一個一個印出來在資料庫中顯示
	public String Interest() {
		
		String[] x = {"sport","music","movie"};
		
		String sum = "";
		
		for(int i=0 ; i < interest.length ; i++) 
		{
			sum = sum + x[i] + ",";
		}
		return sum;
	}

	public void setInterest(String[] interest) {
		this.interest = interest;
	}

	public String getRemark() {
		return remark;
	}

	public void setRemark(String remark) {
		this.remark = remark;
	}
}
```

<br>

/src New Package called ***com.DBCONN***

/src/com/DBCONN New Class called ***DBconn***

```java
package com.DBCONN;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBconn {
	
	public static Connection getConn() {
		
		String url = "jdbc:mysql://localhost:3306/gjun";
		String user = "root";
		String password = "11111111";
		
		try {
			
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection conn = DriverManager.getConnection(url, user, password);
			return conn;
			
		} catch (ClassNotFoundException e) {
			
			System.out.println("No Driver");
			return null;
			
		} catch (SQLException e) {
			
			System.out.println("No Connection");
			return null;
		}
	}
	
	public static void main(String[] args) {
		
		DBconn.getConn();
	}
}
```

<br>

也可以設計成輸入 PortNumber 和 SchemaName (彈性使用)

```java
package com.DBCONN;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBconn {
	
	public static Connection getConn(String PortNum, String SchemaName) {
		
		String url = "jdbc:mysql://localhost:"+PortNum+"/"+SchemaName;
		String user = "root";
		String password = "11111111";
		
		try {
			
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection conn = DriverManager.getConnection(url, user, password);
			return conn;
			
		} catch (ClassNotFoundException e) {
			
			System.out.println("No Driver");
			return null;
			
		} catch (SQLException e) {
			
			System.out.println("No Connection");
			return null;
		}
	}
	
	public static void main(String[] args) {
		
		DBconn.getConn("3306","gjun");
	}
}
```

<br>

/src New Package called ***com.DAO***

/src/com/DAO New Class called ***MemberDAO***

<br>

**功能 - 新增**

```java
package com.DAO;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import com.DBCONN.DBconn;

public class MemberDAO {

	public static void add(String name, String user, String password, String address, String tel, String sex,
							String interest, String remark)
	{
		Connection conn = DBconn.getConn();
		
		String sql = "insert into member(name, user, password, address, tel, sex, interest, remark)"
				     + "values(?, ?, ?, ?, ?, ?, ?, ?)";
		
		try {
			PreparedStatement ps = conn.prepareStatement(sql);
			
			ps.setString(1, name);
			ps.setString(2, user);
			ps.setString(3, password);
			ps.setString(4, address);
			ps.setString(5, tel);
			ps.setString(6, sex);
			ps.setString(7, interest);
			ps.setString(8, remark);
			
			int count = ps.executeUpdate();
			System.out.println("新增幾筆資料 : " + count);
			
		} catch (SQLException e) {
			System.out.println("Error : add - SQLException");
		}
	}
	
	public static void main(String[] args) {
		
		MemberDAO.add("777", "777", "777", "777", "777", "777", "777", "777");
	}
}
```

<br>

**功能 - 登入時驗證帳號和密碼**

```java
	public static member checkID(String user, String password) {
		
		Connection conn = DBconn.getConn();
		
		String sql = "select * from member where user=? and password=?";
		
		try {
			PreparedStatement ps = conn.prepareStatement(sql);
			
			ps.setString(1, user);
			ps.setString(2, password);
			
			ResultSet rs = ps.executeQuery();
			
			if(rs.next()) {
				
				member m = new member();
				m.setUser(rs.getString("user"));
				m.setPassword(rs.getString("password"));
				System.out.println("用戶帳號 : "+m.getUser()+"\n用戶密碼 : "+m.getPassword());
				return m;
			}
			else {
				System.out.println("無此帳號，請去註冊。");
				return null;
			}
		} catch (SQLException e) {
			System.out.println("Error : checkID - SQLException");
			return null;
		}
	}
		
	public static void main(String[] args) {
		
		MemberDAO.checkID("777", "777");
	}
```

<br>

**功能 - 註冊時查詢此帳號有無重複**

```java
	public static member checkUser(String user) {
		
		Connection conn = DBconn.getConn();
		
		String sql = "select * from member where user=?";
		
		try {
			PreparedStatement ps = conn.prepareStatement(sql);
			
			ps.setString(1, user);
			
			ResultSet rs = ps.executeQuery();
			
			if(rs.next()) {
				
				member m = new member();
				m.setUser(rs.getString("user"));
				System.out.println("user : "+m.getUser());
				return m;
			}
			else {
				System.out.println("No user");
				return null;
			}
		} catch (SQLException e) {
			System.out.println("Error : checkUser - SQLException");
			return null;
		}
	}
	
	public static void main(String[] args) {
		
		MemberDAO.checkUser("777");
	}
```

<br>

/src New Package called ***com.Action***

/src/com/Action New Class called ***login*** 、 ***add***

<br>

`login.java`

```java
package com.Action;

import java.util.Map;

import com.DAO.MemberDAO;
import com.model.member;
import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

public class login extends ActionSupport{
	
	private String user;
	private String password;

	public String execute() throws Exception {
		
		member m = MemberDAO.checkID(user, password);
		
		if(m != null) {
			
			// 這個 m 包含為用戶的帳號密碼，將它提升為 session，方便在後面轉頁時還可以識別出是這個用戶作為使用。
			Map session = ActionContext.getContext().getSession();
			session.put("login", m);
			return SUCCESS;
		}
		else {
			return ERROR;
		}
	}

	public String getUser() {
		return user;
	}

	public void setUser(String user) {
		this.user = user;
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

`add.java`

```java
package com.Action;

import com.DAO.MemberDAO;
import com.model.member;
import com.opensymphony.xwork2.ActionSupport;

public class add extends ActionSupport {
	
	private String name;
	private String user;
	private String password;
	private String address;
	private String tel;
	private String sex;
	private String[] interest;
	private String remark;
	
	public String execute() throws Exception {
		
		member m = new member(name, user, password, address, tel, sex, interest, remark);
		
		// m.Interest() 把傳回的陣列 利用這個 method 中的 for迴圈 一個一個取出來
		MemberDAO.add(m.getName(), m.getUser(), m.getPassword(), m.getAddress(), m.getTel(), m.getSex(), m.Interest(), m.getRemark());
		
		return SUCCESS;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getUser() {
		return user;
	}
	public void setUser(String user) {
		this.user = user;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public String getTel() {
		return tel;
	}
	public void setTel(String tel) {
		this.tel = tel;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public String[] getInterest() {
		return interest;
	}
	public void setInterest(String[] interest) {
		this.interest = interest;
	}
	public String getRemark() {
		return remark;
	}
	public void setRemark(String remark) {
		this.remark = remark;
	}
}
```

<br>

`add.java`

增加判斷 - 在註冊時，檢查這帳號是否已經被使用過

```java
	public String execute() throws Exception {
		
		member m = new member(name, user, password, address, tel, sex, interest, remark);
		System.out.println(m.getName() +"\n"+m.Interest());
		
		// 檢查是否已經有這帳號
		member m1 = MemberDAO.checkUser(user);
		
		// 如果沒有這帳號紀錄，才執行新增用戶。
		if( m1 == null) {
			
			// m.Interest() 把傳回的陣列 利用這個 method 中的 for迴圈 一個一個取出來
			MemberDAO.add(m.getName(), m.getUser(), m.getPassword(), m.getAddress(), m.getTel(), m.getSex(), m.Interest(), m.getRemark());
			
			return SUCCESS;
		}
		// 如果有帳號紀錄
		else {
			return ERROR;
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
	
		<action name="login" class="com.Action.login">
		
			<result name="success">welcome.jsp</result>
			<result name="error">error1.jsp</result>
		
		</action>
		
		<action name="add" class="com.Action.add">
		
			<result name="success">success.jsp</result>
			<result name="error">error2.jsp</result>
		
		</action>
	
	</package>

</struts>
```

<br>

/WebContent New JSP File called ***index*** 、 ***login*** 、 ***welcome*** 、 ***error1*** 、 ***add*** 、 ***success*** 、 ***error2***

<br>

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

<table align="center">
	<tr align="center">
		<td><a href="login.jsp">登入</a>
	<tr align="center">
		<td><a href="add.jsp">註冊</a>
</table>

</body>
</html>
```

<br>

`login.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>LOGIN</title>
</head>
<body>

<s:form action="login" method="post" theme="simple">

	<table width="300" align="center" border="1">
	
		<tr>
			<td colspan="2" align="center">登入
		<tr>
			<td align="center">帳號<s:textfield name="user" />
		<tr>
			<td align="center">密碼<s:password name="password" />
		<tr>
			<td align="center">
				<s:submit value="確定" />
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
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>WELCOME</title>
</head>
<body>

<h1>${login.getUser() } Welcome</h1> <br>
<a href="index.jsp">首頁</a>

</body>
</html>
```

<br>

`error1.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>錯誤頁面</title>
</head>
<body>

無此帳號 >>> <a href="add.jsp">註冊</a> <br>
帳密錯誤 >>> <a href="login.jsp">上一頁</a> <br>
<a href="index.jsp">回首頁</a>

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
</head>
<body>

<s:form action="add" method="post" theme="simple">

	<table width="350" align="center" border="1">
	
		<tr>
			<td colspan="2"	align="center">註冊
		<tr>
			<td>姓名<s:textfield name="name" />
		<tr>
			<td>帳號<s:textfield name="user" />
		<tr>
			<td>密碼<s:password name="password" />
		<tr>
			<td>地址<s:textfield name="address" />
		<tr>
			<td>電話<s:textfield name="tel" />
		<tr>
			<td>性別<s:radio list="#{1:'男生', 0:'女生' }" name="sex" />
		<tr>
			<td>興趣<s:checkboxlist list="#{1:'運動', 2:'電影', 3:'音樂' }" name="interest" />
		<tr>
			<td>備註<s:textarea name="remark" />
		<tr>
			<td align="center">
				<s:submit value="CONFIRM" />
				<s:reset value="RESET" />
			</td>
			
	</table>

</s:form>

</body>
</html>
```

<br>

`success.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>註冊成功頁面</title>
</head>
<body>

註冊成功囉 <br>
<a href="login.jsp">登入</a> <br>
<a href="index.jsp">首頁</a>

</body>
</html>
```

<br>

`error2.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>錯誤頁面</title>
</head>
<body>

帳號已被申請<br>
<a href="add.jsp">上一頁</a> <br>
<a href="index.jsp">回首頁</a>

</body>
</html>
```

<br>

### 技術遇到困難點

`add.java`

```java
		member m = new member(name, user, password, address, tel, sex, interest, remark);
		System.out.println(m.getName() +"\n"+m.Interest()); 
```

當add.jsp資料表格沒填時，接收到的是空白

不是我所想的會先給初始值 null

不知道空白要怎麼表達寫成程式碼

```java
m.getName() == ???   // ??? 要怎麼寫成程式碼為空白呢？
```

有待解決 ... ... ...


