# HTML Coding Standards

This documents outlines HTML code standards. 
The intent of the HTML standards is to foster 
cross-browser compatibility, accessibility, simplicity and maintainability.

## Coding Style

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

### HTML5 Specification

All HTML will be written in HTML5.


### Semantic line breaks in source code

Notice strange line breaks in the source code?
It's intentional.
Semantic line breaks help reduce the numbers of diffs in code reviews.


```
Notice strange line breaks in the source code?
It's intentional.
Semantic line breaks help reduce the numbers of diffs in code reviews.`

```

[Read more about Semantic line breaks](http://rhodesmill.org/brandon/2012/one-sentence-per-line/)


#### Validation

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

[Read more about the `lang` attribute in the spec](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element).

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

**HTML5 spec links**
- [Using link](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)
- [Using style](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)
- [Using script](http://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element)


## Syntax

### Semantic Elements

Use semantic elements whenever possible. 
Tags like `<div>` and `<span>` are great for layout but tell the browser nothing about content and require additional markup to make them keyboard accessibile.

  - Paragraphs of text should always be placed in a `<p>` tag. Never use multiple `<br>` tags.
  - Items in list form should always be in `<ul>`, `<ol>`, or `<dl>`. Never use a set of 
  `<div>` or `<p>`.
  - Use an HTML5 Shiv to "teach" older browsers how to handle elements like `<header>`, `<article>`, etc.


### Quotes

Always use double quotes, never single quotes, on attributes

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

For class or ID names refer to the CSS standards.


### Reducing markup

Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML. For example:

```
<!-- Not so great -->
<span class="avatar">
<img src="...">
</span>

<!-- Better -->
<img class="avatar" src="...">
```


### Other
- Don't omit optional closing tags (e.g. `</li>` or `</body>`).
- Use closing tag comments, like `<!-- /.element -->`, sparringly. These add to page load time and overuse counter-acts their purpose
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
`for` attribute wrapping automatically associates the two.
- Never use the placeholder attribute for essential text or inplace of the `<label>` tags/

**Buttons** 
- Form buttons should always include an explicit `type`. 
Use primary buttons for the `type="submit"` button and regular buttons for `type="button"`.
- The primary form button must come first in the DOM, 
especially for forms with multiple submit buttons. 
The visual order should be preserved with float: right; on each button.

``` html
  <label>First Name<input type="text"></label>

```

** More about forms**

+ [cf-forms: Form patterns in Capital Framework](https://github.com/cfpb/cf-forms) 
+ [CFPB Design Manual on Effective Forms](http://cfpb.github.io/design-manual/guides/effective-forms.html)


### Tables

First off, never use tables for layout purposes. 
Tables are for data. 

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

## Accessibility

Your website should aim to meet [Level AA of the WCAG 2.0 rules](http://www.w3.org/TR/WCAG/). 

[Read more about accessibility at CFPB Design Manual](http://cfpb.github.io/design-manual/guides/accessible-interfaces.html)

### ARIA Landmarks

Aria landmark roles can be used by assistive technology to navigate a website.

A few examples of some of the more prominent landmark roles from <a11yproject.com>

+ `<header role="banner">`  
A region of the page that is site focused. Typically your global page header.

+ `<nav role="navigation">` 
Contains navigational links.

+ `<main role="main">`  
Focal content of document. Use only once.

+ `<article role="article">` 
Represents an independent item of content. Use only once on outermost element of this type.

+ `<aside role="complementary">` 
Supporting section related to the main content even when separated.

+ `<footer role="contentinfo">` 
Contains information about the document (meta info, copyright, company info, etc).

+ `<form role="search">`
Add a `search` role to your primary search form. 

[More about WAI-ARIA in HTML from w3c](http://www.w3.org/TR/aria-in-html/)


### Header Flow and Hierarchy

Many screenreaders use heading tags (i.e. `<h1>`, `<h2>`, etc) to navigate websites.
Make sure these heading tags are in order to avoid confusion.

Test for header flow using the WAVE Toolbar.

### Testing For Accessibility

#### Keyboard Test

You don't need any special hardware for your first accesibility test. 
Just take away your mouse.

1. Type in the web address of the website you'd like to test in your URL bar.
2. Hit `enter`.
3. Using only your keyboard, 
navigate to different parts of your website using the `tab`, `shift + tab` and `enter` keys.

You should be able to tab to all interactive elements 
like form fields, links and buttons. 
The order of elements, especially navigation elements, should make logical sense. 

[Read more about keyboard testing on WebAIM](http://webaim.org/techniques/keyboard/).


#### Screenreader testing

Using a screenreader can be a great way to experience how screenreader users experience your webpages. 
WebAIM has a fantastic series and guide for using screenreaders for accesibility testing purposes.

+ [Using Voiceover to evaluate web accesibility](http://webaim.org/articles/voiceover/) - 
Learn how to use the built-in screenreader program on Macs and iOS products.
+ [Using NVDA to evaluate web accesibility](http://webaim.org/articles/nvda/) - 
Learn how to use NVDA, a free and open source screenreader for Windows.

[Read more about screenreader testing on WebAIM](http://webaim.org/articles/screenreader_testing/)


#### Other testing tools

**[WAVE web accessibility evaluation tool by WebAIM](http://wave.webaim.org/)** - 
WAVE will detect missing accessibility tags and poor practices and explain their impact. 
They also have a toolbar extension.

**[Chrome Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb?hl=en)** - 
A Chrome extension that adds an additional section to your developer tools 
to examine accesibility properties. 
Comes with audits to examine contrast, incorrect markup and other common errors.

**[Contrast Ratio Tool](http://leaverou.github.io/contrast-ratio/)** - 
Tests for contrast ratio based on the WCAG 2.0 guidelines on color contrast.

**[Colorblind Web Page Filter](http://colorfilter.wickline.org/)** - 
Applies coverage filters on webpages to test against different types of color blindness


