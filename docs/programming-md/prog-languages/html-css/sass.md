# Tailwind, Sass and MaterialUI notes

## Sass

### Compiling into CSS

Dùng [gulp](https://gulpjs.com/docs/en/getting-started/quick-start) để chạy Sass. Cài gulp bằng npm. Gulp is a **task runner**.

VScode có 1 extension "live sass compiler"

sass react vite

### Syntax

The **SCSS syntax** uses the file extension .scss. With a few small exceptions, it’s a superset of CSS, which means essentially all valid CSS is valid SCSS as well. Because of its similarity to CSS, it’s the easiest syntax to get used to and the most popular.

The **indented syntax** was Sass’s original syntax, so it uses the file extension .sass. Because of this extension, it’s sometimes just called "Sass". The indented syntax supports all the same features as SCSS, but it uses indentation instead of curly braces (không có `{}`) and semicolons to describe the format of the document.

### mixin

f

### Sass partials

[Get your stylesheets more organized with Sass partials](https://www.youtube.com/watch?v=9Ld-aOKsEDk) kevin powel

## Bootstrap

Bootstrap is mobile first: apply the bare minimum of styles to make a layout work at the smallest breakpoint, and then layers on styles to adjust that design for larger devices.

Bootstrap với tailwind breakpoints are similar

Tải source file (not compiled file) về học và edit custom bootstrap. It is written in SASS.

### Container & breakpoints

- Extra small, <576px
- Small, `sm`, >=576px
- Medium, `md`, >=768px => Han solo fly a medium spaceship
- Large, `lg`, >=992px
- Extrra large, `xl`, >=1200px
- Extra extra large, `xxl`, ≥1400px

### Grid System

Giống flexbox có 12 columns.

Bootstrap grid supports six responsive breakpoints. Breakpoints are based on `min-width` media queries, meaning they affect that breakpoint and all those above it (e.g., `.col-sm-4` applies to sm, md, lg, xl, and xxl).

By default `col` are `col-12`, 100% space

Bootstrap's grid system, with its `row` and `col` classes, is built with Flexbox. You can use flex box specifically with `d-flex`

## TailwindCSS

responsive kiểu lớn hơn bằng

## Material UI

k

## PostCSS

[how to use postCSS](https://www.youtube.com/watch?v=ohJcZW60br0) by Kevin Powell

