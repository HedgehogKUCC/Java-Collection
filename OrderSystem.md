## 點餐系統 ( JDBC )

<br>

### MySQL

Create a new schema called ***order***

```mysql
CREATE SCHEMA 'order' DEFAULT CHARACTER SET utf8 ;
```

<br>

Create Table called ***porder***

```mysql
CREATE TABLE 'order'.'porder' (

	'id' INT NOT NULL AUTO_INCREMENT,
	'desk' VARCHAR(45) NULL,
	'pro1' INT NULL,
	'pro2' INT NULL,
	'pro3' INT NULL,
	'member' VARCHAR(45) NULL,
	'sum' INT NULL,
	
PRIMARY KEY('id'))

ENGIN = InnoDB

DEFAULT CHARACTER SET = utf8;
```

<br>

這次使用 NetBeans 撰寫 .class (商業邏輯) 在桌面 New Folder called ***OrderSystemAPI***

NetBeans File New Project

Categories : ***Java***

Projects : ***Java Application***

Project Name : ***order***

/src New Java Package called ***com***

/com New Java Class

`porder.java`

```java
package com;
/**
 * 
 * @author <h1>kucc</h1>
 * @version 1.0
 */
public class porder {
    
    private String desk;
    private int pro1;
    private int pro2;
    private int pro3;
    private String member;
    private int sum;
    
    public porder() { }
    
    /**
     * 
     * @param desk  桌號
     * @param pro1  產品1
     * @param pro2  產品2
     * @param pro3  產品3
     * @param member    是否為會員
     * <br>
     * EX:<br>
     * porder p = new porder("A桌",1,1,1,"Y/N");
     */
    
    public porder(String desk, int pro1, int pro2, int pro3, String member)
    {
        this.desk = desk;
        this.pro1 = pro1;
        this.pro2 = pro2;
        this.pro3 = pro3;
        this.member = member;
        
        sum = pro1*50 + pro2*60 + pro3*70;
    }

    /**
     * 
     * @param desk 修改桌號
     */
    public void setDesk(String desk) {
        this.desk = desk;
    }

    /**
     * 
     * @param pro1 修改產品1
     */
    public void setPro1(int pro1) {
        this.pro1 = pro1;
    }

    /**
     * 
     * @param pro2 修改產品2
     */
    public void setPro2(int pro2) {
        this.pro2 = pro2;
    }

    /**
     * 
     * @param pro3 修改產品3
     */
    public void setPro3(int pro3) {
        this.pro3 = pro3;
    }

    /**
     * 
     * @param member 修改是否為會員
     */
    public void setMember(String member) {
        this.member = member;
    }

    public String getDesk() {
        return desk;
    }

    public int getPro1() {
        return pro1;
    }

    public int getPro2() {
        return pro2;
    }

    public int getPro3() {
        return pro3;
    }

    public String getMember() {
        return member;
    }

    public int getSum() {
    
        // 再次計算確認
        sum = pro1*50 + pro2*60 + pro3*70;
    	
        return sum;
    }
```

<br>

```java
	/**
     * 檢查有無 Driver
     */
    static 
    {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException ex) {
            System.out.println("No Driver");
        }
    }
```

<br>

掛 `mysql-connector-java-8.0.13.jar`

`order` 點右鍵 `Properties` 點 `Libraries`

點 `Add JAR/Folder` 選擇路徑

<br>

```java
	/**
     * 
     * @return 連上資料庫
     * <br><br>
     * EX:<br>
     * Connection conn = porder.getConn();
     */
    public static Connection getConn() {
        
        String url = "jdbc:mysql://localhost:3306/order?useUnicode=true&characterEncoding=utf8";
        String user = "root";
        String pass = "11111111";
        
        try {
            
            Connection conn = DriverManager.getConnection(url, user, pass);
            
            return conn;
            
        } catch (SQLException ex) {
            
            System.out.println("No Connection");
            
            return null;
            
        }
    }
```

注意：

`?useUnicode=true&characterEncoding=utf8` 加上這句資料庫才能識別出中文

<br>

> 新增

