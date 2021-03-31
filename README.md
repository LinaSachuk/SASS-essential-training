# SASS-essential-training
## Sass is the most mature, stable, and powerful professional grade CSS extension language in the world.

https://www.linkedin.com/learning/sass-essential-training/using-the-exercise-files-2

https://github.com/planetoftheweb/sassEssentials

Learn the fundamentals of Syntactically Awesome Stylesheets (Sass), a modern web development language that helps you write CSS better, faster, and with more advanced features. Learn how to install Sass and work with its main features: variables, nesting, partials, and mixins. Plus, learn how to use SassScript to create complex functions from Sass lists and control statements.

## Learning objectives

1. Working with variables
2. Nesting styles
3. Creating mixins
4. Conditional statements and loops in SassScript
5. Extending your mixins
6. Working with lists and maps

### Skills covered

- SASS
- Front-end Development

----------------------------

## Learning SASS

https://sass-lang.com/guide
## Variables

Think of variables as a way to store information that you want to reuse throughout your stylesheet. You can store things like colors, font stacks, or any CSS value you think you'll want to reuse. Sass uses the $ symbol to make something a variable. Here's an example:

```SCSS



// SCSS
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

//  When the Sass is processed, it takes the variables we define for the $font-stack and $primary-color and outputs normal CSS with our variable values placed in the CSS. This can be extremely powerful when working with brand colors and keeping them consistent throughout the site.



// CSS
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}

```

## Nesting

Sass will let you nest your CSS selectors in a way that follows the same visual hierarchy of your HTML. Be aware that overly nested rules will result in over-qualified CSS that could prove hard to maintain and is generally considered bad practice.

With that in mind, here's an example of some typical styles for a site's navigation:

```SCSS

nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

```

You'll notice that the ul, li, and a selectors are nested inside the nav selector. This is a great way to organize your CSS and make it more readable.

## Partials

You can create partial Sass files that contain little snippets of CSS that you can include in other Sass files. This is a great way to modularize your CSS and help keep things easier to maintain. A partial is a Sass file named with a leading underscore. You might name it something like _partial.scss. The underscore lets Sass know that the file is only a partial file and that it should not be generated into a CSS file. Sass partials are used with the @use rule.


## Modules

You don't have to write all your Sass in a single file. You can split it up however you want with the @use rule. This rule loads another Sass file as a module, which means you can refer to its variables, mixins, and functions in your Sass file with a namespace based on the filename. Using a file will also include the CSS it generates in your compiled output!

```SCSS

// _base.scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

```

```SCSS

// styles.scss
@use 'base';

.inverse {
  background-color: base.$primary-color;
  color: white;
}

```

Notice we're using @use 'base'; in the styles.scss file. When you use a file you don't need to include the file extension. Sass is smart and will figure it out for you.


## Mixins


Some things in CSS are a bit tedious to write, especially with CSS3 and the many vendor prefixes that exist. A mixin lets you make groups of CSS declarations that you want to reuse throughout your site. You can even pass in values to make your mixin more flexible. A good use of a mixin is for vendor prefixes. Here's an example for transform.

```SCSS

@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}
.box { @include transform(rotate(30deg)); }

```
To create a mixin you use the @mixin directive and give it a name. We've named our mixin transform. We're also using the variable $property inside the parentheses so we can pass in a transform of whatever we want. After you create your mixin, you can then use it as a CSS declaration starting with @include followed by the name of the mixin.

## Extend/Inheritance

This is one of the most useful features of Sass. Using @extend lets you share a set of CSS properties from one selector to another. It helps keep your Sass very DRY. In our example we're going to create a simple series of messaging for errors, warnings and successes using another feature which goes hand in hand with extend, placeholder classes. A placeholder class is a special type of class that only prints when it is extended, and can help keep your compiled CSS neat and clean.

```SCSS

%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

// This CSS won't print because %equal-heights is never extended.
%equal-heights {
  display: flex;
  flex-wrap: wrap;
}

/* This CSS will print because %message-shared is extended. */
.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}


```

