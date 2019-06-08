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

/src/com New Java Class

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
        return sum;
    }
}
```

<br>

`order` 點右鍵 `Propertiesd` 點 Build &nbsp; >>> &nbsp; Documenting

Document Additional Tags :

- [x] @author
- [x] @version

Browser Window Title : ***點餐系統API***

<br>

`order` 點右鍵 `Clean and Build`

`order` 點右鍵 `Generate Javadoc`

將產生出來的資料從 `dist 資料夾` 裡面 Copy 至 `OrderSystemAPI 資料夾`

還有將 `mysql-connector-java-8.0.13.jar` 放入 `OrderSystemAPI 資料夾`

**這樣就打包好可以給其他人做使用**



<br>

