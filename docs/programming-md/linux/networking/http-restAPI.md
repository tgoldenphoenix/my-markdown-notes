# HTTP, RESTful API notes

JSON body có thể thay thế request param (`/anhao?param1=one&param2=two`) trong trường hợp param quá dài (lỗi `URI too long`)

## RESTful API Basics

- `HTTP` is a stateless communication protocol.
- `RESTful APIs` (Representational State Transfer) is an **architectural style** for designing APIs that often uses HTTP.

The server does NOT store any information or context about previous requests from the client.  
Every interaction is independent, and the server treats each request as if it were new, without any knowledge of prior communications.

- The server does not store session information. Không tốn bộ nhớ phía server.
- Each request from the client **must** include all the necessary authentication tokens, query parameters, or data to complete the request.

rest khác restful???

## HTTP Basics

HTTP is a protocol for fetching resources such as HTML documents, images, videos. It is the foundation of any data exchange on the Web and it is a client-server protocol.

Requests are initiated by the recipient, usually the Web browser.

There are web serve, video server, ad server (quảng cáo).

Clients and servers communicate by exchanging **individual messages** (as opposed to a stream of data).

It is an **application layer** protocol that is sent over `TCP`, or over a TLS-encrypted TCP connection.

HTTP is generally designed to be human-readable,

HTTP is `stateless`, but NOT `sessionless`: While the core of HTTP itself is stateless, HTTP cookies allow the use of stateful sessions. Using header extensibility, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context, or the same state.

## `HTTP` vs `HTTPS`

You might have observed that in the examples presented, I only use HTTP. In practice, however, your applications communicate only over `HTTPS`. For the examples we discuss in this book, the configurations related to Spring Security aren’t different, whether we use HTTP or HTTP.

