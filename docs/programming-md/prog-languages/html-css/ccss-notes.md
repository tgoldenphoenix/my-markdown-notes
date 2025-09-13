# CSS notes

## Basics CSS syntax

CSS **declaration** format: `property-name: value;`.

A CSS **rule set** contains one or more selectors and one or more declarations.  
Declaration ends with `;`, rule set do NOT ends with `;`.

## The Box model

Everything in CSS is a box. CSS box model refers to: content, padding, border, margin và tất cả những properties relate tới 4 ông này.

Element sẽ có default margin/padding set by the browser và thường nó không theo ý mình. Sometimes, we just want to reset the browser default setting for all the margin and padding and just take control of them ourself. That’s why we use a CSS reset.

Default của `box-sizing: content-box;` nên chuyển về `border-box` sẽ dễ set width: property hơn.

Nếu để default là `box-sizing: content-box;` thì khi set `width:` cho elements dùng fixed units như `400px` thì content đúng 400px không tính padding, border, margin.  
Chuyển qua `box-sizing: border-box` thì content + padding + border tổng lại mới là 400px. The margin is NOT included in the calculation of the box-sizing, only the content, padding and border are included.

The `outline:` property is not part of the box model. The difference between it and `border` is that it doesn’t take up space. It is not calculated into the box-model equation.  
outline sẽ nằm bao bên ngoài border.  
`outline-offset:` property cũng hay được dùng, có thể set negative value.

## Centerinng a div

Center `<div>` with auto margin:

1. constraint width: `max-width:`
2. set both `margin-left` and right to `auto` by using `margin-inline: auto`

Each auto margin will try to gobble up as much space as possible.

The `margin-inline:` is part of a collection of **logical properties**, designed to make it easier to internationalize the web. It defines both the logical `inline start` and `end` margins of an element. Thường thì nó sẽ tương đương với `margin-left` & `margin-right`.

## The `position:` properties

The `position:` CSS property sets how an element is positioned in a document. Default là `static`.

The `top, right, bottom`, and `left` properties determine the final location of positioned elements. The `inset:` is a shorthand that corresponds to the `top, right, bottom`, and/or `left` properties.

`position: relative`: The element is positioned according to the normal flow of the document, and then offset _relative to itself_ based on the values of `top`, `right`, `bottom`, and `left`.

`position: absolute`: The element is **removed** from the normal document flow, and no space is created for the element in the page layout.  
`position: absolute` elements need a relative parent. And if it doesn’t have an ancestor element that is positioned relative then it takes the browser window. If it has multiple relative ancestors elements, it will position based on the closest relative parent element.

- `position: fixed` elements are taken _out_ of the flow of the page. Giống `absolute` nhưng không cần parent element. It does not move when you scroll. Example: call zalo, hình ảnh quảng cáo
- `position: sticky;` is just a little different than fixed. It will stay in its normal flow until it reaches a spot that you have defined. Example: sticky header

Khi dùng `fixed` and `absolute` thì block elements sẽ không còn width 100% nữa mà mình phải tự set.

`z-index: 0;` by default.

Có 3 cách để hide elements from visible in the page but still retain their html code:

