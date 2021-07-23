https://sass-lang.com/

# Sass Basics

Before you can use Sass, you need to set it up on your project. If you want to just browse here, go ahead, but we recommend you go install Sass first. Go [here](https://sass-lang.com/install) if you want to learn how to get everything setup.

## Preprocessing

CSS on its own can be fun, but stylesheets are getting larger, more complex, and harder to maintain. This is where a preprocessor can help. Sass lets you use features that don't exist in CSS yet like variables, nesting, mixins, inheritance and other nifty goodies that make writing CSS fun again.

Once you start tinkering with Sass, it will take your preprocessed Sass file and save it as a normal CSS file that you can use in your website.

The most direct way to make this happen is in your terminal. Once Sass is installed, you can compile your Sass to CSS using the sass command. You'll need to tell Sass which file to build from, and where to output CSS to. For example, running sass input.scss output.css from your terminal would take a single Sass file, input.scss, and compile that file to output.css.

You can also watch individual files or directories with the --watch flag. The watch flag tells Sass to watch your source files for changes, and re-compile CSS each time you save your Sass. If you wanted to watch (instead of manually build) your input.scss file, you'd just add the watch flag to your command, like so:

```bash

sass --watch input.scss output.css

```

You can watch and output to directories by using folder paths as your input and output, and separating them with a colon. In this example:


```bash

sass --watch app/sass:public/stylesheets

```

Sass would watch all files in the app/sass folder for changes, and compile CSS to the public/stylesheets folder.

---

## Variables

Think of variables as a way to store information that you want to reuse throughout your stylesheet. You can store things like colors, font stacks, or any CSS value you think you'll want to reuse. Sass uses the $ symbol to make something a variable. Here's an example:

```SCSS

$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

```

When the Sass is processed, it takes the variables we define for the $font-stack and $primary-color and outputs normal CSS with our variable values placed in the CSS. This can be extremely powerful when working with brand colors and keeping them consistent throughout the site.

---

## Nesting

When writing HTML you've probably noticed that it has a clear nested and visual hierarchy. CSS, on the other hand, doesn't.

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

---
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

Some things in CSS are a bit tedious to write, especially with CSS3 and the many vendor prefixes that exist. A mixin lets you make groups of CSS declarations that you want to reuse throughout your site. You can even pass in values to make your mixin more flexible. A good use of a mixin is for vendor prefixes. Here's an example for transform

```SCSS

@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}
.box { @include transform(rotate(30deg)); }

```

To create a mixin you use the @mixin directive and give it a name. We've named our mixin transform. We're also using the variable $property inside the parentheses so we can pass in a transform of whatever we want. After you create your mixin, you can then use it as a CSS declaration starting with @include followed by the name of the mixin.


---

## Extend/Inheritance

This is one of the most useful features of Sass. Using @extend lets you share a set of CSS properties from one selector to another. It helps keep your Sass very DRY. In our example we're going to create a simple series of messaging for errors, warnings and successes using another feature which goes hand in hand with extend, placeholder classes. A placeholder class is a special type of class that only prints when it is extended, and can help keep your compiled CSS neat and clean.


```SCSS

/* This CSS will print because %message-shared is extended. */
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

--- 

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

--- 

# Syntax

## SCSS

The SCSS syntax uses the file extension .scss. With a few small exceptions, it’s a superset of CSS, which means essentially all valid CSS is valid SCSS as well. Because of its similarity to CSS, it’s the easiest syntax to get used to and the most popular.
SCSS looks like this:


```SCSS

@mixin button-base() {
  @include typography(button);
  @include ripple-surface;
  @include ripple-radius-bounded;

  display: inline-flex;
  position: relative;
  height: $button-height;
  border: none;
  vertical-align: middle;

  &:hover { cursor: pointer; }

  &:disabled {
    color: $mdc-button-disabled-ink-color;
    cursor: default;
    pointer-events: none;
  }
}

```

## The Indented Syntax

The indented syntax was Sass’s original syntax, and so it uses the file extension .sass. Because of this extension, it’s sometimes just called “Sass”. The indented syntax supports all the same features as SCSS, but it uses indentation instead of curly braces and semicolons to describe the format of the document.

In general, any time you’d write curly braces in CSS or SCSS, you can just indent one level deeper in the indented syntax. And any time a line ends, that counts as a semicolon. There are also a few additional differences in the indented syntax that are noted throughout the reference.

The indented syntax looks like this:


```SCSS
@mixin button-base()
  @include typography(button)
  @include ripple-surface
  @include ripple-radius-bounded

  display: inline-flex
  position: relative
  height: $button-height
  border: none
  vertical-align: middle

  &:hover
    cursor: pointer

  &:disabled
    color: $mdc-button-disabled-ink-color
    cursor: default
    pointer-events: none


```
--- 

# Structure of a Stylesheet

Just like CSS, most Sass stylesheets are mainly made up of style rules that contain property declarations. But Sass stylesheets have many more features that can exist alongside these.

## Statements 

A Sass stylesheet is made up of a series of statements, which are evaluated in order to build the resulting CSS. Some statements may have blocks, defined using { and }, which contain other statements. For example, a style rule is a statement with a block. That block contains other statements, such as property declarations.

In SCSS, statements are separated by semicolons (which are optional if the statement uses a block). In the indented syntax, they’re just separated by newlines.

1. Universal Statements:
  These types of statements can be used anywhere in a Sass stylesheet:

   - Variable declarations, like $var: value.
   - Flow control at-rules, like @if and @each.
   - The @error, @warn, and @debug rules.

2. CSS Statements:
  These statements produce CSS. They can be used anywhere except within a @function:

   - Style rules, like h1 { /* ... */ }.
   - CSS at-rules, like @media and @font-face.
   - Mixin uses using @include.
   - The @at-root rule.
   - Top-Level Statements permalink

3. Top-Level Statements:
  These statements can only be used at the top level of a stylesheet, or nested within a CSS statement at the top level:

   - Module loads, using @use.
   - Imports, using @import.
   - Mixin definitions using @mixin.
   - Function definitions using @function.

4. Other Statements:

      - Property declarations like width: 100px may  only be used within style rules and some CSS at-rules.
      - The @extend rule may only be used within style rules.


## Expressions

An expression is anything that goes on the right-hand side of a property or variable declaration. Each expression produces a value. Any valid CSS property value is also a Sass expression, but Sass expressions are much more powerful than plain CSS values. They’re passed as arguments to mixins and functions, used for control flow with the @if rule, and manipulated using arithmetic. We call Sass’s expression syntax SassScript.

- Literals: 
The simplest expressions just represent static values:

   1. Numbers, which may or may not have units, like 12 or 100px.
   2. Strings, which may or may not have quotes, like "Helvetica Neue" or bold.
   3. Colors, which can be referred to by their hex representation or by name, like #c6538c or blue.
   4. The boolean literals true or false.
   5. The singleton null.
   6. Lists of values, which may be separated by spaces or commas and which may be enclosed in square brackets or no brackets at all, like 1.5em 1em 0 2em, Helvetica, Arial, sans-serif, or [col1-start].
   7. Maps that associate values with keys, like ("background": red, "foreground": pink).

  - Operations: 

Sass defines syntax for a number of operations:

  1. == and != are used to check if two values are the same.
  2. +, -, *, /, and % have their usual mathematical meaning for numbers, with special behaviors for units that matches the use of units in scientific math.
  3. <, <=, >, and >= check whether two numbers are greater or less than one another.
  and, or, and not have the usual boolean behavior. Sass considers every value “true” except for false and null.
  4. +, -, and / can be used to concatenate strings.
  ( and ) can be used to explicitly control the precedence order of operations.

  - Other Expressions:
  
  1. Variables, like $var.
  2. Function calls, like nth($list, 1) or var(--main-bg-color), which may call Sass core library functions or user-defined functions, or which may be compiled directly to CSS.
  3. Special functions, like calc(1px + 100%) or url(http://myapp.com/assets/logo.png), that have their own unique parsing rules.
  4. The parent selector, &.
  5. The value !important, which is parsed as an unquoted string.

# Special Functions

CSS defines many functions, and most of them work just fine with Sass’s normal function syntax. They’re parsed as function calls, resolved to plain CSS functions, and compiled as-is to CSS. There are a few exceptions, though, which have special syntax that can’t just be parsed as a SassScript expression. All special function calls return unquoted strings.

## url()

The url() function is commonly used in CSS, but its syntax is different than other functions: it can take either a quoted or unquoted URL. Because an unquoted URL isn’t a valid SassScript expression, Sass needs special logic to parse it.

If the url()‘s argument is a valid unquoted URL, Sass parses it as-is, although interpolation may also be used to inject SassScript values. If it’s not a valid unquoted URL—for example, if it contains variables or function calls—it’s parsed as a normal plain CSS function call.

```SCSS

$roboto-font-path: "../fonts/roboto";

@font-face {
    // This is parsed as a normal function call that takes a quoted string.
    src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2");

    font-family: "Roboto";
    font-weight: 100;
}

@font-face {
    // This is parsed as a normal function call that takes an arithmetic
    // expression.
    src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2");

    font-family: "Roboto";
    font-weight: 300;
}

@font-face {
    // This is parsed as an interpolated special function.
    src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2");

    font-family: "Roboto";
    font-weight: 400;
}



```

--- 

## calc(), clamp(), element(), progid:...(), and expression()

The calc(), clamp() and element() functions are defined in the CSS spec. Because calc()’s mathematical expressions conflict with Sass’s arithmetic, and element()’s IDs could be parsed as colors, they need special parsing.

expression() and functions beginning with progid: are legacy Internet Explorer features that use non-standard syntax. Although they’re no longer supported by recent browsers, Sass continues to parse them for backwards compatibility.

Sass allows any text in these function calls, including nested parentheses. Nothing is interpreted as a SassScript expression, with the exception that interpolation can be used to inject dynamic values.

```SCSS

.logo {
  $width: 800px;
  width: $width;
  position: absolute;
  left: calc(50% - #{$width / 2});
  top: 0;
}

```

--- 

## min() and max()

CSS added support for min() and max() functions in Values and Units Level 4, from where they were quickly adopted by Safari to support the iPhoneX. But Sass supported its own min() and max() functions long before this, and it needed to be backwards-compatible with all those existing stylesheets. This led for the need for extra-special syntactic cleverness.

If a min() or max() function call is valid plain CSS, it will be compiled to a CSS min() or max() call. “Plain CSS” includes nested calls to calc(), env(), var(), min(), or max(), as well as interpolation. As soon as any part of the call contains a SassScript feature like variables or function calls, though, it’s parsed as a call to Sass’s core min() or max() function instead.


```SCSS

$padding: 12px;

.post {
  // Since these max() calls don't use any Sass features other than
  // interpolation, they're compiled to CSS max() calls.
  padding-left: max(#{$padding}, env(safe-area-inset-left));
  padding-right: max(#{$padding}, env(safe-area-inset-right));
}

.sidebar {
  // Since these refer to a Sass variable without interpolation, they call
  // Sass's built-in max() function.
  padding-left: max($padding, 20px);
  padding-right: max($padding, 20px);
}

```

---- 

# Style Rules

Style rules are the foundation of Sass, just like they are for CSS. And they work the same way: you choose which elements to style with a selector, and declare properties that affect how those elements look.

```SCSS

.button {
  padding: 3px 10px;
  font-size: 12px;
  border-radius: 3px;
  border: 1px solid #e1e4e8;
}

```

---

## Nesting permalinkNesting

But Sass wants to make your life easier. Rather than repeating the same selectors over and over again, you can write one style rules inside another. Sass will automatically combine the outer rule’s selector with the inner rule’s.


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

Nested rules are super helpful, but they can also make it hard to visualize how much CSS you’re actually generating. The deeper you nest, the more bandwidth it takes to serve your CSS and the more work it takes the browser to render it. Keep those selectors shallow!

1. Selector Lists

Nested rules are clever about handling selector lists (that is, comma-separated selectors). Each complex selector (the ones between the commas) is nested separately, and then they’re combined back into a selector list.

```SCSS

.alert, .warning {
  ul, p {
    margin-right: 0;
    margin-left: 0;
    padding-bottom: 0;
  }
}

```

--- 

## Selector Combinators

You can nest selectors that use combinators as well. You can put the combinator at the end of the outer selector, at the beginning of the inner selector, or even all on its own in between the two.


```SCSS

ul > {
  li {
    list-style-type: none;
  }
}

h2 {
  + p {
    border-top: 1px solid gray;
  }
}

p {
  ~ {
    span {
      opacity: 0.8;
    }
  }
}

```

--- 

## Interpolation

You can use interpolation to inject values from expressions like variables and function calls into your selectors. This is particularly useful when you’re writing mixins, since it allows you to create selectors from parameters your users pass in.


```SCSS

@mixin define-emoji($name, $glyph) {
  span.emoji-#{$name} {
    font-family: IconFont;
    font-variant: normal;
    font-weight: normal;
    content: $glyph;
  }
}

@include define-emoji("women-holding-hands", "👭");

```

You can combine interpolation with the parent selector &, the @at-root rule, and selector functions to wield some serious power when dynamically generating selectors. For more information, see the parent selector documentation.

--- 

## Nesting SCSS

Many CSS properties start with the same prefix that acts as a kind of namespace. For example, font-family, font-size, and font-weight all start with font-. Sass makes this easier and less redundant by allowing property declarations to be nested. The outer property names are added to the inner, separated by a hyphen.

```SCSS

.enlarge {
  font-size: 14px;
  transition: {
    property: font-size;
    duration: 4s;
    delay: 2s;
  }

  &:hover { font-size: 36px; }
}

```

Some of these CSS properties have shorthand versions that use the namespace as the property name. For these, you can write both the shorthand value and the more explicit nested versions.


```SCSS

.info-page {
  margin: auto {
    bottom: 10px;
    top: 2px;
  }
}

```
 
--- 

## Hidden Declarations

Sometimes you only want a property declaration to show up some of the time. If a declaration’s value is null or an empty unquoted string, Sass won’t compile that declaration to CSS at all.


```SCSS

$rounded-corners: false;

.button {
  border: 1px solid black;
  border-radius: if($rounded-corners, 5px, null);
}

```

--- 

## Custom Properties

CSS custom properties, also known as CSS variables, have an unusual declaration syntax: they allow almost any text at all in their declaration values. What’s more, those values are accessible to JavaScript, so any value might potentially be relevant to the user. This includes values that would normally be parsed as SassScript.

Because of this, Sass parses custom property declarations differently than other property declarations. All tokens, including those that look like SassScript, are passed through to CSS as-is. The only exception is interpolation, which is the only way to inject dynamic values into a custom property.

```SCSS

$primary: #81899b;
$accent: #302e24;
$warn: #dfa612;

:root {
  --primary: #{$primary};
  --accent: #{$accent};
  --warn: #{$warn};

  // Even though this looks like a Sass variable, it's valid CSS so it's not
  // evaluated.
  --consumed-by-js: $primary;
}

```

--- 

# Parent Selector

The parent selector, &, is a special selector invented by Sass that’s used in nested selectors to refer to the outer selector. It makes it possible to re-use the outer selector in more complex ways, like adding a pseudo-class or adding a selector before the parent.

When a parent selector is used in an inner selector, it’s replaced with the corresponding outer selector. This happens instead of the normal nesting behavior.

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

--- 

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

--- 

## In SassScript

The parent selector can also be used within SassScript. It’s a special expression that returns the current parent selector in the same format used by selector functions: a comma-separated list (the selector list) that contains space-separated lists (the complex selectors) that contain unquoted strings (the compound selectors).


```SCSS

.main aside:hover,
.sidebar p {
  parent-selector: &;
  // => ((unquote(".main") unquote("aside:hover")),
  //     (unquote(".sidebar") unquote("p")))
}

```

If the & expression is used outside any style rules, it returns null. Since null is falsey, this means you can easily use it to determine whether a mixin is being called in a style rule or not.

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


--- 

# Placeholder Selectors

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

--- 

# Variables SCSS

Sass variables are simple: you assign a value to a name that begins with $, and then you can refer to that name instead of the value itself. But despite their simplicity, they're one of the most useful tools Sass brings to the table. Variables make it possible to reduce repetition, do complex math, configure libraries, and much more.

A variable declaration looks a lot like a property declaration: it’s written <variable>: <expression>. Unlike a property, which can only be declared in a style rule or at-rule, variables can be declared anywhere you want. To use a variable, just include it in a value.


```SCSS

$base-color: #c6538c;
$border-dark: rgba($base-color, 0.88);

.alert {
  border: 1px solid $border-dark;
}

```

--- 

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

--- 

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

--- 

## Flow Control Scope

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

--- 

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

--- 

# Interpolation SCSS

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

---  

## In SassScript SCSS

Interpolation can be used in SassScript to inject SassScript into unquoted strings. This is particularly useful when dynamically generating names (for example for animations), or when using slash-separated values. Note that interpolation in SassScript always returns an unquoted string.


```SCSS

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


--- 

## Quoted Strings

In most cases, interpolation injects the exact same text that would be used if the expression were used as a property value. But there is one exception: the quotation marks around quoted strings are removed (even if those quoted strings are in lists). This makes it possible to write quoted strings that contain syntax that’s not allowed in SassScript (like selectors) and interpolate them into style rules.

```SCSS

.example {
  unquoted: #{"string"};
}

```

While it’s tempting to use this feature to convert quoted strings to unquoted strings, it’s a lot clearer to use the string.unquote() function. Instead of #{$string}, write string.unquote($string)!


--- 

# At-Rules

Much of Sass’s extra functionality comes in the form of new at-rules it adds on top of CSS:

- @use loads mixins, functions, and variables from other Sass stylesheets, and combines CSS from multiple stylesheets together.

- @forward loads a Sass stylesheet and makes its mixins, functions, and variables available when your stylesheet is loaded with the @use rule.

- @import extends the CSS at-rule to load styles, mixins, functions, and variables from other stylesheets.

- @mixin and @include makes it easy to re-use chunks of styles.

- @function defines custom functions that can be used in SassScript expressions.

- @extend allows selectors to inherit styles from one another.

- @at-root puts styles within it at the root of the CSS document.

- @error causes compilation to fail with an error message.

- @warn prints a warning without stopping compilation entirely.

- @debug prints a message for debugging purposes.

- Flow control rules like @if, @each, @for, and @while control whether or how many times styles are emitted.

Sass also has some special behavior for plain CSS at-rules: they can contain interpolation, and they can be nested in style rules. Some of them, like @media and @supports, also allow SassScript to be used directly in the rule itself without interpolation.-

--- 

## @use

The @use rule loads mixins, functions, and variables from other Sass stylesheets, and combines CSS from multiple stylesheets together. Stylesheets loaded by @use are called "modules". Sass also provides built-in modules full of useful functions.

The simplest @use rule is written @use "<url>", which loads the module at the given URL. Any styles loaded this way will be included exactly once in the compiled CSS output, no matter how many times those styles are loaded.


```SCSS

// foundation/_code.scss
code {
  padding: .25em;
  line-height: 0;
}

```

```SCSS

// foundation/_lists.scss
ul, ol {
  text-align: left;

  & & {
    padding: {
      bottom: 0;
      left: 0;
    }
  }
}

```

```SCSS

// style.scss
@use 'foundation/code';
@use 'foundation/lists';

```

--- 

## Loading Members

You can access variables, functions, and mixins from another module by writing <namespace>.<variable>, <namespace>.<function>(), or @include <namespace>.<mixin>(). By default, the namespace is just the last component of the module’s URL.

Members (variables, functions, and mixins) loaded with @use are only visible in the stylesheet that loads them. Other stylesheets will need to write their own @use rules if they also want to access them. This helps make it easy to figure out exactly where each member is coming from. If you want to load members from many files at once, you can use the @forward rule to forward them all from one shared file.


```SCSS

// src/_corners.scss
$radius: 3px;

@mixin rounded {
  border-radius: $radius;
}


```

```SCSS

// style.scss
@use "src/corners";

.button {
  @include corners.rounded;
  padding: 5px + corners.$radius;
}

```

### Choosing a Namespace

By default, a module’s namespace is just the last component of its URL without a file extension. However, sometimes you might want to choose a different namespace—you might want to use a shorter name for a module you refer to a lot, or you might be loading multiple modules with the same filename. You can do this by writing @use "<url>" as <namespace>.

```SCSS

// src/_corners.scss
$radius: 3px;

@mixin rounded {
  border-radius: $radius;
}

```

```SCSS

// style.scss
@use "src/corners" as c;

.button {
  @include c.rounded;
  padding: 5px + c.$radius;
}

```

You can even load a module without a namespace by writing @use "<url>" as *. We recommend you only do this for stylesheets written by you, though; otherwise, they may introduce new members that cause name conflicts!


```SCSS

// src/_corners.scss
$radius: 3px;

@mixin rounded {
  border-radius: $radius;
}

```

```SCSS

// style.scss
@use "src/corners" as *;

.button {
  @include rounded;
  padding: 5px + $radius;
}

```

### Private Members

As a stylesheet author, you may not want all the members you define to be available outside your stylesheet. Sass makes it easy to define a private member by starting its name with either - or _. These members will work just like normal within the stylesheet that defines them, but they won’t be part of a module’s public API. That means stylesheets that load your module can’t see them!

```SCSS

// src/_corners.scss
$-radius: 3px;

@mixin rounded {
  border-radius: $-radius;
}

```

```SCSS

// style.scss
@use "src/corners";

.button {
  @include corners.rounded;

  // This is an error! $-radius isn't visible outside of `_corners.scss`.
  padding: 5px + corners.$-radius;
}

```

---
## Configuration

A stylesheet can define variables with the !default flag to make them configurable. To load a module with configuration, write @use <url> with (<variable>: <value>, <variable>: <value>). The configured values will override the variables’ default values.


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
--- 
## Finding the Module

It wouldn’t be any fun to write out absolute URLs for every stylesheet you load, so Sass’s algorithm for finding a module makes it a little easier. For starters, you don’t have to explicitly write out the extension of the file you want to load; @use "variables" will automatically load variables.scss, variables.sass, or variables.css.

All Sass implementations allow users to provide load paths: paths on the filesystem that Sass will look in when locating modules. For example, if you pass node_modules/susy/sass as a load path, you can use @use "susy" to load node_modules/susy/sass/susy.scss.

Modules will always be loaded relative to the current file first, though. Load paths will only be used if no relative file exists that matches the module’s URL. This ensures that you can’t accidentally mess up your relative imports when you add a new library.

--- 

## Loading CSS 

In addition to loading .sass and .scss files, Sass can load plain old .css files.

```SCSS

// code.css
code {
  padding: .25em;
  line-height: 0;
}

```

```SCSS

// style.scss
@use 'code';

```

CSS files loaded as modules don’t allow any special Sass features and so can’t expose any Sass variables, functions, or mixins. In order to make sure authors don’t accidentally write Sass in their CSS, all Sass features that aren’t also valid CSS will produce errors. Otherwise, the CSS will be rendered as-is. It can even be extended!

--- 

# @forward

The @forward rule loads a Sass stylesheet and makes its mixins, functions, and variables available when your stylesheet is loaded with the @use rule. It makes it possible to organize Sass libraries across many files, while allowing their users to load a single entrypoint file.

The rule is written @forward "<url>". It loads the module at the given URL just like @use, but it makes the public members of the loaded module available to users of your module as though they were defined directly in your module. Those members aren’t available in your module, though—if you want that, you’ll need to write a @use rule as well. Don’t worry, it’ll only load the module once!

If you do write both a @forward and a @use for the same module in the same file, it’s always a good idea to write the @forward first. That way, if your users want to configure the forwarded module, that configuration will be applied to the @forward before your @use loads it without any configuration.


```SCSS

// src/_list.scss
@mixin list-reset {
  margin: 0;
  padding: 0;
  list-style: none;
}

```

```SCSS

// bootstrap.scss
@forward "src/list";

```

```SCSS

// styles.scss
@use "bootstrap";

li {
  @include bootstrap.list-reset;
}

```

--- 

# Adding a Prefix

Because module members are usually used with a namespace, short and simple names are usually the most readable option. But those names might not make sense outside the module they’re defined in, so @forward has the option of adding an extra prefix to all the members it forwards.

This is written @forward "<url>" as <prefix>-*, and it adds the given prefix to the beginning of every mixin, function, and variable name forwarded by the module. For example, if the module defines a member named reset and it’s forwarded as list-*, downstream stylesheets will refer to it as list-reset.


```SCSS

// src/_list.scss
@mixin reset {
  margin: 0;
  padding: 0;
  list-style: none;
}

```

```SCSS

// bootstrap.scss
@forward "src/list" as list-*;

```

```SCSS

// styles.scss
@use "bootstrap";

li {
  @include bootstrap.list-reset;
}

```

## Controlling Visibility

The @forward rule can also load a module with configuration. This mostly works the same as it does for @use, with one addition: a @forward rule’s configuration can use the !default flag in its configuration. This allows a module to change the defaults of an upstream stylesheet while still allowing downstream stylesheets to override them.


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

// _opinionated.scss
@forward 'library' with (
  $black: #222 !default,
  $border-radius: 0.1rem !default
);

```

```SCSS

// style.scss
@use 'opinionated' with ($black: #333);

```

--- 

# @import

Sass extends CSS's @import rule with the ability to import Sass and CSS stylesheets, providing access to mixins, functions, and variables and combining multiple stylesheets' CSS together. Unlike plain CSS imports, which require the browser to make multiple HTTP requests as it renders your page, Sass imports are handled entirely during compilation.

Sass imports have the same syntax as CSS imports, except that they allow multiple imports to be separated by commas rather than requiring each one to have its own @import. Also, in the indented syntax, imported URLs aren’t required to have quotes.


The Sass team discourages the continued use of the @import rule. Sass will gradually phase it out over the next few years, and eventually remove it from the language entirely. Prefer the @use rule instead. (Note that only Dart Sass currently supports @use. Users of other implementations must use the @import rule instead.)


```SCSS

// foundation/_code.scss
code {
  padding: .25em;
  line-height: 0;
}

```


```SCSS

// foundation/_lists.scss
ul, ol {
  text-align: left;

  & & {
    padding: {
      bottom: 0;
      left: 0;
    }
  }
}

```

```SCSS

// style.scss
@import 'foundation/code', 'foundation/lists';

```

---

## Importing CSS

In addition to importing .sass and .scss files, Sass can import plain old .css files. The only rule is that the import must not explicitly include the .css extension, because that’s used to indicate a plain CSS @import.


```SCSS

// code.css
code {
  padding: .25em;
  line-height: 0;
}

```

```SCSS

// style.scss
@import 'code';

```

CSS files imported by Sass don’t allow any special Sass features. In order to make sure authors don’t accidentally write Sass in their CSS, all Sass features that aren’t also valid CSS will produce errors. Otherwise, the CSS will be rendered as-is. It can even be extended!

----

## @mixin and @include

Mixins are defined using the @mixin at-rule, which is written @mixin <name> { ... } or @mixin name(<arguments...>) { ... }. A mixin’s name can be any Sass identifier, and it can contain any statement other than top-level statements. They can be used to encapsulate styles that can be dropped into a single style rule; they can contain style rules of their own that can be nested in other rules or included at the top level of the stylesheet; or they can just serve to modify variables.

Mixins are included into the current context using the @include at-rule, which is written @include <name> or @include <name>(<arguments...>), with the name of the mixin being included.

```SCSS

@mixin reset-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

@mixin horizontal-list {
  @include reset-list;

  li {
    display: inline-block;
    margin: {
      left: -2px;
      right: 2em;
    }
  }
}

nav ul {
  @include horizontal-list;
}

```

---

## Arguments

Mixins can also take arguments, which allows their behavior to be customized each time they’re called. The arguments are specified in the @mixin rule after the mixin’s name, as a list of variable names surrounded by parentheses. The mixin must then be included with the same number of arguments in the form of SassScript expressions. The values of these expression are available within the mixin’s body as the corresponding variables.

```SCSS

@mixin rtl($property, $ltr-value, $rtl-value) {
  #{$property}: $ltr-value;

  [dir=rtl] & {
    #{$property}: $rtl-value;
  }
}

.sidebar {
  @include rtl(float, left, right);
}

```
Normally, every argument a mixin declares must be passed when that mixin is included. However, you can make an argument optional by defining a default value which will be used if that argument isn’t passed. Default values use the same syntax as variable declarations: the variable name, followed by a colon and a SassScript expression. This makes it easy to define flexible mixin APIs that can be used in simple or complex ways.

```SCSS

@mixin replace-text($image, $x: 50%, $y: 50%) {
  text-indent: -99999em;
  overflow: hidden;
  text-align: left;

  background: {
    image: $image;
    repeat: no-repeat;
    position: $x $y;
  }
}

.mail-icon {
  @include replace-text(url("/images/mail.svg"), 0);
}

```

### Passing Arbitrary Arguments 

Just like argument lists allow mixins to take arbitrary positional or keyword arguments, the same syntax can be used to pass positional and keyword arguments to a mixin. If you pass a list followed by ... as the last argument of an include, its elements will be treated as additional positional arguments. Similarly, a map followed by ... will be treated as additional keyword arguments. You can even pass both at once!

```SCSS

$form-selectors: "input.name", "input.address", "input.zip" !default;

@include order(150px, $form-selectors...);

```
### Content Blocks

In addition to taking arguments, a mixin can take an entire block of styles, known as a content block. A mixin can declare that it takes a content block by including the @content at-rule in its body. The content block is passed in using curly braces like any other block in Sass, and it’s injected in place of the @content rule.

```SCSS

@mixin hover {
  &:not([disabled]):hover {
    @content;
  }
}

.button {
  border: 1px solid black;
  @include hover {
    border-width: 2px;
  }
}

```
--- 

# @function

Functions allow you to define complex operations on SassScript values that you can re-use throughout your stylesheet. They make it easy to abstract out common formulas and behaviors in a readable way.

Functions are defined using the @function at-rule, which is written @function <name>(<arguments...>) { ... }. A function’s name can be any Sass identifier. It can only contain universal statements, as well as the @return at-rule which indicates the value to use as the result of the function call. Functions are called using the normal CSS function syntax.

```SCSS

@function pow($base, $exponent) {
  $result: 1;
  @for $_ from 1 through $exponent {
    $result: $result * $base;
  }
  @return $result;
}

.sidebar {
  float: left;
  margin-left: pow(4, 3) * 1px;
}

```

## Arguments

Arguments allow functions’ behavior to be customized each time they’re called. The arguments are specified in the @function rule after the function’s name, as a list of variable names surrounded by parentheses. The function must be called with the same number of arguments in the form of SassScript expressions. The values of these expression are available within the function’s body as the corresponding variables

### Optional Arguments

Normally, every argument a function declares must be passed when that function is included. However, you can make an argument optional by defining a default value which will be used if that arguments isn’t passed. Default values use the same syntax as variable declarations: the variable name, followed by a colon and a SassScript expression. This makes it easy to define flexible function APIs that can be used in simple or complex ways.

```SCSS

@function invert($color, $amount: 100%) {
  $inverse: change-color($color, $hue: hue($color) + 180);
  @return mix($inverse, $color, $amount);
}

$primary-color: #036;
.header {
  background-color: invert($primary-color, 80%);
}

```

### @return

The @return at-rule indicates the value to use as the result of calling a function. It’s only allowed within a @function body, and each @function must end with a @return.

When a @return is encountered, it immediately ends the function and returns its result. Returning early can be useful for handling edge-cases or cases where a more efficient algorithm is available without wrapping the entire function in an @else block.

```SCSS

@use "sass:string";

@function str-insert($string, $insert, $index) {
  // Avoid making new strings if we don't need to.
  @if string.length($string) == 0 {
    @return $insert;
  }

  $before: string.slice($string, 0, $index);
  $after: string.slice($string, $index);
  @return $before + $insert + $after;
}

```
--- 
# @extend

Sass’s @extend rule. It’s written @extend <selector>, and it tells Sass that one selector should inherit the styles of another.

```SCSS

.error {
  border: 1px #f00;
  background-color: #fdd;

  &--serious {
    @extend .error;
    border-width: 3px;
  }
}

```
When one class extends another, Sass styles all elements that match the extender as though they also match the class being extended. When one class selector extends another, it works exactly as though you added the extended class to every element in your HTML that already had the extending class. You can just write class="error--serious", and Sass will make sure it’s styled as though it had class="error" as well.

Of course, selectors aren’t just used on their own in style rules. Sass knows to extend everywhere the selector is used. This ensures that your elements are styled exactly as if they matched the extended selector.

```SCSS

.error:hover {
  background-color: #fee;
}

.error--serious {
  @extend .error;
  border-width: 3px;
}

```
# @error

When writing mixins and functions that take arguments, you usually want to ensure that those arguments have the types and formats your API expects. If they aren't, the user needs to be notified and your mixin/function needs to stop running.

Sass makes this easy with the @error rule, which is written @error <expression>. It prints the value of the expression (usually a string) along with a stack trace indicating how the current mixin or function was called. Once the error is printed, Sass stops compiling the stylesheet and tells whatever system is running it that an error occurred.

```SCSS

@mixin reflexive-position($property, $value) {
  @if $property != left and $property != right {
    @error "Property #{$property} must be either left or right.";
  }

  $left-value: if($property == right, initial, $value);
  $right-value: if($property == right, $value, initial);

  left: $left-value;
  right: $right-value;
  [dir=rtl] & {
    left: $right-value;
    right: $left-value;
  }
}

.sidebar {
  @include reflexive-position(top, 12px);
  //       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  // Error: Property top must be either left or right.
}

```

# @warn

When writing mixins and functions, you may want to discourage users from passing certain arguments or certain values. They may be passing legacy arguments that are now deprecated, or they may be calling your API in a way that’s not quite optimal.

The @warn rule is designed just for that. It’s written @warn <expression> and it prints the value of the expression (usually a string) for the user, along with a stack trace indicating how the current mixin or function was called. Unlike the @error rule, though, it doesn’t stop Sass entirely.

```SCSS

$known-prefixes: webkit, moz, ms, o;

@mixin prefix($property, $value, $prefixes) {
  @each $prefix in $prefixes {
    @if not index($known-prefixes, $prefix) {
      @warn "Unknown prefix #{$prefix}.";
    }

    -#{$prefix}-#{$property}: $value;
  }
  #{$property}: $value;
}

.tilt {
  // Oops, we typo'd "webkit" as "wekbit"!
  @include prefix(transform, rotate(15deg), wekbit ms);
}

```
--- 

# @debug

```SCSS

Sometimes it’s useful to see the value of a variable or expression while you’re developing your stylesheet. That’s what the @debug rule is for: it’s written @debug <expression>, and it prints the value of that expression, along with the filename and line number.

```


```SCSS

@mixin inset-divider-offset($offset, $padding) {
  $divider-offset: (2 * $padding) + $offset;
  @debug "divider offset: #{$divider-offset}";

  margin-left: $divider-offset;
  width: calc(100% - #{$divider-offset});
}

```

--- 

# @at-root

The @at-root rule is usually written @at-root <selector> { ... } and causes everything within it to be emitted at the root of the document instead of using the normal nesting. It's most often used when doing advanced nesting with the SassScript parent selector and selector functions.

For example, suppose you want to write a selector that matches the outer selector and an element selector. You could write a mixin like this one that uses the selector.unify() function to combine & with a user’s selector.

## Beyond Style Rules

On its own, @at-root only gets rid of style rules. Any at-rules like @media or @supports will be left in. If this isn’t what you want, though, you can control exactly what it includes or includes using syntax like media query features, written @at-root (with: <rules...>) { ... } or @at-root (without: <rules...>) { ... }. The (without: ...) query tells Sass which rules should be excluded; the (with: ...) query excludes all rules except those that are listed.

```SCSS

@media print {
  .page {
    width: 8in;

    @at-root (without: media) {
      color: #111;
    }

    @at-root (with: rule) {
      font-size: 1.2em;
    }
  }
}

```

In addition to the names of at-rules, there are two special values that can be used in queries:

- rule refers to style rules. For example, @at-root (with: rule) excludes all at-rules but preserves style rules.

- all refers to all at-rules and style rules should be excluded.

--- 

# @if and @else

The @if rule is written @if <expression> { ... }, and it controls whether or not its block gets evaluated (including emitting any styles as CSS). The expression usually returns either true or false—if the expression returns true, the block is evaluated, and if the expression returns false it’s not.

```SCSS

@mixin avatar($size, $circle: false) {
  width: $size;
  height: $size;

  @if $circle {
    border-radius: $size / 2;
  }
}

.square-av {
  @include avatar(100px, $circle: false);
}
.circle-av {
  @include avatar(100px, $circle: true);
}
@else permalink

```

## @else

An @if rule can optionally be followed by an @else rule, written @else { ... }. This rule’s block is evaluated if the @if expression returns false.

```SCSS

$light-background: #f2ece4;
$light-text: #036;
$dark-background: #6b717f;
$dark-text: #d2e1dd;

@mixin theme-colors($light-theme: true) {
  @if $light-theme {
    background-color: $light-background;
    color: $light-text;
  } @else {
    background-color: $dark-background;
    color: $dark-text;
  }
}

.banner {
  @include theme-colors($light-theme: true);
  body.dark & {
    @include theme-colors($light-theme: false);
  }
}

```

## @else if 

You can also choose whether to evaluate an @else rule’s block by writing it @else if <expression> { ... }. If you do, the block is evaluated only if the preceding @if‘s expression returns false and the @else if’s expression returns true.

In fact, you can to chain as many @else ifs as you want after an @if. The first block in the chain whose expression returns true will be evaluated, and no others. If there’s a plain @else at the end of the chain, its block will be evaluated if every other block fails.

```SCSS

@use "sass:math";

@mixin triangle($size, $color, $direction) {
  height: 0;
  width: 0;

  border-color: transparent;
  border-style: solid;
  border-width: math.div($size, 2);

  @if $direction == up {
    border-bottom-color: $color;
  } @else if $direction == right {
    border-left-color: $color;
  } @else if $direction == down {
    border-top-color: $color;
  } @else if $direction == left {
    border-right-color: $color;
  } @else {
    @error "Unknown direction #{$direction}.";
  }
}

.next {
  @include triangle(5px, black, right);
}

```

## Truthiness and Falsiness

 Anywhere true or false are allowed, you can use other values as well. The values false and null are falsey, which means Sass considers them to indicate falsehood and cause conditions to fail. Every other value is considered truthy, so Sass considers them to work like true and cause conditions to succeed.

For example, if you want to check if a string contains a space, you can just write string.index($string, " "). The string.index() function returns null if the string isn’t found and a number otherwise.

## @each

The @each rule makes it easy to emit styles or evaluate code for each element of a list or each pair in a map. It’s great for repetitive styles that only have a few variations between them. It’s usually written @each <variable> in <expression> { ... }, where the expression returns a list. The block is evaluated for each element of the list in turn, which is assigned to the given variable name.


```SCSS

$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}


```


## With Maps

You can also use @each to iterate over every key/value pair in a map by writing it @each <variable>, <variable> in <expression> { ... }. The key is assigned to the first variable name, and the element is assigned to the second.

```SCSS

$icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

@each $name, $glyph in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
  }
}



```

## Destructuring

If you have a list of lists, you can use @each to automatically assign variables to each of the values from the inner lists by writing it @each <variable...> in <expression> { ... }. This is known as destructuring, since the variables match the structure of the inner lists. Each variable name is assigned to the value at the corresponding position in the list, or null if the list doesn’t have enough values.


```SCSS

$icons:
  "eye" "\f112" 12px,
  "start" "\f12e" 16px,
  "stop" "\f12f" 10px;

@each $name, $glyph, $size in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
    font-size: $size;
  }
}