```java
	/**
     * 增加訂單資料
     */
    public void add() {
       
       String sql = "insert into porder(desk, pro1, pro2, pro3, member, sum) values(?, ?, ?, ?, ?, ?)";
       
        try (   Connection conn = porder.getConn();
                PreparedStatement ps = conn.prepareStatement(sql);) 
        {
            
            ps.setString(1, getDesk());
            ps.setInt(2, getPro1());
            ps.setInt(3, getPro2());
            ps.setInt(4, getPro3());
            ps.setString(5, getMember());
            ps.setInt(6, getSum());
            
            ps.executeUpdate();
            
        } catch (SQLException ex) {
            
            System.out.println("Error-add SQLException");
        }
    }
```

<br>

> 查詢全部

```java
 	/**
     * 
     * @return 查詢全部資料
     */
    public static ResultSet queryAll() {
        
        String sql = "select * from porder";
        
        ResultSet rs = null;
        
        try
        {
            Connection conn = porder.getConn();
            Statement st = conn.createStatement();
            rs = st.executeQuery(sql);
            
        } catch (SQLException ex) {
            
            System.out.println("Error-queryAll");
            
        } 
        
        return rs;
    }
    
    /* 測試
    public static void main(String[] args) throws SQLException {
       
        ResultSet rs = porder.queryAll();
        
        while(rs.next()) {
           
           System.out.println
           (
                "ID : "+rs.getInt("id")+
                "\tDESK : "+rs.getString("desk")+
                "\tPRO1 : "+rs.getInt("pro1")+
                "\tPRO2 : "+rs.getInt("pro2")+
                "\tPRO3 : "+rs.getInt("pro3")+
                "\tMEMBER : "+rs.getString("member")+
                "\tSUM : "+rs.getInt("sum")
           );
           
       }
    }
    */
```

<br>

> 查詢桌號

```java
	/**
     * 
     * @param desk 桌號
     * @return 回傳查詢的桌號資料
     */
    public static ResultSet queryDesk(String desk) {
        
        String sql = "select * from porder where desk='"+desk+"'";
            
        ResultSet rs = null;
        
        try {
                  
            Connection conn = porder.getConn();
            Statement st = conn.createStatement();
            rs = st.executeQuery(sql);
            
        } catch (SQLException ex) {
            
            System.out.println("Error-queryDesk");
        }
        
        return rs;
    }
    
    public static void main(String[] args) throws SQLException {
       
        ResultSet rs = porder.queryDesk("A桌");
        
        while(rs.next()) {
           
           System.out.println
           (
                "ID : "+rs.getInt("id")+
                "\tDESK : "+rs.getString("desk")+
                "\tPRO1 : "+rs.getInt("pro1")+
                "\tPRO2 : "+rs.getInt("pro2")+
                "\tPRO3 : "+rs.getInt("pro3")+
                "\tMEMBER : "+rs.getString("member")+
                "\tSUM : "+rs.getInt("sum")
           );
           
       }
    }
```

<br>

> 查詢總價

```java
	/**
     * 
     * @param sum 總價
     * @return 回傳查詢的總價資料
     */
    public static ResultSet querySum(int start, int end) {
        
        String sql = "select * from porder where sum between '"+start+"' and '"+end+"'";
        
        ResultSet rs = null;
        
        try {
            
            Connection conn = porder.getConn();
            Statement st = conn.createStatement();
            rs = st.executeQuery(sql);
            
        } catch (SQLException ex) {
        
            System.out.println("Error-querySum");
        }
        
        return rs;
    }
    
    public static void main(String[] args) throws SQLException {
       
        ResultSet rs = porder.querySum(180,500);
        
        while(rs.next()) {
           
           System.out.println
           (
                "ID : "+rs.getInt("id")+
                "\tDESK : "+rs.getString("desk")+
                "\tPRO1 : "+rs.getInt("pro1")+
                "\tPRO2 : "+rs.getInt("pro2")+
                "\tPRO3 : "+rs.getInt("pro3")+
                "\tMEMBER : "+rs.getString("member")+
                "\tSUM : "+rs.getInt("sum")
           );
           
       }
    }
```