1. `display: none` => not good for accessibility (screen readers can’t read it since it completely remove the element from the flow of the page.
2. `opacity: 0`
3. `position: absolute;` rồi chỉnh cho ra ngoài viewport => still in the flow of the page and screen reader can still read it (ví dụ hình trên).

You can [center elements](https://www.joshwcomeau.com/css/center-a-div/#centering-with-positioned-layout-3) that floats above the content such as a dialog, a prompt, or a banner with the `position:` property.

The term **Flow layout** refers to the default way in which elements are position according to the order (flow) of the HTML source code.

## The `display:` property

- Block-level elements: `<main>, <header>, <section>, <article>, <p>, <div>, <li>, <h1> tới <h6>`
- Inline-level elements: `<span>, <a>, <button>, <em>, <img>, <input>, <label>`

Block displayed element by default will automatically add margin so that they take up 100% width of what is available to them (of their parent element). And they **stack** on each other as well.

- inline elements only take up the width of their content unless you put extra padding on them.
- You can NOT apply margin-top/bottom, width and height properties to inline elements.
- padding khi apply có thể gây lỗi hiển thị
- Inline elements can NOT have block-displayed children elements. Block elements on the other hand can have inline elements as children.

`display: inline-block;` will be handy when:

- inline-block is a hybrid because it stay inline with the paragraph and do not create a new line and allow some properties that normally wouldn’t be able to be applied to an inline display.
- Margin/padding + height: có thể apply lên inline-block elements mà ko gây lỗi nhé

`display: none;` completely remove the element from the page. It is still in the html tho. But the user won’t see it.

- It remove that element from the “document flow”. That means anyone using assistive technology screen reader would not be able to read anything there.
- There are other ways to make elements in-visible but still have it in the document flow where accessibility might be still available.

## Float

When we float a `<div>` element, it will take it out of the normal flow of the page and our text will wrap around it.

Now float is only used to float any elements left and right and you can wrap text around it.

You can also use float inside a container but remember to use display: flow-root; to the container element so the container can wrap around the full float element and doesn’t shrink based on the text content  alone tránh lỗi hiển thị vì float: remove the element from the normal flow the page.

## Flexbox

Distinguish Flexbox vs Grid Layout:

- Flexbox is a one-dimensional layout method for arranging items in rows or columns. Items flex (expand) to fill additional space or shrink to fit into smaller spaces.
- CSS Grid Layout is a two-dimensional layout system for the web. It lets you lay content out in rows and columns. It has many features that make building complex layouts straightforward.

**Summary:** Flexbox layout is most appropriate to the components of an application, and small-scale layouts, while the Grid layout is intended for larger scale layouts. Grid is more rigid, you have full control. Flex box is flexible.

Khi apply display: flex; lên 1 container element. Cái container element đó now gọi là “flex container” và tất cả element trong cái flex container gọi là “flex items”. Sau đó mình sẽ dùng justify-content: property để chỉnh vị trí .của các flex items. Default của nó là justify-content: flex-start;

Properties on the **flex container**:

- `justify-content: [flex-start, flex-end, center, space-between, space-around];` align items for the main axis (default left-right) => justify-conteXt for the X-axis.
- `align-items: [flex-start, flex-end, center, stretch];` for cross axis (default top-bottom) => AlYgn-Ytem for the Y-axis
- `align-content`: determines the spacing between lines. When there is only one line, it has no effect.

`flex-wrap` by default is `nowrap`.

`flex-direction`: `row (default) row-reverse column column-reverse`

Properties on the **Children (flex items)**:

`flex: 0 1 auto` is default. It is shorthand for grow, shrink, and basis. So by default, item will not grow but will shrink.  
The default `flex-basis: auto` means items' size are not equal but are based on their **content size**. If you set it with a single number value, like `flex: 5;`, that changes the flex-basis to 0 or 0% `flex: 5 1 0`. Flex items now have no original basis and the free space available are now equally distributed among them. Flex items will now have equal size (equal columns) and grow or shrink to fit the flex container.

Setting `flex-basis: 300px` also makes flex item equal size and is `300px` width.  
`flex-basis: 100%` also creates equal columns because all flex item start with 100% of parent with but then they shrink equally with `flex-shrink: 1` (default) => equal columns

- `flex-grow: 0` is not grow. `1, 2, 3` is grow proportionally. `flex-shrink` is also proportionaly.

`align-self` accepts the same values as `align-items`.

Grid & flex items both have the `order` property which controls the order in which items appear in the flex/grid container. It default value is `0`. By default, items are laid out in the source order. Kiểu như là `-1 0 0 0 1 2 5`, bigger order stand behind. Example: `order: 1;`  
The other property in common is `gap` (used in the container).

You can center elements both horizontally & vertically with flexbox. It also works for multiple children.

## Grid layout

Khi apply display: grid; lên 1 container element. Cái container element đó now gọi là “grid container” và tất cả element trong cái grid container gọi là “grid items”. Sau đó mình sẽ dùng properties liên quan tới grid layout theo ý mình muốn.

By default, you get a one column grid.

Property on **grid container**:

- `grid-template-columns: 1fr 1fr`. Fraction (fr) is a unit specifically used with CSS grid layout.
- `grid-template-columns: repeat(4, 1fr)` => repeat 4 lần “1fr”
- `repeat(2, 1fr 2fr)` => 1fr 2fr 1fr 2fr
- `repeat(8, 12.5%)` => eight columns each with 12.5% width
- `grid-template-columns: 100px 3em 40%;` => any size you want. You have full control!

Grid also introduces a new unit, the fractional `fr`. Each fr unit allocates one share of the available space. For example, if two elements are set to 1fr and 3fr respectively, the space is divided into 4 equal shares; the first element occupies 1/4 and the second element 3/4 of any leftover space.  
When columns are set with pixels, percentages, or ems, any other columns set with fr will divvy up the space that's left over:

- `grid-template-columns: 50px repeat(3, 1fr) 50px;` => A 50 pixel column on the left, and a 50 pixel column on the right and three more `1fr` columns that take up the remaining space in between.
- `grid-template-columns: 75px 3fr 2fr;` => A 75 pixel column of weeds on the left side of your garden. 3/5 of the remaining space is growing carrots, while 2/5 has been overrun with weeds.

Với flexbox mình không control được number of columns. Với grid mình cho template 4 column là always 4 columns. Nếu có thêm child element thì grid tự động add new row (implicit rows) but always stay 4 column. Vậy nên khi xài grid chỉ cần khai báo number of column, còn row không cần declare `grid-template-row` mà để tự nó tạo khi có child element mới.

- grid-auto-rows: minmax(150px, auto);
- minmax() CSS function: specify min value rồi tới max value
- space between the cells (column-gap, row-gap) được gọi là **gutter**.

- `align-items: start | end | center | stretch;` => along the block (column) axis
- `justify-content: start | end | center | stretch | space-around | space-between | space-evenly;` => along the inline (row) axis

Properties on the **grid items**:

- `grid-column` Shorthand for `grid-column-start` + `grid-column-end`. The grid lines & columns start counting at `1` from left-> right. Negative values count from left<-right.
- `grid-column: span 2` span 2 columns regardless of line number. You can also specify negative span value.
- `grid-column: 4` start at line number 4 don't need grid column end
- `grid-column: 2 / 4;` start on the 2nd vertical grid line and end on the 4th grid line.
- `grid-column: 2 / span 3`;

`grid-row-start`, `grid-row` giống y chang.

If typing out both `grid-column` and `grid-row` is too much for you, there's yet another shorthand for that. `grid-area` accepts four values separated by slashes: `grid-row-start, grid-column-start, grid-row-end, followed by grid-column-end`.

You can overlap grid items one on top of another and combine with `order`.

```css
/*one grid item many properties*/
.grid_item{
  grid-column-start: 4;
  /* grid-row-start: 1;
  grid-row-end: span 2; */
  grid-row: 1 / span 2;
}
```

Nhớ add properties theo thứ tự general > specific: column > row > start > end. Cái nào không cần thì không declare. Only add specific khi cần.

## Import CSS and specificity

- external CSS: dùng thẻ `<link rel="stylesheet" href="style.css">` trong `<head>`
- internal CSS: dùng thẻ `<style></style>` trong `<head>`
- Inline CSS: dùng `style=""` attribute

Cái `<style>` internal element không có được ưu tiên (precedency) trước external CSS `<link>`. Hình trên là đơn giản vì external CSS Line15 được đọc sau theo thứ tự top-bottom nên sẽ over-written cái internal CSS nằm trên.  
However, the in-line CSS `style=””` attribute) does take precedent over the other 2 types of CSS because it’s applied directly to the element itself.

The **specificity** is not a decimal number but a triad that consists of three components: A, B, and C. Specificities are compared by comparing the three components in order `(A,B,C)`

- A: id-like specificity
- B: class-like specificity: class selectors, pseudo-class, attribute selector
- C: element-like specificity: element (HTMl type/tag selector), pseudo-element

## Selectors, combinators

The CSS **Universal Selector** or wildcard selector `*` selects all elements. You typically only see this for what is called a **CSS reset**.  
A universal selector (*) adds no specificity.

The three simple selectors: type selectors (HTML tags, element selector), id and class selectors.

Typically you should use classes and sometimes type selectors but **rarely** if ever should you use an id selector (like `#featured {}`) inside of your CSS. Id selectors do have some valid uses (in HTML to linking a form input back to a label element and other use cases in JavaScript as well). But we try to keep IDs out of CSS.  
Class selectors are ideal because they are slightly more specific than element selectors but without the heavy-handedness of IDs.

**Chaining selectors** is a general term refers to many different way of chaining selectors: compound selector, empty space, selector group, etc...  
When a complex or compound selector is used, each part of that selector adds up to the specificity.

A CSS **selector group** or **selector list** is a comma-separated list of selectors. It selects all the matching nodes. Note: tránh nhầm lẫn với **descendant combinator** which use empty spaces.  
For example you can group multiple selectors with commas `h1, h2`. But if you forgot the comma như `h1 h2` thì nó sẽ tìm `<h2>` là children của `<h1>` để apply rules.

A [compound selector](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors/Selector_structure#compound_selector) is a sequence of simple selectors that are not separated by any combinator (not even the empty space). It targets a single element that simultaneously matches all the conditions specified by the simple selectors within the compound selector:

- `p.highlight`: Selects any `<p>` element that also has the class `highlight`. This will not select a <span> element with the class highlight, nor will it select a <p> element without that class.`
- `div#main-content`: Selects a <div> element that has the ID of `main-content`.
- `.button.primary`: Selects any element that possesses both the class `button` and the class `primary`.
- `input[type="email"]`: Selects an `<input>` element with `type="email"`.
- `a:hover`: Selects an `<a>` element when the user's mouse cursor is hovering over it.

A **complex selector** is a sequence of one or more simple and/or compound selectors that are separated by combinators, including the white space descendant combinator.

Specificity can over-write the “cascade” rule in CSS (top->bottom like a waterfall). Class selectors are more specific than element selector and therefore will over-write element selectors no matter where they are in the top-bottom order.  
[Specificity calculator](https://specificity.keegan.st/) của cisco

**Attribute selectors** (select elements based on an attribute or attribute value); use square brackets `[]`

The key difference between `tr[class="total"]` and `tr.total` is that the first will select tr elements where the only class is `total`. The second will select tr elements where the class includes total.

### Combinators

There are four different combinators in CSS: the empty space-" ", `>`, `+`, `~`.

The [descendant combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_combinator) (a single space bar character) — combines two selectors such that elements matched by the second selector are selected if they have an ancestor (parent, parent's parent, parent's parent's parent, etc.) element matching the first selector. Selectors that utilize a descendant combinator are called descendant selectors.  
So something like `.ancestor .child {}` would select an element with the class child if it has an ancestor with the class ancestor. Another way to think of it is child will only be selected if it is nested inside of ancestor, _no matter how deep_.

There’s really no limit to how many combinators you can add to a rule, so `.one .two .three .four` would be totally valid. This would just select an element that has a class of four if it has an ancestor with a class of three, and if that ancestor has its own ancestor with a class of two, and so on. You generally want to avoid trying to select elements that need this level of nesting, though, as it can get pretty confusing and long, and it can cause issues when it comes to specificity.

A (direct) **child combinator** `>` is really just an offshoot of the descendant combinator, only it is more specific than the descendant combinator because **it only selects direct children of an element**, rather than any descendant.

The [next-sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Next-sibling_combinator) (adjacent sibling) `+` separates two selectors and matches the second element only if it immediately follows the first element, and both are children of the same parent element.  

The [subsequent-sibling combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Subsequent-sibling_combinator) or general sibling `~` separates two selectors and matches all instances of the second element that follow the first element (not necessarily immediately) and share the same parent element.

### New CSS Nesting (2023)

August 2023

Descenndant combinator by nesting instead of using space bar.

In `SASS`, the `&` is called the [parent selector](https://sass-lang.com/documentation/style-rules/parent-selector/).

In native CSS, `&` is called the [nesting selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Nesting_selector).

CSS Nesting (native css) uses the ampersand `&` symbol to reference the parent selector, similar to Sass/SCSS. This allows writing nested rules that compile to descendant selectors, pseudo-classes, and modifier classes. It works a little differently than Sass, in that we can't use it to make BEM style selectors like &__child, but it's still super handy.

### Pseudo Selectors

**Pseudo selectors** include both pseudo classes and pseudo elements.

- **pseudo class**: a selector that selects elements that are in a specific **state**; use a single colon for example `:hover, :focus, visited, active, link`. Those are different states of the same element.
- **pseudo element**: select and style a part of an element like you’ve added a new HTML element into your document; uses two colons for example: `::first-line, ::first-letter`

- The `::before, ::after` pseudo-element creates a pseudo element that will have content.
- The `::first-letter, ::first-line`

- The `:not()` pseudo class will select whatever does not have what you specify. `:not()` by itself adds nothing to the specificity calculation.
- The `:is()` pseudo class also does not itself add to the specificity calculation
- The `:where` pseudo class cũng giống :is nhưng nó ko có specificity gì hết.

The `:any-link` pseudo class selects both the :link (un-visited link) and the :visited pseudo classes. Hình 2 chức năng giống hình 1, nhưng ngắn hơn.

The `:nth-child` pseudo class will be based on the order within the HTML document. Nếu mình dùng flex hay grid để thay đổi thứ tự display on the page thì nó không làm thay đổi thứ tự trong HTML document.  

- `:nth-child(odd), :nth-child(even)` => select odd childs  
- `:nth-child(1)` == `:first-child`

## CSS variables

The `:root` pseudo-class. Everything inherits from it. It refers to the document root. Even the <html> tag inherits from the :root pseudo-class.  
variable names always start with two hyphens (or two dashes)
Muốn apply variables in CSS rules thì dùng var() function.
You can have 2 different variable names store the same values.

CSS variables không chỉ dùng cho colors. Nó còn có thể dùng cho typography values.
FF là font-family
FS là font-size, XL là extra large

Tạo dark mode của website bằng CSS. We need to define a separate color there. But we do it inside of a media query. Mình tạo 1 :root pseudo-class inside a media query để chứa varialbles for dark mode colors.
Cái máy của ông thầy trong phần setting chỉnh là prefer dark mode.

## CSS funtions

The `min()` function always select the smallest of the two available. One of these is an absolute value, and the other will change its value. The absolute value we provided was the largest it would ever get. Hình trên 2.25rem là 1 absolute value. There are some notes when using min() function.

1. You should only provide one absolute value, the other one should be relative.
2. It is better to choose viewport height (vh) units for fonts thay vì choose viewport width (vw).
Dùng percentage for the relative value cũng ko tốt, nên dùng (vh).

The `max()` function is the opposite of the min(). It will always choose the largest of the two. The absolute value we provided was the smallest it will ever get.  
remember max() provides a minimum absolute value, and min() provides a maximum absolute value.

The `clamp([minimun], [ideal size], [maximum])` function.
Thay vì dùng min() hay max() thì dùng clamp() is the best option.

The filter: property can be used with many CSS functions:\

- brightness(150%)
- hue-rotate(180deg) => you will always get a complementary color if you provide 180deg

The attr() function (attribute function).
Chú ý .tooltip:hover::before selector stack a pseudo-element on a pseudo-class

The repeat() function.
minmax() is a CSS math function that is specific to grid. minmax([smalles grid item], [largest grid item])

## Media Querry

**Media queries** allow you to apply CSS styles depending on a device's media type (such as print vs. screen) or other features or characteristics such as screen resolution or orientation, aspect ratio, browser viewport width or height, user preferences such as preferring reduced motion, data usage, or transparency.

The `viewport <meta>` tag is necessary to enable our media queries and allow responsive design in our web pages.

Logical operators can be used to construct more complex media queries. The 'and' logical operator is used to query two media conditions. For example, a media query that targets a display width between 500px and 1000px would be:

```css
@media (min-width: 500px) and (max-width: 1000px){
}
```

Btw, bình thường không ai đang dùng device mà đi change size hết (mở ChromeDevtool lên change screen size manually). Nên mình cũng ko cần quan tâm tới việc làm cho cái transitions between different media queries nó “smooth” hay gì đâu.

Media queries allow you to modify your sizes based on specific characteristics and parameters. Most of the time, we care about the width of the browser viewport. This is key to responsive design. We don't care about height.

condition `min-width` nghĩa là “starting from”. Còn `max-width` nghĩa là “up to”.  
`min-width: 481px;` means: all the style đã viết trong stylesheet before this media query will be applied UNTIL we got to 481px (từ 0-481px), and then they would still be applied nhưng những CSS rules viết trong media querry sẽ bắt đầu override.

Mobile design thường sẽ là 1-column design. Desktop là 12 column design.

min-width and max-width with pixels as a breakpoint is very commonly used. Nhưng vẫn có những condition khác nhé như: orientation: landscape, min-aspect-ratio: 16/9;

- Desktop First chúng ta sẽ sử dụng thuộc tính `max-width`.
- **Mobile First** sử dụng `min-width`.

If you're targeting multi-platform, mobile first is always the correct way.

## Animation in CSS

The main ingredient we need to create an **animation** is some CSS that changes. Animation trong CSS có 3 family of properties là: transform, transition, and animation. Tụi nó có những đặc điểm riêng

The `:first-child` pseudo-class selector will select the first div. Ngoài ra còn có `:nth-child` and `:last-child`.

The `transform:` property dùng để transform elements in CSS. Nó có thể dùng với nhiều functions như sau:

- `translateX(), translateY()` will move the element left-right-up-down
- `rotateX()` để xoay theo chiều kim đồng hồ
- `scaleX(), scaleY()`
- `skewX(), skewY()` làm element bị "bóp méo"

Nếu chỉ dùng `transform` mà không dùng `transition` thì không có animation gì hết. The changes in CSS will happen instantaneously.  
The `transition:` CSS property is a shorthand property. It can take a number of values, but only two are required:

1. The **name** of the property we wish to animate such as `transform`. When `all` is specified, any CSS property that changes will be transitioned.
2. The duration of the animation

There are several [timing functions](https://www.joshwcomeau.com/animation/css-transitions/#timing-functions-2) available to us in CSS. We can specify which one we want to use with the `transition-timing-function` property:

the `animation:` là một short-hand property. Animation khác **transition** ở chỗ là mình phải define keyframe.

## Colors

### Opacity

Opacity `1` is default. Opacity `0.5` thì see through (transparent) 50%.

With the CSS opacity property, you can control how opaque or transparent an element is. With the value 0, or 0%, the element will be completely transparent, and at 1.0, or 100%, the element will be completely opaque like it is by default.

To add an alpha channel to an rgb color, use the rgba() function instead. The rgba() function works just like the rgb() function, but takes one more number from 0 to 1.0 for the alpha channel.

You can also use an alpha channel with hsl() and hex colors:\

- Hex color with alpha channel: `#3b7e20cc`
- hsl() color with alpha channel: hsla(223, 59%, 31%, 0.8)

## Images

foreground images: images on the page
background images: images on the background.

Một số properties dùng chung với background-image:

- background-repeat: [repeat, no-repeat, repeat-x, repeat-y]; Default là repeat.
- background-size: [cover, contain];
- background-position: ;

Cái width=”” and height=”” HTML attributes là để cho browser biết để reserve that space to prevent content layout shift in case a failure of the CSS file to load or something like that. We can override width and height with CSS

[filter-function](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function) are graphical effects that can change the appearance of an input image: `blur(), opacity()`

### Foreground vs background images

When constructing a website there are two different types of image: foreground images and background images.

As a rule of thumb:

1. Foreground Image (i.e. `<img>` tag) = content
2. Background Image (i.e. image specified in CSS) = design

This means if your image represents a piece of content, then it should be a foreground image – that is, an `<img>` tag. If your image represents a piece of design, then it should be a background image and included in your CSS file.

Next, consider these statements:

1. I want a different image to display at different screen sizes.
2. I want no image to display at all on certain devices or at certain sizes.
3. I’m not bothered about whether this image appears on a print out of the page.
4. I’m not bothered about whether this image appears if CSS is turned off in the user’s browser.
5. There isn’t any appropriate ALT text for this image.

If any or all of the above statements are true then the image is probably “design” and you might consider using a background image in the site’s CSS.

Sometimes there can be confusion as to which type of image to use. You’ll notice in the first `<img>` example above, the logo has an “ALT” attribute. This stands for “alternative text” and is a description of what the image represents. This alternate description is displayed if an image fails to load, or if the user has images turned off in their browser settings.

ALT attributes not only make images more user-friendly, but they’re also good for SEO. One or two of our more tech savvy clients have sometimes asked if a particular background image on their website includes ALT text. The issue here is that background images don’t/can’t feature an ALT attribute, and it becomes important to consider the differences between foreground and background images more closely.

## Inheritance

Inheritance is where another element inherit the properties from its parent element. Inheritance is handy because it keeps us from writing repeated code. Keep our code DRY (Don’t Repeat Yourself).

The <body> element is parent to every other element. To take advantage of inheritance and write DRY code, we could use the <body> element or <html> element.

Typically, anything related to font and typography is inherited (color, line-height, alignment, etc). However, any property that are not related to those things are not inherited.

Note that the form elements `<button>, <input>, <textarea>, select` typically do _NOT_ inherit those font setting.

## Typography

Cái `font-size` property trong `<body>` sẽ được inherited cho tất cả elements inside `<body>`.

`line-height:` will change height of text elements like `<p>, <h1>, <h2>`

`color: property` => dùng để chỉnh text color. Shorthand là khi `#000000` có thể ghi ngắn là `#000`, nhưng \#010203 thì không ghi shorthand được.

Dùng color palette picking tool,  color contrast checker.

Màu `\#333` màu đen rất đẹp.

text properties(as in <p>)

- `color:` property: change text color (như trong <p>).
- `background-color:`, background: will do the same thing but shorter.
- `font-size:, font-weight:, font-family:, font-style:`
- text-transform: uppercase; => chuyển hết text sang UPPERCASE.
- text-decoration:, text-indent:
- text-align:
- line-height:, letter-spacing:, word-spacing:

font-family:

- serif => academic papers
- sans-serif => use commonly on the web

## Unit & Size

The CSS `clamp()` function is used to set a value that will adjust responsively between a minimum value and a maximum value depending on the size of the viewport.

The only absolute length units we will commonly used is pixel. We don’t usually use an absolute value pixel for a font-size: because the user can change their references and the font-size won’t change, it is fixed. It will not change no matter what.

`rem` is a relative length unit. This is the unit we will typically used for font-size and it is relates to the font-size of the root element.

we shouldn’t set a font-size on the root element. We want to let the browser handle that by default.

`<p>` có font-size là 1rem by default.

rem = root element = default font-size set by the browser. Most browser default root element là 16px, nên 1 rem = 16px. Khi set font-size thì dùng rem ko dùng em.\
the em doesn’t stand for “element” that M.

em is “inherited” in the sense that it is relatives to its parent element. rem is not “inherited” because it always relates to the root size. We use em on margin or padding. em dùng trên margin và padding thì sẽ relaties to the element’s font size.

2 other relative length units là `vw và vh`.  

By default the `<body>` is not the full-height of the page, it only grows with the content (hình số 2). But sometimes, we want a <body> element that is the full-height of the page even if we don’t have enough content to do that. And we can do that with viewport height unit (vh). Hình số 3, nếu dùng height: thay vì min-height: thì nó sẽ limit <body> to 100vh và content trong <body> sẽ out-grow cái <body> height.

Font-size using em or % inherited and gets added on top of whatever it got from its parent. Như hình trên thì h1 sẽ có font-size là 3em + 2em = 5em. Unit px is NOT inherited, it does not matter what the parents are.

rem (root em) will ignore all of the parent settings for the font-size and just set it relatively to the root. => you should use rem instead of em or % because it is the most adaptable and also the most reliable and least error-prone.

Font and text properties are usually inherited. However, they do not inherit the text inside a <form> (<input> or <button>). Muốn tụi này inherit thì phải set explicitly ra

text-transform: property khá hay, có thể nhận các values sau:

- Capitalize: Kiểu Như Vầy Nè :-))
- lowercase, uppercase => affect all letters.

text-indent: mặc định là không có. Nên dùng em ko dùng rem

## Styling links

Các properties dùng cho link elements (<a>)

- text-decoration:, cursor:, color:,
- The pseudo classes a:visited {}, a:hover {}, a:active{}

## Styling lists

property for ordered list <ol>. All these properties can also be applied to un-order list as well (<ul>)

- list-style-type: => chỉnh 1.2.3 or a.b.c, cái cách đánh số thứ tự.
- list-style-position:
- list-style-image: => chọn 1 hình của mình làm bullet
- list-style: => shorthand cho 3 cái properties ở trên (Line21 hình trên).
- text-align:, line-height:
- color: => change both the bullet and the text colors.

ul li:nth-child(odd) and ul li:nth-child(even) sẽ tính chẳn lẻ. This is a pseudo class

The pseudo-element (not pseudo class) ::marker {} will affect all marker.

## Organizing your CSS

BEM - Block, Element, Modifier là 1 naming convention người ta dùng nhiều.

## Debugging

Sometimes, a property you applied but not show any effect on the page là bởi vì specificity => dùng specificity calculator page để check.

## Refrences

[The Odin project](https://www.theodinproject.com/)
