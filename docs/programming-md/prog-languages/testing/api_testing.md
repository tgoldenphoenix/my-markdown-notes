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

## Performance Testing

### Types of Performance Test

- The most common types that are carried out include the following:
  * Load tests
  * Stress tests
  * Soak tests
  * Baseline tests

A load test is used to help us understand how our application might behave when subjected to a reasonable or expected level of requests. During a load test, our application is subjected to a target number of requests that have either been estimated or are based on production statistics. A load test can help us assess whether our application meets performance targets for availability, concurrency or throughput, and response time

Because the goal is to understand how the application works with the closest approximation to real application use, a load test will typically include delays and pauses that are experienced during interactions with our site, such as users filling in forms or other applications carrying out actions.

---

Whereas a load test shows us how a product will handle an expected amount of load, a stress test seeks to push a product to its limit to understand what that limit is. Typically, a stress test will slowly ramp up the number of virtual users accessing our application until the volume of load begins to strain the application, perhaps causing it to exhibit errors or unacceptable response times for requests.

---

Not all performance issues are related to the load volume an application is under. They may appear over time due to memory leaks or servers filling up with too much data. Therefore, with a `soak test`, the goal is to run a limited amount of load over a lengthy amount of time. Unlike load or stress tests that might run for an hour or two, a soak test might be run over many hours to attempt to trigger potential time-based performance issues that might manifest in a live situation. The goal is to see if spikes occur in later periods of a soak test in areas such as response times or hardware usage (CPU, RAM, disk I/O, etc.).

---

`Baseline tests` are usually run in conjunction with other types of tests, such as load and stress. Unlike other performance tests that generate a large number of virtual users, a baseline test is executed with a single virtual user for a set number of times. The information collated from this test can then be compared to load or stress test results to determine the amount of performance degradation that might occur as the load on our application increases.

For example, a baseline test with a single virtual user might show that CPU usage is 30%, but when a load test is run with 100 virtual users, the CPU usage peaks at 35%. A 30% usage for a single run might not be ideal, but the comparison between results shows that the application is relatively capable of taking on load.

### Types of measurements for performance tests

Availability—When an application is under load, we want to ensure that it is up and its services are available. As we apply the load, we can measure whether the application is still running and available by tracking how the system responds. For example, we can measure 400 and 500 status codes to see if they start to appear when the system is under heavy load. 



## Book resource

Navigate to `http://localhost:8080` to access the site

You can access restful-booker-platform via either `http://local host:8080` or <https://automationintesting.online/>

The user login details are:

- Username: admin
- Password: password
