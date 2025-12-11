# API Testing

## Terminologies

In the Chrome dev tool's network tab, `XHRs`, which stand for `XMLHttpRequests`, are HTTP requests sent from the browser to the API in the background. These can be used for situations in which we want to asynchronously modify data for either the UI or the backend without having to update the entire page. For example, an XHR request to `/branding/` could be used to update the home page images and details without having to do a whole-page refresh.

## Postman HTTP Client

Postman is an HTTP testing client.

[reqres](https://reqres.in/)

postman scripts là javascript. `pm` là object postman.

You can change the HTTP method of an API enpoint from `GET` to `OPTION` to discover other HTTP methods that can be called on that endpoint in the response headers under `Allow`.  (You can find the response headers in Postman by selecting the Headers tab in the bottom half of the window.) 

Investigate the request headers to discover custom cookies being sent (specifically, the mention of `token` in cookie value). 

## Documenting APIs with Swagger/OpenAPI 3

The `Swagger toolset` offers a range of tools that we can take advantage of that we’ll discuss shortly, but for now, our focus will be on using the specification schema called `OpenAPI 3` to document our API design.

If for any reason Swagger is not a toolset that works for you, other tools offer API design tooling, such as the Postman API design and Stoplight, which both support the OpenAPI format.

Swagger dùng syntax của `YAML` để viết documentation cho API endpoints.

`Swagger UI` has the ability to take your API design documentation and present it in an easy-to-read UI that clearly presents your API’s functionality.

---

**Documenting GraphQL APIs**

Because GraphQL doesn’t use OpenAPI for defining schemas, the Swagger toolset (at the time of writing) doesn’t support GraphQL. However, open source libraries offer a similar experience for GraphQL, such as graphql-playground and graphiql, which are maintained by the GraphQL community

## Book resource

Navigate to `http://localhost:8080` to access the site

You can access restful-booker-platform via either `http://local host:8080` or <https://automationintesting.online/>

The user login details are:

- Username: admin
- Password: password
