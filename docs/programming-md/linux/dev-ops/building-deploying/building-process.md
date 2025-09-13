# Project Build & managing process

## The building process

Compilation: chuyển src code thành html, css, javascript that can run on the browser.

### Compiler & transpiler

The definitions of **transpiler** and "compiler" are blurry. Both of them do translate a program from one language to another language while keeping the behaviour.

We usually name it a "compiler" when it produces an executable binary, a lower-level output than the input was, e.g. C to assembler, or Java to Java bytecode.

We usually name it a **transpiler** when its output is on a similar level as the input, e.g. Python to JavaScript or the other way round or modern Javascript to older Javascript.

**Babel** can be defined as both a compiler & a transpiler.

Modern project build systems, such as **webpack**, provide a means to run a transpiler automatically on every code change, so it’s very easy to integrate into the development process.

### Polyfills

A script that updates/adds new functions is called “polyfill”. It “fills in” the gap and adds missing implementations.

## Redering pattern

- CSR: client-side rendering
- SSR: server-side rendering: More work on the client than the traditional server-rendered method. Ex: Next.js
- SSG: Static site generation
- ISR: Incremental static regeneration
- SPA: Single Page Application (React)

Similar terms in other fields:

- IRS: [Internal Revenue Service](https://en.wikipedia.org/wiki/Internal_Revenue_Service)
- SSI: [State Security Incorporation](https://www.ssi.com.vn/tin-tuc/tin-tuc-su-kien-ssi/tcbc-cong-ty-co-phan-chung-khoan-sai-gon-hosessi-ky-niem-18-nam-thanh-lap-va-chinh-thuc-doi-ten-thanh-cong-ty-co-phan-chung-khoan-ssi)

### Static Websites

A static website is a type of website that consists of a set of HTML, CSS, and JavaScript files that are served to the client's browser without any server-side processing or database integration.

Static site generators, such as Jekyll, Hugo, or Gatsby.js.

### (SSR) Server-side rendering

Server-side rendering (SSR) is a technique for rendering web pages on the server-side before sending the rendered HTML to the client's browser. In SSR, the server generates the HTML content of a web page based on the requested URL and data, and sends it to the client's browser as a complete HTML document.

- Logic từ đơn giản (validation, đọc dữ liệu) cho đến phức tạp (phân quyền, thanh toán) đều nằm ở phía server
- Logic để routing – chuyển trang nằm ở server
- Logic để render – hiển thị trang web cũng nằm ở server nốt

Nhược điểm

- Mỗi lần người dùng chuyển trang là site phải load lại nhiều lần, gây khó chịu
- Nặng server vì server phải xử lý nhiều logic và dữ liệu. Có thể sử dụng caching để giảm tải.
- Tốn băng thông vì server phải gửi nhiều dữ liệu thừa và trùng  (HTML, header, footer). Có thể sử dụng CDN để giảm tải.
- Tương tác không tốt như Client Side rendering vì trang phải refresh, load lại nhiều lần.

Các trang web sử dụng cơ chế này:

- Toàn bộ những trang web được build từ CMS như Joomla, WordPress.

Improve initial loading times and SEO (search engine optimization) but can be slower for dynamic content.

### (SPA) Single Page Applications with Client-side Rendering

Client-side rendering (CSR) is a common technique, especially in libraries like React and frameworks like Vue.js, Angular.  
việc render HTML, CSS sẽ được thực hiện ở client (Tức JavaScript ở trình duyệt)  
The term client-side rendering informs you whether the page, when it is delivered to the browser from the server, can be displayed immediately; or whether it needs to first load and execute javascript in order to put anything useful on the screen.

A Single Page Application (SPA) is a type of web application rendered with Client-side rendering (CSR). This means that all necessary HTML, CSS, and JavaScript files are loaded at once when the user first loads the page. Then Javascript dynamically updates the content as the user interacts with the page, without requiring a full page reload.  
The term single-page app informs you that you do not do a full-page reload when you transition between routes.

You can have single-page apps that are server-side-rendered; just as, theoretically, you can have a **multi-page app/site** whose pages are client-side-rendered.

Đặc điểm:

- Những logic đơn giản (validation, đọc dữ liệu, sorting, filtering) nằm ở client side
- Logic để routing (chuyển trang), render (hiển thị) dữ liệu thì 96.69% là nằm ở client side
- Logic phức tạp (thanh toán, phân quyền) hoặc cần xử lý nhiều (data processing, report) vẫn nằm ở server side.

Ưu điểm:

- Page chỉ cần load một lần duy nhất. Khi user chuyển trang hoặc thêm dữ liệu, JavaScript sẽ lấy và gửi dữ liệu từ server qua AJAX. User có thể thấy dữ liệu mới mà không cần chuyển trang.

Nhược điểm:

- Initial load sẽ chậm hơn nếu không biết optimize. Lý do là broser phải tải toàn bộ JavaScript về (khá nặng), parse và chạy JS, gọi API để lấy dữ liệu từ server (chậm), sau đó render dữ liệu
- Đòi hỏi project phải chia làm 2 phần riêng là back-end (REST api) và front-end nên khó code hơn

Vì Client-side rendering rất phù hợp cho những ứng dụng cần tương tác nhiều, hầu hết web của các công ty công nghệ, công ty startup đều đùng cơ chế này

Provide a fast and interactive user experience but can be slower for initial loading times and bad for SEO.

When building your react-app, you can see that there is only one App.js from where your entire web-app is loaded in fragments and components. This behaviour of rendering components and pages on a single page and changing the DOM( is a single page behaviour and hence the name), instead of loading a new page with new content, this makes it feel like a single application.

Open the inspector and you see the HTML has nothing, just links to javascript.

### Static site generation (SSG)

HTML generated by server and send to client's browser as a static file.

Provide excellent performance and security but can be less flexible for dynamic content.

**References:**

[freeCodeCamp blog post](https://www.freecodecamp.org/news/rendering-patterns/) very good

[What are source maps?](https://www.youtube.com/watch?v=FIYkjjFYvoI&t=6s)

## ESLint

ESLint là để làm clean code (đẹp mắt) dành cho JavaScript.

ESLint is a static code analysis tool for identifying problematic patterns found in JavaScript code.

Chạy script `lint` trước không có lỗi rồi mới chạy `build`.

[Lint](https://en.wikipedia.org/wiki/Lint_(software)) is the computer science term for a static code analysis tool used to flag programming errors, bugs, stylistic errors and suspicious constructs. The term originates from a Unix utility that examined C language source code.[2] A program which performs this function is also known as a "linter".

### Rules

Reference

[trungquandev-public-utilities-algorithms](https://github.com/trungquandev/trungquandev-public-utilities-algorithms) github repo

## Vite

Vite is a faster Alternative To CRA (create react app)

Vite là build tool, NextJs là framework. Khi dùng vite thì nó cho mình một template đơn giản nhất. Còn framework thì nó có sẵn một đồng opiniated libraries lúc mới mở lên (folder structure, routing, etc). Nhưng phải học vite tự cấu hình trước rồi mới học lên framework NextJs.

Other choices are: webpack

CRA use webpack under the hood.

web pack bundle every time you make a change to the src code, which can be slow.

In development, Vite use **esbuild** which is faster than bundle. When you need to bundle for production, vite use **rollup**.

Nên dùng vite để học. Dùng nextjs or remix nó không học được gì.

## npm

### basics

[npm](https://en.wikipedia.org/wiki/Npm) (Node Package Manager) is a package manager for the JavaScript programming language maintained by npm, Inc., a subsidiary of GitHub. npm is the default package manager for the JavaScript runtime environment **Node.js** and is included as a recommended feature in the Node.js installer. NPM is written in JavaScript nên nếu không cài đặt Node trước thì rất khó dùng npm.

Thường muốn cài npm thì phải cài từ trang chủ [nodejs](https://nodejs.org/en). Hai cái nằm trong /usr/local/bin/. Không cài nodejs bằng installer mà cài thông qua `nvm`. **NVM** stands for Node Version Manager, and as the name implies, it helps you manage your Node Versions. With NVM, you can install Node versions and specify the version of Node that a project uses. Node version managers allow you to install and switch between multiple versions of Node.js and npm on your system so you can test your applications on multiple versions of npm to ensure they work for users on different versions.

Muốn uninstall 2 thằng này cũng khá phức tạp.

Terminal emulator iTerm không tự động `source ~/.zshrc` when open. Phải vào setting chỉnh [here](https://stackoverflow.com/questions/50689939/why-do-you-need-to-source-zshrc-for-every-new-shell-in-iterm).

**References:**

[npm CLI command doc](https://docs.npmjs.com/cli/v11/commands/npm)

[Downloading and installing Node.js and npm official doc](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) (using nvm)

[download/update nodejs using nvm official doc](https://nodejs.org/en/download)

[all nvm commands](https://gist.github.com/chranderson/b0a02781c232f170db634b40c97ff455) (very useful)

### NPM-related commands

`npm init` => create a `package.json` file.

`npm doctor` Check the health of your npm environment

`which nvm` will not work, since nvm is a sourced shell function, not an executable binary. Phải dùng [cách này](https://github.com/nvm-sh/nvm?tab=readme-ov-file#verify-installation).

`nvm ls` => coi node version installed.

Phải chỉnh nvm default version of node. Mặc định là 20 => `nvm use`

`-D` == `--save-dev`. Must be capical D

`source ~/.zshrc; clear`

## Yarn

> just like your project dependencies must be locked, so should be the package manager itself.

`Corepack` is an official Node.js tool letting you define which package manager version you want to use on a per-project basis, just like your lockfile does for your project dependencies.

Yarn is one of the main JavaScript package managers; An alternative to the npm package manager; Written in: TypeScript, JavaScript.

`yarn dev --host` có url network, lấy smart phone trong cùng đường mạng connect được. Tiện để dev responsive. Hoặc mở thêm cái máy tính khác.

[difference between yarn registry and npm registry](https://stackoverflow.com/questions/58071109/difference-between-yarn-registry-and-npm-registry)

References

[Yarn official doc](https://classic.yarnpkg.com/en/docs)

## AI tools

ChatGPT & Github copilot
