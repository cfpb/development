# Accessibility

Your website should aim to meet
[Level AA of the WCAG 2.0 rules](http://www.w3.org/TR/WCAG/),
which have guidelines for creating accessible web applications
that surpass the current 508 standards
and are expected to become the basis for new 508 standards in the future.

Your website should follow the following four principles.

- Perceivable: Can the user perceive everything on the page?
- Operable: Can a user operate all parts of a page regardless of their input device?
- Understandable: Can a user understand the page?
- Robust: Is the page robust enough to be read by assistive devices and multiple browsers?

To understand more about how we build accessible interfaces according to
POUR principles and get a checklist for these guidelines,
[Read more about accessibility at CFPB Design Manual](http://cfpb.github.io/design-manual/guides/accessible-interfaces.html).

## Testing For Accessibility

Testing for accessibility should be done throughout the project lifecycle.

### Keyboard Testing

You don't need any special hardware for your first accesibility test.
Keyboard testing is making sure that every part of your website
can be accessed by tabbing through the keyboard and
is done with the `tab`, `shift + tab` and `enter` keys.

By resolving issues with keyboard testing first, you can resolve other issues
with inaccessible content with screenreaders or other assistive devices.

To turn on full keyboard access on a Mac, you may need to visit
[System Preferences >> Keyboard](img/full-keyboard-access.png).

1. Type in the web address of the website you'd like to test in your URL bar.
2. Hit `enter`.
3. Using only your keyboard,
navigate to different parts of your website using
the `tab`, `shift + tab` and `enter` keys.

** Issues to look for **
-[ ] When you a link is selected with the keyboard, it should have a
non-color indicator of focus such as an outline.
-[ ] Look for a logical order for all links on the page
-[ ] If there's any information that is triggered by hoverover,
like help text or tooltips,
this should be able to accessed with the keyboard.

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


## Tools and linters

- [Sublime Linter with Tidy](https://github.com/SublimeLinter/SublimeLinter-for-ST2)
- [Grunt HTML Lint](https://www.npmjs.com/package/grunt-html)


## WAI-ARIA and HTML5
From the [A11yProject.com](http://a11yproject.com/posts/getting-started-aria/)

> Native HTML semantics should still be used whenever possible,
> but ARIA is useful when certain design patterns
> or interactions make it impossible to do so.
> For example, a complex tabbed-interface has no semantic equivalent with HTML,
> but a `role="tablist"` and its related attributes can be added
> to provide this detail to screen readers.
> ARIA is also useful to describe newer HTML elements that may not yet
> have full cross-browser support or be understood by screen readers.


## ARIA Landmarks

Aria landmark roles can be used by assistive technology to navigate a website.
HTML5 is moving towards including some of the functions in elements like `main`,
but [accessibility support for HTML5 elements is still lacking in some browsers](http://www.html5accessibility.com/).
For now, including both the HTML5 element and the ARIA landmark
(ex: `main` and `role="main"`)
is recommended.

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

Do not skip numbers in descending order (i.e. `<h1>` to `<h4>`) or
have excessive or sections unlabeled by a header.

You can also label `<section>`s with the `aria-labelledby=""` property.

``` html
<section aria-labelledby="KittensHeader">
  <span id="KittensHeader">All About Kittens</span>
  <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit.</p>
</section>
```

Test for header flow using the WAVE Toolbar.

[Read more about aria-labelledby at MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-labelledby_attribute)





## Credits

Accessibility guidelines are from [a11yproject.org](a11yproject.org), [WebAIM](http://webaim.org/), and Matt Long's [It's Tired In Here accessibility guidelines](http://itstiredinhere.com/accessibility/).
