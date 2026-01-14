# Spring Web Framework Notes

## Terminologies

`docker run --name mysql-8.0.36 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -d arm64v8/mysql:8.0.36-oracle`

MVC SoC separation of Concern

aspect oriented lập trình hướng khía cạnh

## Maven

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

### Lombok

Lombok’s magic is applied at compile time, so there’s no need for it to be available at run time. Excluding it like this keeps it out of the resulting JAR or WAR file.

```xml
pom.xml

<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <configuration>
        <excludes>
          <exclude>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
          </exclude>
        </excludes>
      </configuration>
    </plugin>
  </plugins>
</build>
```

The Lombok dependency provides you with Lombok annotations (such as `@Data`) at development time and with automatic method generation at compile time. But you’ll also need to add Lombok as an extension in your IDE, or your IDE will complain, with errors about missing methods and final properties that aren’t being set. Visit <https://projectlombok.org/> to find out how to install Lombok in your IDE of choice.

It bears repeating that when using Lombok, you must install the Lombok plugin into your IDE. Without it, your IDE won’t be aware that Lombok is providing getters, setters, and other methods and will complain that they are missing.

### POM

For maven, `pom.xml` is the `build file`

- spring dependency:
  * a library (a `.jar` file) that your code needs to compile and **run**. It is included in your final application.
- Spring plugins:
  * Only used At Build-time (when you type `mvn clean install`). 

You need to worry only about which version of Spring Boot you’re using (inside `<parent>spring-boot-starter-parent</>`). You can trust that the versions of the libraries brought in transitively (spring `starter` dependencies) will be compatible for a given version of Spring Boot.

### JAR vs WAR

- JAR (Java Archive):
  * Purpose: Primarily used for packaging and distributing standalone Java applications, libraries, or components.
  * Content: Contains compiled Java classes, resources (like images or property files), and a META-INF directory with metadata.

- WAR (Web Application Archive):
  * Purpose: Specifically designed for packaging and deploying web-based Java applications (servlets, JSPs, HTML, CSS, JavaScript, etc.).
  * Content: Contains all the components of a web application, including compiled Java classes, resources, static web files, and a WEB-INF directory. The WEB-INF directory is crucial and contains web.xml (the deployment descriptor), classes (compiled Java code), and lib (dependent JAR files).
  * Deployment: Deployed within a web server or application server environment (e.g., Apache Tomcat, Jetty, JBoss), not as a standalone executable.
  
### XML

`xmlns` là viết tắt của `XML namespace`, dùng để định danh các thẻ và thuộc tính trong tài liệu XML, giúp phân biệt các yếu tố có tên giống nhau nhưng thuộc các bộ khác nhau.

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

While these domain model POJOs/JavaBeans are used within a Spring application, they are not necessarily Spring Beans themselves unless explicitly configured as such (e.g., by annotating them with `@Component`, `@Entity`, or defining them with @Bean in a @Configuration class).

### Spring bean vs. Java Beans

JavaBeans là những Class có thuộc tính được khai `private`, muốn truy cập những thuộc tính này phải dùng setters, getters. Java Bean có mục đích là:

- Gather together and store related information of an object into a Class. Ví dụ `SinhVien` là một nhóm đối tượng có các thuộc tính như họ tên, giới tính. Mục đích cuối cùng là lưu trữ dữ liệu trong quá trình xử lý nghiệp vụ một cách tiện lợi nhất.
- Vì Java Bean để lưu dữ liệu nên class JB không có chứa methods xử lý nghiệp vụ (business logic, tính toán, truy cập database).

Trong DOTNET cũng có bean. Javascript cũng có bean.

Nếu không có Java Bean.

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

## Spring Basics

At its core, Spring offers a `container`, often referred to as the `Spring application context`, that creates and manages application components. These components, or `beans`, are wired together inside the Spring application context to make a complete application

The act of wiring beans together is based on a pattern known as `dependency injection (DI)`. Rather than have components create and maintain the life cycle of other beans that they depend on, a `dependency-injected application` relies on a separate entity (the container) to create and maintain all components and inject those into the beans that need them. This is done typically through constructor arguments or property accessor methods.

Historically, the way you would guide Spring’s application context to wire beans together was with one or more XML files that described the components and their relationship to other components.\
In recent versions of Spring, however, a Java-based configuration is more common (using annotation).

Java-based configuration offers several benefits over XML-based configuration, including greater type safety and improved refactorability. Even so, explicit configuration with either Java or XML is necessary only if Spring is unable to automatically configure the components.

---

Spring Boot applications tend to bring everything they need with them and don’t need to be deployed to some application server (Tomcat). You never deployed your application to Tomcat—Tomcat is a part of your application!

## Controller Layer

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

## Spring MVC

