# Spring Boot API

## Terminologies

`docker run --name mysql-8.0.36 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -d arm64v8/mysql:8.0.36-oracle`

MVC SoC separation of Concern

aspect oriented lập trình hướng khía cạnh

## Maven

[Maven overview video by Marco Codes](https://www.youtube.com/watch?v=Xatr8AZLOsE&t=1140s)

Maven + github action dùng cho CI/CD. Mỗi lần commit thì sẽ chạy github action để build, test automatically để application không bị lỗi.

You can have multiple module with their own maven/gradle build files sort of like `.gitignore` file.

Maven stored all dependency in a file `.m2` chứ không phải mỗi project mỗi tải về như Nodejs.

You can install maven using homebrew. The command is `mvn` not `maven`.
[This video](https://www.youtube.com/watch?v=Xatr8AZLOsE&t=69s) demonstrates how to install from zip file and add to PATH.

The file `mvnw` is called "maven wrapper". It is usually found is Spring Boot projects and is used on Mac & Linux. The other file `mvnw.cmd` is for window with the .cmd postfix.
If your project have the maven wrapper. Your computer don't need to have maven install. You can use the command `mvnw -v` instead. But somebody need to have `mvn` installed on their machine to create the maven wrapper file in the first place using `mvn wrapper:wrapper`

`mvn validate` or `mvnw validate` check coi `pom.xml` file có lỗi gì không.

The file `pom.xml` define your project.

Maven plugin khác dependency. Plugin dùng cho testing, deploying. Dependency dùng cho program.

groupId & artifactId là unique trên web repository của maven.

`mvn clean` delete the target folder
`mvn compile` compile code in `src/` and put them inside `target/` folder

`mvn compile test`

`mvn package` tạo file `.jar` để send for friend.

### Gradle

[video by Marco](https://www.youtube.com/watch?v=gKPMKRnnbXU&list=PLIRBoI92yMam1HaUYrMAaPdbZKV1BFW0F&index=2) Coi video maven của ông này trước.

**Apache Groovy** is a Java-syntax-compatible object-oriented programming language for the Java platform. Nó giống như python for Java user.

**Gradle** is a build automation tool for multi-language software development. It introduces a Groovy- and Kotlin-based domain-specific language contrasted with the XML-based project configuration used by Maven. Nếu dùng groovy để viết config cho gradle thì được gọi là **groovy gradle project**. Còn nếu dùng Kotlin thì khác.

`gradlew` là wrapper. Cái `gradlew.bat` là cho máy window.

`gradlew build`

## Java Servlets, Spring MVC, Spring Boot and Spring without Boot

A **Java Servlet** is a Java class that extends the capabilities of servers, typically web servers, by responding to various types of requests, most commonly HTTP requests. Servlets are a key component in building dynamic web applications using Java.

**JSP, or JavaServer Pages**, is a server-side technology that enables developers to create dynamic web pages by embedding Java code directly within HTML. It is an extension of Java Servlets and part of the Java EE (now Jakarta EE) platform.  
JSP is converted into servlet & HTML.

**Spring MVC** is a web application framework built on top of Servlets. It provides a higher level of abstraction, simplifying web development through conventions, annotations, and a clear architectural pattern (Model-View-Controller).  
Không dùng react mà dùng thymeleaf cho phần "View". This is a server-side rendering (SSR) technology.

- Spring MVC:
  * manual configuration (XML config)
  * Typically deployed as a WAR file in a separate application server.
  * more boilerplate code
- Spring Boot:
  * opinionated default configs, reducing manual setup.
  * Can be deployed as a standalone executable JAR with an embedded server.
  * Minimizes boilerplate code

Spring web bao gồm restAPI (`@RestController`) & MVC (`@Controller`)

Web container: Apache Tomcat, Glassfish, etc

`web.xml` is the Deployment Descriptor map HTTP request với từng servlet xử lý cho phù hợp.

Spring boot sẽ lo phần: configuration xml, embedded tomcat server. Developer dùng annotation like (`@Component`) để "communicate" with spring boot.  
Spring boot has its opinionated configurations whether you like it or not.

Spring Framework without boot mình phải tự làm configuration bằng một file `.xml` specifying which class is component.
Spring MVC is a web framework built on Java Servlet API. Tự config, không có embeded Tomcat server

spring framework 6.x còn spring boot là 3.x version.

spring boot tích hợp sẳn Tomcat, nên gọi là no-server.

Spring Profiles = configuration.

Nếu không xài spring boot annotation thì sẽ phải tự config xml vài cái sau đây:

- Which class is component class để container biết tạo object (Spring bean)
- setter injection, constructor injection, autowire

Spring Boot cũng có configuration trong file `application.properties` nhưng nó dễ hơn xml nhiều. Và Spring chủ yếu dùng annotations.  
Behind the scene, spring Boot still uses spring Framework.

Console java app có thể run in the jvm. Nhưng web app thì phải run in a web server (web container, tomcat) để có thể nhận và response HTTP requests. Java servlets run in the container.

Ultimately, behind the scene, spring, particularly Spring MVC, use servlets and it run on tomcat. Nhưng Spring embed tomcat in the project nên mình không phải install.

web server only send JSON data and not the layout. Native app có layout lúc tải về. Web app thì có react, angular lo phần HTML.

- `@RestController` REST api = Representational State Transer. It means you transer only the data (state) to the client not the layout.
- `@Controller` có thể trả về client data hoặc layout (Thymeleaf, jsp - JavaServer Pages)

## Model, POJO, java Bean, spring beans

A POJO, or **Plain Old Java Object**, is a simple Java object that is NOT bound by any special restrictions or framework-specific requirements. It's essentially a regular Java class that can be used in any Java application and is not tied to any specific framework like EJB (Enterprise JavaBeans) or JPA.  
POJOs are typically straightforward classes used primarily for **data storage**, often containing private fields and public getter and setter methods to access and modify these fields.

These POJOs can also be considered JavaBeans if they follow the JavaBean conventions (private fields, public getters/setters, a no-argument constructor, and implementing Serializable).

While these domain model POJOs/JavaBeans are used within a Spring application, they are not necessarily Spring Beans themselves unless explicitly configured as such (e.g., by annotating them with @Component, @Entity, or defining them with @Bean in a @Configuration class).

### Spring bean vs. Java Beans

Spring beans:

- Have public default (no argument) constructors
- allow access to their properties using accessor (getter and setter) methods
- implement `java.io.Serializable`
- Spring beans need not always be JavaBeans. Spring beans might not implement the java.io.Serializable interface, can have arguments in their constructors, etc.

- **Spring Bean** là objects được spring quản lý trong **Application Context** hay còn gọi là **IoC Container**.
- `@Component, @Service, @Repository, @Controller` là beans.

A class annotated with `@Entity` in Java Persistence API (JPA) is not typically managed as a Spring Bean by default. Instead, JPA entities are managed by an EntityManager which handles their lifecycle (persistence, updates, deletion) in relation to a database.  
While Spring Data JPA simplifies working with JPA entities by providing repository interfaces, the entities themselves are usually instantiated by the persistence provider (like Hibernate) and not directly by the Spring IoC container.

Khi có những utility object chỉ dùng một lần thì tự `new` object khỏi xài bean cũng được.

## Controller layer

No business logic inside controller.

Controller only for accepting the requests and responding to the clients.

Service chứa business logic

`@Service` is `@Component` behind the scene. So spring will create service objects in its container and inject cho mình xài.

Spring web có include **Jackson** library to convert data từ dạng java objects sang dạng JSON and vice versa.

- `@PathVariable` là gắn vào url `localhost:8080/student/5`
- `@RequestParam` là dùng dấu ? `student?id=5`

HTTP methods

Web:

- Get (Read)
- Post (CUD)

API:

- GET
- POST, PUT DELETE

Khi làm API, post add user key-value phải giống bên entity class. For example "firstName" and "firstname" là khác nhau.

DTO dùng để giảm tải, enhance security. Mapper dùng để chuyển đổi data qua JPA.

why DI phải gọi interface ko gọi class.

Tại sao service có interface mà controller không có:

Services encapsulate business logic. An interface provides an abstract contract for this logic, hiding the concrete implementation details. This allows for easy swapping of service implementations without affecting the calling code (e.g., a controller).

Interfaces facilitate easier unit testing. Controllers are often tested in integration tests that involve the web layer, rather than strictly unit tests that mock away all dependencies. Unit testing individual methods within a controller can still be done, but the primary benefit of interfaces (swapping implementations for testing) is less critical here.

Controllers often interact directly with the web framework's features (e.g., request mapping, data binding, view rendering). These interactions are typically tied to the framework's specifics and are not easily abstracted by an interface.

The Dependency Inversion Principle (DIP) is a SOLID design principle that states high-level and low-level modules should depend on abstractions, not concrete implementations. This leads to more flexible and maintainable code.

## Repository layer

N-Tier Architecture (the repository pattern)

Các layer chỉ được gọi layer ngay bên dưới nó. Không được gọi nhảy cóc. Ví dụ controller chi được gọi service, không được gọi repository.

There is no syntactic difference between a **JavaBean** and another class -- a class is a JavaBean if it follows the following standards. Java bean khác với spring bean.

## IoC and Dependency Injection (DI)

- 1 and only one service instance can be used by multiple classes.
- The classes that use DI can be shorter and easier to understand.

Tầng controller phải tạo object cua tầng service bằng `new`. Rồi Service lại phải tạo object của tầng repository bằng `new`.  
Nếu có khoảng 100 classes thì tạo và quản lý object rất mệt. Developer chỉ muốn tập trung phần business logic, phần **Object Creation** cần được đơn giản hóa. Thế là nó đẻ ra khái niệm **IoC (Inversion of Control)**.

Nếu mình tự tạo objects & manage thì mình là người control. Nếu mình đùn đẩy trách nhiệm đó cho bên khác thì đó là IoC. IoC là một khái niem, còn Dependency Injection là một technique giúp hiện thực hóa khái niệm IoC.

DI is the actual implementation of the **IoC philosophy** in Java and Spring.

- IoC is a **principle**.
- DI is the Design Pattern, in Spring, to achieve IoC. DI chỉ đảm nhiệm một phần của IoC, không phải hoàn toàn IoC.

Spring Framework sẽ tạo và quản lý object rồi inject vào cho developer xài. Developer không dùng `new` để tạo object mà ask Spring to inject the object.

- Constructor Injection: used when variable must have value when create object
- Setter Injection: used for optional variables
- Field Injection: Loose Coupling, not recommended

Object are created inside the JVM. Spring has its own container inside the JVM called the **IoC Container** (or Spring container). This IoC container is nothing but an object of type `Application Context`.

Spring will create object inside its IoC container. Nếu tự tạo bằng `new` thì tạo trong JVM không tạo trong IoC container.

### Autowiring

`main()` need object of `Developer`. `Developer` lại muốn dùng object của `Laptop`.

wiring = connect. autowired = auto connect

`@Autowired` tìm class để connect **By Type** `Laptop` not by name of the object `laptop`. Và nó dựa trên quan hệ IS-A.

`Laptop` IS-A `Computer` nên Spring sẽ tự động biết và inject `Laptop` object vào field khai là `Computer`.

## Aspect Oriented Programming (AOP)

In the context of Aspect-Oriented Programming (AOP), a **cross-cutting concern** refers to a functionality or aspect of a system that affects multiple, distinct parts of the application, often spanning across different modules or layers. These concerns are "cross-cutting" because they don't fit neatly into a single, isolated module but rather "cut across" the primary business logic.

## Bugs & fixes

`There is already 'accountController' bean method` => lỗi controller bị trùng url [stack overflow](https://stackoverflow.com/questions/54885516/springboot-gets-an-error-there-is-already-controller-bean-method-when-two-get)
Nếu mở 2 spring api ở 2 window intellij sẽ bị đụng port 8080

## References

vào spring.io -> chọn 1 projects -> tab learn -> reference Doc hoặc API doc. API doc chi tiết hơn reference Doc
