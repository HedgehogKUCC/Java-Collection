# Struts2 + Hibernate

MySQL

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
	
PRIMARY KEY('id) )

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

public class Login {

	private Integer id;
	private String name;
	private String password;
	private boolean role;
	
	// Getters / Setters
```

```java
package org.model;

public class Book {

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

import java.util.Date;

public class Student {

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

import java.util.Date;

public class Lend {

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