```
--- 

# @for


The @for rule, written @for <variable> from <expression> to <expression> { ... } or @for <variable> from <expression> through <expression> { ... }, counts up or down from one number (the result of the first expression) to another (the result of the second) and evaluates a block for each number in between. Each number along the way is assigned to the given variable name. If to is used, the final number is excluded; if through is used, it's included.


```SCSS

$base-color: #036;

@for $i from 1 through 3 {
  ul:nth-child(3n + #{$i}) {
    background-color: lighten($base-color, $i * 5%);
  }
}


```

--- 

# @while

The @while rule, written @while <expression> { ... }, evaluates its block if its expression returns true. Then, if its expression still returns true, it evaluates its block again. This continues until the expression finally returns false.

```SCSS

@use "sass:math";

/// Divides `$value` by `$ratio` until it's below `$base`.
@function scale-below($value, $base, $ratio: 1.618) {
  @while $value > $base {
    $value: math.div($value, $ratio);
  }
  @return $value;
}

$normal-font-size: 16px;
sup {
  font-size: scale-below(20px, 16px);
}

```

## Truthiness and Falsiness 



```SCSS

Anywhere true or false are allowed, you can use other values as well. The values false and null are falsey, which means Sass considers them to indicate falsehood and cause conditions to fail. Every other value is considered truthy, so Sass considers them to work like true and cause conditions to succeed.

