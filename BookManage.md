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

***org.maodel*** , 需要的 class ( 與 資料庫中 Table 有關 )

***org.dao*** , 管理資料庫的 method

***org.action*** , 管理畫面的操作流程 (Control) 與 View 有關

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

public class Lend implememts Serializable {

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

<h3>

修改 主索引 assigned 變成 native

加上 屬性 catalog="SCHOOL" 對應到資料庫的Schema

</h3>

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
		<id name="id" type="java.lang.Integer>
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
		<property name="name" type="java.lang.String>
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
		<property name="bookId" type="java.lang.String>
			<column name="BOOKID" />
		</property>
		<property name="readerId" type="java.lang.String>
			<column name="READERID" />
		</property>
		<property name="bookName" type="java.lang.String>
			<column name="BOOKNAME" />
		</property>
		<property name="publisher" type="java.lang.String>
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