In any of these configuration scenarios, you need a certificate signed by a certification authority (CA). Using this certificate, the client that calls the endpoint knows whether the response comes from the authentication server and that nobody intercepted the com-munication. You can buy such a certificate if you need it. If you only need to configure HTTPS to test your application, you can generate a self-signed certificate using a tool such as OpenSSL (<https://www.openssl.org/>)

## HTTP Messages Format

You can see HTTP messages in a browser's **Network tab** in the developer tools, or if you print HTTP messages to the console using CLI tools such as curl

There are two types of HTTP messages, requests and responses, each with its own format.

- Request:
  - The `start-line` (first line) contains, in order:
    - An HTTP method: `GET, POST, PUT, DELETE`
    - `Resource path` without the protocol (`http://`), domain name (`developer.mozilla.org`) or the TCP port (here, `80`). Example looks like `/images/2025/`, `/users`.
    - `Version` of the HTTP protocol.
  - Optional `headers block` for additional information for the servers: language, MIME types, allowed formats trả về. Thông tin dưới dạng key-value pairs.
  - An **empty line** indicating the `header` of the message is completed.
  - A body which contain the resource sent. **Only** `PATCH, POST, and PUT` requests have a body. Example: JSON, data in multiple parts, a string of key-value pais.

- Response:
  * The `start-line` (called a **status line** in responses) contains, in order:
    * Protocol version.
    * Status code: indicating if the request was successful or not, and why.
    * A **status message**: a non-authoritative short description of the status code.
  - A `headers block`, like those for requests. Example: format trả về
  - An **empty line** indicating the header of the message is complete.
  - Optionally, a body containing the fetched resource. Example: HTML code, JSON

The start-line and headers of the HTTP message are collectively known as the head of the requests, and the part afterwards that contains its content is known as the body.

An HTTP request first line example: `GET / HTTP/1.1` => path is `/`, `HTTP/1.1` là protocol version

Request body example: `name=FirstName+LastName&email=bsmth%40example.com`

The most commonly used API based on HTTP is the `Fetch API`, which can be used to make HTTP requests **from JavaScript**. The Fetch API replaces the `XMLHttpRequest API`.

With TCP the default port, for an HTTP server on a computer, is port `80`. Other ports can also be used, like 8000 or 8080.

## HTTP Methods

`PUT` vs `PATCH` khác nhau cái gì

## Media types (MIME types)

A **media type** (formerly known as a **Multipurpose Internet Mail Extensions or MIME type**) indicates the nature and format of a document, file, or assortment of bytes.

Có dạng chung là: `type/subtype;parameter=value`. Trong đó `type & subtype` is required; `parameter` is optional.

Example: `text/plain;charset=UTF-8`

- There are two classes of type: discrete and multipart. 
  * `Discrete types` are types which represent a single file or medium, such as a single text or music file, or a single video.
  * A `multipart type` represents a document that's comprised of **multiple component parts**, each of which may have its own individual MIME type; or, a multipart type may encapsulate multiple files being sent together in one transaction.

- The discrete types are:
  * `application`: Any kind of binary data that doesn't fall explicitly into one of the other types; either data that will be executed or interpreted in some way or binary data that requires a specific application or category of application to use. Generic binary data (or binary data whose true type is unknown) is application/octet-stream. Other common examples include application/pdf, application/pkcs8, and application/zip
  * `audio`: example `audio/mpeg, audio/vorbis`

`application/octet-stream` is a MIME type that identifies a file as a generic binary file of unknown type. It is the default for binary data that doesn't have a more specific MIME type and is often used for file uploads.

## Request Headers & Body

The HTTP `"Authorization":` request header can be used to provide credentials that authenticate a user agent with a server, allowing access to protected resources.

In a POST request, the `"Content-Type":` header tells the server the format of the request's body.

`POST`; `Content-Type:application/x-www-form-urlencoded`; Form parameters => trong request body sẽ có dạng:

```text
POST /api/v2/wikis?apiKey=xmnb1h2nlENjJcfqQEYhGS7B0eQ6ZE7l9tGtWPGFM6cjRZ5k2kL99ByUNeO2tXlU HTTP/1.1
User-Agent: PostmanRuntime/7.49.0
Accept: */*
Cache-Control: no-cache
Postman-Token: e6ce50d3-828f-459c-ac12-42f16f291d13
Host: ever-rise.backlog.jp
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 137
 
projectId=40038&name=page_created_by_script&content=An%20Hao%20created%20this%20page%20by%20calling%20Backlog's%20API.&mailNotify=false
```

The HTTP GET method typically does not include a request body. The HTTP specification defines that a GET request body has no semantic meaning, and therefore, servers are not obligated to process or understand it. In `fetch()`, you cannot include a body with `GET`.

- You can supply the **body** as an instance of any of the following types:
  - a string
  - ArrayBuffer
  - TypedArray
  - DataView
  - `Blob`
  - File
  - `URLSearchParams` object to encode form data
  - FormData
  - ReadableStream
- Other objects are converted to strings using their toString() method.

GET requests don't have a body, but you can still send data to the server by appending it to the URL as a query string. This is a common way to send form data to the server. You can do this by using `URLSearchParams` to encode the data, and then appending it to the URL

```javascript
const params = new URLSearchParams();
params.append("username", "example");

// GET request sent to https://example.org/login?username=example
const response = await fetch(`https://example.org/login?${params}`);
```

## Including Credentials

- In the context of the Fetch API, a credential is an extra piece of data sent along with the request that the server may use to authenticate the user. All the following items are considered to be credentials:
  - HTTP cookies
  * TLS (Transport Layer Security) client certificates
  * The Authorization and Proxy-Authorization headers.

## HTTP Responds

API của nulbab's backlog trả về `response`, từ đó ta gọi `data = await response.json()`.

- GET wiki page returns:
  * 404 Not Found nếu id bị sai: `response.status = 404; response.ok = false`

- GET wiki page list returns:
  * array of objects
  * empty array if not found: `response.status = 200; response.ok = true` mặc dù không tìm thấy gì. Nên nó sẽ không nhảy vào điều kiện `if` để `throw` error.

### Response Status Codes

Responses are grouped into five classes: informational responses, successful responses, redirects, client errors, and server errors.

1xx informational response

An informational response indicates that the request was received and understood and is being processed. It alerts the client to wait for a final response.

---

`2xx success` indicates that the action requested by the client was received, understood, and accepted.

- `200 OK`: Standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request, the response will contain an entity describing or containing the result of the action.
- `201 Created` The request has been fulfilled, resulting in the creation of a new resource.

---

A `3xx redirection` status indicates that the client must take additional action, generally URL redirection, to complete the request.

`301 Moved Permanently`: Requested object has been permanently moved; the new URL is specified in `Location:` header of the response message. The client software will automatically retrieve the new URL.

---

A `4xx client error` status code is for situations in which an error seems to have been caused by the client. The server should include an entity containing an explanation of the error situation. 

- `400 Bad Request`: This is a generic error code indicating that the request could not be understood by the server. The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax, size too large, invalid request message framing, or deceptive request routing).
- `401 Unauthorized`: The HTTP `401` Unauthorized status code is a bit ambiguous. Usually, it’s used to represent a **failed authentication** rather than an authorization. Developers employ it in the design of the application for cases such as missing or incor-rect credentials. For a failed authorization, we’d probably use the 403 Forbidden status. Generally, an HTTP 403 means that the server identified the caller of the request, but they don’t have the needed privileges for the call that they are trying to make.
- `403 Forbidden`: The request was valid, but the server refuses action. This may be due to the user not having permission to a resource or needing an account of some sort, or attempting a prohibited action (e.g. creating a duplicate record where only one is allowed)
- `404 Not Found`: The requested document does not exist on this server.

---

`5xx server error`: indicates that the server is aware that it has encountered an error or is otherwise incapable of performing the request.

The server should include an entity containing an explanation of the error situation, and indicate whether it is a temporary or permanent condition.

- `500 Internal Server Error`: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.
- `505 HTTP Version Not Supported`: The requested HTTP protocol version is not supported by the server.

- các status quan trọng trong adrepo:
  - 200
  - 400
  - 401
  - 403
  - 500

## Caching

- Client-Side Caching: The client can cache responses to avoid redundant requests to the server. This is especially useful for static resources such as images, configuration files, or infrequently changing data.
- Server-Side Caching: Servers can implement caching for resources that are commonly requested. This improves response times and reduces the load on the server, ensuring better scalability.

## Parameter

- Trong Spring REST API thì:
  - `@RequestParam` là `?brand=apple`; case in-sensitive, `Apple` hay `apple` both ok;
  - `@PathVariable` là `product/{productId}`
  - `RequestBody` là raw json inside body

## Fetch API

Fetch is the modern replacement for `XMLHttpRequest`: unlike XMLHttpRequest, which uses callbacks, Fetch is promise-based.

The `fetch()` function returns a Promise which is fulfilled with a Response object representing the server's response. You can then check the request status and extract the body of the response in various formats, including text and JSON, by calling the appropriate method on the response.

```javascript
async function getData() {
  const url = "https://example.org/products.json";
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.error(error.message);
  }
}
```

The fetch() function will reject the promise on some errors, but not if the server responds with an error status like 404: so we also check the response status and throw if it is not OK.

Otherwise, we fetch the response body content as JSON by calling the json() method of Response, and log one of its values. Note that like fetch() itself, json() is asynchronous, as are all the other methods to access the response body content.

### Handling the response

As soon as the browser has received the response status and headers from the server (and potentially before the response body itself has been received), the promise returned by fetch() is fulfilled with a `Response` object.

The promise returned by fetch() will reject on some errors, such as a network error or a bad scheme. However, if the server responds with an error like 404, then fetch() fulfills with a Response, so we have to check the status before we can read the response body.

- The Response interface provides a number of methods to retrieve the entire body contents in a variety of different formats:
Response.arrayBuffer()
  * Response.blob()
  * Response.formData()
  * Response.json()
  * Response.text()

These are all asynchronous methods, returning a Promise which will be fulfilled with the body content.

## User-Server Interaction: Cookies

An HTTP server is stateless. However, it is often desirable for a Web site to identify users, either because the server wishes to restrict user access or because it wants to serve content as a function of the user identity. For these pur-poses, HTTP uses cookies. Cookies allow sites to keep track of users.

## Web Caching

A **Web cache**—also called a **proxy server**—is a network entity that satisfies HTTP requests on the behalf of an origin Web server.

## Blob

In JavaScript, a `Blob` (Binary Large Object) is an object that represents immutable, raw binary data. It's often used to handle file-like data in web applications, such as images, videos, or other binary content.

## Axios
