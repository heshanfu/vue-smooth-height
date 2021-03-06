
# Vue Smooth Height (VSM)
A Vue mixin that answers the question, "How do I transition height: auto?"

When the component's data is changed (i.e. when the `updated` lifecycle hook is called), its current height will smoothly transition to the new height, instead of instantly resizing. If the transition is interrupted, it will transition from the interrupted height to the new height.

Note that this library has no overlap with Vue's built in transition components.

## Demo
https://jsfiddle.net/axfwg1L0/47/

## Installation

Download via npm:
```shell
$ npm install vue-smooth-height
```

Include via cdn:
```html
<script src="https://unpkg.com/vue-smooth-height"></script>
```

## Usage

Module:

```javascript
<template>
    <div>
        <div ref="container"></div>
    </div>
</template>

<script>
import smoothHeight from 'vue-smooth-height';

export default {
    mixins:[smoothHeight],
    mounted(){
        this.$smoothElement({
            el: this.$refs.container,
        })
    },
}
</script>
```

Browser:

The mixin is available via the global variable `SmoothHeight`

```javascript
Vue.component('myComponent', {
    mixins:[SmoothHeight],
    mounted(){
        this.$smoothElement({
            el: this.$refs.container,
        })
    },
     template: 
        `
        <div>
            <div ref="container"></div>
        </div>
        `
})
```

## CSS Transitions
VSM will check if the element has a height transition, either through the stylesheet or inline styles. If it exists, VSM will use that. If it doesn't, it will apply `transition: height .5s` to the element's inline style, and append any existing transition properties it finds.

For example, if the element has this style:

```
.element {
    transition: opacity 1s;
}
```

VSM will set ```style="transition: opacity 1s, height .5s;"``` on the element during the transition. After the transition, it will remove the inline style.

For compatibility, do not set your other transitions on the element as inline styles, as they will be overridden.

## API
### $smoothElement(options)
#### options: Object | Array

Can be a single options object,
or an array of options objects.

Enables smooth height transition on an element.


**Option**|**Types**|**Default**|**Description**
-----|-----|-----|-----
el|Element, String|null|Required. A reference to the element, or a selector string. Use a selector string if the element is not rendered initially. If the selector string is a class, the first query match will be used.
transition|String|<nobr>`height .5s`</nobr>| The CSS shorthand `transition` property. Use this option if you don't want to use stylesheets (`<style>...</style>`). 
hideOverflow|Boolean|false|If the element has `overflow-y: auto`, a scrollbar can temporarily appear during the transition. Set this option to `true` to hide the scrollbar during the transition.
debug|Boolean|false|Logs messages at certain times within the transition lifecycle.


### $unsmoothElement(options)
#### options: Object | Array

Can be a single options object,
or an array of options objects.

Disables smooth height behavior on an element. Registered elements that have the same `el` as the passed in options will be unregistered. 

## Examples:


```javascript

mounted(){
    // Register with element reference
    this.$smoothElement({
        el: this.$refs.container,
    })
    // Register with classname
    this.$smoothElement({
        el: '.container',
    })
    // Pass an array of options
    this.$smoothElement([
        {
            el: this.$refs.container,
        },
        {
            el: '.container',
            hideOverflow: true,
            transition: 'height 1s ease-in-out .15s'
            debug: true,
        }
    ])
    // If the element reference is a component, 
    // make sure to pass in its "$el" property.
    this.$smoothElement({
        el: this.$refs.container.$el,
    })

},

```