# Nextjs

## About

Next.js is actually both a backend and frontend framework. It’s a full-stack JavaScript framework that allows you to build both client-side (frontend) and server-side (backend) components in a single project.

Server-Side Rendering (SSR): With Next.js, you can fetch data on the server and send pre-rendered pages to the client, improving performance and SEO.  
In nextjs, you can also do SSG (static site generation).

NextJS is a react framework, it uses nodejs as the backend engine.

Unlike a traditional React app where the entire application is loaded and rendered on the client, Next.js allows the first page load to be rendered by the server, which is great for SEO & performance.

## Routing

You will probably have to deal with "routes" in both your front end and back end. The important thing is to understand what they actually mean in any given situation.

The interaction between your front end and back end all happens via HTTP calls. Each HTTP call has a request method (e.g. GET, POST, etc.) and a path (e.g. "r/learnprogramming"). Your back end uses its "routes" to decide which function will handle that HTTP request. Your back end "routes" are just a configuration that lets you define how requests map to functions in your back end code.

Front end "routes" kind of mimic this behavior, but everything happens inside your browser. A front end route is just an easy way to say, "Load up this part of the front end app." A given link or button or something will cause a certain JavaScript function to execute, and that may or may not result in HTTP calls to the back end.

Frontend routing, often used in single-page applications (SPAs), manages navigation within the browser without full page reloads. Backend routing, on the other hand, handles how the server responds to different URL requests, often by serving different HTML pages.

Backend routing is how traditional websites handle navigation, where clicking a link sends a request to the server, which then sends back a new HTML page.

### Pages Router and App Router

- **Page Router** is the traditional routing method in Next.js, relying on a file-system-based approach where each file in the pages directory automatically becomes a route. For example, `pages/about.js` maps to /about.
- **App Router**: Introduced in Next.js 13, it utilizes a new app directory and focuses on React Server Components (RSCs) and server-centric routing. Routes are defined by directories and files within the app directory, offering more granular control.

Routing conventions:

1. All routes must live inside the `.src/app/` directory.
2. Route files must be named either `page.js` or `page.tsx`.
3. Each **folder** represents a segment of the URL path.

When these conventions are followed, the file automatically becomes available as a route.

`layout.txs` is auto-generated if not exist.

