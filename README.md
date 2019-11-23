# vue-sticky-component

Component that makes element inside it sticky. It simple and SSR friendly

# Install

```
npm install vue-sticky-component --save
```

#Usage

Simple top limitation example. Its trigger when sticky block top position on screen less than `20px`:
```html
<div>    
    <sticky-component 
            class="sticky-container"        
            v-bind:top-offset="20"
    >
        Lorem ipsum dolor sit amet, consectetur adipisicing elit.
    </sticky-component>
</div>
```

This one, add also bottom position limitation. In this case, block always be visible on screen and its position will be bettwen `20px` from top and `100px` from bottom 

```html
<div>    
    <sticky-component 
            class="sticky-container"        
            v-bind:top-offset="20"
            v-bind:bottom-offset="100"
    >
        Lorem ipsum dolor sit amet, consectetur adipisicing elit.
    </sticky-component>
</div>
```

```html
<div ref="container">    
    <sticky-component 
            class="sticky-container"        
            v-bind:container="$refs.container"
            v-bind:top-offset="20"
    >
        <div class="sticky">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Aspernatur debitis distinctio eos odio, porro saepe voluptates?
            Accusamus error expedita facere, minus nulla quo.
            Amet cum dolor optio quam. Quam, sed?
        </div>
    </sticky-component>
</div>
```
# Options

* `container` *(HTMLElement)* - must be parent DOM element. Sticky block top and bottom position will be limited corresponding this container geometry
* `container-top-offset` *(Number)* - minimum distance between parent container top position and sticky block 
* `container-bottom-offset` *(Number)* - minimum distance between parent container bottom position and sticky block    
