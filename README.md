# code-architecture

This is a short tutorial, how to structure and organize code for our `Vue.js` or `Nuxt.js` projects.
It's important to stick with these rules, so the code of our projects remains maintainable.

## 1. Build `pages` with modular `components`

Let's start with an example of a page, called `about`. The page contains a `hero_section`, a `team_section`, and a `bio_section`.

I've seen people putting all the content of these sections into one single `Vue.js` component, which is clearly not what we want. Instead, create one `component` for each `section` and import it into your `page component`, like this.

in `@/components/pages/about/section-hero.vue`

```vue
<template>
  <div>
    <h1>This is the HeroSection component</h1>
  </div>
</template>

<script>
  export default{}
</script>
```

in `@/components/pages/about/section-team.vue`

```vue
<template>
  <div>
    <h1>This is the TeamSection component</h1>
  </div>
</template>

<script>
  export default{}
</script>
```

in `@/components/pages/about/section-bio.vue`

```vue
<template>
  <div>
    <h1>This is the BioSection component</h1>
  </div>
</template>

<script>
  export default{}
</script>
```
Then import all section components into the page. Now you have a beautiful and clean page.

in `@/pages/about.vue`

```vue
<template>
  <div>
    <h1>This is the Page component</h1>
    <SectionHero />
    <SectionTeam />
    <SectionBio />
  </div>
</template>

<script>

  import SectionHero from '@/components/pages/about/section-hero'
  import SectionTeam from '@/components/pages/about/section-team'
  import SectionBio from '@/components/pages/about/section-bio'

  export default{
    components:{
      SectionHero,
      SectionTeam,
      SectionBio,
    }
  }
</script>
```

## 2. Write modular `SCSS` code.

I've seen people writing a lot of global `scss` into `Vue.js` applications, which makes the app less maintainable and results in a loss of all of the great benefits that the modular structure of `Vue.js` comes along with.

Please **pay a lot of attention** to the following checklist.

- All `Vue.js` components `style tags` **must** be scoped. If the modification of `bootstrap-vue` components doesn't work with the `scoped` attribute, you will need to find a way around, or create your own component instead. Removing the `scoped` attribute in order to modify a `boostrap-vue` component won't be accepted.

```vue
<template>
  <div>
    <h1>This is a component</h1>
  </div>
</template>

<script>

  export default{}
</script>

<style lang="scss" scoped>

</style>
```
 
- It is **forbidden** to write global `css` without any kind of scoping:

  This is an example how you **can** and how you **cannot** style a `p` tag:
  
  ```scss
  
    //this is not allowed
    
    p{
      margin:0
    }
    
    //this is allowed
    
    .my-layout{
      p{
        margin:0
      }
    } 
    
  ```
  
  If you want to write a `_reset.scss` file, you also **must** use a layout wrapper:
  
  ```scss
  
  //this is not allowed  
  
  ul {
     list-style: none;
   }
  
   //this is allowed
   
  .my-layout{
    ul {
       list-style: none;
     }
  }
  ```
  
  It's **forbidden** to setup rules for `body` or `html` tags:
  
    ```scss
      
      //this is not allowed
    
      body,
      html {
        margin: 0;
        padding: 0;
        border: 0;
        outline: 0;
        font-size: 100%;
        background: 0 0;
      }
  ```

  All `vue components` that are used on **more than one page** (like `header`, `footer`, `navbar`, `sidebar`) **MUST NOT** be wrapped inside the `layout` wrapper.
  
  ```html

  <!--This is not allowed-->

  <div class="my-layout">
    <header></header>
    <div class="container"></div>
    <footer></footer>
  </div>

  <!--This is allowed-->

  <header></header>
  <div class="my-layout">
    <div class="container"></div>
  </div>
  <footer></footer>

  ```
  
  The only css rule that can and **should** be set globally is the `font-family` rule.
  
  ```scss
  
    //this is allowed
    body{
      font-family: 'Open Sans', sans-serif;
    }
    
  ```  

- Any component-related styling must be written inside the component with a `scoped` attribute.

- Responsive `scss` code **must** also go inside the component. Please **don't** write a global `responsive.scss` file.   

- **Never** write css syntax. Always write scss syntax.

So instead of this:

```scss
.section-head .section-head__container .section-head__container__title {
  color:green;
}
```

write this:

```scss
.section-head {
  &__container{
    &__title{
      color:green;
    }  
  }
}
```

- The following is **not** a requirement, but we really would appreciate it very much: Use the `BEM` (Block-Element-Modifier) naming convention for scss classes. The concept is simple: 

You have a `block`:
```html
<div class="my-block"></div>
```

And your block has one or more `elements`. A block and its elements are connected with `__`
```html
<div class="my-block">
  <div class="my-block__header"></div>
  <div class="my-block__body"></div>
</div>
```

If some of your `block`s `elements` have a different style, the `modifier` joins the game. `modifiers` are connected with `--`:
```html
<div class="my-block">
  <div class="my-block__header my-block__header--default"></div>
  <div class="my-block__body my-block__body--default"></div>
</div>

<div class="my-block">
  <div class="my-block__header my-block__header--lg"></div>
  <div class="my-block__body my-block__body--lg"></div>
</div>

<div class="my-block">
  <div class="my-block__header my-block__header--sm"></div>
  <div class="my-block__body my-block__body--sm"></div>
</div>

```

This is much more maintainable than this:
```html
<div class="my-block">
  <div class="header default"></div>
  <div class="body default"></div>
</div>

<div class="my-block">
  <div class="header lg"></div>
  <div class="body lg"></div>
</div>

<div class="my-block">
  <div class="header sm"></div>
  <div class="body sm"></div>
</div>

```

