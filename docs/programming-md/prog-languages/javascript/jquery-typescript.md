# TypeScript & jQuery

## Typescript

`tsc` (typescript compiler) is a command-line tool that you can instal globally

`tsc --init` to create the typescript config

[inferred type](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#defining-types): to suggest indirectly that something is true

TypeScript is all about **type safety**. Nothing else. TS does NOT add any additional to JS. It only allows you to write JS in a more pricise manner.

`2 + "2" = "22"` is valid in JS. It is NOT type safety.

TS only add **static checking** to JS. Static checking có trong Java.

You write **a lot more** code in TS, and it is harder to read & understand. These are tradeoffs to avoid runtime error.

TS is a development tool, your production code still run in JS. It is installed as a dev dependency.

## Types

Type `void`

Don't use the keyword `any` in TS.

`number` is for numbers like 42. JavaScript does not have a special runtime value for integers, so there’s no equivalent to `int` or `float` - everything is simply number

You don't need to specify types when the context is obvious enough. This is called **Type Inference** in TS.

## Type Aliases vs Interface

2 cái này có similar syntax. Interface dùng nếu có function. Đọc official doc for more information.

## Generics

generic

## tsconfig.json

The presence of a `tsconfig.json` file in a directory indicates that the directory is the root of a TypeScript project. The tsconfig.json file specifies the root files and the compiler options required to compile the project.

## jQuery

One important thing to know is that jQuery is just a **JavaScript library**.

The jQuery library exposes its methods and properties via two properties of the window object called `jQuery` and $. $ is simply an alias for `jQuery` and it's often employed because it's shorter and faster to write.

jQuery có hàm `.ready()` để đảm bảo JS code run after the DOM is loaded. Nếu không dùng jQuery thì phải đặt `<script>` tag của `main.js` ở dòng cuối của `<body>` trong `index.html`

AJAX, which stands for Asynchronous JavaScript and XML, is a set of web development techniques used to create more dynamic and responsive web applications. Instead of requiring a full page reload for every user interaction, AJAX allows for the asynchronous exchange of data between the client (browser) and the server in the background.

**References:**

[doc trên jquery.com](https://learn.jquery.com/about-jquery/how-jquery-works/) hơi khó hiểu cho beginner

[doc trên W3school](https://www.w3schools.com/jquery/default.asp) more beginner friendly

