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
