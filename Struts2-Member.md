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

