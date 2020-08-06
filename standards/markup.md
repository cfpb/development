# HTML Standards

This documents outlines HTML code standards.
The intent of the HTML standards is to foster
cross-browser compatibility, accessibility, simplicity and maintainability.

## Table of Contents

1. [Coding Style](#coding-style)
1. [Head](#head)
1. [Syntax](#syntax)
1. [Patterns](#patterns-for)
1. [Credits](#credits)

## Coding Style

### HTML5 Specification

All HTML will be written in HTML5.


### Indentation

HTML should be indented to reflect logical structure.
However, to avoid extreme levels of indentation,
do not indent for the `<html>`, `<head>`, or `<body>` tags;
start indenting after that level.

Use soft tabs with four spaces to guarantee code renders the same in every environment.

For example:

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

### Semantic line breaks in source code

Notice strange line breaks in the source code?
It's intentional.
Semantic line breaks help reduce the numbers of diffs in code reviews.


```
Notice strange line breaks in the source code?
It's intentional.
Semantic line breaks help reduce the numbers of diffs in code reviews.
```

[Read more about Semantic line breaks](http://rhodesmill.org/brandon/2012/one-sentence-per-line/)


### Validation

All pages should be verified against the [W3C validator](http://validator.w3.org/) to ensure that
the markup is well formed.
This is not in itself a guarantee of good code,
but it helps to eliminate problems which can be avoided via automation.
It should not be considered a substitute for manual code review.


## Head

### Doctype

Always use a proper doctype that triggers standards mode in your browser.
Quirks mode should always be avoided.

For simplicity, use the html5 doctype:


``` html
<!DOCTYPE html>
```


### Language attribute

From the HTML5 spec:

> Authors are encouraged to specify a lang attribute on the root html element, giving the
document's language. This aids speech synthesis tools to determine what pronunciations to use,
translation tools to determine what rules to use, and so forth.

Additionally, this is a requirement for accessibility for screenreaders to
read the content in the correct language.

[Read more about the `lang` attribute in the spec](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element).

Head to Sitepoint for a [list of language codes](http://reference.sitepoint.com/html/lang-codes).

``` html
<html lang="en-us">
<!-- ... -->
<span lang="es">El Congreso estableció el CFPB para proteger
a los consumidores mediante el cumplimiento de las leyes financieras federales
para protección de los consumidores.</span>
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

**HTML5 spec links**
- [Using link](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)
- [Using style](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)
- [Using script](http://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element)


## Syntax

### Semantic Elements

Use semantic elements whenever possible.
Tags like `<div>` and `<span>` are great for layout but tell the browser nothing about content and require
additional markup to make them keyboard accessibile (also see tabindex rule in <a href="#other">Other</a>).

  - Paragraphs of text should always be placed in a `<p>` tag. Never use multiple `<br>` tags.
  - Items in list form should always be in `<ul>`, `<ol>`, or `<dl>`. Never use a set of
  `<div>` or `<p>`.
  - Use an HTML5 Shiv to "teach" older browsers how to handle elements like `<header>`, `<article>`, etc.


### Quotes

Always use double quotes, never single quotes, on attribute values.

HTML5 allows the use of unquoted attribute values (e.g., `rel=stylesheet`), but for interoperability
between editors and tools, as well as readability, we require the use of double quoted values.

_The exception to this is boolean attributes covered below._


### Boolean attributes

Many attributes don't require a value to be set, like `disabled` or `checked`, so don't set them.

``` html
<input type="text" disabled>

<input type="checkbox" value="1" checked>

<select>
    <option value="1" selected>1</option>
</select>
```

_For more information, read the [WhatWG section](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes)._


### Self-closing Elements

HTML elements that do not have matching closing tags (e.g., `<hr>` and `<br>`) should not be closed
with a trailing slash (i.e., do not use `<hr />` or `<br />`).


### Case

Elements and attributes should be written in lowercase. Attribute values intended for humans to
read (such as `title` or `alt`) should be in mixed case, but otherwise they should also be in
lowercase.

For class or ID naming conventions refer to the CSS standards.


### Reducing markup

Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML. For example:

```html
<!-- Not so great -->
<span class="avatar">
<img src="...">
</span>

<!-- Better -->
<img class="avatar" src="...">
```


### Other
- Don't omit optional closing tags (e.g. `</li>` or `</body>`).
- Use closing tag comments, like `<!-- /.element -->`, sparingly. These add to page load time and overuse counter-acts their purpose.
- Don't set `tabindex` manually; rely on the browser to set the order.


## Patterns for

### Forms

When designing forms,
think about form validation and good markup to ensure users with screenreaders can access them.

**General**
- Make sure the form follows a logical layout when navigating with a keyboard
- Lean towards radio or checkbox lists instead of select menus.
- When using radio buttons or checkbox lists, use the `<fieldset>` tag to group your elements together and describe the group with `<legend>`.

**Labels**
- When possible, wrap radio and checkbox inputs and their text in `<label>`s to reduce the need for
`for` attribute. Wrapping automatically associates the two.
- Never use the placeholder attribute for essential text or in place of the `<label>` tags/

**Buttons**
- Form buttons should always include an explicit `type`.
Use primary buttons for the `type="submit"` button and regular buttons for `type="button"`.
- The primary form button must come first in the DOM,
especially for forms with multiple submit buttons.
The visual order should be preserved with float: right; on each button.

``` html
<form action="" method="post" id="pizzaform">
    <label>First Name<input type="text" id="pizzaform_name"></label>
    <fieldset>
    <legend>Favorite Pizza Topping?</legend>
        <label>
            <input id="pizzaform_radio-pepperoni" name="pizzaform_radio-pepperoni" type="radio" />
            Pepperoni
        </label>
        <label>
            <input id="pizzaform_radio-cheese" name="pizzaform_radio-cheese" type="radio" />
            Cheese
        </label>
    </fieldset>
    <button type="submit">Go!</button>
</form>
```

**More about forms**

+ [Form patterns in Design System](https://cfpb.github.io/design-system/patterns/designing-forms)


### Links that open in new tabs

Links should only open in new tabs (`target="_blank"`) in situations where
users enter data or make selections that would be lost if they left the page.
When doing this, be sure to also include `rel="noopener noreferrer"` to mitigate
[potential vulnerabilities](https://www.jitbit.com/alexblog/256-targetblank---the-most-underestimated-vulnerability-ever/).

Additionally, add an `aria-label` that includes the link text and informs screen reader users
that the link will open in a new tab. For example:
`aria-label="Learn why some county data are unavailable. (Link opens in new tab.)"`

Full example:

```html
<a href="


### Tables

First off, never use tables for layout purposes.
Tables are for data only.

Make use of `<thead>`, `<tfoot>`, `<tbody>`, and `<th>` tags (and scope attribute) when appropriate.
Without this markup, screen readers will identify tables as layout tables instead of data tables.

(Note:
`<tfoot>` goes above `<tbody>` for speed reasons.
You want the browser to load the footer before a table full of data.)

``` html
<table summary="This is a table of Scrabble score's from yesterday's game.">
    <thead>
        <tr>
            <th scope="col">Player</th>
            <th scope="col">Scrabble Score</th>
        </tr>
    </thead>
    <tfoot>
        <tr>
            <th scope="row">All Players</td>
            <td>484</td>
        </tr>
    </tfoot>
    <tbody>
        <tr>
            <th scope="row">Aaron</td>
            <td>136</td>
        </tr>
        <tr>
            <th scope="row">Sam</td>
            <td>105</td>
        </tr>
        <tr>
          <th scope="row">Xochitl</td>
          <td>243</td>
        </tr>
    </tbody>
</table>
```

[Read more about tables at It's Tired in Here](http://itstiredinhere.com/accessibility/#tables).

## Credits

This guide includes inspiration and guidelines from the [GitHub Styleguide](https://github.com/styleguide/) and [Code Guide by @mdo](http://codeguide.co/#html-syntax).
