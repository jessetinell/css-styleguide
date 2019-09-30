## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formatting

* Prefer dashes over camelCasing in class names.
  - Underscores are okay if you are using BEM
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### Atomic Design, ITCSS, BEM

We encourage some combinations of BEM, ITCSS and Atomic design for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets


**Atomic design** is a methodology for creating design systems. 

  * Short demo of Atomic design [Play video](http://patternlab.io/assets/atomic-design.mp4)
  * [Post by Brad Frost](http://bradfrost.com/blog/post/atomic-web-design/)
  * A CSS Architecture Worth Loving [BEM & Atomic](https://www.lullabot.com/articles/bem-atomic-design-a-css-architecture-worth-loving)
  
  
**ITCSS** is a scalable and maintainable css architecture

  * [What is ITCSS?](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/)
  

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We recommend a variant of BEM with hyphen-separated-lowercase “blocks”.

**Example**

```html
<div class="listing-card">
  <h1 class="listing-card__title>Nice day in Stockholm, eh?</h1>
  <div class="listing-card__content">
    <p>With an average temperature of -35°c the Swedes are rushing to get the best spot at the beach. While having a fika, eating surströmming and shopping at Ikea inbetween.</p>
  </div>
  <a href="#" class="listing-card__link">Read more</a>
</div>
```

```css
/* listing-card.scss */
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` is the “block” and represents the higher-level component
  * `.listing-card__title` is an “element” and represents a descendant of `.listing-card` that helps compose the block as a whole.
  * `.listing-card--featured` is a “modifier” and represents a different state or variation on the `.listing-card` block.


[Putting it together with Atomic, ITCSS & BEM](https://www.silverstripe.org/blog/better-css-putting-it-together-with-atomic-itcss-and-bem/)


### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.


### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .button-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .button-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .button {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).


### Variable naming

For naming color-variables we recommend using "real" names generated by [Name Those Colors](http://www.noxproductions.se/namethosecolors/) with an inline-comment describing the color.

**Bad**

```css
$yellow: #dde80c;
$light-yellow: #eefa03;
$lighter-yellow: #edf728;
```

**Good**

```css
$bitter-lemon: #dde80c; // Dark yellow
$chartreuse-yellow: #eefa03; // Yellow
$golden-fizz: #edf728; // Light yellow
```


### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.


## Media queries
Media queries for a specific component should be set in the component's file. 

**Bad**

```scss
/* listing-card.scss */
.listing-card {
  float:left;
}


/* all-media-queries.scss */
@media (max-width: 600px) {
  .listing-card {
    float:none;
  }
}
```

**Good**

```scss
/* listing-card.scss */
.listing-card {
  float:left;
}

@media (max-width: 600px) {
  .listing-card {
    float:none;
  }
}
```
