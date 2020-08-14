# CSS & Less Standards

This style guide aims to provide the ground rules for CFPB's CSS,
such that it's highly readable and consistent across different developers on a team.

## Table of Contents

1. [General Practices](#general-practices)
1. [Property Order](#property-order)
1. [Naming Conventions](#naming-conventions)
1. [Property values](#property-values)
1. [Media Queries](#media-queries)
1. [Less](#less)
    - [Less General Practices](#less-general-practices)
    - [Selectors and Nesting](#selectors-and-nesting)
    - [Variables](#variables)
    - [Comments](#comments)

## General Practices

- Use soft-tabs with a four-space indent
- One line per selector
- Always end property declarations with a semicolon
- Put spaces after `:` in property declarations
- Put spaces before `{` in rule declarations
- Place `{` on the same line as the (last) selector and `}` on its own line

For example:

```css
.btn,
.btn__big {
    color: #000;
}
```

## Property Order

Declare properties in three groups:

1. Display & box model
   1. Changing the current `display` or `box-sizing` values
   2. Box model properties in order from inside-out
   3. `overflow` behavior
2. Positioning
   1. Changing the current `float` or `position` values
   2. Position properties in typical CSS clockwise fashion (`top`, `bottom`, `left`, `right`), followed by `z-index`
3. Everything else
   - Alphabetical order

We recommend this ordering so that when you first look at a component's style declaration,
you can quickly get a sense of how a component is sized and positioned,
which affects the flow of the rest of the page,
and then move on to the properties that only affect that component itself.

```css
.selector {
    /* Display & Box Model (mode switching, then box-model from inside out, then overflow handling) */
    display: inline-block;
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    padding: 10px;
    border: 10px solid #333;
    margin: 10px;
    overflow: hidden;

    /* Positioning (mode switching, then positioning counterclockwise from top, then z-index */
    float: left;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 10;

    /* Other (alphabetical) */
    background: #000 url('../img/bg.png') no-repeat center top;
    color: #fff;
    cursor: pointer;
    font-family: sans-serif;
    font-size: 16px;
    font-style: italic;
    font-weight: bold;
    line-height: 1;
    list-style: none;
    text-align: right;
}
```

_N.B.: It is not necessary to include the group headings in your actual CSS, or to include a blank line between groups.
The headings and blank lines are just for added clarity in this example._

## Naming Conventions
Custom [BEM ("Block Element Modifier")](https://en.bem.info/method/definitions/) format:

```css
.block-name
.block-name_element-name
.block-name__block-modifier
.block-name_element-name__element-modifier
```

### Avoid creating elements of modifiers
Appending an element name to a modifier class can result in a confusing class name like `.list__space_item`.
Avoid this in favor of using a descendant, like this: `.list__spaced .list_item`.

### `id` attribute

While the `id` attribute might be fine in HTML and JavaScript, it should be **avoided entirely** inside stylesheets for a few reasons:

- ID selectors are not reusable
- Can lead to priority issues

##### Good

```css
.ur-name {
    // ...
}
```

##### Bad

```css
#ur-name {
    // ...
}
```

Just assign a class name to the element.

## Property values

These rules apply to your CSS property values

- If the value of a property is `0`, do not specify units
- The `!important` rule should be aggressively avoided.
  - Keep style rules in a sensible order
  - Compose styles to dissipate the need for an `!important` rule
  - Fine to use in limited cases
    - Overlays
    - Declarations of the `display: none !important;` type
- Use hex color codes `#000` unless there's an explicit need for an `rgba` declaration
- Avoid mixing units
- Unit-less `line-height` is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.

##### Good

```css
.btn {
    color: #222;
}

.btn-red {
    color: #f00;
}
```

##### Bad

```css
.btn-red {
    color: #f00 !important;
}

.btn {
    color: #222;
}
```

## Media Queries
In most cases styles should be declared mobile first, then enhanced with `min-width` media queries.
By doing this we create a base experience that all devices can use and one that does not require media query support.

## Less

At the CFPB we use Less as our CSS preprocessor.

### Less General Practices

- Organize your Less code into multiple files/modules
- Prefer nested selectors `.foo { .bar {} }` vs `.foo .bar {}`
  - _Only if both `.foo` and `.foo .bar` need styling_
- Don't nest selectors beyond 3 levels deep

### Selectors and Nesting

Styles shouldn't need to be nested more than three levels deep. This includes pseudo-selectors.
If you find yourself going further, think about re-organizing your rules _(either the specificity needed, or the layout of the nesting)_.

### Variables

- Colors: (use generic variable names like recent consumerfinance.gov update)
- Breakpoints: standard variable names for commonly used breakpoints

### Comments

[Use JSDocs](http://usejsdoc.org/) style comments for comment blocks and `//` for inline comments.

#### Purpose
Comments **aren't meant to explain what** the code does. Good **code is supposed to be self-explanatory**.
If you're thinking of writing a comment to explain what a piece of code does, chances are you need to change the code itself.
Good comments are supposed to **explain why** code does something that may not seem to have a clear-cut purpose.

#### Example

```less
/**
 * Default button styles
 * @class .btn
 * @elements a, button
 */

.btn {
    display: inline-block;

    &:focus {
        background-color: @btn-bg-hover;
        outline: 1px dotted @btn-bg;
        // outline-offset is not supported everywhere but it adds a nice touch where it is
        outline-offset: 1px;
    }
}
```

## Credits

Some of this document is based on [Nicolas Bevacqua's fbc-style-guide](https://github.com/bevacqua/css), which is licensed under the MIT license.