<br>

> 查詢會員＋總價範圍

```java
	/**
     * 
     * @param member 會員
     * @param start 起始值
     * @param end   終端值
     * @return 回傳查詢是否為會員且總價範圍內的資料
     */
    public static ResultSet queryMember(String member, int start, int end) {
        
        String sql = "select * from porder where member = '"+member+"'and sum between '"+start+"' and '"+end+"'";
        
        ResultSet rs = null;
        
        try {
            
            Connection conn = porder.getConn();
            Statement st = conn.createStatement();
            rs = st.executeQuery(sql);
            
        } catch (SQLException ex) {
            
            System.out.println("Error-queryMember");
        }
        
        return rs;
    }
    
    public static void main(String[] args) throws SQLException {
       
        ResultSet rs = porder.queryMember("Y",100,600);
        
        while(rs.next()) {
           
           System.out.println
           (
                "ID : "+rs.getInt("id")+
                "\tDESK : "+rs.getString("desk")+
                "\tPRO1 : "+rs.getInt("pro1")+
                "\tPRO2 : "+rs.getInt("pro2")+
                "\tPRO3 : "+rs.getInt("pro3")+
                "\tMEMBER : "+rs.getString("member")+
                "\tSUM : "+rs.getInt("sum")
           );
           
       }
    }
```

<br>

> 修改訂單

```java
   /**
     * <p>Description: 修改訂單</p>
     * @param id      主鍵
     * @param desk    桌號
     * @param pro1    產品1
     * @param pro2    產品2
     * @param pro3    產品3
     * @param member  是否會員
     */
    public static void update(int id, String desk, int pro1, int pro2, int pro3, String member) {
        
        int sum = pro1*50 + pro2*60 + pro3*70;
        String sql = "update porder set desk = '"+desk+"',pro1 = '"+pro1+"',pro2 = '"+pro2+"',pro3 = '"+pro3+"',member = '"+member+"',sum = '"+sum+"' where id = '"+id+"'";
        
        try {
            
            Connection conn = porder.getConn();
            PreparedStatement ps = conn.prepareStatement(sql);
            
            ps.executeUpdate();
            
        } catch (SQLException ex) {
            
            System.out.println("Error-update");
        }
    }
    
    public static void main(String[] args) throws SQLException {
       
        porder.update(6, "D桌", 7, 7, 7, "Y");
    }
```

<br>

> 刪除訂單

```java
   /**
     * <p>Description: 刪除訂單</p>
     * @param id  主鍵
     */
    public static void delete(int id) {
        
        String sql = "delete from porder where id = '"+id+"'";
        
        try {
            
            Connection conn = porder.getConn();
            PreparedStatement ps = conn.prepareStatement(sql);
            
            ps.executeUpdate();
            
        } catch (SQLException ex) {
            
            System.out.println("Error-delete");
        }
    }
    
    public static void main(String[] args) throws SQLException {
       
        porder.delete(6);
    }
```

注意：

id 是資料庫自動增加，所以刪除後也無法在新增和修改。

<br>

### 設定 JavaDoc

`order` 點右鍵 `Propertiesd` 點 Build &nbsp; >>> &nbsp; Documenting

Document Additional Tags :

- [x] @author
- [x] @version

Browser Window Title : ***點餐系統API***

<br>

### .jar產生

`order` 點右鍵 `Clean and Build`

`order` 點右鍵 `Generate Javadoc`

將產生出來的資料從 `dist 資料夾` 裡面 Copy 至 `OrderSystemAPI 資料夾`

**這樣就打包好可以給其他人做使用**

[點餐系統API](https://github.com/HedgehogKUCC/Java-Collection/tree/master/picture/OrderSystemAPI)

<br>

/WebContent New JSP File

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
<div id="title-font">Hedgehog Breakfast</div>
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
<div id="end-font">台北市大安區公園路</div>
</body>
</html>
```

<br>

`index.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Hedgehog Ordering</title>
<link rel="stylesheet" type="text/css" href="Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<table width=350 align=center>
				
				<tr>
					<td align=center class="index"><a href="add/input.jsp">1) 新增</a>
				<tr>
					<td align=center class="index">2) 查詢
				<tr>
					<td align=center class="index">3) 修改
				<tr>
					<td align=center class="index">4) 刪除
				
			</table>
			
		<tr>
			<td align=center id="end">
			<jsp:include page="end.jsp" />
	
	</table>
