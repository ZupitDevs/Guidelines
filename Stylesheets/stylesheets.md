Source: [Stylesheets Claws](https://guides.pixelgrade.com/development-guides/stylesheets-claws/). \
The following is intented for code written using the SCSS preprocessor.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of contents**

- [Syntax and Formatting](#syntax-and-formatting)
  - [SINGLE DECLARATIONS](#single-declarations)
  - [DECLARATION ORDER](#declaration-order)
  - [SHORTHAND NOTATION](#shorthand-notation)
- [Commenting](#commenting)
- [Dealing with specificity](#dealing-with-specificity)
  - [SELECTING IDS](#selecting-ids)
  - [MAYBE IT'S NOT THAT !IMPORTANT](#maybe-its-not-that-important)
  - [SAFELY INCREASING SPECIFICITY](#safely-increasing-specificity)
  - [MIXINS AND @EXTEND](#mixins-and-extend)
- [Architecture](#architecture)
  - [FOLDER STRUCTURE](#folder-structure)
  - [NAMING CONVENTIONS](#naming-conventions)
    - [Objects: o-](#objects-o-)
    - [Components: c-](#components-c-)
    - [Utility: u-](#utility-u-)
    - [State: is-, has-](#state-is--has-)
    - [JavaSript Hooks: js-](#javasript-hooks-js-)
    - [Naming conventions](#naming-conventions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
___





# Syntax and Formatting
* Use soft-tabs with a four space indent. Spaces are the only way to guarantee code renders the same in any person’s environment.
    ```SCSS
    .element {
        display: block;
    }
    ```

* Include one space before the opening brace of declaration blocks for legibility.
    ```SCSS
    .selector { }
    ```

* Include one space after `:` in property declarations.
    ```SCSS
    .selector { display: block; }
    ```

* Place closing braces of declaration blocks on a new line.
    ```SCSS
    .selector {
        display: block;
        height: auto;
    }
    ```

* Put line breaks between rulesets.
    ```SCSS
    .selector-1 {
        display: block;
        height: auto;
    }

    .selector-2 {
        display: inherit;
    }
    ```

* When grouping selectors, keep individual selectors to a single line.
    ```SCSS
    .selector,
    .another-selector,
    .and-another {
        display: block;
        height: auto;

        color: #FFFFFF;
        background-color: #000000;
    }
    ```

* Each declaration should appear on its own line for more accurate error reporting.
End all declarations with a semicolon. The last declaration's semicolon is optional, but your code is more error prone without it.
    ```SCSS
    .selector {
        display: block;
        height: auto;

        color: #FFFFFF;
        background-color: #000000;
    }
    ```

* Comma-separated property values should include a space after each comma (e.g., box-shadow).
    ```SCSS
    .selector {
        box-shadow: inset 5px 5px 10px #000000,
                    inset -5px -5px 10px #454545,
                    inset -10px -5px 12px #334332;
    }
    ```

* Don't include spaces after commas within rgb(), rgba(), hsl(), hsla(), or rect() values. This helps differentiate multiple color values (comma, no space) from multiple property values (comma with space).
    ```SCSS
    .selector {
        background-color: rgb(0, 0, 0);
        background-color: rgb(0,0,0);
    }
    ```

* Don't prefix property values or color parameters with a leading zero (e.g., `.5` instead of `0.5`).
    ```SCSS
    .selector {
        opacity: 0.5;
        opacity: .5; // prefferred

        margin-top: -0.5em; // prefferred when using negative values
    }
    ```

* Lowercase all HEX values within the same declaration. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
    ```SCSS
    .selector {
        box-shadow: 10px #FAFAFA, -5px #ccc, 12px #DDD;
        box-shadow: 10px #fafafa, -5px #ccc, 12px #ddd; // preffered
    }
    ```

* Quote attribute values in selectors. They’re only optional in some cases, and it’s a good practice for consistency.
    ```SCSS
    input[type=text] { ... }
    input[type="text"] { ... }
    ```

* Avoid specifying units for zero values.
    ```SCSS
    .selector { margin: 0px; }
    .selector { margin: 0; }
    ```

* Use HEX color codes unless using rgba() in raw CSS (SCSS’ rgba() function is overloaded to accept hex colors as a parameter).
    ```SCSS
    // SCSS
    .selector {
        // both variants supported
        background-color: rgba(0,0,0, .5);
        background-color: rgba(#000000, .5);
    }
    ```
    ```CSS
    /* CSS */
    .selector {
        background-color: rgba(#000000, .5); /* unsupported -> incorrect */
        background-color: rgba(0,0,0, .5); /* supported */
    }
    ```



## SINGLE DECLARATIONS
In instances where a rule set includes only one declaration, consider removing line breaks for readability and faster editing. Any rule set with multiple declarations should be split to separate lines.
```SCSS
.selector { display: block; }
```
The key factor here is error detection—e.g., a CSS validator stating you have a `syntax error on Line 183`. With a single declaration, there's no missing it. With multiple declarations, separate lines is a must for your sanity.



## DECLARATION ORDER
We group by type within that scope we toggle between arbitrary order and order by line length. Types are ordered by their importance, from loose to more specific properties.

* Extends
* Positioning – position, top, right, bottom, left, z-index
* Display, flex properties, and box model
* Border and other of its properties(style, width, color)
* Background and color
* Typographic
* Other

```SCSS
.element {
    @extend %font-size-16;
    @extend %clearfix;

    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 1;

    display: flex;
    flex: 1 1 auto;
    height: auto;
    @import width-full(); /* width: 100%; */

    border: 1px solid #fff;
    border-radius: 5px;

    color: #cecece;
    background-color: #000;

    font-family: sans-serif;
    font-size: 100%;
    font-style: regular;
    text-align: center;
    text-transform: uppercase;

    opacity: 1;
    transform: none;
    pointer-events: auto;
}
```



## SHORTHAND NOTATION
Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:

* padding
* margin
* font
* background
* border
* border-radius

```SCSS
.selector {
    /* margin: 0 auto; */
    margin-left: auto;
    margin-right: auto;
}
```

Often times we don't need to set all the values a shorthand property represents. For example, HTML headings only set top and bottom `margin`, so when necessary, only override those two values. Excessive use of shorthand properties often leads to sloppier code with unnecessary overrides and unintended side effects.





# Commenting
Double slash `//` comments should be used for comment blocks in SCSS (instead of `/* */`).
```SCSS
/* Comment in SCSS - valid but not encouraged */
// Comment in SCSS - valid and encouraged
```
```CSS
/* Comment in CSS */
````

# Dealing with specificity
* Never use IDs in CSS, ever. They have no advantage over classes (anything you can do with an ID, you can do with a class), they cannot be reused, and their specificity is way, way too high. Even an infinite number of chained classes will not trump the specificity of one ID.
* Do not nest selectors unnecessarily. If `.header-nav {}` will work, never use `.header .header-nav{};` to do so will literally double the specificity of the selector without any benefit.
* Do not qualify selectors unless you have a compelling reason to do so. If `.nav {}` will work, do not use `ul.nav {};` to do so would not only limit the places you can use the `.nav` class, but it also increases the specificity of the selector, again, with no real gain.
* Make heavy use of classes because they are the ideal selector: low specificity (or rather, all classes have the same specificity, so you have a level playing field), great portability, and high reusability.



## SELECTING IDS
```HTML
<div id="nasty">
	...
</div>
```
Naturally, given that we can’t edit this HTML to use a class instead of (or alongside) the ID, we’d opt for something like this:
```SCSS
#nasty {
    ...
}
```
Now we have an ID in our CSS, and that is not a good precedent to set. Instead, we should do something like this:
```SCSS
[id="nasty"] {
    ...
}
```
This is an attribute selector. This is not selecting on an ID per se, but on an element with an attribute of id which also has a certain value.

The beauty of this selector is that it has the exact same specificity as a class, so we’re selecting a chunk of the DOM based on an ID, but never actually increasing our specificity beyond that of our classes that we’re making liberal use of elsewhere.

Just because we know a way of using IDs without introducing their heightened specificity, it does not mean we should go back to using IDs in our CSS; they still have the problem of not being reusable. Only use this technique when you have no other option, and you cannot replace an ID in some markup with a class.



## MAYBE IT'S NOT THAT !IMPORTANT
`!important` should be avoided at all costs. There are certain cases where `!important` can or should be used. For example utility classes and / or when you want to overwrite some inline CSS which for some reason cannot be removed. In this case there should be a CSS comment above the line where `!important` is used to explain the reason it was used.

## SAFELY INCREASING SPECIFICITY
You can chain a selector with itself to increase its specificity. That is to say `.btn.btn {}` will apply to the same elements as `.btn {}` but with double the specificity. We can take this as far as we need to: `.btn.btn.btn.btn {}` but hopefully we’ll never get that far.

## MIXINS AND @EXTEND
Mixins are powerful tools which can be used to leverage your work and also handy shortcuts. The most valuable use of mixins is generating CSS. It is very helpful for having DRY code and changes in different modules come with less effort. We should at most times try to keep the declaration and usage of mixins inside the same files so we don’t create any unneeded dependencies.\
The same applies for `@extend` which must not be used inside components! Components should be independent chunks of code with high portability. Also using `@extend` proves to be less performant than using mixins (probably even repeating code) when the source is gzipped. (see [http://csswizardry.com/2016/02/mixins-better-for-performance/](http://csswizardry.com/2016/02/mixins-better-for-performance/))





# Architecture
The Inverted Triangle CSS Methodology is a scalable and maintainable CSS architecture introduced by Harry Roberts which gives you a set of rules to help you deal with CSS specifics like global namespace, cascade and selector specificity.

One of the key principles of ITCSS is that it separates your CSS codebase to several sections (called layers), which take form of the inverted triangle:

* **Settings** — global settings and variables declarations;
* **Tools** — globally used mixins and functions;
* **Generic** — reset and / or normalize styles;
* **Elements** — styling for bare HTML elements;
* **Objects** — class-based selectors which define undecorated design patterns;
* **Components** — specific UI components;
* **Trumps** — utilities and helper classes with ability to override anything;
* **Shame** — the place for prototyping, quick fixes and overrides;

## FOLDER STRUCTURE
```
|-- base
|-- components
|-- generic
|-- tools
    |-- _mixins.scss
|-- objects
|-- trumps
|-- vendor
|-- _main.scss
|-- _settings.scss
|-- _shame.scss
|-- style.scss
```

Most times we will include in our project third-party plugins with their own CSS. These files should reside in the vendor folder and the place in the Inverted Triangle would theoretically be the Components layer.

## NAMING CONVENTIONS
In no particular order, here are the individual namespaces and a brief description. We’ll look at each in more detail in a moment, but the following list should acquaint you with the kinds of thing we’re hoping to achieve.

### Objects: o-
Signify that the element which it is applied to is an Object. It can be used for layout purposes or abstracting visual patterns. These elements’ properties shouldn’t be modified by other elements or based on context. Modifying the object itself should be avoided.

### Components: c-
Components are well-defined pieces of UI and their properties are mostly for decoration purposes. The scope to which changes to these elements’ styling would apply is easily to detect and explain.

### Utility: u-
Utilities classes are in no way attached to any piece of ui or code. Each class has a specific purpose and they come from methodologies like Tachyons / Basscss / Atomic CSS. They are more suited when developing for an environment where you have full control over the markup but that’s not the case for WordPress.

### State: is-, has-
State classes define a scope which each elements are styled in a certain way cause of the current state of the application. They are usually temporary or optional

### JavaSript Hooks: js-
These classes don’t have any stylistic purposes and are used to bind JavaScript to them for a certain behavior.

### Naming conventions
The absence of scope in CSS is one of the main reasons code gets bad. Nesting is not an option since it brings unwanted specificity to your selectors. This leaves us with the only option of using a naming convention for the class names used. \
[**BEM**](http://getbem.com/naming/) \
[**BEMIT**](http://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further/)

---

### References (from the source mentioned at the begining of the document)
* [http://cssguidelin.es/](http://cssguidelin.es/)
* [http://codeguide.co/](http://codeguide.co/)
* [https://github.com/airbnb/css](https://github.com/airbnb/css)
* [http://primercss.io/guidelines/](http://primercss.io/guidelines/)
* [http://csswizardry.com/2012/11/code-smells-in-css/](http://csswizardry.com/2012/11/code-smells-in-css/)
* [http://csswizardry.com/2013/05/scope-in-css/](http://csswizardry.com/2013/05/scope-in-css/)
* [http://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further/](http://csswizardry.com/2013/05/scope-in-css/)