For example, if you want to check if a string contains a space, you can just write string.index($string, " "). The string.index() function returns null if the string isn’t found and a number otherwise.

```

--- 

# CSS At-Rules

Sass supports all the at-rules that are part of CSS proper. To stay flexible and forwards-compatible with future versions of CSS, Sass has general support that covers almost all at-rules by default. A CSS at-rule is written @<name> <value>, @<name> { ... }, or @<name> <value> { ... }. The name must be an identifier, and the value (if one exists) can be pretty much anything. Both the name and the value can contain interpolation.

```SCSS

@namespace svg url(http://www.w3.org/2000/svg);

@font-face {
  font-family: "Open Sans";
  src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2");
}

@counter-style thumbs {
  system: cyclic;
  symbols: "\1F44D";
}

```
 ## @media

The @media rule does all of the above and more. In addition to allowing interpolation, it allows SassScript expressions to be used directly in the feature queries.

```SCSS

$layout-breakpoint-small: 960px;

@media (min-width: $layout-breakpoint-small) {
  .hide-extra-small {
    display: none;
  }
}

```

When possible, Sass will also merge media queries that are nested within one another to make it easier to support browsers that don’t yet natively support nested @media rules.

```SCSS

@media (hover: hover) {
  .button:hover {
    border: 2px solid black;

    @media (color) {
      border-color: #036;
    }
  }
}


```

--- 

## @supports

The @supports rule also allows SassScript expressions to be used in the declaration queries.


```SCSS

@mixin sticky-position {
  position: fixed;
  @supports (position: sticky) {
    position: sticky;
  }
}

.banner {
  @include sticky-position;
}

```

## @keyframes

The @keyframes rule works just like a general at-rule, except that its child rules must be valid keyframe rules (<number>%, from, or to) rather than normal selectors.

```SCSS

@keyframes slide-in {
  from {
    margin-left: 100%;
    width: 300%;
  }

  70% {
    margin-left: 90%;
    width: 150%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}

```

--- 
# Numbers

Numbers in Sass have two components: the number itself, and its units. For example, in 16px the number is 16 and the unit is px. Numbers can have no units, and they can have complex units. See Units below for more details.

```SCSS

@debug 100; // 100
@debug 0.8; // 0.8
@debug 16px; // 16px
@debug 5px * 2px; // 10px*px (read "square pixels")

```

## Units

Sass has powerful support for manipulating units based on how real-world unit calculations work. When two numbers are multiplied, their units are multiplied as well. When one number is divided by another, the result takes its numerator units from the first number and its denominator units from the second. A number can have any number of units in the numerator and/or denominator.

```SCSS

@debug 4px * 6px; // 24px*px (read "square pixels")
@debug math.div(5px, 2s); // 2.5px/s (read "pixels per second")

// 3.125px*deg/s*em (read "pixel-degrees per second-em")
@debug 5px * math.div(math.div(30deg, 2s), 24em); 

$degrees-per-second: math.div(20deg, 1s);
@debug $degrees-per-second; // 20deg/s
@debug math.div(1, $degrees-per-second); // 0.05s/deg

```

## Precision 

Sass numbers support up to 10 digits of precision after the decimal point. This means a few different things:

Only the first ten digits of a number after the decimal point will be included in the generated CSS.

Operations like == and >= will consider two numbers equivalent if they’re the same up to the tenth digit after the decimal point.

If a number is less than 0.0000000001 away from an integer, it’s considered to be an integer for the purposes of functions like list.nth() that require integer arguments.


```SCSS

@debug 0.012345678912345; // 0.0123456789
@debug 0.01234567891 == 0.01234567899; // true
@debug 1.00000000009; // 1
@debug 0.99999999991; // 1

```

# Strings

Strings are sequences of characters (specifically Unicode code points). Sass supports two kinds of strings whose internal structure is the same but which are rendered differently: quoted strings, like "Helvetica Neue", and unquoted strings (also known as identifiers), like bold. Together, these cover the different kinds of text that appear in CSS.

You can convert a quoted string to an unquoted string using the string.unquote() function, and you can convert an unquoted string to a quoted string using the string.quote() function.

```SCSS

@use "sass:string";

@debug string.unquote(".widget:hover"); // .widget:hover
@debug string.quote(bold); // "bold"

```

--- 

## Escapes

All Sass strings support the standard CSS escape codes:

Any character other than a letter from A to F or a number from 0 to 9 (even a newline!) can be included as part of a string by writing \ in front of it.

Any character can be included as part of a string by writing \ followed by its Unicode code point number written in hexadecimal. You can optionally include a space after the code point number to indicate where the Unicode number ends.

```SCSS

@debug "\""; // '"'
@debug \.widget; // \.widget
@debug "\a"; // "\a" (a string containing only a newline)
@debug "line1\a line2"; // "line1\a line2"
@debug "Nat + Liz \1F46D"; // "Nat + Liz 👭"

```

## Quoted 

Quoted strings are written between either single or double quotes, as in "Helvetica Neue". They can contain interpolation, as well as any unescaped character except for:

\, which can be escaped as \\;
' or ", whichever was used to define that string, which can be escaped as \' or \";
newlines, which can be escaped as \a (including a trailing space).
Quoted strings are guaranteed to be compiled to CSS strings that have the same contents as the original Sass strings. The exact format may vary based on the implementation or configuration—a string containing a double quote may be compiled to "\"" or '"', and a non-ASCII character may or may not be escaped. But that should be parsed the same in any standards-compliant CSS implementation, including all browsers.

```SCSS

@debug "Helvetica Neue"; // "Helvetica Neue"
@debug "C:\\Program Files"; // "C:\\Program Files"
@debug "\"Don't Fear the Reaper\""; // "\"Don't Fear the Reaper\""
@debug "line1\a line2"; // "line1\a line2"

$roboto-variant: "Mono";
@debug "Roboto #{$roboto-variant}"; // "Roboto Mono"

```

## String Indexes 

Sass has a number of string functions that take or return numbers, called indexes, that refer to the characters in a string. The index 1 indicates the first character of the string. Note that this is different than many programming languages where indexes start at 0! Sass also makes it easy to refer to the end of a string. The index -1 refers to the last character in a string, -2 refers to the second-to-last, and so on.

```SCSS

@use "sass:string";

@debug string.index("Helvetica Neue", "Helvetica"); // 1
@debug string.index("Helvetica Neue", "Neue"); // 11
@debug string.slice("Roboto Mono", -4); // "Mono"

```



# Colors

Sass has built-in support for color values. Just like CSS colors, they represent points in the sRGB color space, although many Sass color functions operate using HSL coordinates (which are just another way of expressing sRGB colors). Sass colors can be written as hex codes (#f2ece4 or #b37399aa), CSS color names (midnightblue, transparent), or the functions rgb(), rgba(), hsl(), and hsla().

```SCSS

@debug #f2ece4; // #f2ece4
@debug #b37399aa; // rgba(179, 115, 153, 67%)
@debug midnightblue; // #191970
@debug rgb(204, 102, 153); // #c69
@debug rgba(107, 113, 127, 0.8); // rgba(107, 113, 127, 0.8)
@debug hsl(228, 7%, 86%); // #dadbdf
@debug hsla(20, 20%, 85%, 0.7); // rgb(225, 215, 210, 0.7)

```

CSS supports many different formats that can all represent the same color: its name, its hex code, and functional notation. Which format Sass chooses to compile a color to depends on the color itself, how it was written in the original stylesheet, and the current output mode. Because it can vary so much, stylesheet authors shouldn’t rely on any particular output format for colors they write.

Sass supports many useful color functions that can be used to create new colors based on existing ones by mixing colors together or scaling their hue, saturation, or lightness.


```SCSS

$venus: #998099;

@debug scale-color($venus, $lightness: +15%); // #a893a8
@debug mix($venus, midnightblue); // #594d85

```

---

# Lists

Lists contain a sequence of other values. In Sass, elements in lists can be separated by commas (Helvetica, Arial, sans-serif), spaces (10px 15px 0 0), or slashes as long as it’s consistent within the list. Unlike most other languages, lists in Sass don’t require special brackets; any expressions separated with spaces or commas count as a list. However, you’re allowed to write lists with square brackets ([line1 line2]), which is useful when using grid-template-columns.

Sass lists can contain one or even zero elements. A single-element list can be written either (<expression>,) or [<expression>], and a zero-element list can be written either () or []. Also, all list functions will treat individual values that aren’t in lists as though they’re lists containing that value, which means you rarely need to explicitly create single-element lists.

## Slash-Separated Lists

Lists in Sass can be separated by slashes, to represent values like the font: 12px/30px shorthand for setting font-size and line-height or the hsl(80 100% 50% / 0.5) syntax for creating a color with a given opacity value. However, slash-separated lists can’t currently be written literally. Sass historically used the / character to indicate division, so while existing stylesheets transition to using math.div() slash-separated lists can only be written using list.slash().


## Using Lists

Many of these functions take or return numbers, called indexes, that refer to the elements in a list. The index 1 indicates the first element of the list. Note that this is different than many programming languages where indexes start at 0! Sass also makes it easy to refer to the end of a list. The index -1 refers to the last element in a list, -2 refers to the second-to-last, and so on.

Access an Element permalinkAccess an Element
Lists aren’t much use if you can’t get values out of them. You can use the list.nth($list, $n) function to get the element at a given index in a list. The first argument is the list itself, and the second is the index of the value you want to get out.

```SCSS

@debug list.nth(10px 12px 16px, 2); // 12px
@debug list.nth([line1, line2, line3], -1); // line3

```

### Do Something for Every Element

This doesn’t actually use a function, but it’s still one of the most common ways to use lists. The @each rule evaluates a block of styles for each element in a list, and assigns that element to a variable.

```SCSS

$sizes: 40px, 50px, 80px;

@each $size in $sizes {
  .icon-#{$size} {
    font-size: $size;
    height: $size;
    width: $size;
  }
}


```

### Add to a List 

It’s also useful to add elements to a list. The list.append($list, $val) function takes a list and a value, and returns a copy of the list with the value added to the end. Note that because Sass lists are immutable, it doesn’t modify the original list.


```SCSS

@debug append(10px 12px 16px, 25px); // 10px 12px 16px 25px
@debug append([col1-line1], col1-line2); // [col1-line1, col1-line2]

```

### Find an Element in a List

If you need to check if an element is in a list or figure out what index it’s at, use the list.index($list, $value) function. This takes a list and a value to locate in that list, and returns the index of that value.

```SCSS

@debug list.index(1px solid red, 1px); // 1
@debug list.index(1px solid red, solid); // 2
@debug list.index(1px solid red, dashed); // null

```

If the value isn’t in the list at all, list.index() returns null. Because null is falsey, you can use list.index() with @if or if() to check whether a list does or doesn’t contain a given value.


```SCSS

@use "sass:list";

$valid-sides: top, bottom, left, right;

@mixin attach($side) {
  @if not list.index($valid-sides, $side) {
    @error "#{$side} is not a valid side. Expected one of #{$valid-sides}.";
  }

  // ...
}

```

## Immutability

Lists in Sass are immutable, which means that the contents of a list value never changes. Sass’s list functions all return new lists rather than modifying the originals. Immutability helps avoid lots of sneaky bugs that can creep in when the same list is shared across different parts of the stylesheet.

You can still update your state over time by assigning new lists to the same variable, though. This is often used in functions and mixins to collect a bunch of values into one list.

```SCSS

@use "sass:list";
@use "sass:map";

$prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms);

