# Servlet, JSP, Spring MVC - Thymeleaf

## Spring MVC

chỉ có `@GetMapping` & `@PostMapping`, API mới có Put, Delete

## Thymeleaf

Trong forms dùng **selection expressions**: `*{firstName}`

**Message expression** trong file `application.properties`. `#{welcome.text}`

**Link expression** `@{manage/edit}`

## Jsp servlet

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
  - Cả 2 đều là chuyển trang phía server, client không liên quan.
  - Forward chuyển dữ liệu đi theo.
  - A forwarding is done without letting the client know that, It is used to do internal communication at server (chuyển trang phía server). The URL in Browser remains unchanged
  - request attributes and parameters are preserved during the forward.
  - If the previous scope is required, or the user doesn’t need to be informed, but the application also wants to perform an internal action then use forwarding.
  - login thành công => forward
- `sendRedirect`:
  - Chuyển trang không kèm theo dữ liệu.
  - Redirect chạy nhanh hơn forward => ưu điểm của re-direct.
  - A redirect sends an **HTTP 302** response to the browser (client), instructing it to make a new request to a different URL. Client changes to the new URL, A new request is created.
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

JSP servlet & tất cả modern web framework có 8 chữ cốt lõi:

1. get-post
2. redirect-forward
3. attribute vs parameter
4. request vs session
5. rảnh thì học thêm cookie (9 chữ tất cả)

- `setAttribute() & getAttritube()` diễn ra ở server => attribute là dữ liệu do client tự bịa đặt ra gởi cho chính server. Thằng này set attribute rồi gởi cho thằng jsp/servlet khác nhận attribute xử lý logic rồi render ra html trả về client.
  - you set an attribute in a Servlet and read it from a JSP. These can be used for any object, not just string.
  - Thuộc dạng string attribute-object data
- `setParameter()` là ở phía client gởi cho server, server sẽ nhận bằng `getParameter()` (trong url) => parameter là dữ liệu client gởi cho server
  - For example `http://example.com/servlet?parameter=1`. They can only return String.

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

## Strut

Apache Struts, commonly referred to as Struts in the context of Java, is an open-source, free framework for building Java web applications. It was created by the Apache Software Foundation and primarily focuses on implementing the Model-View-Controller (MVC) architectural pattern.
