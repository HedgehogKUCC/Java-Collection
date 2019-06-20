### 設計管理者登入

/WebContent/Content New Folder `admin`

/admin New JSP File

`admin.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>管理者頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css" >
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<table width=300 align=center class="border">
			
				<caption id="admin">${M.getName() } 歡迎您回來</caption>
			
				<tr>
					<td class="row"><a href="../admin/query.jsp">查詢</a>
				<tr>
					<td class="row"><a href="../admin/update.jsp">修改</a>
				<tr>
					<td class="row"><a href="../admin/delete.jsp">刪除</a>
				
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

![admin](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/admin.jpg)

<br>

`query.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css" >
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<form method="post" action="queryAll">
				<table width=300 align=center>
					
					<tr>
						<td align=center><input type="submit" value="查詢全部" />
				
				</table>
			</form>
			<hr>
			<table width=300 align=center class="border">
			
			<tr>
			
				<c:forEach var="i" begin="0" end="10" step="1">
					
						<td align=center ><c:out value="${ rs[i] }" /> </td>
					
				</c:forEach>
						
			</table>
		<tr>
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

![queryAll-1](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/queryAll-1.jpg)

![queryAll-2](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/queryAll-2.jpg)

<br>

修改 `query.jsp` 部分內容

```jsp
<c:forEach var="rs" items="${rs}">
   <tr>
      <td align="right"><c:out value="${rs}" /> </td>
   </tr>
</c:forEach>
```

![queryAll-3](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/queryAll-3.jpg)

<br>

`update.jsp`

```jsp

```

<br>

`delete.jsp`

```jsp

```

<br>

### Control

`loginAction.java`

```java
package action;

import java.util.Map;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;

import dao.MemberDao;
import model.member;

public class loginAction extends ActionSupport {
	
	private String name;
	private String password;
	
	public String execute() throws Exception {
		
		//boolean b = MemberDao.checkID(name, password);
		
		ApplicationContext a = new ClassPathXmlApplicationContext("dao/spring3.xml");
		
		member m = (member)a.getBean("sp3M");
		
		MemberDao md = (MemberDao)a.getBean("sp3MD");
		
		Boolean b = md.checkID(name, password);
		
		if( b == true )
		{
			m = new member(name, password);
			
			// 增加判斷，帳號和密碼有沒有符合為管理者權限，符合就轉頁至管理者頁面。
			if(m.getName().equals("KUCC") && m.getPassword().equals("777")) {
				
				Map session = ActionContext.getContext().getSession();
				
				session.put("M", m);
				
				return "Admin";
			}
			
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

`DBConn.java`

```java
    public static Connection getConn() {
		
		String url = "jdbc:mysql://localhost:3306/company";
		String user = "root";
		String pass = "11111111";
		
		Connection conn;
	
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			conn = DriverManager.getConnection(url, user, pass);
			return conn;
		} catch (ClassNotFoundException e) {
			System.out.println("No Driver");
			return null;
		} catch (SQLException e) {
			System.out.println("No Connection");
			return null;
		}
	}
```

<br>

`MemberDao.java`

使用這方法要把 ResultSet Object 傳至前端頁面，使用JSTL會有問題。

```java
    public static ResultSet queryAll() {
		
		String sql = "select * from member";
		
		ResultSet rs = null;
		
		try {
			
			Connection conn = DBConn.getConn();
			PreparedStatement ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			
		} catch (SQLException e) {
			System.out.println("Error-MemberQueryAll");
		}
		return rs;
	}