@function prefixes-for-browsers($browsers) {
  $prefixes: ();
  @each $browser in $browsers {
    $prefixes: list.append($prefixes, map.get($prefixes-by-browser, $browser));
  }
  @return $prefixes;
}

@debug prefixes-for-browsers("firefox" "ie"); // moz ms

```

## Argument Lists

When you declare a mixin or function that takes arbitrary arguments, the value you get is a special list known as an argument list. It acts just like a list that contains all the arguments passed to the mixin or function, with one extra feature: if the user passed keyword arguments, they can be accessed as a map by passing the argument list to the meta.keywords() function.

```SCSS

@use "sass:meta";

@mixin syntax-colors($args...) {
  @debug meta.keywords($args);
  // (string: #080, comment: #800, variable: #60b)

  @each $name, $color in meta.keywords($args) {
    pre span.stx-#{$name} {
      color: $color;
    }
  }
}

@include syntax-colors(
  $string: #080,
  $comment: #800,
  $variable: #60b,
)

```

---

# Maps

Maps in Sass hold pairs of keys and values, and make it easy to look up a value by its corresponding key. They’re written (<expression>: <expression>, <expression>: <expression>). The expression before the : is the key, and the expression after is the value associated with that key. The keys must be unique, but the values may be duplicated. Unlike lists, maps must be written with parentheses around them. A map with no pairs is written ().
Maps allow any Sass values to be used as their keys. The == operator is used to determine whether two keys are the same.

## Using Maps

Since maps aren’t valid CSS values, they don’t do much of anything on their own. That’s why Sass provides a bunch of functions to create maps and access the values they contain.

### Look Up a Value

Maps are all about associating keys and values, so naturally there’s a way to get the value associated with a key: the map.get($map, $key) function! This function returns the value in the map associated with the given key. It returns null if the map doesn’t contain the key.

```SCSS

