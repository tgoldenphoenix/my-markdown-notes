# JDBC & Hibernate

Giải quyết nhu cầu **Persistence data** in Java application.

## JDBC

Java Database Connectivity (JDBC) API and the Java Persistence API (JPA).

JDBC có nhược điểm. Trong java, data được stored trong objects such as `Student`. Dung JDBC phải viết perpared statements lấy data về. Rồi sau đó phải convert data về java object rất bất tiện.  
Khi dùng ORM Framework (Object-Relationship mapping) thì không cần viết JDBC insert query vào DB, mình chỉ cần tạo class, sau đó tạo object chứa properties đưa cho ORM tool rồi nó tự insert, get data, update cho mình. Mình không phải viết SQL queries trong code Java.

JDBC is an API provided by Java. Hibernate uses JDBC (Java Database Connectivity) internally.

Hibernate is an ORM tool. Muốn dùng Hibernate phải tải 2 maven dependencies:

1. postgres JDBC driver. Hibernate uses JDBC under the hood.
2. Hibernate core

- `Statement`:
  - run static SQL only once, no parameter
  - Vulnerable to SQL injection attacks. User can input SQL code vào field password & không có escape thì cả.
- `PreparedStatement`:
  - run many times, with parameter
  - Safe against SQL injection since it use `?` placeholders for parameters. The database driver automatically escapes or handles the parameter values, preventing them from being interpreted as executable SQL code.

JDBC chạy nhanh hơn hibernate, nhưng khó code hơn (tradeoff).

```java
try (Statement stmt = con.createStatement()) {
  String tableSql = "CREATE TABLE IF NOT EXISTS employees"
  + "(emp_id int PRIMARY KEY AUTO_INCREMENT, name varchar(30),"
  + "position varchar(30), salary double)";
  stmt.execute(tableSql);
  // another example
  Statement stmt = con.createStatement();
  ResultSet rs = stmt.executeQuery("SELECT a, b, c FROM Table1");

}
// preparedStatement
String updatePositionSql = "UPDATE employees SET position=? WHERE emp_id=?";
try (PreparedStatement pstmt = con.prepareStatement(updatePositionSql)) {
  pstmt.setString(1, "lead developer");
  pstmt.setInt(2, 1);
  int rowsAffected = pstmt.executeUpdate();
}
```

- `executeQuery()` for SELECT instructions; returns an object of type `ResultSet`
- executeUpdate() for updating the data or the database structure
- execute() can be used for both cases above when the result is unknown

## JPA & Hibernate

**JPA (Java Persistence API)** is a Java standard/specification (it is NOT a tool) that allows us to bind Java objects to records in a relational database. It’s one possible approach to do ORM, allowing the developer to retrieve, store, update, and delete data in a relational database using Java objects. Several implementations are available for the JPA specification.  
Don't be confused with JSP Servlet.

Hibernate ORM tool phải implement the JPA standard. Và những ORM tools khác cũng phải follow theo JPA standard.

Hide the SQL from the developer, everything is done with java classses and annotation.

**Hibernate ORM** (or simply Hibernate) is an object–relational mapping tool for the Java programming language. It provides a framework for mapping an object-oriented domain model to a relational database.

Hibernate provides a SQL inspired language called **Hibernate Query Language (HQL)** for writing SQL-like queries against Hibernate's data objects.

Java thì quan tâm class name. Hibernate thì quan tâm `@Entity` name (HQL - Hibernate Query Language). SQL thì quan tâm `@Table` name.

**JPQL, or Jakarta Persistence Query Language** (formerly Java Persistence Query Language), is a platform-independent, object-oriented query language used within the Jakarta Persistence API (JPA) specification. It enables developers to interact with databases using Java objects and their relationships, abstracting away the direct manipulation of SQL and database tables.

JPQL queries are **based on java class** and their properties, not on DB tables name and table column names

**Hibernate ORM** (or simply Hibernate) is an **object–relational mapping**  tool for the Java programming language.

HQR = Hibernate Query Language
JPQL = Jakarta Persistence Query Language

**JPA** ([Java persisten API](https://en.wikipedia.org/wiki/Jakarta_Persistence)) is a Jakarta EE application programming interface specification that describes the management of relational data in enterprise Java applications. \
"Persist" means to add/update your java object into the DB.

You can use the JPA specification annotation with hibernate as a _provider_. You only use HQR when you need specific hibernate feature. If you use a framework like Spring data, the choice had already been made for you.

Inside a class if you have any [auxiliary constructor](https://www.geeksforgeeks.org/scala-auxiliary-constructor/), you must include at least one empty constructor so hibernate can construct empty objects when it fetches data out of the DB table.

[hibernate & JPA Crash Course](https://www.youtube.com/watch?v=xHminZ9Dxm4&t=23s) by my man Marco very good!

[hibernate with postgresql](https://www.youtube.com/watch?v=v1IFQWzuSrw) (video) còn đây là [blog post](https://www.sqlservercentral.com/articles/postgresql-hibernate-integration) thích cái nào coi cái đó

A [SessionFactory](https://docs.jboss.org/hibernate/orm/6.5/javadocs/org/hibernate/SessionFactory.html) represents an "instance" of Hibernate

[jsp hibernate CRUD with source](https://www.youtube.com/watch?v=R-jy6g-DAdU)

**JPA Criteria Queries API**/ dynamic SQL allows you to dynamically change your SQL query base on the user input in your form.

## SessionFactory

Why can we create object of an interface `SessionFactory` in Hibernate?

No, in Java you cannot instantiate an interface directly.  
However, you can have: **A reference variable of an interface type**, which points to an **object of a class that implements that interface**.

`SessionFactory factory = configuration.buildSessionFactory();`

Here:

- `SessionFactory` is an interface.
- `buildSessionFactory()` returns an instance of a class that implements SessionFactory (e.g. SessionFactoryImpl).
- This is polymorphism.

## Mapping

Identifying the owning entity within a relationship is important because the owning side is most often, if not always, where the `@JoinColumn` annotation must be specified.  
Bảng có JoinColumn cũng là bảng có `@ManyToOne`.

In the Post entity the `@JoinColumn` annotation is specified above the field Series to denote the foreign key to be used to identify a Post’s respective Series. JoinColumn sẽ sinh ra một cột trong database. Bảng chứa foreign key này cũng chính là the owning entity in the relationship.

If the relationship between the Post and Series entities was required to be bidirectional, meaning the Post entities should be accessible from the Series, the inverse side of the relationship (Series) must be annotated with `@OneToMany`, with a mappedBy element defined.  The mappedBy element should point to the field on the owning side of the relationship (Post) that specifies the `@JoinColumn` used to associate the entities.

`mappedBy` trùng với tên thuộc tính của entity class, còn