What the above code does is tells .message, .success, .error, and .warning to behave just like %message-shared. That means anywhere that %message-shared shows up, .message, .success, .error, & .warning will too. The magic happens in the generated CSS, where each of these classes will get the same CSS properties as %message-shared. This helps you avoid having to write multiple class names on HTML elements.

You can extend most simple CSS selectors in addition to placeholder classes in Sass, but using placeholders is the easiest way to make sure you aren't extending a class that's nested elsewhere in your styles, which can result in unintended selectors in your CSS.

Note that the CSS in %equal-heights isn't generated, because %equal-heights is never extended.

## Operators

Doing math in your CSS is very helpful. Sass has a handful of standard math operators like +, -, *, /, and %. In our example we're going to do some simple math to calculate widths for an aside & article.

```SCSS

.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}

```

We've created a very simple fluid grid, based on 960px. Operations in Sass let us do something like take pixel values and convert them to percentages without much hassle.

## Parent Selector

The parent selector, &, is a special selector invented by Sass that’s used in nested selectors to refer to the outer selector. It makes it possible to re-use the outer selector in more complex ways, like adding a pseudo-class or adding a selector before the parent.

```SCSS

.alert {
  // The parent selector can be used to add pseudo-classes to the outer
  // selector.
  &:hover {
    font-weight: bold;
  }

  // It can also be used to style the outer selector in a certain context, such
  // as a body set to use a right-to-left language.
  [dir=rtl] & {
    margin-left: 0;
    margin-right: 10px;
  }

  // You can even use it as an argument to pseudo-class selectors.
  :not(&) {
    opacity: 0.8;
  }
}

```

Because the parent selector could be replaced by a type selector like h1, it’s only allowed at the beginning of compound selectors where a type selector would also be allowed. For example, span& is not allowed.

## Adding Suffixes

You can also use the parent selector to add extra suffixes to the outer selector. This is particularly useful when using a methodology like BEM that uses highly structured class names. As long as the outer selector ends with an alphanumeric name (like class, ID, and element selectors), you can use the parent selector to append additional text.

```SCSS

.accordion {
  max-width: 600px;
  margin: 4rem auto;
  width: 90%;
  font-family: "Raleway", sans-serif;
  background: #f4f4f4;

  &__copy {
    display: none;
    padding: 1rem 1.5rem 2rem 1.5rem;
    color: gray;
    line-height: 1.6;
    font-size: 14px;
    font-weight: 500;

    &--open {
      display: block;
    }
  }
}

```

## In SassScript 

The parent selector can also be used within SassScript. It’s a special expression that returns the current parent selector in the same format used by selector functions: a comma-separated list (the selector list) that contains space-separated lists (the complex selectors) that contain unquoted strings (the compound selectors).

```SCSS
@mixin app-background($color) {
  #{if(&, '&.app-background', '.app-background')} {
    background-color: $color;
    color: rgba(#fff, 0.75);
  }
}

@include app-background(#036);

.sidebar {
  @include app-background(#c6538c);
}


```
If the & expression is used outside any style rules, it returns null. Since null is falsey, this means you can easily use it to determine whether a mixin is being called in a style rule or not.

## Advanced Nesting 

You can use & as a normal SassScript expression, which means you can pass it to functions or include it in interpolation—even in other selectors! Using it in combination with selector functions and the @at-root rule allows you to nest selectors in very powerful ways.

For example, suppose you want to write a selector that matches the outer selector and an element selector. You could write a mixin like this one that uses the selector.unify() function to combine & with a user’s selector.

```SCSS

@use "sass:selector";

@mixin unify-parent($child) {
  @at-root #{selector.unify(&, $child)} {
    @content;
  }
}

.wrapper .field {
  @include unify-parent("input") {
    /* ... */
  }
  @include unify-parent("select") {
    /* ... */
  }
}

```