$font-weights: ("regular": 400, "medium": 500, "bold": 700);

@debug map.get($font-weights, "medium"); // 500
@debug map.get($font-weights, "extra-bold"); // null

```

### Do Something for Every Pair 

This doesn’t actually use a function, but it’s still one of the most common ways to use maps. The @each rule evaluates a block of styles for each key/value pair in a map. The key and the value are assigned to variables so they can easily be accessed in the block.

```SCSS

$icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

@each $name, $glyph in $icons {
  .icon-#{$name}:before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
  }
}

```

### Add to a Map

It’s also useful to add new pairs to a map, or to replace the value for an existing key. The map.set($map, $key, $value) function does this: it returns a copy of $map with the value at $key set to $value.

```SCSS

@use "sass:map";

$font-weights: ("regular": 400, "medium": 500, "bold": 700);

@debug map.set($font-weights, "extra-bold", 900);
// ("regular": 400, "medium": 500, "bold": 700, "extra-bold": 900)
@debug map.set($font-weights, "bold", 900);
// ("regular": 400, "medium": 500, "bold": 900)

```

Instead of setting values one-by-one, you can also merge two existing maps using [map.merge($map1, $map2)][].

```SCSS

@use "sass:map";

$light-weights: ("lightest": 100, "light": 300);
$heavy-weights: ("medium": 500, "bold": 700);

