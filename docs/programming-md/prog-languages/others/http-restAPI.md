# HTTP, RESTful API notes

json body có thể thay thế request param (`/anhao?param1=one&param2=two`) trong trường hợp param quá dài (lỗi `URI too long`)

PUT vs PATCH khác nhau cái gì

`application/octet-stream` is a MIME type that identifies a file as a generic binary file of unknown type. It is the default for binary data that doesn't have a more specific MIME type and is often used for file uploads.

In JavaScript, a Blob (Binary Large Object) is an object that represents immutable, raw binary data. It's often used to handle file-like data in web applications, such as images, videos, or other binary content.

## REST api

HTTP is a communication protocol; REST is an architectural style for designing APIs that often uses HTTP.

RESTful APIs (Representational State Transfer)

The server does not store any information or context about previous requests from the client.  
Every interaction is independent, and the server treats each request as if it were new, without any knowledge of prior communications.

- The server does not store session information. Không tốn bộ nhớ phía server.
- Each request from the client **must** include all the necessary authentication tokens, query parameters, or data to complete the request.

HTTP is also a stateless protocol

## HTTP

HTTP is a protocol for fetching resources such as HTML documents, images, videos. It is the foundation of any data exchange on the Web and it is a client-server protocol.

Requests are initiated by the recipient, usually the Web browser.

There are web serve, video server, ad server (quảng cáo).

Clients and servers communicate by exchanging individual messages (as opposed to a stream of data).

It is an application layer protocol that is sent over TCP, or over a TLS-encrypted TCP connection.

HTTP is generally designed to be human-readable,

HTTP is stateless, but not sessionless: While the core of HTTP itself is stateless, HTTP cookies allow the use of stateful sessions. Using header extensibility, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context, or the same state.

## HTTP messages

You can see HTTP messages in a browser's **Network tab** in the developer tools, or if you print HTTP messages to the console using CLI tools such as curl

There are two types of HTTP messages, requests and responses, each with its own format.

- Request:
  - The **start-line** (first line) contains, in order:
    - An HTTP method: `GET, POST, PUT, DELETE`
    - **Resource path** without the protocol (`http://`), domain name (`developer.mozilla.org`) or the TCP port (here, `80`). Example looks like `/images/2025/`, `/users`.
    - Version of the HTTP protocol
  - Optional **headers block** for additional information for the servers: language, MIME types, allowed formats trả về. Thông tin dưới dạng key-value pairs.
  - An **empty line** indicating the header of the message is complete.
  - A body which contain the resource sent. **Only** `PATCH, POST, and PUT` requests have a body. Example: JSON, data in multiple parts, a string of key-value pais.
- Response:
  - The **start-line** (called a **status line** in responses) contains, in order:
    - Protocol version
    - Status code: indicating if the request was successful or not, and why.
    - A **status message**: a non-authoritative short description of the status code.
  - A headers block, like those for requests. Example: format trả về
  - An **empty line** indicating the header of the message is complete.
  - Optionally, a body containing the fetched resource. Example: HTML code, JSON

The start-line and headers of the HTTP message are collectively known as the head of the requests, and the part afterwards that contains its content is known as the body.

An HTTP request first line example: `GET / HTTP/1.1` => path is `/`, `HTTP/1.1` là protocol version

Request body example: `name=FirstName+LastName&email=bsmth%40example.com`

The most commonly used API based on HTTP is the `Fetch API`, which can be used to make HTTP requests **from JavaScript**. The Fetch API replaces the `XMLHttpRequest API`.

With TCP the default port, for an HTTP server on a computer, is port `80`. Other ports can also be used, like 8000 or 8080.

## Media types (MIME types)

A **media type** (formerly known as a **Multipurpose Internet Mail Extensions or MIME type**) indicates the nature and format of a document, file, or assortment of bytes.

Có dạng chung là: `type/subtype;parameter=value`. Trong đó `type & subtype` is required; parameter is optional.

Example: `text/plain;charset=UTF-8`

There are two classes of type: discrete and multipart. Discrete types are types which represent a single file or medium, such as a single text or music file, or a single video. A multipart type represents a document that's comprised of multiple component parts, each of which may have its own individual MIME type; or, a multipart type may encapsulate multiple files being sent together in one transaction.

- The discrete types are:
  - `application`: Any kind of binary data that doesn't fall explicitly into one of the other types; either data that will be executed or interpreted in some way or binary data that requires a specific application or category of application to use. Generic binary data (or binary data whose true type is unknown) is application/octet-stream. Other common examples include application/pdf, application/pkcs8, and application/zip
  - `audio`; example `audio/mpeg, audio/vorbis`

## Response status codes

Responses are grouped into five classes: informational responses, successful responses, redirects, client errors, and server errors.

- 200: OK. The request has succeeded.
- 301: Moved Permanently. This response code means that the URI of requested resource has been changed.
- 404: Not Found. The server cannot find the requested resource.

## Caching

- Client-Side Caching: The client can cache responses to avoid redundant requests to the server. This is especially useful for static resources such as images, configuration files, or infrequently changing data.
- Server-Side Caching: Servers can implement caching for resources that are commonly requested. This improves response times and reduces the load on the server, ensuring better scalability.

## parameter

- Trong Spring REST API thì:
  - `@RequestParam` là `?brand=apple`; case in-sensitive, `Apple` hay `apple` both ok;
  - `@PathVariable` là `product/{productId}`
  - `RequestBody` là raw json inside body