When Sass is nesting selectors, it doesn’t know what interpolation was used to generate them. This means it will automatically add the outer selector to the inner selector even if you used & as a SassScript expression. That’s why you need to explicitly use the @at-root rule to tell Sass not to include the outer selector.

## Placeholder Selectors

Sass has a special kind of selector known as a “placeholder”. It looks and acts a lot like a class selector, but it starts with a % and it's not included in the CSS output. In fact, any complex selector (the ones between the commas) that even contains a placeholder selector isn't included in the CSS, nor is any style rule whose selectors all contain placeholders,

```SCSS

.alert:hover, %strong-alert {
  font-weight: bold;
}

%strong-alert:hover {
  color: red;
}

```

What’s the use of a selector that isn’t emitted? It can still be extended! Unlike class selectors, placeholders don’t clutter up the CSS if they aren’t extended and they don’t mandate that users of a library use specific class names for their HTML.

```SCSS

%toolbelt {
  box-sizing: border-box;
  border-top: 1px rgba(#000, .12) solid;
  padding: 16px 0;
  width: 100%;

  &:hover { border: 2px rgba(#000, .5) solid; }
}

.action-buttons {
  @extend %toolbelt;
  color: #4285f4;
}

.reset-buttons {
  @extend %toolbelt;
  color: #cddc39;
}

```

Placeholder selectors are useful when writing a Sass library where each style rule may or may not be used. As a rule of thumb, if you’re writing a stylesheet just for your own app, it’s often better to just extend a class selector if one is available.

## Sass variables 

Sass variables are simple: you assign a value to a name that begins with $, and then you can refer to that name instead of the value itself. But despite their simplicity, they're one of the most useful tools Sass brings to the table. Variables make it possible to reduce repetition, do complex math, configure libraries, and much more.

A variable declaration looks a lot like a property declaration: it’s written <variable>: <expression>. Unlike a property, which can only be declared in a style rule or at-rule, variables can be declared anywhere you want. To use a variable, just include it in a value.

```SCSS

$base-color: #c6538c;
$border-dark: rgba($base-color, 0.88);

.alert {
  border: 1px solid $border-dark;
}


```



## Default Values

Normally when you assign a value to a variable, if that variable already had a value, its old value is overwritten. But if you’re writing a Sass library, you might want to allow your users to configure your library’s variables before you use them to generate CSS.

To make this possible, Sass provides the !default flag. This assigns a value to a variable only if that variable isn’t defined or its value is null. Otherwise, the existing value will be used.

Variables defined with !default can be configured when loading a module with the @use rule. Sass libraries often use !default variables to allow their users to configure the library’s CSS.

To load a module with configuration, write @use <url> with (<variable>: <value>, <variable>: <value>). The configured values will override the variables’ default values. Only variables written at the top level of the stylesheet with a !default flag can be configured.

```SCSS

// _library.scss
$black: #000 !default;
$border-radius: 0.25rem !default;
$box-shadow: 0 0.5rem 1rem rgba($black, 0.15) !default;

code {
  border-radius: $border-radius;
  box-shadow: $box-shadow;
}


```

```SCSS

// style.scss
@use 'library' with (
  $black: #222,
  $border-radius: 0.1rem
);

```

## Built-in Variables

Variables that are defined by a built-in module cannot be modified.

```SCSS

@use "sass:math" as math;

// This assignment will fail.
math.$pi: 0;


```

## Scope

Variables declared at the top level of a stylesheet are global. This means that they can be accessed anywhere in their module after they’ve been declared. But that’s not true for all variables. Those declared in blocks (curly braces in SCSS or indented code in Sass) are usually local, and can only be accessed within the block they were declared.

```SCSS
$global-variable: global value;

.content {
  $local-variable: local value;
  global: $global-variable;
  local: $local-variable;
}

.sidebar {
  global: $global-variable;

  // This would fail, because $local-variable isn't in scope:
  // local: $local-variable;
}


```

## Shadowing 

Local variables can even be declared with the same name as a global variable. If this happens, there are actually two different variables with the same name: one local and one global. This helps ensure that an author writing a local variable doesn’t accidentally change the value of a global variable they aren’t even aware of.