@debug map.merge($light-weights, $heavy-weights);
// ("lightest": 100, "light": 300, "medium": 500, "bold": 700)

```

### Immutability

Maps in Sass are immutable, which means that the contents of a map value never changes. Sass’s map functions all return new maps rather than modifying the originals. Immutability helps avoid lots of sneaky bugs that can creep in when the same map is shared across different parts of the stylesheet.

You can still update your state over time by assigning new maps to the same variable, though. This is often used in functions and mixins to track configuration in a map.

```SCSS

@use "sass:map";

$prefixes-by-browser: ("firefox": moz, "safari": webkit, "ie": ms);

@mixin add-browser-prefix($browser, $prefix) {
  $prefixes-by-browser: map.merge($prefixes-by-browser, ($browser: $prefix)) !global;
}

@include add-browser-prefix("opera", o);
@debug $prefixes-by-browser;
// ("firefox": moz, "safari": webkit, "ie": ms, "opera": o)

```

---

# Booleans

Booleans are the logical values true and false. In addition their literal forms, booleans are returned by equality and relational operators, as well as many built-in functions like math.comparable() and map.has-key().

```SCSS

@use "sass:math";

@debug 1px == 2px; // false
@debug 1px == 1px; // true
@debug 10px < 3px; // false
@debug math.comparable(100px, 3in); // true

