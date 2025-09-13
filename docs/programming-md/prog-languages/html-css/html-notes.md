# HTML notes

## Meta Charset UTF 8

`<meta charset=“utf-8”>` is an HTML tag that makes it possible to use emojis and other characters outside of the ASCII character set on your webpage. If you don’t use the tag, then you will need to look up HTML entities to manually insert an emoji or other character.

The [UTF-8 character set](https://en.wikipedia.org/wiki/UTF-8) (Unicode Transformation Format - Định dạng chuyển đổi Unicode 8-bit) covers all characters and symbols that exist in this universe (emoji, Chinese, Japanese, Korean, Hebrew, Russia, Arabic, etc).

- ASCII map between 0-127 (128 different characters)
- Unicode

**HTML entities** are special codes used to represent characters that are either reserved characters in HTML, characters not available on a standard keyboard, or characters that might be interpreted as HTML code.

- `&lt;`: Represents the less-than sign (<).
- `&gt;`: Represents the greater-than sign (>).
- `&amp;`: Represents the ampersand (&).
- `&nbsp;`: Represents a non-breaking space.
- `&copy;`: Represents the copyright symbol (©).
- `&#8364;`: Represents the euro symbol (€

## Combo box

Dropdown menu (select menu):

```html
<select name="pets" id="pet-select">
  <option value="">--Please choose an option--</option>
  <option value="dog">Dog</option>
  <option value="cat">Cat</option>
  <option value="hamster">Hamster</option>
</select>
```

Checkboxes (square boxes) có thể chọn nhiều boxes cùng một lúc:

```html
<input type="checkbox" id="vehicle1" name="vehicle1" value="Bike">
<label for="vehicle1"> I have a bike</label><br>

<input type="checkbox" id="vehicle2" name="vehicle2" value="Car">
<label for="vehicle2"> I have a car</label><br>

<input type="checkbox" id="vehicle3" name="vehicle3" value="Boat">
<label for="vehicle3"> I have a boat</label><br>
```

Radio buttons: Only one radio button in a given group can be selected at the same time. Radio buttons are typically rendered as small circles, which are filled or highlighted when selected.

Group must have the same `name=""`

```html
<fieldset>
  <legend>Select a maintenance drone:</legend>

  <div>
    <input type="radio" id="huey" name="drone" value="huey" checked />
    <label for="huey">Huey</label>
  </div>

  <div>
    <input type="radio" id="dewey" name="drone" value="dewey" />
    <label for="dewey">Dewey</label>
  </div>

  <div>
    <input type="radio" id="louie" name="drone" value="louie" />
    <label for="louie">Louie</label>
  </div>
</fieldset>
```

## Markup Language vs Programming language

HTML, xml, json are not programming language like javascript

Hypertext Markup Language (HTML):

- Define text, images
- Define web structure: headings, paragraphs, links

XML (eXtensible Markup Language)

- Mainly used to store and transport arbitrary, complex datas with a focus on flexibility and the ability to define custom tags. Whereas HTML is very specifically meant to construct a web document. One could think of HTML as a particular extension of XML, meant for communicating web pages between web servers and web browsers. Another concrete example of XML is SVG, for describing vector graphics.
- XML có cấu trúc phức tạo hơn JSON, provide more feature hơn JSON nên được dùng để exchange data between different systems and applications (game development)
- Uses tags to define elements, similar to HTML, but allows for user-defined tags and attributes, making it more extensible

JSON (JavaScript Object Notation)

- Dựa trên Javascript object, primarily used for data transmission between a server and a web application
- Lightweight and easy for machines to parse, making it ideal for web APIs. JSON uses key-value pairs which is simpler and more compact than XML

## Semantic elements

`<header>` can encompass a broader range of introductory content, while `<nav>` is specifically for navigation links.  
Đôi khi chỉ cần dùng `nav`, không cần dùng `header`.  
Trong phần `<footer>` có thể có thêm 1 cái `<nav>` thứ 2 nữa

Những semantic emelents chính trong website:

```html
<body>
  <header>
    <!-- logo + navigation -->
  </header>

  <main>
    <section></section>
    <section></section>
  </main>

  <footer></footer>
</body>
```

Semantic HTML is good for website reader, for google search SEO

- The `<strong>` tag is used to define text with strong importance. The content inside is typically displayed in bold.
- `<em>` is displayed in italic. However, it should not be used to apply italic styling; use the CSS font-style property for that purpose.

## ARIA

Accessible Rich Internet Applications **(ARIA)** is a set of [roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles) and [attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes) that define ways to make web content and web applications (especially those developed with JavaScript) more accessible to people with disabilities.
