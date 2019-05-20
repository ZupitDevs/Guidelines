Source: [Vue.js Style Guide](https://vuejs.org/v2/style-guide/)

Here are some (hopefully) generally applicable rules for writing components in JavaScript frameworks (but at the time of writing, focused on the Vue.js framework).

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of contents**

- [Multi-word component names](#multi-word-component-names)
- [Component style scoping](#component-style-scoping)
- [Component files](#component-files)
- [Single-file component filename casing](#single-file-component-filename-casing)
- [Base component names](#base-component-names)
- [Single-instance component names](#single-instance-component-names)
- [Tightly coupled component names](#tightly-coupled-component-names)
- [Order of words in component names](#order-of-words-in-component-names)
- [Self-closing components](#self-closing-components)
- [Component name casing in templates](#component-name-casing-in-templates)
- [Component name casing in JS/JSX](#component-name-casing-in-jsjsx)
- [Full-word component names](#full-word-component-names)
- [Prop name casing](#prop-name-casing)
- [Multi-attribute elements](#multi-attribute-elements)
- [Component/instance options order (adapted)](#componentinstance-options-order-adapted)
- [Element attribute order](#element-attribute-order)
- [Single-file component top-level element order](#single-file-component-top-level-element-order)
- [Simple expressions in templates](#simple-expressions-in-templates)
- [Simple computed properties](#simple-computed-properties)
- [Quoted attribute values](#quoted-attribute-values)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
___




# [Multi-word component names](https://vuejs.org/v2/style-guide/#Multi-word-component-names-essential)
Component names should always be multi-word, except for root `App` components, and built-in components provided by Vue (or other framework), such as `<transition>` or `<component>`.

This [prevents conflicts](http://w3c.github.io/webcomponents/spec/custom/#valid-custom-element-name) with existing and future HTML elements, since all HTML elements are a single word.
```javascript
// Bad
Vue.component( 'todo', {
  // ...
})
export default {
  name: 'Todo',
  // ...
}
```
```javascript
// Good
Vue.component( 'todo-item', {
  // ...
})
export default {
  name: 'TodoItem',
  // ...
}
```





# [Component style scoping](https://vuejs.org/v2/style-guide/#Component-style-scoping-essential)
For applications, styles in a top-level `App` component and in layout components may be global, but all other components should always be scoped.

This is only relevant for single-file components. It does not require that the `scoped` attribute be used. Scoping could be through CSS modules, a class-based strategy such as [BEM](http://getbem.com/), or another library/convention.

**Component libraries, however, should prefer a class-based strategy instead of using the** `scoped` **attribute.**

This makes overriding internal styles easier, with human-readable class names that don’t have too high specificity but are still very unlikely to result in a conflict.
```javascript
// Bad
<template>
  <button class="btn btn-close">X</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>
```
```javascript
// Good
<template>
  <button class="button button-close">X</button>
</template>

<!-- Using the `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>
```
```javascript
// Good
<template>
  <button :class="[ $style.button, $style.buttonClose ]">X</button>
</template>

<!-- Using CSS modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>
```
```javascript
// Good and preffered!
<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- Using the BEM convention -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```





# [Component files](https://vuejs.org/v2/style-guide/#Component-files-strongly-recommended)
Whenever a build system is available to concatenate files, each component should be in its own file.
This helps you to more quickly find a component when you need to edit it or review how to use it.
```javascript
// Bad
Vue.component( 'TodoList', {
  // ...
})
Vue.component( 'TodoItem', {
  // ...
})
```
```
// Good
components/
|- TodoList.js
|- TodoItem.js

components/
|- TodoList.vue
|- TodoItem.vue
```




# [Single-file component filename casing](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended)
**Filenames of single-file components should either be always PascalCase or always kebab-case.**

PascalCase works best with autocompletion in code editors, as it’s consistent with how we reference components in JS(X) and templates, wherever possible. However, mixed case filenames can sometimes create issues on case-insensitive file systems, which is why kebab-case is also perfectly acceptable.
```
// Bad
components/
|- mycomponent.vue

components/
|- myComponent.vue
```
```
// Good
components/
|- MyComponent.vue

components/
|- my-component.vue
```





# [Base component names](https://vuejs.org/v2/style-guide/#Base-component-names-strongly-recommended)
Base components (a.k.a. presentational, dumb, or pure components) that apply app-specific styling and conventions should all begin with a specific prefix, such as `Base`, `App`, or `V`.
```
// Bad
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```
```
// Good
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue

components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue

components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue
```





# [Single-instance component names](https://vuejs.org/v2/style-guide/#Single-instance-component-names-strongly-recommended)
Components that should only ever have a single active instance should begin with the `The` prefix, to denote that there can be only one.

This does not mean the component is only used in a single page, but it will only be used once per page. These components never accept any props, since they are specific to your app, not their context within your app. If you find the need to add props, it’s a good indication that this is actually a reusable component that is only used once per page for now.
```
// Bad
components/
|- Heading.vue
|- MySidebar.vue
```
```
// Good
components/
|- TheHeading.vue
|- TheSidebar.vue
```





# [Tightly coupled component names](https://vuejs.org/v2/style-guide/#Tightly-coupled-component-names-strongly-recommended)
Child components that are tightly coupled with their parent should include the parent component name as a prefix.

If a component only makes sense in the context of a single parent component, that relationship should be evident in its name. Since editors typically organize files alphabetically, this also keeps these related files next to each other.
```
// Bad
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue

components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue
```
```
// Good
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue

components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```





# [Order of words in component names](https://vuejs.org/v2/style-guide/#Order-of-words-in-component-names-strongly-recommended)
Component names should start with the highest-level (often most general) words and end with descriptive modifying words.
```
// Bad
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```
```
// Good
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```





# [Self-closing components](https://vuejs.org/v2/style-guide/#Self-closing-components-strongly-recommended)
**Components with no content should be self-closing in single-file components, string templates, and JSX - but never in DOM templates.**

Components that self-close communicate that they not only have no content but **are meant to have no content**. It’s the difference between a blank page in a book and one labeled “This page intentionally left blank.” Your code is also cleaner without the unnecessary closing tag.

Unfortunately, HTML doesn’t allow custom elements to be self-closing - only official “void” elements. That’s why the strategy is only possible when Vue’s template compiler can reach the template before the DOM, then serve the DOM spec-compliant HTML.
```HTML
// Bad
<!-- In single-file components, string templates, and JSX -->
<MyComponent></MyComponent>
<!-- In DOM templates -->
<my-component/>
```
```HTML
// Good
<!-- In single-file components, string templates, and JSX -->
<MyComponent/>
<!-- In DOM templates -->
<my-component></my-component>
```





# [Component name casing in templates](https://vuejs.org/v2/style-guide/#Component-name-casing-in-templates-strongly-recommended)
In most projects, component names should always be PascalCase in single-file components and string templates - but kebab-case in DOM templates.

PascalCase has a few advantages over kebab-case:

* Editors can autocomplete component names in templates because PascalCase is also used in JavaScript.
* `<MyComponent>` is more visually distinct from a single-word HTML element than `<my-component>`, because there are two character differences (the two capitals), rather than just one (a hyphen).
* If you use any non-Vue custom elements in your templates, such as a web component, PascalCase ensures that your Vue components remain distinctly visible.
Unfortunately, due to HTML’s case insensitivity, DOM templates must still use kebab-case.

Also note that if you’ve already invested heavily in kebab-case, consistency with HTML conventions and being able to use the same casing across all your projects may be more important than the advantages listed above. In those cases, using kebab-case everywhere is also acceptable.
```HTML
// Bad
<!-- In single-file components and string templates -->
<mycomponent/>
<!-- In single-file components and string templates -->
<myComponent/>
<!-- In DOM templates -->
<MyComponent></MyComponent>
```
```HTML
// Good
<!-- In single-file components and string templates -->
<MyComponent/>
<!-- In DOM templates -->
<my-component></my-component>
<!-- Everywhere -->
<my-component></my-component>
```





# [Component name casing in JS/JSX](https://vuejs.org/v2/style-guide/#Component-name-casing-in-JS-JSX-strongly-recommended)
Component names in JS/JSX should always be PascalCase, though they may be kebab-case inside strings for simpler applications that only use global component registration through Vue.component.
```javascript
// Bad
Vue.component( 'myComponent', {
  // ...
})

import myComponent from './MyComponent.vue'

export default {
  name: 'myComponent',
  // ...
}

export default {
  name: 'my-component',
  // ...
}
```
```javascript
// Good
Vue.component( 'MyComponent', {
  // ...
})

Vue.component( 'my-component', {
  // ...
})

import MyComponent from './MyComponent.vue'

export default {
  name: 'MyComponent',
  // ...
}
```





# [Full-word component names](https://vuejs.org/v2/style-guide/#Full-word-component-names-strongly-recommended)
Component names should prefer full words over abbreviations.

The autocompletion in editors makes the cost of writing longer names very low, while the clarity they provide is invaluable. Uncommon abbreviations, in particular, should always be avoided.
```
// Bad
components/
|- SdSettings.vue
|- UProfOpts.vue
```
```
// Good
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```





# [Prop name casing](https://vuejs.org/v2/style-guide/#Prop-name-casing-strongly-recommended)
Prop names should always use camelCase during declaration, but kebab-case in templates and JSX.

We’re simply following the conventions of each language. Within JavaScript, camelCase is more natural. Within HTML, kebab-case is.
```javascript
// Bad
props: {
  'greeting-text': String
}
```
```HTML
<WelcomeMessage greetingText="hi" />
```
```javascript
// Good
props: {
  greetingText: String
}
```
```HTML
<WelcomeMessage greeting-text="hi" />
```





# [Multi-attribute elements](https://vuejs.org/v2/style-guide/#Multi-attribute-elements-strongly-recommended)
Elements with multiple attributes should span multiple lines, with one attribute per line.

In JavaScript, splitting objects with multiple properties over multiple lines is widely considered a good convention, because it’s much easier to read. Our templates and JSX deserve the same consideration.
```HTML
// Bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c" />
```
```HTML
// Good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```





# [Component/instance options order](https://vuejs.org/v2/style-guide/#Component-instance-options-order-recommended) (adapted)
Component/instance options should be ordered consistently.\
This is the default order we recommend for component options:
* props
* store state
    - store getters
    - store actions
* local State (local reactive properties)
    - local properties (data)
    - computed
* events
    - watchers
    - lifecycle events
        - beforeCreate
        - created
        - mounted
        - ...
* non-reactive properties
    - methods

Vue.js component example (using [TypeScript](https://github.com/Microsoft/TypeScript-Vue-Starter), [vue-class-component](https://github.com/vuejs/vue-class-component) and [vue-property-decorator](https://github.com/kaorun343/vue-property-decorator))
```javascript
@Component({
    computed: { ...mapGetters( 'storeModule', {
        storeGetData: 'getData'
    })},
    methods: { ...mapActions( 'storeModule', {
        storeDoAction: 'doAction'
    })}
})
export default class MyNewComponent extends Vue {
    // Store getters
    storeGetData:Array<String>;
    // Store actions
    storeDoAction:void;

    // Component properties
    componentProperty1:Number = 5;
    componentProperty2:Boolean = true;

    // Computed properties
    get isThatTrue() {
        return this.componentProperty2;
    }

    // Watchers
    @Watch( 'componentProperty1' )
    componentProperty1Updated( newValue ) {
        // Do something with new value
    }

    // Lifecycle hooks
    created() {
        // Component was created
    }

    mounted() {
        // Component was mounted
    }

    // Component methods
    method1() {
    }

    method2 = () => {}
};
```





# [Element attribute order](https://vuejs.org/v2/style-guide/#Element-attribute-order-recommended)
The attributes of elements (including components) should be ordered consistently.

This is the default order we recommend for component options. They’re split into categories, so you’ll know where to add custom attributes and directives.

1. Definition (provides the component options)
    - `is`
2. List Rendering (creates multiple variations of the same element)
    - `v-for`
3. Conditionals (whether the element is rendered/shown)
    - `v-if`
    - `v-else-if`
    - `v-else`
    - `v-show`
    - `v-cloak`
4. Render Modifiers (changes the way the element renders)
    - `v-pre`
    - `v-once`
5. Global Awareness (requires knowledge beyond the component)
    - `id`
    - `class`
6. Unique Attributes (attributes that require unique values)
    - `ref`
    - `key`
    - `slot`
7. Two-Way Binding (combining binding and events)
    - `v-model`
8. Other Attributes (all unspecified bound & unbound attributes)
9. Events (component event listeners)
    - `v-on`
10. Content (overrides the content of the element)
    - `v-html`
    - `v-text`
```HTML
  <!-- Bad -->
  <div
    ref="header"
    v-for="item in items"
    v-once
    id="uniqueID"
    v-model="headerData"
    my-prop="prop"
    v-if="!visible"
    is="header"
    @click="functionCall"
    v-text="textContent">
  </div>
```
```HTML
  <!-- Good -->
  <div
    is="header"
    v-for="item in items"
    v-if="!visible"
    v-once
    id="uniqueID"
    ref="header"
    v-model="headerData"
    my-prop="prop"
    @click="functionCall"
    v-text="textContent">
  </div>
```




# [Single-file component top-level element order](https://vuejs.org/v2/style-guide/#Single-file-component-top-level-element-order-recommended)
Single-file components should always order `<script>`, `<template>`, and `<style>` tags consistently, with `<style>` last, because at least one of the other two is always necessary.

```HTML
// Bad (style/script/template)
<style>/* ... */</style>
<script>/* ... */</script>
<template>...</template>

// Inconsistency in order between ComponentA and ComponentB
<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```
```HTML
// Good
<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

// Or
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```





# [Simple expressions in templates](https://vuejs.org/v2/style-guide/#Simple-expressions-in-templates-strongly-recommended)
Component templates should only include simple expressions, with more complex expressions refactored into computed properties or methods.

Complex expressions in your templates make them less declarative. We should strive to describe what should appear, not how we’re computing that value. Computed properties and methods also allow the code to be reused.
```HTML
Bad
{{
  fullName.split(' ').map(function (word) {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}
```
```HTML
Good
<!-- In a template -->
{{ normalizedFullName }}
```
```javascript
// The complex expression has been moved to a computed property
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```





# [Simple computed properties](https://vuejs.org/v2/style-guide/#Simple-computed-properties-strongly-recommended)
Complex computed properties should be split into as many simpler properties as possible.
```javascript
// Bad
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}
```
```javascript
// Good
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```





# [Quoted attribute values](https://vuejs.org/v2/style-guide/#Quoted-attribute-values-strongly-recommended)
**Non-empty HTML attribute values should always be inside quotes (single or double, whichever is not used in JS).**\
While attribute values without any spaces are not required to have quotes in HTML, this practice often leads to avoiding spaces, making attribute values less readable.
```HTML
<!-- Bad -->
<input type=text>
<AppSidebar :style={width:sidebarWidth+'px'} />
```
```HTML
<!-- Good -->
<input type="text">
<AppSidebar :style="{ width: sidebarWidth + 'px' }" />
```
---
To be considered: [eslint-plugin-vue](https://github.com/vuejs/eslint-plugin-vue).