```

You can work with booleans using boolean operators. The and operator returns true if both sides are true, and the or operator returns true if either side is true. The not operator returns the opposite of a single boolean value.

```SCSS

@debug true and true; // true
@debug true and false; // false

@debug true or false; // true
@debug false or false; // false

@debug not true; // false
@debug not false; // true

```

## Using Booleans

You can use booleans to choose whether or not to do various things in Sass. The @if rule evaluates a block of styles if its argument is true:

```SCSS

@mixin avatar($size, $circle: false) {
  width: $size;
  height: $size;

  @if $circle {
    border-radius: $size / 2;
  }
}

.square-av {
  @include avatar(100px, $circle: false);
}
.circle-av {
  @include avatar(100px, $circle: true);
}


```

The if() function returns one value if its argument is true and another if its argument is false:


```SCSS

@debug if(true, 10px, 30px); // 10px
@debug if(false, 10px, 30px); // 30px

```

## Truthiness and Falsiness

Anywhere true or false are allowed, you can use other values as well. The values false and null are falsey, which means Sass considers them to indicate falsehood and cause conditions to fail. Every other value is considered truthy, so Sass considers them to work like true and cause conditions to succeed.

For example, if you want to check if a string contains a space, you can just write string.index($string, " "). The string.index() function returns null if the string isn’t found and a number otherwise.

---


# null

The value null is the only value of its type. It represents the absence of a value, and is often returned by functions to indicate the lack of a result.

```SCSS

