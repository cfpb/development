# JavaScript Standards

This style guide aims to provide the ground rules for CFPB's JavaScript code,
such that it's highly readable and consistent across different developers on a
team.


## Table of Contents

1. [Linting](#linting)
1. [Modules](#modules)
1. [Strict Mode](#strict-mode)
1. [Spacing](#spacing)
1. [Naming](#naming)
1. [Semicolons](#semicolons)
1. [Strings](#strings)
1. [Variable Declaration](#variable-declaration)
1. [Conditionals](#conditionals)
1. [Equality](#equality)
1. [Ternary Operators](#ternary-operators)
1. [Functions](#functions)
1. [Prototypes](#prototypes)
1. [Object Literals](#object-literals)
1. [Array Literals](#array-literals)
1. [Regular Expressions](#regular-expressions)
1. [`console` Statements](#console-statements)
1. [Comments](#comments)
1. [Variable Naming](#variable-naming)
1. [Polyfills](#polyfills)
1. [Everyday Tricks](#everyday-tricks)
1. [Credits](#credits)

## Linting

Lint your code using ESLint.
[CFPB-tailored ESLint options](https://github.com/cfpb/development/blob/main/.eslintrc)
exist.

## Modules

This style guide assumes you're using a module system such as [CommonJS][1],
[AMD][2], [ES6 Modules][3], or any other kind of module system. If you are
using CommonJS, refer to the [CommonJS guide](creating-commonjs-modules.md)
for structuring your modules. Modules systems provide individual scoping,
avoid leaks to the `global` object, and improve code base organization by
**automating dependency graph generation**, instead of having to resort to
manually creating multiple `<script>` tags.

Module systems also provide us with dependency injection patterns, which are
crucial when it comes to testing individual components in isolation.

## Strict Mode

**Always** put [`'use strict';`][4] at the top of your global code (code that
is not exported), modules already use strict mode and thus don't require it.
Strict mode allows you to catch nonsensical behavior, discourages poor
practices, and _is faster_ because it allows compilers to make certain
assumptions about your code.

## Spacing

Spacing must be consistent across every file in the application. To this end,
using something like [`.editorconfig`][5] configuration files is highly
encouraged. Here are the defaults we suggest to get started with JavaScript
indentation.

```ini
# editorconfig.org
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

We always use 2 spaces for indentation in JavaScript files. The `.editorconfig`
file can take care of that for us and everyone would be able to create the
correct spacing by pressing the tab key.

Spacing doesn't just entail tabbing, but also the spaces before, after, and in
between arguments of a function declaration. Try to keep this spacing
consistent throughout the codebase.

Where possible, improve readability by keeping lines below the 80-character
mark.

## Naming

- Functions, variables, methods, objects and instances should be named using
  `camelCase`.
- Constructors and prototypes should use `PascalCase`.
- Symbolic constants should use `UPPERCASE`.
- Encapsulated (private) variables and methods should use
  `_underscoreCamelCase`.
- ~~Prefix variables or properties that reference a jQuery object with `$` or
  `_$`
  (depending on exposure). For example: `var $button = $( '.btn-class' );`.~~
  _We are currently phasing jQuery out of our codebases. If you are working on
  new code please use the common es5 and es6 features with polyfills to support
  older browsers if necessary._

## Semicolons`;`

We advocate for using semicolons at the end of each line. This choice is done
to avoid potential issues with Automatic Semicolon Insertion _(ASI)_.

## Strings

- Use single quotes (`'`) for inline strings without variables or conditions.
- Use [es6 template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
  for inline strings with variables or conditions as well as multiline templates.
- Use double quotes (`"`) for attribute values within templates.

##### Bad

```js
const name = "Jane";
const message = 'oh hai ' + name + "!";
const input = "<input value='" + message + "'" +
                     "type='text'" +
                     "class='a-text-input'>";
```

##### Good

```js
const name = 'Jane';
const message = `oh hai ${name}!`;
const input = `
  <input value="${ message }"
         type="text"
         class="a-text-input">`;
```

Usually you'll be a happier JavaScript developer if you hack together a
parameter-replacing method like [`util.format` in Node][12]. That way it'll be
far easier to format your strings, and the code looks a lot cleaner too.

## Variable Declaration

Always declare variables in **a consistent manner**, and at the top of their
scope, keeping variable declarations to single statements, including
declarations that aren't assigned a value.

##### Bad

```js
const foo = 1,
      bar = 2;

let baz, pony;

let a
  , b;
```

```js
const foo = 1;

if ( foo > 1 ) {
  const bar = 2;
}
```
##### Good

(Because they're consistent with each other, not because of the style.)

```js
const foo = 1;
const bar = 2;

let baz;
let pony;

let a;
let b;
```

```js
const foo = 1;
let bar;

if ( foo > 1 ) {
  bar = 2;
}
```

## Conditionals

**Brackets are enforced**. This, together with a reasonable spacing strategy
will help you avoid mistakes such as [Apple's SSL/TLS bug][14].

##### Bad

```js
if ( err ) throw err;
```

##### Good

```js
if ( err ) { throw err; }
```

It's even better if you avoid keeping conditionals on a single line, for the
sake of text comprehension.

##### Better

```js
if ( err ) {
  throw err;
}
```

## Equality

Avoid using `==` and `!=` operators, always favor `===` and `!==`. These
operators are called the "strict equality operators," while [their
counterparts will attempt to cast the operands][15] into the same value type.

##### Bad

```js
function isEmptyString( text ) {
  return text == '';
}

isEmptyString( 0 );
// <- true
```

##### Good

```js
function isEmptyString( text ) {
  return text === '';
}

isEmptyString( 0 );
// <- false
```

## Ternary Operators

Ternary operators are fine for clear-cut conditionals, but unacceptable for
confusing choices. As a rule, if you can't eye-parse it as fast as your brain
can interpret the text that declares the ternary operator, chances are it's
probably too complicated for its own good.

jQuery is a prime example of a codebase that's
[**filled with nasty ternary operators**][16].

##### Bad

```js
function calculate( a, b ) {
  return a && b ? 11 : a ? 10 : b ? 1 : 0;
}
```

##### Good

```js
function getName( mobile ) {
  return mobile ? mobile.name : 'Generic Player';
}
```

In cases that may prove confusing just use `if` and `else` statements instead.

## Functions

When declaring a function, always use the [function declaration form][17]
instead of [function expressions][18]. Because [hoisting][19].

##### Bad

```js
const sum = ( x, y ) => {
  return x + y;
};
// Note for clarity, the issue is not arrow functions, but the expression.
```

##### Good

```js
function sum( x, y ) {
  return x + y;
}
```

That being said, there's nothing wrong with function expressions that are just
[currying another function][20].

##### Good

```js
const plusThree = sum.bind( null, 3 );
```

Keep in mind that [function declarations will be hoisted][21] to the top of the
scope so it doesn't matter the order they are declared in. That being said, you
should always keep functions at the top level in a scope, and avoid placing
them inside conditional statements.

##### Bad

```js
if ( Math.random() > 0.5 ) {
  sum( 1, 3 );

  function sum( x, y ) {
    return x + y;
  }
}

```

##### Good

```js
if ( Math.random() > 0.5 ) {
  sum( 1, 3 );
}

function sum( x, y ) {
  return x + y;
}
```

```js
function sum( x, y ) {
  return x + y;
}

if ( Math.random() > 0.5 ) {
  sum( 1, 3 );
}
```

If you need a _"no-op"_ method you can use either `Function.prototype`, or
`function noop () {}`. Ideally a single reference to `noop` is used throughout
the application.

Whenever you have to manipulate an array-like object, cast it to an array.

##### Bad

```js
const divs = document.querySelectorAll( 'div' );

for ( let i = 0; i < divs.length; i++ ) {
  console.log( divs[i].innerHTML );
}
```

##### Good

```js
const divs = document.querySelectorAll( 'div' );

[].slice.call( divs ).forEach( div => {
  console.log( div.innerHTML );
} );
```

However, be aware that there is a [substantial performance hit][22] in V8
environments when using this approach on `arguments`. If performance is a
major concern, avoid casting `arguments` with `slice` and instead use a `for`
loop.

#### Bad
```js
const args = [].slice.call( arguments );
```

#### Good
```js
let i;
const args = new Array( arguments.length );
for ( i = 0; i < args.length; i++ ) {
    args[i] = arguments[i];
}
```

Don't declare functions inside of loops.

##### Bad

```js
const values = [1, 2, 3];
let i;

for ( i = 0; i < values.length; i++ ) {
  setTimeout( () => {
    console.log( values[i] );
  }, 1000 * i );
}
```

```js
const values = [1, 2, 3];
let i;

for ( i = 0; i < values.length; i++ ) {
  setTimeout( i => {
    return () => {
      console.log( values[i] );
    };
  }( i ), 1000 * i );
}
```

##### Good

```js
const values = [1, 2, 3];
let i;

for ( i = 0; i < values.length; i++ ) {
  setTimeout(  i => {
    console.log( values[i] );
  }, 1000 * i, i );
}
```

```js
const values = [1, 2, 3];
let i;

for ( i = 0; i < values.length; i++ ) {
  wait( i );
}

function wait( i ) {
  setTimeout( () => {
    console.log( values[i] );
  }, 1000 * i );
}
```

Or even better, just use `.forEach` which doesn't have the same caveats as
declaring functions in `for` loops.

##### Better

```js
[1, 2, 3].forEach( function( value, i ) {
  setTimeout( () => {
    console.log( value );
  }, 1000 * i );
} );
```

Whenever a method is non-trivial, make the effort to **use a named function
declaration rather than an anonymous function**. This will make it easier to
pinpoint the root cause of an exception when analyzing stack traces.

##### Bad

```js
function once( fn ) {
  let ran = false;
  return () => {
    if ( ran ) { return };
    ran = true;
    fn.apply( this, arguments );
  };
}
```

##### Good

```js
function once( fn ) {
  let ran = false;
  return function run() {
    if ( ran ) { return };
    ran = true;
    fn.apply( this, arguments );
  };
}
```

Avoid keeping indentation levels from raising more than necessary by using
guard clauses instead of flowing `if` statements.

##### Bad

```js
if ( car ) {
  if ( black ) {
    if ( turbine ) {
      return 'batman!';
    }
  }
}
```

```js
if ( condition ) {
  // 10+ lines of code
}
```

##### Good

```js
if ( !car ) {
  return;
}
if ( !black ) {
  return;
}
if ( !turbine ) {
  return;
}
return 'batman!';
```

```js
if ( !condition ) {
  return;
}
// 10+ lines of code
```

## Prototypes

Hacking native prototypes should be avoided at all costs, use a method instead.
If you must extend the functionality in a native type, try using something like
[poser][23] instead.

##### Bad

```js
String.prototype.half = () => {
  return this.substr( 0, this.length / 2 );
};
```

##### Good

```js
function half( text ) {
  return text.substr( 0, text.length / 2 );
}
```

**Avoid prototypical inheritance models** unless you have a very good
_performance reason_ to justify yourself.

- Prototypical inheritance boosts puts need for `this` through the roof
- It's way more verbose than using object literals
- It causes headaches when creating `new` objects
- Needs a closure to hide valuable private state of instances
- Just use object literals instead

## Object Literals

Instantiate using the egyptian notation `{}`. Use factories instead of
constructors.

## Array Literals

Instantiate using the square bracketed notation `[]`. If you have to declare a
fixed-dimension array for performance reasons then it's fine to use the
`new Array( length )` notation instead.

## Regular Expressions

Keep regular expressions in variables, don't use them inline. This will vastly
improve readability.

##### Bad

```js
if ( /\d+/.test( text ) ) {
  console.log( 'so many numbers!' );
}
```

##### Good

```js
const numeric = /\d+/;
if ( numeric.test( text ) ) {
  console.log( 'so many numbers!' );
}
```

Also [learn how to write regular expressions][25], and what they actually do.
Then you can also [visualize them online][26].

## `console` statements

Preferably bake `console` statements into a service that can easily be
disabled in production. Alternatively, don't ship any `console.log` printing
statements to production distributions.

## Comments

### JSDocs
[Use JSDocs](http://usejsdoc.org/) where applicable for JavaScript commenting.

### Purpose
Comments **aren't meant to explain what** the code does. Good **code is
supposed to be self-explanatory**. If you're thinking of writing a comment to
explain what a piece of code does, chances are you need to change the code
itself. The exception to that rule is explaining what a regular expression
does. Good comments are supposed to **explain why** code does something that
may not seem to have a clear-cut purpose.

##### Bad

```js
// Create the centered container.
const p = $( '<p/>' );
p.center( div );
p.text( 'foo' );
```

##### Good

```js
const container = $( '<p/>' );
const contents = 'foo';
container.center( parent );
container.text( contents );
megaphone.on( 'data', value => {
  // The megaphone periodically emits updates for container.
  container.text( value );
} );
```

```js
// One or more digits somewhere in the string.
const numeric = /\d+/;
if ( numeric.test( text ) ) {
  console.log( 'so many numbers!' );
}
```

Commenting out entire blocks of code _should be avoided entirely_, that's why
you have version control systems in place!

## Variable Naming

Variables must have meaningful names so that you don't have to resort to
commenting what a piece of functionality does. Instead, try to be expressive
while succinct, and use meaningful variable names.

##### Bad

```js
function a( x, y, z ) {
  return z * y / x;
}
a( 4, 2, 6 );
// <- 3
```

##### Good

```js
function ruleOfThree( had, got, have ) {
  return have * got / had;
}
ruleOfThree( 4, 2, 6 );
// <- 3
```

## Polyfills

Where possible use the native browser implementation and include
[a polyfill that provides that behavior][27] for unsupported browsers. This
makes the code easier to work with and less involved in hackery to make things
just work.

Although we continue to support IE8 and 9, we do not support scripted
interactions and rely instead on progressive enhancement to provide IE8 and 9
users with a functioning experience.

If you can't patch a piece of functionality with a polyfill, then
[wrap all uses of the patching code][28] in a globally available method that
is accessible from everywhere in the application.

## Everyday Tricks

Use `||` to define a default value. If the left-hand value is [falsy][29] then
the right-hand value will be used. Be advised, that because of loose type
comparison, inputs like `false`, `0`, `null` or `''` will be evaluated as
falsy, and converted to default value. For strict type checking use
`if (value === void 0) { value = defaultValue }`.

```js
function a( value ) {
  const defaultValue = 33;
  const used = value || defaultValue;
}
```

Use `.bind` to [partially-apply][30] functions.

```js
function sum( a, b ) {
  return a + b;
}

const addSeven = sum.bind( null, 7 );

addSeven( 6 );
// <- 13
```

Use `Array.prototype.slice.call` to cast array-like objects to true arrays.

```js
const args = Array.prototype.slice.call( arguments );
```

Use [event emitters][31] on all the things!

```js
const emitter = contra.emitter();

body.addEventListener( 'click', () => {
  emitter.emit('click', e.target );
} );

emitter.on( 'click', elem => {
  console.log( elem );
} );

// simulate click
emitter.emit( 'click', document.body );
```

Use `Function()` as a _"no-op"_.

```js
cb => {
  setTimeout( cb || Function(), 2000 );
}
```

## Credits

The original version of this document is based on [Nicolas Bevacqua's functon
qualityGuide](https://github.com/bevacqua/js), which is licensed under the MIT
license.


  [1]: http://wiki.commonjs.org/wiki/CommonJS
  [2]: http://requirejs.org/docs/whyamd.html
  [3]: http://eviltrout.com/2014/05/03/getting-started-with-es6.html
  [4]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode
  [5]: http://editorconfig.org
  [6]: http://dailyjs.com/2012/12/24/javascript-survey-results/
  [7]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
  [8]: https://github.com/mdevils/node-jscs
  [9]: http://www.jslint.com/
  [10]: https://github.com/jshint/jshint/
  [11]: https://github.com/eslint/eslint
  [12]: http://nodejs.org/api/util.html#util_util_format_format
  [13]: https://github.com/visionmedia/jade
  [14]: https://www.imperialviolet.org/2014/02/22/applebug.html
  [15]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators
  [16]: https://github.com/jquery/jquery/blob/c869a1ef8a031342e817a2c063179a787ff57239/src/ajax.js#L117
  [17]: http://stackoverflow.com/q/336859/389745
  [18]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function
  [19]: https://github.com/buildfirst/buildfirst/tree/master/ch05/04_hoisting
  [20]: http://ejohn.org/blog/partial-functions-in-javascript/
  [21]: https://github.com/buildfirst/buildfirst/tree/master/ch05/04_hoisting
  [22]: https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments
  [23]: https://github.com/bevacqua/poser
  [24]: http://blog.ponyfoo.com/2013/11/19/fun-with-native-arrays
  [25]: http://blog.ponyfoo.com/2013/05/27/learn-regular-expressions
  [26]: http://www.regexper.com/#%2F%5Cd%2B%2F
  [27]: http://remysharp.com/2010/10/08/what-is-a-polyfill/
  [28]: http://blog.ponyfoo.com/2014/08/05/building-high-quality-front-end-modules
  [29]: http://james.padolsey.com/javascript/truthy-falsey/
  [30]: http://ejohn.org/blog/partial-functions-in-javascript/
  [31]: https://github.com/bevacqua/contra#%CE%BBemitterthing-options
  [32]: https://github.com/bevacqua/css
  [33]: https://github.com/bevacqua/js/issues
