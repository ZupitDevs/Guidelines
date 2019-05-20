Source: [JavaScript Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/javascript/)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of contents**

- [Spacing](#spacing)
  - [Objects](#objects)
  - [Arrays and Function Calls](#arrays-and-function-calls)
  - [Examples of good spacing:](#examples-of-good-spacing)
- [Indentation and Line Breaks](#indentation-and-line-breaks)
  - [Blocks and Curly Braces](#blocks-and-curly-braces)
  - [Multi-line Statements](#multi-line-statements)
- [Assignments and Globals](#assignments-and-globals)
  - [Declaring Variables with const and let](#declaring-variables-with-const-and-let)
  - [Naming Conventions](#naming-conventions)
  - [Abbreviations and Acronyms](#abbreviations-and-acronyms)
  - [Class Definitions](#class-definitions)
  - [Constants](#constants)
- [Comments](#comments)
- [Equality](#equality)
- [Type Checks](#type-checks)
- [Strings](#strings)
- [Switch Statements](#switch-statements)
- [Best practices](#best-practices)
  - [Arrays](#arrays)
  - [Objects](#objects-1)
  - [Iteration](#iteration)
  - [Underscore.js Collection Functions](#underscorejs-collection-functions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
___




# Spacing
Use spaces liberally throughout your code. “When in doubt, space it out.”

These rules encourage liberal spacing for improved developer readability. The minification process creates a file that is optimized for browsers to read and process.

* indentation with tabs (4-spaces tab size);
* no whitespace at the end of line or on blank lines;
* `if`/`else`/`for`/`while`/`try` blocks should always use braces, and always go on multiple lines;
* unary special-character operators (e.g., `++`, `--`) must not have space next to their operand;
* any `,` and `;` must not have preceding space;
* any `;` used as a statement terminator must be at the end of the line;
* any `:` after a property name in an object definition must not have preceding space;
* the `?` and `:` in a ternary conditional must have space on both sides;
* no filler spaces in empty constructs (e.g., `{}`, `[]`, `fn()`);
* there should be a new line at the end of each file;
* any ! negation operator should have a following space;
* all function bodies are indented by one tab, even if the entire file is wrapped in a closure;
* spaces may align code within documentation blocks or within a line, but only tabs should be used at the start of a line.



## Objects
Object declarations can be made on a single line if they are short. When an object declaration is too long to fit on one line, there must be one property per line. Property names only need to be quoted if they are reserved words or contain special characters:
```javascript
// Preferred
var obj = {
    ready: 9,
    when: 4,
    'you are': 15,
};
var arr = [
    9,
    4,
    15,
];

// Acceptable for small objects and arrays
var obj = { ready: 9, when: 4, 'you are': 15 };
var arr = [ 9, 4, 15 ];

// Bad
var obj = { ready: 9,
    when: 4, 'you are': 15 };
var arr = [ 9,
    4, 15 ];
```



## Arrays and Function Calls
Always include extra spaces around elements and arguments:
```javascript
array = [ a, b ];

foo( arg );

foo( 'string', object );

foo( options, object[ property ] );

foo( node, 'property', 2 );

prop = object[ 'default' ];

firstArrayElement = arr[ 0 ];
```



## Examples of good spacing:
```javascript
if ( condition ) {
    doSomething( 'with a string' );
} else if ( otherCondition ) {
    otherThing( {
        key: value,
        otherKey: otherValue
    } );
} else {
    somethingElse( true );
}

while ( ! condition ) {
    iterating++;
}

for ( let i = 0; i < 100; i++ ) {
    object[ array[ i ] ] = someFn( i );
    $( '.container' ).val( array[ i ] );
}

try {
    // Expressions
} catch ( e ) {
    // Expressions
}
```



# Indentation and Line Breaks
Indentation and line breaks add readability to complex statements.

Tabs should be used for indentation. Even if the entire file is contained in a closure (i.e., an immediately invoked function), the contents of that function should be indented by one tab:
```javascript
( function() {
    // Expressions indented

    function doSomething() {
        // Expressions indented
    }
} )();
```



## Blocks and Curly Braces
`if`, `else`, `for`, `while`, and `try` blocks should always use braces, and always go on multiple lines. The opening brace should be on the same line as the function definition, the conditional, or the loop. The closing brace should be on the line directly following the last statement of the block.
```javascript
const a, b, c;

if ( myFunction() ) {
    // Expressions
} else if ( ( a && b ) || c ) {
    // Expressions
} else {
    // Expressions
}
```



## Multi-line Statements
When a statement is too long to fit on one line, line breaks must occur after an operator.
```javascript
// Bad
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c
    + ' is ' + ( a + b + c ) + '</p>';

// Good
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c +
    ' is ' + ( a + b + c ) + '</p>';
```
Lines should be broken into logical groups if it improves readability, such as splitting each expression of a ternary operator onto its own line, even if both will fit on a single line.
```javascript
// Acceptable
var baz = ( true === conditionalStatement() ) ? 'thing 1' : 'thing 2';

// Better
var baz = firstCondition( foo ) && secondCondition( bar ) ?
    qux( foo, bar ) :
    foo;
```

When a conditional is too long to fit on one line, each operand of a logical operator in the boolean expression must appear on its own line, indented one extra level from the opening and closing parentheses.
```javascript
if (
    firstCondition() &&
    secondCondition() &&
    thirdCondition()
) {
    doStuff();
}
```










# Assignments and Globals



## Declaring Variables with const and let
For code written using ES2015 or newer, `const` and `let` should always be used in place of `var`. **A declaration should use `const` unless its value will be reassigned, in which case `let` is appropriate.**

Unlike `var`, it is not necessary to declare all variables at the top of a function. Instead, they are to be declared at the point at which they are first used.



## Naming Conventions
Variable and function names should be full words, using camel case with a lowercase first letter.
Names should be descriptive, but not excessively so. Exceptions are allowed for iterators, such as the use of `i` to represent the index in a loop.



## Abbreviations and Acronyms
Acronyms must be written with each of its composing letters capitalized. This is intended to reflect that each letter of the acronym is a proper word in its expanded form.

All other abbreviations must be written as camel case, with an initial capitalized letter followed by lowercase letters.

If an abbreviation or an acronym occurs at the start of a variable name, it must be written to respect the camelcase naming rules covering the first letter of a variable or class definition. For variable assignment, this means writing the abbreviation entirely as lowercase. For class definitions, its initial letter should be capitalized.
```javascript
// "Id" is an abbreviation of "Identifier":
const userId = 1;

// "DOM" is an acronym of "Document Object Model":
const currentDOMDocument = window.document;

// Acronyms and abbreviations at the start of a variable name are consistent
// with camelcase rules covering the first letter of a variable or class.
const domDocument = window.document;
class DOMDocument {}
class IdCollection {}
```



## Class Definitions
Constructors intended for use with new should have a capital first letter (UpperCamelCase).
A class definition must use the UpperCamelCase convention, regardless of whether it is intended to be used with new construction.
```javascript
class Earth {
    static addHuman( human ) {
        Earth.humans.push( human );
    }

    static getHumans() {
        return Earth.humans;
    }
}

Earth.humans = [];
```



## Constants
An exception to camel case is made for constant values which are never intended to be reassigned or mutated. Such variables must use the SCREAMING_SNAKE_CASE convention.

In almost all cases, a constant should be defined in the top-most scope of a file. It is important to note that [JavaScript's `const` assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) is conceptually more limited than what is implied here, where a value assigned by const in JavaScript can in-fact be mutated and is only protected against reassignment. A constant as defined in these coding guidelines applies only to values which are expected to never change and is a strategy for developers to communicate intent more so than it is a technical restriction.





# Comments
Comments come before the code to which they refer, and should always be preceded by a blank line. Capitalize the first letter of the comment, and include a period in the end when writing full sentences. There must be a single space between the comment token (//) and the comment text.
```javascript
someStatement();

// Explanation of something complex on the next line
$( 'p' ).doSomething();

// This is a comment that is long enough to warrant being stretched
// over the span of multiple lines.
```

Inline comments are allowed as an exception when used to annotate special arguments in formal parameter lists:
```javascript
function foo( types, selector, data, fn, /* INTERNAL */ one ) {
    // Do stuff
}
```





# Equality
Strict equality checks (`===`) must be used in favor of abstract equality checks (`==`).





# Type Checks
These are the preferred ways of checking the type of an object:
* `String`: `typeof object === 'string'`
* `Number`: `typeof object === 'number'`
* `Boolean`: `typeof object === 'boolean'`
* `Object`: `typeof object === 'object' or _.isObject( object )`
* `Function`: `_.isFunction( object )` ([`_.isFunction`](https://underscorejs.org/#isFunction))
* `Array`: `_.isArray( object )` ([`_.isArray`](https://underscorejs.org/#isArray))
* `Element`: `object.nodeType or _.isElement( object )` ([`_.isElement`](https://underscorejs.org/#isElement))
* `null`: `object === null`
* `null` or `undefined`: `object == null`
* `undefined`:
    - Global Variables: `typeof variable === 'undefined'`
    - Local Variables: `variable === undefined`
    - Properties: `object.prop === undefined`
    - Any of the above: `_.isUndefined( object )` ([`_.isUndefined`](https://underscorejs.org/#isUndefined))





# Strings
Use single-quotes for string literals:
```javascript
var myStr = 'strings should be contained in single quotes';
```
When a string contains single quotes, they need to be escaped with a backslash (`\`):
```javascript
// Escape single quotes within strings:
'Note the backslash before the \'single quotes\'';
```





# Switch Statements
The usage of switch statements is generally discouraged but can be useful when there are a large number of cases – especially when multiple cases can be handled by the same block, or fall-through logic (the `default` case) can be leveraged.

When using switch statements:

* Use a break for each case other than `default`. When allowing statements to “fall through,” note that explicitly.
* Indent `case` statements one tab within the `switch`.
```javascript
switch ( event.keyCode ) {
    // ENTER and SPACE both trigger x()
    case $.ui.keyCode.ENTER:
    case $.ui.keyCode.SPACE:
        x();
        break;
    case $.ui.keyCode.ESCAPE:
        y();
        break;
    default:
        z();
}
```
It is not recommended to `return` a value from within a switch statement: use the `case` blocks to set values, then `return` those values at the end.
```javascript
function getKeyCode( keyCode ) {
    let result;

    switch ( event.keyCode ) {
        case $.ui.keyCode.ENTER:
        case $.ui.keyCode.SPACE:
            result = 'commit';
            break;
        case $.ui.keyCode.ESCAPE:
            result = 'exit';
            break;
        default:
            result = 'default';
    }

    return result;
}
```





# Best practices



## Arrays
Creating arrays in JavaScript should be done using the shorthand [] constructor rather than the new Array() notation.
```javascript
var myArray = [];
```
You can initialize an array during construction:
```javascript
var myArray = [ 1, 'WordPress', 2, 'Blog' ];
```
In JavaScript, associative arrays are defined as objects.



## Objects
There are many ways to create objects in JavaScript. Object literal notation, `{}`, is both the most performant, and also the easiest to read.
```javascript
var myObj = {};
```
Object literal notation should be used unless the object requires a specific prototype, in which case the object should be created by calling a constructor function with `new`.
```javascript
var myObj = new ConstructorMethod();
```
Object properties should be accessed via dot notation unless the key is a variable or a string that would not be a valid identifier:
```javascript
prop = object.propertyName;
prop = object[ variableKey ];
prop = object['key-with-hyphens'];
```





## Iteration
When iterating over a large collection using a `for` loop, it is recommended to store the loop’s max value as a variable rather than re-computing the maximum every time:
```javascript
// Good & Efficient
var i, max;

// getItemCount() gets called once
for ( i = 0, max = getItemCount(); i < max; i++ ) {
    // Do stuff
}

// Bad & Potentially Inefficient:
// getItemCount() gets called every time
for ( i = 0; i < getItemCount(); i++ ) {
    // Do stuff
}
```



## Underscore.js Collection Functions
Learn and understand [Underscore’s collection and array methods](https://underscorejs.org/#collections). These functions, including `_.each`, `_.map`, and `_.reduce`, allow for efficient, readable transformations of large data sets.

Underscore also permits jQuery-style chaining with regular JavaScript objects:
```javascript
var obj = {
    first: 'thing 1',
    second: 'thing 2',
    third: 'lox'
};

var arr = _.chain( obj )
    .keys()
    .map( function( key ) {
        return key + ' comes ' + obj[ key ];
    } )
    // Exit the chain
    .value();

// arr === [ 'first comes thing 1', 'second comes thing 2', 'third comes lox' ]
```