Spring MVC chỉ có `@GetMapping` & `@PostMapping`, API mới có Put, Delete

Spring APIs (REST Controllers) typically use `@RequestBody`, while Spring MVC (Web Controllers) use `@ModelAttribute`.

`Model` is an object that ferries data between a controller and whatever view is charged with rendering that data. Ultimately, data that’s placed in `Model` attributes is copied into the servlet request attributes, where the view can find them and use them to render a page in the user’s browser.

methods that are also annotated with `@ModelAttribute` are invoked when a request is handled and will construct the model object.

## IoC and Dependency Injection (DI)

DI and AOP are central to everything in Spring. Thus you must understand how to use these principal functions of Spring to be able to use the rest of the framework.

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

DI is a way of associating application objects such that the objects don’t need to know where their dependencies come from or how they’re implemented. Rather than acquiring dependencies on their own, dependent objects are given the objects that they depend on. Because dependent objects often only know about their injected objects through interfaces, coupling is kept low.

### Autowiring

`main()` need object of `Developer`. `Developer` lại muốn dùng object của `Laptop`.

wiring = connect. autowired = auto connect

`@Autowired` tìm class để connect **By Type** `Laptop` not by name of the object `laptop`. Và nó dựa trên quan hệ IS-A.

`Laptop` IS-A `Computer` nên Spring sẽ tự động biết và inject `Laptop` object vào field khai là `Computer`.

## Aspect Oriented Programming (AOP)

In the context of Aspect-Oriented Programming (AOP), a **cross-cutting concern** refers to a functionality or aspect of a system that affects multiple, distinct parts of the application, often spanning across different modules or layers. These concerns are "cross-cutting" because they don't fit neatly into a single, isolated module but rather "cut across" the primary business logic.

Like DI, AOP supports **loose coupling** of application objects. But with AOP, **application-wide concerns** (such as transactions and security) are decoupled from the objects to which they’re applied.

AOP enables you to centralize in one place—an aspect—logic that would normally be scattered throughout an application. When Spring wires your beans together, these aspects can be woven in at runtime, effectively giving the beans new behavior.

## Java EE

Java EE (now called Jakarta EE) is primarily used for building enterprise-level web-based application (web app, backends, api).

- javax
  * Java EE (Enterprise Edition)
  * The **older** namespace for Java EE APIs, such as javax.servlet and javax.persistence
  * Applications using Java EE 8 and earlier versions use javax packages.
- jakarta
  * Jakarta EE (Jakarta Enterprise Edition)
  * The new namespace for Jakarta EE APIs, such as jakarta.servlet and jakarta.persistence
  * Applications built on Jakarta EE 9 and later versions use jakarta packages.
- Reason for change: Trademark and intellectual property issues after Oracle donated Java EE to the Eclipse Foundation; To comply with the new branding and avoid trademark restrictions
- Migrating to Jakarta EE requires updating all import statements from `javax.*` to `jakarta.*`

`DispatcherServlet` = front controller = central `Servlet`

EJB  (Enterprise JavaBeans) cũng là một technologies used for developing enterprise Java applications nhưng nó khác Spring.

## Thymeleaf

Trong forms dùng `selection expressions`: `*{firstName}`

**Message expression** trong file `application.properties`. `#{welcome.text}`

**Link expression** `@{manage/edit}`

`<img th:src="@{/images/TacoCloud.png}"/>`

This line uses a Thymeleaf `th:src` attribute and an `@{...}` expression to reference the image with a context-relative path.

## JSP Servlet

- JSP servlet & tất cả modern web framework có 8 chữ cốt lõi:
  1. get-post
  2. redirect-forward
  3. attribute vs parameter
  4. request vs session
  5. rảnh thì học thêm cookie (9 chữ tất cả)

an `.html` file is static while `.jsp` file is dynamic html. You can inject data into jsp html.

Phải dùng JDBC, tạo statement

mỗi file servelt chỉ có một endpoint

- `doGet()`:
  - Primarily used for retrieving data from the server.
  - Parameters are appended to the URL as a query string (e.g., `example.com/search?query=java`).
  - Suitable for non-sensitive data and operations that do not modify server-side resources.
  - Limited data length due to URL length restrictions.
- `doPost()`:
  - Primarily used for submitting data to the server to create or update resources.
  - Parameters are sent in the request body, not visible in the URL.
  - Suitable for sensitive data (e.g., passwords, personal information) and operations that modify server-side resources (e.g., form submissions, file uploads).
  - No practical limit on data length.

- `forward`:
  - Cả 2 (forward & redirect) đều là chuyển trang phía server, client không liên quan.
  - Forward chuyển dữ liệu đi theo. **KHÔNG** tạo request object mới mà chỉ chuyển tiếp request. The browser's URL remains unchanged.
  - Request attributes and parameters are preserved during the forward.
  - If the previous scope is required, but the application also wants to perform an internal action then use forwarding.
  - login thành công => forward kèm theo dữ liệu user
