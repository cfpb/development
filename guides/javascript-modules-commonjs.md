# Creating CommonJS Modules

## Table of Contents

1. [Modules Types](#module-types)
1. [Module Naming](#module-naming)
1. [Functional Module Structure](#functional-module-structure)
1. [Class Module Structure](#class-module-structure)

## Module Types

**Functional Modules** -
These are modules that will be imported into other parts of the codebase
and expose methods that will be used directly. These kind of modules
might include utility methods or initialize page-specific functionality.
For example:

```js
var simpleMath = require( './simple-math' );

…

simpleMath.addNumbers( 2 , 3 );
```

**Class Modules** -
These are modules that will have instances instantiated through the `new` keyword. For example:

```js
var AClass = require( './AClass' );

…

var aClass = new AClass();
aClass.publicMethod();
```


## Module Naming

When importing modules using a `require( … )` call, follow these guidelines
for the variable name used to reference the module:

### Third-party libraries
For well-known libraries, use a name that aligns with how that plugin will be
expected to be accessed:

```js
var $ = require( 'jquery' );
var _ = require( 'underscore' );
```

### Single-word functional modules
Match the variable name with the filename of the module.

```js
var mod = require( './mod' );
var widget = require( './widget' );
var util = require( './util/util' );
```

### Multi-word functional modules
Use camelCase for the variable name and dash-case for the filename.

```js
var aModule = require( './a-module' );
var acmeWidget = require( './acme-widget' );
var colorUtil = require( './util/color-util' );
```

### Class modules
For class modules, PascalCase the module and filename.

```js
var AClass = require( './AClass' );
var AcmeWidget = require( './AcmeWidget' );
var ColorUtil = require( './util/ColorUtil' );
```

## Functional Module Structure

Modules that will expose methods that will be used directly.

The following example shows two public methods being exposed
and one private method:

```js
// simple-math.js
/* ==========================================================================
  This is an example functional module for adding/subtracting two numbers.
  ========================================================================== */

'use strict';

/**
* @param {number} num1 A number to add…
* @param {number} num2 …to another number.
* @returns {number} Total of two numbers combined together.
*/
function addNumbers( num1, num2 ) {
  _isValidNumber( num1 );
  _isValidNumber( num2 );
  return num1 + num2;
}

/**
* @param {number} num1 A number to subtract…
* @param {number} num2 …from another number.
* @returns {number} Total of two numbers subtracted from one another.
*/
function subtractNumbers( num1, num2 ) {
  _isValidNumber( num1 );
  _isValidNumber( num2 );
  return num1 - num2;
}

/**
* @param {number} value A value to check for number literal-ness.
* @throws {Error} Throws error if provided value isn't a number.
*/
function _isValidNumber( num ) {
  if ( typeof( num ) !== 'number' ) {
    throw new Error( 'Please provide a number!' );
  }
}

// Expose the public methods.
module.exports = {
  addNumbers: addNumbers,
  subtractNumbers: subtractNumbers
};
```

Some functional modules will access the DOM and need initialization.

This example includes a private property:

```js
// button-clicker.js
/* ==========================================================================
  This is an example functional module for adding events to a button.
  ========================================================================== */

'use strict';

// Internal property reference for the DOM element.
var _buttonDom;

/**
* Set internal properties and assign event listeners.
*/
function init() {
  _buttonDom = document.querySelector( '.a-btn' );

  // Add Events.
  _buttonDom.addEventListener( 'click', _btnClickHandler , false)
}

/**
* Pop up an alert when the button is clicked.
* @param {Object} event The click event object.
*/
function _btnClickHandler( event ) {
  alert( 'A button was clicked!' );
}

// Expose the public methods.
module.exports = {
  init: init
};
```

If applicable, initialization could happen at the time the module is required:

```js
var buttonClicker = require( './button-clicker' ).init();
```

## Class Module Structure

Modules that will have instances instantiated through the `new` keyword.

This class module example includes one public method:


```js
// AClass.js
/* ==========================================================================
  Optional code comment talking about what this module is.
  ========================================================================== */

'use strict';

/**
* AClass
* @class
*/
function AClass() {
  // Initialization can happen here, like an `init` method.
}

/**
* @returns {Object} A reference to the instance.
*/
function publicMethod() {
  // Returning the instance allows for method chaining.
  return this;
}

// Assigning the methods on the class prototype by reference allows named
// methods to be used, which will show up in ESLint offenses, for instance.
AClass.prototype.publicMethod = publicMethod;

// Export only the class constructor for the file.
module.exports = AClass;
```