</body>
</html>
```

<br>

/WebContent New Folder `Content`、`add`、`query`、`update`、`delete`、`img` ( 將圖片 `1.jpg`、`2.jpg`、`3.jpg` 放入 )

/Content New Folder `css`

/css New CSS File

`ku.css`

```css
@charset "UTF-8";

#title
{
	background-color: #F2E2BA;
	color: #2554C7;
}

#title-font
{
	font-size: 60px;
}

#content
{
	background-color: #BAF2D8;
}

.index
{
	font-size: 36px;
}

#border
{
	border: 2px dotted black;
}

#end
{
	background-color: #F2E2BA;
	color: #2554C7;
}

#end-font
{
	font-size: 40px;
}

#caption
{
	font-size: 46px;
	border: 2px dotted yellow;
}
```

<br>

![index.jsp](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/OrderSystem_img/OrderIndex.jpg)

<br>

/add New JSP File `input`、`check`、`finish`

`input.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>訂餐頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="../add">
			<table width=500 height=350 align=center id="border">
				
				<tr>
					<td colspan=3 align=center>桌號
					<select name="desk">
						<option value="A桌">A桌
						<option value="B桌">B桌
						<option value="C桌">C桌
						<option value="D桌">D桌
					</select>
				
				<tr>
					<td align=center>A餐
					<td align=center>B餐
					<td align=center>C餐
					
				<tr>
					<td align=center>
					<img src="../Content/img/1.jpg" width="150" height="100" alt="product1">
					<br>
					數量:
					<select name="pro1">
						<option value="0">0
						<option value="1">1
						<option value="2">2
						<option value="3">3
						<option value="4">4
						<option value="5">5
						<option value="6">6
						<option value="7">7
						<option value="8">8
						<option value="9">9
						<option value="10">10
					</select>
					
					<td align=center>
					<img src="../Content/img/2.jpg" width="150" height="100" alt="product2">
					<br>
					數量:
					<select name="pro2">
						<option value="0">0
						<option value="1">1
						<option value="2">2
						<option value="3">3
						<option value="4">4
						<option value="5">5
						<option value="6">6
						<option value="7">7
						<option value="8">8
						<option value="9">9
						<option value="10">10
					</select>
					
					<td align=center>
					<img src="../Content/img/3.jpg" width="150" height="100" alt="product3">
					<br>
					數量:
					<select name="pro3">
						<option value="0">0
						<option value="1">1
						<option value="2">2
						<option value="3">3
						<option value="4">4
						<option value="5">5
						<option value="6">6
						<option value="7">7
						<option value="8">8
						<option value="9">9
						<option value="10">10
					</select>
				
				<tr>
					<td colspan=3 align=center>會員
					<input type="radio" name="member" value="Y" checked />YES
					<input type="radio" name="member" value="N" />NO
					
				<tr>
					<td colspan=3 align=center>
					<input type="submit" value="送出" />
					<input type="reset" value="重填" />
				
				
			</table>
			</form>
		<tr>
			<td align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

注意：

`<img src=" 使用相對路徑 " >`

`<form  action="../add">`

<br>

![input.jsp](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/OrderSystem_img/OrderInput.jpg)

<br>