- `sendRedirect`:
  - Chuyển trang không kèm theo dữ liệu. Force the client to make a new request. The browser's URL is changed.
  - Redirect chạy nhanh hơn forward => ưu điểm của re-direct.
  - To discard the scope or if the new content isn’t associated with the original request – such as a redirect to a login page or completing a form submission – then use redirecting.
  - This is useful when we want to send the user to a different domain or server outside of our web application.
  - đăng nhập thất bại, thông báo server đang bị lỗi

Trong spring thường bỏ redirect và chỉ xử dụng forward.

```java
// re-direct
response.sendRedirect("https://www.google.com");

// forward
RequestDispatcher rd = request.getRequestDispatcher("products.jsp");
rd.forward(request, response);
```

JSPs are the **high-level abstraction** of servlets because they are converted into servlets class before execution begins. Because the JSP is built on top of the servlet, it has access to all important Java APIs such as JDBC, JNDI, and EJB.
JSP và Servlet giống y chang nhau, same capability.
Servlet có thể render giao diện, nhưng nếu chèn code html vào class server của java thì rất dài & rồi rắm. Nên sau này nó mới ra đời JSP.

Trong java bản chất chỉ có class, jsp không phải class nên nó phải convert về servlet class before execution.

Unfortunately, JSPs are slow to compile, hard to debug, leave basic form validation and type conversion to the developer, and lack support for security.

In light of the MVC design pattern, the servlet acts as a controller and JSP as a view.

---

- `Attritube`:
  * diễn ra ở server; attribute là dữ liệu do phía server tự bịa đặt ra attach to the request client gởi lên.
  * Server có thể forward request cùng với attribute. Một servlet nhận request từ client, set attribute vào request đó rồi forward request cho một jsp/servlet khác nhận attribute xử lý logic rồi render ra html trả về client.
  * you set an attribute in a Servlet and read it from a JSP. These can be used for any object, not just string.
  * Thuộc dạng string attribute-object data
- `Parameter`:
  * là dữ liệu ở phía client gởi cho server (trong url), server sẽ nhận bằng `requesst.getParameter()`. Server không thể set parameter mà chỉ có thể set attribute.
  * For example `http://example.com/servlet?parameter=1&name=anhao`
  * Parameter can only return String. Số `1` là string không phải number

- Trong Spring REST API thì:
  * `@RequestParam` là `?brand=apple`; case in-sensitive, `Apple` hay `apple` both ok;
  * `@PathVariable` là `product/{productId}`
  * `RequestBody` là raw json inside body

- `HttpServletRequest`:
  * Scope: Represents a single client request to the server. Its attributes are available only for the duration of that specific request.
  * Lifetime: Created when a client sends a request and destroyed after the server sends the response.
  * `Purpose`: Used to access data sent by the client in the current request (e.g., form parameters, request headers) and to store data that is only needed for processing that specific request (e.g., query results before rendering a JSP).
  * Example: Retrieving a form field value using request.getParameter("fieldName").
- `HttpSession`:
  * Scope: Represents a continuous interaction between a specific client and the web application over multiple requests. Its attributes are available across all requests within that session.
  * Lifetime: Created when a client first interacts with the application and remains active until explicitly invalidated, timed out due to inactivity, or the browser is closed.
  * Purpose: Used to maintain stateful information about a particular user across multiple pages or requests (e.g., user login status, shopping cart contents, user preferences).
  * Example: Storing a user ID after successful login using session.setAttribute("userId", user.getId()).

### DispatcherServlet

The **DispatcherServlet** is the front controller in the Spring MVC framework that acts as a single entry point for all incoming HTTP requests, routing them to the appropriate controller for processing and then delegating the response to the appropriate view.

### Mapping servlet

- Có 2 cách mapping servlet:
  * dùng annotation
  * Dùng `web.xml`

## Strut

Apache Struts, commonly referred to as Struts in the context of Java, is an open-source, free framework for building Java web applications. It was created by the Apache Software Foundation and primarily focuses on implementing the Model-View-Controller (MVC) architectural pattern.

## Bugs & fixes

`There is already 'accountController' bean method` => lỗi controller bị trùng url [stack overflow](https://stackoverflow.com/questions/54885516/springboot-gets-an-error-there-is-already-controller-bean-method-when-two-get)
Nếu mở 2 spring api ở 2 window intellij sẽ bị đụng port 8080

## References

vào spring.io -> chọn 1 projects -> tab learn -> reference Doc hoặc API doc. API doc chi tiết hơn reference Doc
