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




