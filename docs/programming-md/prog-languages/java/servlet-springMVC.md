# Servlet, JSP & Thymeleaf

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

- **Attritube**:
  * diễn ra ở server; attribute là dữ liệu do phía server tự bịa đặt ra attach to the request client gởi lên.
  * Server có thể forward request cùng với attribute. Một servlet nhận request từ client, set attribute vào request đó rồi forward request cho một jsp/servlet khác nhận attribute xử lý logic rồi render ra html trả về client.
  * you set an attribute in a Servlet and read it from a JSP. These can be used for any object, not just string.
  * Thuộc dạng string attribute-object data
- **Parameter**:
  * là dữ liệu ở phía client gởi cho server (trong url), server sẽ nhận bằng `requesst.getParameter()`. Server không thể set parameter mà chỉ có thể set attribute.
  * For example `http://example.com/servlet?parameter=1&name=anhao`
  * Parameter can only return String. Số `1` là string không phải number

- Trong Spring REST API thì:
  * `@RequestParam` là `?brand=apple`; case in-sensitive, `Apple` hay `apple` both ok;
  * `@PathVariable` là `product/{productId}`
  * `RequestBody` là raw json inside body

- `HttpServletRequest`:
  - **Scope**: Represents a single client request to the server. Its attributes are available only for the duration of that specific request.
  - **Lifetime**: Created when a client sends a request and destroyed after the server sends the response.
  - **Purpose**: Used to access data sent by the client in the current request (e.g., form parameters, request headers) and to store data that is only needed for processing that specific request (e.g., query results before rendering a JSP).
  - Example: Retrieving a form field value using request.getParameter("fieldName").
- `HttpSession`:
  - Scope: Represents a continuous interaction between a specific client and the web application over multiple requests. Its attributes are available across all requests within that session.
  - Lifetime: Created when a client first interacts with the application and remains active until explicitly invalidated, timed out due to inactivity, or the browser is closed.
  - Purpose: Used to maintain stateful information about a particular user across multiple pages or requests (e.g., user login status, shopping cart contents, user preferences).
  - Example: Storing a user ID after successful login using session.setAttribute("userId", user.getId()).

## DispatcherServlet

The **DispatcherServlet** is the front controller in the Spring MVC framework that acts as a single entry point for all incoming HTTP requests, routing them to the appropriate controller for processing and then delegating the response to the appropriate view.

## Mapping servlet

- Có 2 cách mapping servlet:
  * dùng annotation
  * Dùng `web.xml`

## JAR vs WAR

- JAR (Java Archive):
  * Purpose: Primarily used for packaging and distributing standalone Java applications, libraries, or components.
  * Content: Contains compiled Java classes, resources (like images or property files), and a META-INF directory with metadata.

- WAR (Web Application Archive):
  * Purpose: Specifically designed for packaging and deploying web-based Java applications (servlets, JSPs, HTML, CSS, JavaScript, etc.).
  * Content: Contains all the components of a web application, including compiled Java classes, resources, static web files, and a WEB-INF directory. The WEB-INF directory is crucial and contains web.xml (the deployment descriptor), classes (compiled Java code), and lib (dependent JAR files).
  * Deployment: Deployed within a web server or application server environment (e.g., Apache Tomcat, Jetty, JBoss), not as a standalone executable.

## XML

`xmlns` là viết tắt của `XML namespace`, dùng để định danh các thẻ và thuộc tính trong tài liệu XML, giúp phân biệt các yếu tố có tên giống nhau nhưng thuộc các bộ khác nhau.

## Strut

Apache Struts, commonly referred to as Struts in the context of Java, is an open-source, free framework for building Java web applications. It was created by the Apache Software Foundation and primarily focuses on implementing the Model-View-Controller (MVC) architectural pattern.

