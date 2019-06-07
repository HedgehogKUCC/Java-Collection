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




