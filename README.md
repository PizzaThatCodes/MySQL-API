# MySQL-API

Simple Java API for MySQL

## Installation

**Notice:** The version can be changed. See the badge at the top of this page for the latest version.

### Maven

```xml
<repositories>
    <repository>
        <id>pyzatech-repo-releases</id>
        <url>https://maven.pizzalover.dev/releases</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.pyzatech</groupId>
        <artifactId>mysql-api</artifactId>
        <version>1.0</version>
    </dependency>
</dependencies>
```

### Gradle
```Gradle
allprojects {  
 repositories {  
  maven { url 'https://maven.pizzalover.dev/releases' }  
 }  
}  
 
dependencies {  
   implementation 'com.pyzatech:mysql-api:1.0'  
}  
```

## Usage

### Init
```Java
MySQL sql = new MySQL();
```

### Set Credentials
```Java
sql.setHost("localhost");
sql.setUser("sqluser");
sql.setPassword("sqlpassword");
sql.setDb("database");

sql.setPort(3306); // Optional. Default: 3306
sql.setDebug(false); // Optional. Default: false
```


### Connect to MySQL
```Java
boolean: sql.connect();
```

Database interaction methods check the connection before each operation and reconnect if it is no longer valid. If the connection drops while a query is already running, that operation returns its normal failure result and the API reconnects for the next operation; it does not automatically replay writes that may have already reached MySQL.

### Checks
```Java
boolean: isConnected();
boolean: isDebug();
```

### MySQL Data interaction 
**Default way**
```Java
boolean: sql.tableInsert("myTable", "name, age", "Robert", "32"); // INSERT Statement
boolean: sql.rowUpdate("myTable", new UpdateValue("age", "45"), "name = 'Robert'"); // UPDATE Statement
Result: sql.rowSelect("myTable", "*", "name = 'Robert'"); // SELECT Statement
boolean: sql.custom("DELETE * FROM myTable;"); // Custom SQL Statement
```

**Request builder**
```java
// sql.tableInsert();
Insert ins = new Insert();
ins.setTable("myTable");
ins.setColumns("column1, column2");
ins.setData("value1", "value2");
boolean: sql.tableInsert(ins);  // Multiple Builders accepted (sql.tableInsert(ins, ins2, ins3)


// sql.rowUpdate();
Update up = new Update();
up.setTable("myTable");
up.setValue(new UpdateValue("column1",  "value3"));
up.setFilter("column2 = 'value2'");
boolean: sql.rowUpdate(up);  // Multiple Builders accepted (sql.rowUpdate(up, up2, up3)


// sql.rowSelect();
Select select = new Select();
select.setTable("myTable");
select.setColumns("*"); // Optional default '*'
select.setFilter(""); // Optional default ''
Result: sql.rowSelect(select); // Only one Builder accepted!
```


#### Result
 
 ```Java
 List<Row> rowList = res.getRows();
 for(Row r : rowList) {
     System.out.println(r.get("column1") + "   " + r.get("column2"));
 }
 ```

### Close Connection
```Java
sql.close();
```
