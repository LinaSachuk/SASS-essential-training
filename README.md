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






```SCSS




```