`check.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="com.porder"%>
<!-- import="com.porder" 查看 order.jar 當時的路徑  -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>確認頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<%
	porder p = (porder)session.getAttribute("P");
%>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<table width=500 height=350 align=center id="border">
			
				<tr>
					<td colspan=2 align=center>
					<h2>訂單確認</h2>
				
				<tr>
					<td colspan=2 align=center>
					<hr>
				
				<tr>
					<td width=50 align=center>桌號
					<td width=350 align=center>${P.getDesk() }
				<tr>
					<td width=50 align=center>A餐
					<td width=350 align=center><%= p.getPro1() %>
				<tr>
					<td width=50 align=center>B餐
					<td width=350 align=center><%= p.getPro2() %>
				<tr>
					<td width=50 align=center>C餐
					<td width=350 align=center><%= p.getPro3() %>
				<tr>
					<td width=50 align=center>會員
					<td width=350 align=center><%= p.getMember() %>
					
				<tr>
					<td colspan=2 align=center>
					<hr>
					
				<tr>
					<td width=50 align=center>總共
					<td width=350 align=center><%= p.getSum() %>
					
				<tr>
					<td colspan=2 align=center>
					<a href="input.jsp">上一頁</a>
					<a href="finish.jsp">確定</a>
				
			</table>
			
		<tr>
			<td align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

![check.jsp](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/OrderSystem_img/OrderCheck.jpg)

<br>

`finish.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="com.porder"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>完成訂單頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<%
	porder p = (porder)session.getAttribute("P");
	p.add();
%>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<table width=500 height=350 align=center id="border">
			
				<tr>
					<td align=center>
					<h1>訂單完成</h1>
					<a href="../index.jsp">首頁</a>
			
			</table>
			
		<tr>
			<td align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

