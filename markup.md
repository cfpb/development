# HTML Coding Standards

This documents outlines HTML code standards. The intent of the HTML standards is to foster 
cross-browser compatibility, accessibility, simplicity and maintainability.


## Syntax

- Use soft tabs with four spaces to guarantee code renders the same in any environment.
- Nested elements should be indented once (four spaces).
- Paragraphs of text should always be placed in a `<p>` tag. Never use multiple `<br>` tags.
- Items in list form should always be in `<ul>`, `<ol>`, or `<dl>`. Never use a set of 
`<div>` or `<p>`.
- Every form input that has text attached should utilize a `<label>` tag-especially `radio` or 
`checkbox` elements.
- Even though quotes around attributes are optional, always use quotes around attributes 
for readability.
- Always use double quotes, never single quotes, on attributes.
- Donâ€™t omit optional closing tags (e.g. `</li>` or `</body>`).
- Avoid trailing slashes in self-closing elements. For example, `<br>`, `<hr>`, `<img>`, and `<input>`.
- Use closing tag comments, like `<!-- /.element -->`,sparringly. These add to page load time and overuse counter-acts their purpose.
- Don't set `tabindex` manuallyâ€”rely on the browser to set the order.


## Indentation

HTML should be indented to reflect logical structure. However, to avoid extreme levels of
indentation, do not indent for the `<html>`, `<head>`, or `<body>` tags; start indenting after
that level. For example:

``` html
<html>
<head>
<title>Page title</title>
</head>
<body>
<div class="wrapper">
<h1>Heading here</h1>
</div>
</body>
</html>
```


## HTML5 Specification

All HTML will be written in HTML5.


## Validation

All pages should be verified against the [W3C validator](http://validator.w3.org/) to ensure that
the markup is well formed. This is not in itself a guarantee of good code, but it helps to eliminate problems which can be avoided via automation. It should not be considered a substitute for
manual code review.


## Accessibility

We work hard to make our sites and services as accessible and usable as we can for everyone who needs to use them.


## Head

### Doctype

Always use a proper doctype that triggers standards mode in your browser. Quirks mode should always be avoided.
For simplicity, use the html5 doctype:


``` html
<!DOCTYPE html>
```


### Language attribute

From the HTML5 spec:

> Authors are encouraged to specify a lang attribute on the root html element, giving the 
document's language. This aids speech synthesis tools to determine what pronunciations to use, 
translation tools to determine what rules to use, and so forth.

Read more about the `lang` attribute 
[in the spec](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element).

Head to Sitepoint for a [list of language codes](http://reference.sitepoint.com/html/lang-codes).

``` html
<html lang="en-us">
<!-- ... -->
</html>
```


### Character encoding

Quickly and easily ensure proper rendering of your content by declaring an explicit character 
encoding. When doing so, you may avoid using character entities in your HTML, provided their 
encoding matches that of the document (generally utf-8).

``` html
<head>
<meta charset="utf-8">
</head>
```


### CSS and JavaScript includes

HTML5 removed the need for the type attribute that was previously required for code validity but 
which had no effect on browser parsing. There is no need to specify a `type` when including CSS 
and JavaScript files as `text/css` and `text/javascript` are their respective defaults.

``` html
<!-- External CSS -->
<link rel="stylesheet" href="/path/file.css">

<!-- In-document CSS -->
<style>
/* ... */
</style>

<!-- JavaScript -->
<script src="/path/file.js"></script>
```

#### HTML5 spec links
- [Using link](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)
- [Using style](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)
- [Using script](http://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element)


## Boolean attributes

Many attributes don't require a value to be set, like `disabled` or `checked`, so don't set them.

``` html
<input type="text" disabled>

<input type="checkbox" value="1" checked>

<select>
<option value="1" selected>1</option>
</select>
```

_For more information, read the [WhatWG section](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes)._


## Quotes

HTML5 allows the use of unquoted attribute values (e.g., `rel=stylesheet`), but for interoperability
between editors and tools, as well as readability, we require the use of double quoted values.

_The exception to this is boolean attributes covered above._


## Self-closing Elements

HTML elements that do not have matching closing tags (e.g., `<hr>` and `<br>`) should not be closed
with a trailing slash (i.e., do not use `<hr />` or `<br />`).


## Case

Elements and attributes should be written in lowercase. Attribute values intended for humans to
read (such as `title` or `alt`) should be in mixed case, but otherwise they should also be in 
lowercase.

For class or ID names refer to the CSS standards.


## Reducing markup

Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML. For example:

```
<!-- Not so great -->
<span class="avatar">
<img src="...">
</span>

<!-- Better -->
<img class="avatar" src="...">
```


## Forms

- Lean towards radio or checkbox lists instead of select menus.
- When possible, wrap radio and checkbox inputs and their text in `<label>`s to reduce the need for
`for` attributesâ€”wrapping automatically associates the two.
- Form buttons should always include an explicit `type`. Use primary buttons for the 
`type="submit"` button and regular buttons for `type="button"`.
- The primary form button must come first in the DOM, especially for forms with 
multiple submit buttons. The visual order should be preserved with float: right; on each button.


## Tables

Make use of `<thead>`, `<tfoot>`, `<tbody>`, and `<th>` tags (and scope attribute) when appropriate. (Note: `<tfoot>` goes above `<tbody>` for speed reasons. You want the browser to load the footer before a table full of data.)

``` html
<table summary="This is a chart of invoices for 2011.">
<thead>
<tr>
<th scope="col">Table header 1</th>
<th scope="col">Table header 2</th>
</tr>
</thead>
<tfoot>
<tr>
<td>Table footer 1</td>
<td>Table footer 2</td>
</tr>
</tfoot>
<tbody>
<tr>
<td>Table data 1</td>
<td>Table data 2</td>
</tr>
</tbody>
</table>
```