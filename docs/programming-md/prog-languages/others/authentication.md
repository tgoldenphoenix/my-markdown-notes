# API Authentication

## HTTP headers

HTTP headers let the client and the server pass additional information with a message in a request or response.

Note that they are case-insensitive.

## Web storage vs Cookie Storage

In the browser, **Cookie Storage** is different from **Web Storage** which consists of two types: Local and Session Storage.

- `sessionStorage` — data is persisted only for the duration of the page session
- `localStorage` — data persists even when the browser is closed and reopened

## JWT (JSON Web Tokens) and Auth0

JWT is commonly used for authentication and authorization.

Phải phân biệt **Session based** vs **Token authentication**.

The original approach on the web is cookied-based server-side sessions  
In **Session-based authentication**, user data is store as a "session" on the server machine (not to be confused with session-storage on the client browser). Sau đó server gởi về cho client một cookie chứa session ID. The session ID is stored in the browser's **cookie jar** which is a place in the browser to save cookies as key-value pairs.  
Những lần sau đó khi client gởi request đính kèm cookie để server nhận diện. Cách này có những nhược điểm là:

1. Nếu có > 2 servers thì user phải login 2 hay nhiều lần (vì các servers không thể share session). Đặc biệt ngày nay, most cloud application are scaled horizontally (more servers instead of one bigger server)
2. Nều dùng micro-services thì user phải login nhiều lần (tương tự ý 1)
3. Can be vulnerable to CSRF attack

In **JWT-based authentication**, When user log-in, the server, with its private key, will store the user data (user id, user name) in an **access token** and send it back to the client machine along with a refresh token. The token can be stored in local storage of cookie on the client side.  
Server machine only need to have the secret key used to check the token. Server do NOT need to store user credentials.  
Nhiều server/micro-services có chung sercret key nên user only log in một lần duy nhất.

Typically, JWT is stored in Local Storage or Cookies (storage).

JWT có 3 phần

1. header: chứa algorithm
2. payload: chứa user data, expire date, life span
3. verify signature: nếu client modify data of the token thì server sẽ biết ngay vì không khớp với phần thứ 3 này.

Remember, **JWT is not encrypted** by any means. Rather, it is encoded in Base64. Try decoding any JWT on [jwt.io](jwt.io). Client & hacker hoàn toàn có thể đọc được data. Chỉ là họ không thể modify data vì không có secret key thôi.

JWT and Auth0 are both related to authentication and authorization, but they serve different purposes.

- JWT (JSON Web Token) is a standard for securely transmitting information between parties as a JSON object. It's a _technology_ used for representing claims to be transferred.
- Auth0 is an authentication and authorization **platform** that provides a service for managing user identities and access to applications and APIs. Essentially, Auth0 uses JWTs as part of its authentication process
- **OAuth 2.0** is a standardized authorization protocol, Auth0 is a company that sells an identity management platform with authentication and authorization services that implements the OAuth2 protocol (among others).

There are two types of tokens:

1. access token: keys that allow users to access sensitive information without repeated login requests.
2. Refresh token:

## Cookies

The internet & HTTP were initially created to share documents (HTML, Javascript, CSS). The server do NOT remember who you are (your login credentials for example). HTTP is **stateless**.

Cookies are typically used for authentication, personalization (help server to remember who you are), and tracking. It helps in state persistence

- Server gắn cookie vào response gởi về client. Cookie là key-value pair.
- Client store cookie in browser storage
- Những lần sau đó, client sẽ gắn cookie vào HTTP request gởi lên server.

Cookies therefore can be used by advertising companies' server (third-party cookies) to remember who you are, thus tracking your online activity.

First-party cookies are usually used for good purposes (remember your login information). Third-party cookies are for tracking you.

Here’s an important note — **browsers automatically send cookies** (no client-side code needed) along with every request via the cookie request header. This is exactly why Cookie (storage) is vulnerable to CSRF attacks.

## Stop comparing JWT & Cookie

Neither JWT nor Cookie are authentication mechanisms on their own.

- JWT is simply a token format.
- A cookie is an HTTP state management mechanism really.

As demonstrated, a web cookie can contain JWT and can be stored within your browser’s Cookies storage.

So, we need to stop comparing JWT vs Cookie.

## Spring Security

In Spring Security, `.anyRequest().authenticated()` is a configuration method used within the HttpSecurity configuration to specify that all incoming requests to the application, which have not been explicitly matched by previous rules, must be authenticated.

## OAuth 2.0

In OAuth 2.0, a **resource server** is the application that holds the protected resources (like user data) and responds to client requests for those resources. It's the API that the client is trying to access using an access token obtained from the authorization server. Essentially, it's where the actual data or functionality being accessed resides.

## XSS Attack

a.k.a “Cross-Site Scripting” attack.

With the `HttpOnly` flag, the cookies are not accessible through JavaScript, thus making it immune to XSS attacks.

## CSRF Attack

a.k.a “Cross-Site Request Forgery” attack.

## Hash passswords with Bcrypt algorithm

Từ chuỗi băm không thể reverse lại password được.

- Thuật toán **MD5** tu một dữ liệu đầu vào luôn cho ra đúng một chuỗi hash, không có random.
- Bcrypt cùng một password mỗi lần hash cho ra chuỗi 32 ký tự khác nhau vì nó nhúng salt random

mỗi lần hash bcrypt sẽ có một chuỗi **salt** random.

