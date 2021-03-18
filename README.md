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

## Skills covered

- SASS
- Front-end Development


### Variables

Think of variables as a way to store information that you want to reuse throughout your stylesheet. You can store things like colors, font stacks, or any CSS value you think you'll want to reuse. Sass uses the $ symbol to make something a variable. Here's an example:

```SCSS

// CSS
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}


// SCSS
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

```
