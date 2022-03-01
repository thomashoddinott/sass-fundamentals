**Solutions to challenges: https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises**

https://sass-lang.com/guide

https://sass-lang.com/documentation

https://css-tricks.com/bem-101/

### CSS Pitfalls

**CSS wasn't designed for the rich applications we have today**

- No encapsulation
- No variables
- Not composable  
- Bad modularity primitives
- Globals
- Beating-into-submission-driven-development
  ==> Using `!Important` and negative margins, etc.

### Preprocessor Benefits

Sass, LESS, Stylus, PostCSS

- Compile to CSS
- Parameterised (variables)
- Composable
- Modular
- Plug in to your existing tools

**Why?**

- Adds stuff that should have been in CSS
- Style is faster to build & easier to maintain
- D.R.Y.
- It's easier to keep your styles organised 
- Easy to set up
- Toolkits on top of preprocessors 

### Syntax, Nesting, & Selectors

**S**yntactically **A**wesome **S**tyle **S**heets 

Two file formats:

1. Closer to CSS -- more popular

```scss
.foo { 
  color: green;
  font-size: 1.3rem;
}
```

2. Whitespace file formatted

```scss
.foo
	color: green
	font-size: 1.3rem
```

**Nesting & Scoping**

Styles can placed in the declaration block of a parent element

```scss
.container {
  max-width: 600px;
  width: 100%;
  margin: auto;
  background: #eee;
  
  .sidebar,
  .main { padding: 10px }
  
  .main {
    margin-left: 220px;
    min-height: 100vh;
    border-left: 2px solid #333
  }
  
  .sidebar {
    width: 200px;
    float: left
  }
}
```

^ these are called *descendant* style rules

### Sass @import and Variables

Importing sass gives modularity like we get into ordinary programming languages

```scss
@import '_variables';
```

```
src/sass
	_layout.scss 
	_variables.scss
	app.scss
	other.scss
```

Underscore files are called partials ==> not compiled into css files by sass compiler. They are imported into other !underscored files (which are compiled to normal css)

Global variables are defined with the $

```scss
$error_color: #f00 !default; // global 

.alert-error {
  $text_color: #ddd; // local
  background-color: $error-color;
  color: $text_color;
  text-shadow: 0 0 2px darken($text_color, 40%) // a built-in sass function
}
```

**Comments**

Sass preserves `/* comments */` but removes `// comments`

### Sass Mixins and Arguments

- Allow for re-use of style
- A function which returns a style
- Best practice: separate from styles, like variables

```scss
@mixin alert-text($color) {
  background-color: $color;
  color: white;
  font-variant: small-caps;
}

.error-text {
  @include alert-text(blue);
}

.has-error:after {
  @include alert-text(red);
  display: inline-block;
  content: attr(data-error);
}
```

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/mixins/src/sass)

### Default Argument Values

- We can define default values for arguments
- Arguments can be provided in order, or by name
- When "keyword args" used, order is ignored

```scss
@mixin alert-text($color: #f33) {
  background-color: $color;
  color: white;
  font-variant: small-caps;
}

h1 {
  @include alert-text(blue)
}
h2 {
  @include alert-text($color: green)
}
```

If an argument is set to null it won't be expressed in the emitted CSS.

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/range)

### Functions

Read the docs: https://sass-lang.com/documentation/modules

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/functions)

### @if Sass Directive

```scss
@mixin foo($size) {
  font-size: $size;
  @if $size > 20 {
    line-height: $size;
  }
}

.small {
  @include foo(14px);
}
.large {
  @include foo(24px);
}
```

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/if)

### Data Structures

**@for**

```scss
@for $i from 1 through 3 {
  ul:nth-child(3n + #{$i}) {
    background-color: lighten($base-color, $i * 5%);
  }
}
```

**lists**

```scss
$mylist = 0 0 2px #000;

.foo {
	box-shadow: $mylist
}
```

**@each**

```scss
$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}
```

**nth**

Used to pick an item out of an array, like: `arr[i]` in JS. They are 1-indexed.

```scss
$gradients: 
  (to left top, blue, red);
  (to left top, blue, yellow);

.foo {
  background: linear-gradient(nth($gradients, 2));
}
```

**Maps**

key:value pairs

Great for storing themes

```scss
$mymap: (
	dark: #009,
  light: #66f
);

@mixin theme-button($t) {
  color: map-get($mymap, $t)
}

.btn-dark {
  @include theme-button('dark')
}
.btn-light {
  @include theme-button('light')
}
```

### Nudging Classes

Used for iteration

CSS classes like this are very tedious to write 

```css
m-t-5 {
  margin-top: 5
}
```

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/tiny) 

^ confusing, but it drives the point home

### BEM

BEM is a standard for writing good (maintainable) CSS

**Block** - Standalone entity, meaningful on its own

`header, container, menu, checkbox, input, button`

Basically a component

**Element** - A part of a block that has not standalone meaning, and is semantically tied to its block

 `__menu-item, __list-item, __checkbox-caption, __header-title`

**Modifier** - A flag on a block or element, used to change appearance and/or behaviour

`--disabled, --highlighted, --checked, --size-big, --colour-yellow`

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/bem)

### Style reuse via Mixins

```scss
@mixin danger {
  background: red;
  color: white;
}

.btn-danger {
  @include danger();
  padding: 2px;
}

.alert-danger {
  @include danger();
  width: 100%;
}
```

Generates CSS: 

```css
.btn-danger {
  background: red;
  color: white;
  padding: 2px;
}

.alert-danger {
  background: red;
  color: white;
  width: 100%;
}
```

... and this would cause our CSS to grow a lot as our `mixin` grows

**@extend**

```scss
.danger {
  background: red;
  color: white;
}

.btn-danger {
  @extend .danger;
  padding: 2px;
}

.alert-danger {
	@extend .danger;
  width: 100%
}
```

Generates CSS:

```css
.danger,
.btn-danger,
.alert-danger {
  background: red;
  color: white;
}

.btn-danger {
  padding: 2px;
}

.alert-danger {
  width: 100%;
}
```

**@extend - Placeholders** ???

:warning: Extend can get out of control quickly without **placeholders**

https://sass-lang.com/documentation/style-rules/placeholder-selectors

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/extend)

### Writing Sass Functions

```scss
$w: 2px;
@function double($x) {
	@return 2 * $x;
}

.thin-border {
  border-width: $w;
}
.thick-border {
  border-width: double($w);
}
```

CSS:

```css
.thin-border {
  border-width: 2px;
}
.thick-border {
  border-width: 4px;
}
```

[exercise](https://github.com/thomashoddinott/sass-fundamentals/tree/femasters/final/exercises/luminance)