```

<br>

將上面方法改為如下

```java
    public static List queryAll() {
		
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs;
		List list = null;
		
		String sql = "select * from member";
		
		try {
			
			conn = DBConn.getConn();
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			list = convertList(rs);
			
		} catch (SQLException e) {
			System.out.println("Error-MemberQueryAll");
		} finally {
			
			try {
				ps.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			
			try {
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		return list;
	}
	
	// 因為 List 只能存放元素的索引而不能存放元素的值，所以我們要用到 Map
	private static List convertList(ResultSet rs) throws SQLException {
		
		List list = new ArrayList();
		
		// 取出這 ResultSet 物件欄位的數量、類型和屬性
		ResultSetMetaData md = rs.getMetaData();
		
		// 獲取列的數量
		int columnCount = md.getColumnCount();		
		
		while(rs.next()) {
			
			Map rowData = new HashMap();   // 宣吿 Map
			
			for(int i = 1; i <= columnCount; i++) {
				rowData.put(md.getColumnName(i), rs.getObject(i));  // 獲取鍵名與值
			}
			list.add(rowData);
		}
		
		return list;
	}
```

<br>

/src New XML

`query.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
    
    <bean id="query" class="dao.MemberDao" />
    
</beans>
```

<br>

/src/action New Class

`queryAllAction.java`

```java
package action;

import java.sql.ResultSet;
import java.util.ArrayList;

import javax.servlet.http.HttpServletRequest;

import org.apache.struts2.ServletActionContext;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.opensymphony.xwork2.ActionSupport;

import dao.MemberDao;
import model.member;

public class queryAllAction extends ActionSupport {

	private String name;
	private String password;
	
	public String execute() throws Exception {
		
		ApplicationContext a = new ClassPathXmlApplicationContext("query.xml");
		MemberDao m = (MemberDao) a.getBean("query");
		
		List list = m.queryAll();
		
		/*
		ArrayList<String> array = new ArrayList<>();
		
		String RS ;
		
		while(rs.next()) {
			
			
			 RS = rs.getString("id") + "\t" + rs.getString("name") + "\t" + rs.getString("password");
			 array.add(RS);
			
		}
		*/
		
		HttpServletRequest request = ServletActionContext.getRequest();
		
		request.getSession().setAttribute("rs", list);
		
		return SUCCESS;
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

`struts.xml`

```xml
	<action name="queryAll" class="action.queryAllAction">
		
			<result name="success">/Content/admin/query.jsp</result>
			<!--  <result name="error">/Content/admin/admin.jsp</result> -->
		
   </action>
```

<br>

使用 Hibernate 進行 SQL語法 搜尋資料庫

`MemberDao.java`

```java
    public static List queryHibAll() {
		
		// SQL語法搜尋 member
		String sql = "select * from member";
		
		// Hibernate 取得 Session
		Session se = DBConn.getSession();
		
		// 建立 SQLQuery，將 sql 帶入參數使用
		SQLQuery sq = se.createSQLQuery(sql);
		
		// 資料庫與模組映射
		sq.addEntity("M", member.class);
		
		// 轉成 List
		List li = sq.list();
		
		return li;
	}
```

記得將 `queryAllAction.java` 裡面這行修改成 `List list = m.queryHibAll();`

<br>

`query.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢頁面</title>
<link rel="stylesheet" type="text/css" href="../css/st.css" >
</head>
<body>
	<table width=750 align=center>
	
		<tr>
			<td class="title" align=center><jsp:include page="../../title.jsp" />
		<tr>
			<td height=400 class="content">
			<form method="post" action="queryAll">
				<table width=300 align=center>
					
					<tr>
						<td align=center><input type="submit" value="查詢全部" />
				
				</table>
			</form>
			<hr>
			<table width=300 align=center border="1" class="border">
			
				<tr align=center bgcolor="yellow">
					<td>ID<td>NAME<td>PASSWORD
					
				<c:forEach var="rs" items="${rs}">
					
					<tr align=center>
						<td><c:out value="${rs.getId()}" /> </td>
						<td><c:out value="${rs.getName()}" /> </td>
						<td><c:out value="${rs.getPassword()}" /> </td>
					</tr>
					
				</c:forEach>	
						
			</table>
		<tr colspan="3">
			<td class="end" align=center><jsp:include page="../../end.jsp" />
	
	</table>
</body>
</html>
```

使用 Hibernate 取出來的 `<c:out value="${rs}" />` 會是記憶體位置 `model.member@40fd540b`

由此可知已經有搜尋到資料庫資料，將藉由`EL語法`取出每列各欄位的值

但顯示出來的表格似乎需要修改

![queryHibAll-1](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/queryHibAll-1.jpg)

<br>

修改樣式表

`st.css`

```css
.border
{
	border: dotted black;
	border-collapse: collapse;
}
```

變得好看了 ！！！

![queryHibAll-2](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/ElectronicMall_img/queryHibAll-2.jpg)

<br>