```SCSS

$variable: global value;

.content {
  $variable: local value;
  value: $variable;
}

.sidebar {
  value: $variable;
}


```

If you need to set a global variable’s value from within a local scope (such as in a mixin), you can use the !global flag. A variable declaration flagged as !global will always assign to the global scope.

```SCSS

$variable: first global value;

.content {
  $variable: second global value !global;
  value: $variable;
}

.sidebar {
  value: $variable;
}


```

The !global flag may only be used to set a variable that has already been declared at the top level of a file. It may not be used to declare a new variable.

## Flow Control

Variables declared in flow control rules have special scoping rules: they don’t shadow variables at the same level as the flow control rule. Instead, they just assign to those variables. This makes it much easier to conditionally assign a value to a variable, or build up a value as part of a loop.

```SCSS

$dark-theme: true !default;
$primary-color: #f8bbd0 !default;
$accent-color: #6a1b9a !default;

@if $dark-theme {
  $primary-color: darken($primary-color, 60%);
  $accent-color: lighten($accent-color, 60%);
}

.button {
  background-color: $primary-color;
  border: 1px solid $accent-color;
  border-radius: 3px;
}


```

Variables in flow control scope can assign to existing variables in the outer scope, but they can’t declare new variables there. Make sure the variable is already declared before you assign to it, even if you need to declare it as null.

## Advanced Variable Functions

The Sass core library provides a couple advanced functions for working with variables. The meta.variable-exists() function returns whether a variable with the given name exists in the current scope, and the meta.global-variable-exists() function does the same but only for the global scope.

Users occasionally want to use interpolation to define a variable name based on another variable. Sass doesn’t allow this, because it makes it much harder to tell at a glance which variables are defined where. What you can do, though, is define a map from names to values that you can then access using variables.

```SCSS

@use "sass:map";

$theme-colors: (
  "success": #28a745,
  "info": #17a2b8,
  "warning": #ffc107,
);

.alert {
  // Instead of $theme-color-#{warning}
  background-color: map.get($theme-colors, "warning");
}


```

## Interpolation

Interpolation can be used almost anywhere in a Sass stylesheet to embed the result of a SassScript expression into a chunk of CSS. Just wrap an expression in #{} in any of the following places:

- Selectors in style rules
- Property names in declarations
- Custom property values
- CSS at-rules
- @extends
- Plain CSS @imports
- Quoted or unquoted strings
- Special functions
- Plain CSS function names
- Loud comments

```SCSS

@mixin corner-icon($name, $top-or-bottom, $left-or-right) {
  .icon-#{$name} {
    background-image: url("/icons/#{$name}.svg");
    position: absolute;
    #{$top-or-bottom}: 0;
    #{$left-or-right}: 0;
  }
}

@include corner-icon("mail", top, left);

```

## Interpolation In SassScript 

Interpolation can be used in SassScript to inject SassScript into unquoted strings. This is particularly useful when dynamically generating names (for example for animations), or when using slash-separated values. Note that interpolation in SassScript always returns an unquoted string.

```SASS

@mixin inline-animation($duration) {
  $name: inline-#{unique-id()};

  @keyframes #{$name} {
    @content;
  }

  animation-name: $name;
  animation-duration: $duration;
  animation-iteration-count: infinite;
}

.pulse {
  @include inline-animation(2s) {
    from { background-color: yellow }
    to { background-color: red }
  }
}

```

It’s almost always a bad idea to use interpolation with numbers. Interpolation returns unquoted strings that can’t be used for any further math, and it avoids Sass’s built-in safeguards to ensure that units are used correctly.

Sass has powerful unit arithmetic that you can use instead. For example, instead of writing #{$width}px, write $width * 1px—or better yet, declare the $width variable in terms of px to begin with. That way if $width already has units, you’ll get a nice error message instead of compiling bogus CSS.


## Quoted Strings




```SASS



```