@use "sass:map";
@use "sass:string";

@debug string.index("Helvetica Neue", "Roboto"); // null
@debug map.get(("large": 20px), "small"); // null
@debug &; // null

```

If a list contains a null, that null is omitted from the generated CSS.

```SCSS

$fonts: ("serif": "Helvetica Neue", "monospace": "Consolas");

h3 {
  font: 18px bold map-get($fonts, "sans");
}

```

If a property value is null, that property is omitted entirely.

```SCSS

$fonts: ("serif": "Helvetica Neue", "monospace": "Consolas");

h3 {
  font: {
    size: 18px;
    weight: bold;
    family: map-get($fonts, "sans");
  }
}

```

null is also falsey, which means it counts as false for any rules or operators that take booleans. This makes it easy to use values that can be null as conditions for @if and if().



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

---

# Functions

Functions can be values too! You can’t directly write a function as a value, but you can pass a function’s name to the [meta.get-function() function][] to get it as a value. Once you have a function value, you can pass it to the meta.call() function to call it. This is useful for writing higher-order functions that call other functions.

```SCSS

@use "sass:list";
@use "sass:meta";
@use "sass:string";

/// Return a copy of $list with all elements for which $condition returns `true`
/// removed.
@function remove-where($list, $condition) {
  $new-list: ();
  $separator: list.separator($list);
  @each $element in $list {
    @if not meta.call($condition, $element) {
      $new-list: list.append($new-list, $element, $separator: $separator);
    }
  }
  @return $new-list;
}

$fonts: Tahoma, Geneva, "Helvetica Neue", Helvetica, Arial, sans-serif;

content {
  @function contains-helvetica($string) {
    @return string.index($string, "Helvetica");
  }
  font-family: remove-where($fonts, meta.get-function("contains-helvetica"));
}

```

--- 

# Operators

Sass supports a handful of useful operators for working with different values. These include the standard mathematical operators like + and *, as well as operators for various other types:

== and != are used to check if two values are the same.

+, -, *, /, and % have their usual mathematical meaning for numbers, with special behaviors for units that matches the use of units in scientific math.

<, <=, >, and >= check whether two numbers are greater or less than one another.

and, or, and not have the usual boolean behavior. Sass considers every value “true” except for false and null.

+, -, and / can be used to concatenate strings.

## Order of Operations


```SCSS



```

```SCSS



```



```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```



```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```




```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```




```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```




```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```




```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```




```SCSS



```

```SCSS



```

```SCSS



```

```SCSS



```