![finish.jsp](https://github.com/HedgehogKUCC/Java-Collection/blob/master/picture/OrderSystem_img/OrderFinish.jpg)

<br>

/src New Package called ***com***

/com New Servlet

`add.java`

```servlet
package com;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/add")
public class add extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public add() {
        super();
    }

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		//將請求的資訊，設定字元編碼:UTF-8
		request.setCharacterEncoding("UTF-8");
		
		//將回應的資訊，設定內容類型: MIME: text/html; 字元設定 = UTF-8
		response.setContentType("text/html; charset=UTF-8");
		
		String DESK = request.getParameter("desk");
		int PRO1 = Integer.parseInt(request.getParameter("pro1"));
		int PRO2 = Integer.parseInt(request.getParameter("pro2"));
		int PRO3 = Integer.parseInt(request.getParameter("pro3"));
		String MEMBER = request.getParameter("member");
		
		porder p = new porder(DESK, PRO1, PRO2, PRO3, MEMBER);
		
		HttpSession session = request.getSession();
		
		session.setAttribute("P", p);
		
		response.sendRedirect("add/check.jsp");
	}
}
```

<br>

/WebContent/query New JSP File

> 查詢主頁

`query.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<table width=350 align=center>
				
				<caption id="caption">請選擇查詢條件</caption>
				
				<tr>
					<td align=center class="index"><a href="queryAll.jsp">1) 全部</a>
				<tr>
					<td align=center class="index"><a href="queryDesk.jsp">2) 桌號</a>
				<tr>
					<td align=center class="index"><a href="querySum.jsp">3) 總價</a>
				<tr>
					<td align=center class="index"><a href="queryMember.jsp">4) 會員</a>
				<tr>
					<td align=center><a href="../index.jsp">首頁</a>
				
			</table>
			
		<tr>
			<td align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

> 查詢全部

`queryAll.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢全部</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css" >
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<table width=350 align=center>
				
				<tr>
				<%
					ResultSet rs = porder.queryAll();
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println
					(
						"<tr><td colspan=7 align=center>"+
						"<a href=\"query.jsp\">上一頁</a>"+"&nbsp; &nbsp;"+
						"<a href=\"../index.jsp\">回首頁</a>"
					);
				%>
				
			</table>
			
		<tr>
			<td align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

> 查詢桌號

`queryDesk.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢桌號</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css" >
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="queryDesk.jsp" >
			
				<table width=350 align=center>
					
					<tr>
						<td align=center><h3>依桌號查詢</h3>
					<tr>
						<td align=center>桌號
						<select name="desk">
							<option value="A桌">A桌
							<option value="B桌">B桌
							<option value="C桌">C桌
							<option value="D桌">D桌
						</select>
						<input type="submit" value="OK" />
			
				</table>
				
			</form>	
				<hr>
				<%
					// 避免中文產生亂碼
					request.setCharacterEncoding("UTF-8");
				
					String DESK = request.getParameter("desk");
				
					ResultSet rs ;
					
					// String 初始值為 null，利用這個 null 來作一個判斷。
					if ( DESK == null )
					{
						rs = porder.queryAll();
					}
					else
					{
						rs = porder.queryDesk(DESK);
					}
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println("<tr><td colspan=7 align=center>");
					out.println("</table>");
	
				%>
				<br>
				<a href="query.jsp">上一頁</a>
				<a href="../index.jsp">回首頁</a>
			
		<tr>
			<td colspan=7 align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

> 查詢總價範圍

`querySum.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢總價範圍</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css" >
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="querySum.jsp" >
			
				<table width=350 align=center>
					
					<tr>
						<td align=center><h3>依總價查詢</h3>
					<tr>
						<td align=center>
						金額:
						從<input type="text" name="start" size="10" value="0" />
						到<input type="text" name="end" size="10" value="0" />
						<input type="submit" value="OK" />
			
				</table>
				
			</form>	
				<hr>
				<%
					// 避免中文產生亂碼
					request.setCharacterEncoding("UTF-8");
					
					int START, END;
				
				    String st = request.getParameter("start");
				    String en = request.getParameter("end");
				
				    ResultSet rs ;
					if ( st == null || en == null || st.length() == 0 || en.length() == 0 )
					{
						rs = porder.queryAll();
					}
					else
					{
						START = Integer.parseInt(st);
						END = Integer.parseInt(en);
						rs = porder.querySum(START, END);
					}
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println("<tr><td colspan=7 align=center>");
					out.println("</table>");
	
				%>
				<br>
				<a href="query.jsp">上一頁</a>
				<a href="../index.jsp">回首頁</a>
			
		<tr>
			<td colspan=7 align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

> 查詢會員和總價範圍

`queryMember.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>查詢會員＆總價</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="queryMember.jsp" >
			
				<table width=350 align=center>
					
					<tr>
						<td align=center><h3>依會員＆總價查詢</h3>
					<tr>
						<td align=center>會員
						<input type="radio" name="member" value="Y" checked >Yes
						<input type="radio" name="member" value="N">NO
						<br>金額:
						從<input type="text" name="start" size="10" value="0" />
						到<input type="text" name="end" size="10" value="0" />
						<input type="submit" value="OK" />
			
				</table>
				
			</form>	
				<hr>
				<%
					// 避免中文產生亂碼
					request.setCharacterEncoding("UTF-8");
					
					int START, END;
				
					String MEMBER = request.getParameter("member");
					String st = request.getParameter("start");
					String en = request.getParameter("end");
					
					ResultSet rs ;
					if ( st == null || en == null || st.length() == 0 || en.length() == 0 )
					{
						rs = porder.queryAll();
					}
					else
					{
						START = Integer.parseInt(st);
						END = Integer.parseInt(en);
						rs = porder.queryMember(MEMBER,START, END);
					}
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println("<tr><td colspan=7 align=center>");
					out.println("</table>");
	
				%>
				<br>
				<a href="query.jsp">上一頁</a>
				<a href="../index.jsp">回首頁</a>
			
		<tr>
			<td colspan=7 align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

注意：

`querySum.jsp`、`queryMember.jsp`

第一次進去時的確 String 等於 null，所以才會顯示出查詢全部。

但是把初始值兩個 0 都去掉只剩空格就會產生出

`java.lang.NumberFormatException: For input string: ""`

代表是輸入空字符，所以多加上 `.length() == 0` 

```java
if ( st == null || en == null || st.length() == 0 || en.length() == 0 )
{
	rs = porder.queryAll();
}
```

<br>

> 修改訂單

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>修改頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="update.jsp" >
			
				<table width=500 align=center>
					
					<caption id="caption">修改訂單</caption>
					
					<tr>
						<td align=center>單號
						<input type="text" name="id" size="3" value="0"/>
						
						桌號
						<select name="desk">
							<option value="A桌">A桌
							<option value="B桌">B桌
							<option value="C桌">C桌
							<option value="D桌">D桌
						</select>
						
						A餐
						<select name="pro1">
							<option value="0">0
							<option value="1">1
							<option value="2">2
							<option value="3">3
							<option value="4">4
							<option value="5">5
							<option value="6">6
							<option value="7">7
							<option value="8">8
							<option value="9">9
							<option value="10">10
						</select>
						
						B餐
						<select name="pro2">
							<option value="0">0
							<option value="1">1
							<option value="2">2
							<option value="3">3
							<option value="4">4
							<option value="5">5
							<option value="6">6
							<option value="7">7
							<option value="8">8
							<option value="9">9
							<option value="10">10
						</select>
						
						C餐
						<select name="pro3">
							<option value="0">0
							<option value="1">1
							<option value="2">2
							<option value="3">3
							<option value="4">4
							<option value="5">5
							<option value="6">6
							<option value="7">7
							<option value="8">8
							<option value="9">9
							<option value="10">10
						</select>
						<br>
						
						會員
						<input type="radio" name="member" value="Y" checked>YES
						<input type="radio" name="member" value="N">NO &nbsp;
						<input type="submit" value="OK" />
			
				</table>
				
			</form>	
				<hr>
				<%
					request.setCharacterEncoding("UTF-8");
					
					int ID, PRO1, PRO2, PRO3;
					
					String id = request.getParameter("id");
					String DESK = request.getParameter("desk");
					String p1 = request.getParameter("pro1");
					String p2 = request.getParameter("pro2");
					String p3 = request.getParameter("pro3");
					String MEMBER = request.getParameter("member");
					
					try
					{
						if(id != null && DESK != null && p1 != null && p2 != null && p3 != null)
						{
							ID = Integer.parseInt(id);
							PRO1 = Integer.parseInt(p1);
							PRO2 = Integer.parseInt(p2);
							PRO3 = Integer.parseInt(p3);
						
							porder.update(ID, DESK, PRO1, PRO2, PRO3, MEMBER);
						}
					} catch (NumberFormatException e) {
						e.printStackTrace();
					}
					
					ResultSet rs = porder.queryAll();
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println("<tr><td colspan=7 align=center>");
					out.println("</table>");
					
				%>
				<br>
				<a href="../index.jsp">回首頁</a>
			
		<tr>
			<td colspan=7 align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

<br>

> 刪除訂單

`delete.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="com.porder, java.sql.ResultSet" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>刪除頁面</title>
<link rel="stylesheet" type="text/css" href="../Content/css/ku.css">
</head>
<body>
	<table width=600 align=center border=1>
	
		<tr>
			<td align=center id="title">
			<jsp:include page="../title.jsp" />
		
		<tr>
			<td height=400 align=center id="content">
			
			<form method="post" action="delete.jsp" >
			
				<table width=350 align=center>
					
					<caption id="caption">刪除訂單</caption>
					
					<tr>
						<td align=center>輸入單號
						<input type="text" name="id" size="3" value="0" />
						<input type="submit" value="OK" />
			
				</table>
				
			</form>	
				<hr>
				<%
					request.setCharacterEncoding("UTF-8");
					
					String ID = request.getParameter("id");
					
					if( ID != null)
					{
						porder.delete(Integer.parseInt(ID));
					}
					
					ResultSet rs = porder.queryAll();
					
					out.println("<table width=500 align=center>");
					out.println("<tr bgcolor=yellow align=center><td>ID<td>DESK<td>A餐<td>B餐<td>C餐<td>會員<td>總價");
					
					while(rs.next())
					{
						out.println
						(
							"<tr align=center>"+
								"<td>"+rs.getInt("id")+
								"<td>"+rs.getString("desk")+
								"<td>"+rs.getInt("pro1")+
								"<td>"+rs.getInt("pro2")+
								"<td>"+rs.getInt("pro3")+
								"<td>"+rs.getString("member")+
								"<td>"+rs.getInt("sum")
						);
					}
					
					out.println("<tr><td colspan=7 align=center>");
					out.println("</table>");
				%>
				<br>
				<a href="../index.jsp">回首頁</a>
			
		<tr>
			<td colspan=7 align=center id="end">
			<jsp:include page="../end.jsp" />
	
	</table>
</body>
</html>
```

