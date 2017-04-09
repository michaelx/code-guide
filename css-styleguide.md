# CSS Styleguide

Best practices and guidelines for writing CSS with approachable formatting, syntax, and more.

## General

- Don’t overuse preprocessor features. Keep it as simple as it can be.

## Syntax

- Use soft-tabs with two spaces. Spaces are the only way to guarantee code renders the same in any person’s environment.
- When grouping selectors, keep individual selectors to a single line.
- Include one space before the opening brace of declaration blocks for legibility.
- Place closing braces `}` of declaration blocks on a new line.
- Include one space after `:` for each declaration.
- Each declaration should appear on its own line for more accurate error reporting.
- End all declarations with a semi-colon. The last declaration's is optional, but your code is more error prone without it.
- Comma-separated property values _(not multiple color values)_ should include a space after each comma (e.g., `font-family: Helvetica, Arial, sans-serif`).
- Lowercase all hex values, e.g., `#fff`. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
- Use shorthand hex values where available, e.g., `#fff` instead of `#ffffff`.
- Quote attribute values in selectors, e.g., `input[type="text"]`. [They’re only optional in some cases](http://mathiasbynens.be/notes/unquoted-attribute-values#css), and it’s a good practice for consistency.
- Put spaces before and after child selector `div > span` instead of `div>span`.
- Use unit-less line-height as it’s just a multiplier of the font-size, e.g., `line-height: 1.5` instead of `line-height: 1.5em`.

### Don’ts

- Don't include spaces after commas _within_ `rgb()`, `rgba()`, `hsl()`, `hsla()`, or `rect()` values. This helps differentiate multiple color values (comma, no space) from multiple property values (comma with space).
- Don’t use leading zeros for decimal values, write `opacity: .4;` instead of `opacity: 0.4;`.
- Avoid specifying units for zero values, e.g., `margin: 0;` instead of `margin: 0px;`.
- Avoid using HTML tags in CSS selectors. E.g. Prefer `.o-modal {}` over `div.o-modal {}`. Always prefer using a class over HTML tags (with some exceptions like CSS resets).
- Don't use ids in selectors. `#header` is overly specific compared to, for example `.header` and is much harder to override.
- Don’t nest more than 3 levels deep. Nesting selectors increases specificity, meaning that overriding any CSS set therein needs to be targeted with an even more specific selector. This quickly becomes a significant maintenance issue.
- Avoid using nesting for anything other than pseudo selectors and state selectors. E.g. nesting `:hover`, `:focus`, `::before`, etc. is OK, but nesting selectors inside selectors should be avoided.
- Don't `!important`. If you must, leave a comment, and prioritise resolving specificity issues before resorting to `!important`. `!important` greatly increases the power of a CSS declaration, making it extremely tough to override in the future. It’s only possible to override with another `!important` declaration later in the cascade. The only case [when you should](http://csswizardry.com/2016/05/the-importance-of-important/) use `!important` is for utility classes.
- Don’t use `margin-top`. Vertical margins [collapse](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing). Always prefer `padding-top` or `margin-bottom` on preceding elements.
- Avoid shorthand properties (unless you really need them). It can be tempting to use, for instance, `background: #fff` instead of `background-color: #fff`, but doing so overrides other values encapsulated by the shorthand property. (In this case, `background-image` and its associative properties are set to “none”. This applies to all properties with a shorthand: `border`, `margin`, `padding`, `font`, etc.

```sass
/* Bad */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0px 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* Good */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

## Declaration order

Properties and nested declarations should appear in the following order, with line breaks between groups:

1. Any `@` rules
2. Layout and box-model properties: `margin`, `padding`, `box-sizing`, `overflow`, `position`, `display`, `width/height`, etc.
3. Typographic properties: `font-*`, `line-height`, `letter-spacing`, `text-*`, etc.
4. Stylistic properties: `color`, `background-*`, `animation`, `border`, etc.
5. UI properties: appearance, cursor, user-select, pointer-events, etc.
6. Pseudo-elements: `::after`, `::before`, `::selectio`n, etc.
7. Pseudo-selectors: `:hover`, `:focus`, `:active`, etc.
8. Modifier classes
9. Nested elements

Here’s a comprehensive example:

```scss
.c-button {
  @extend %link--plain;

  display: inline-block;
  padding: 6px 12px;

  text-align: center;
  font-weight: 600;

  background-color: color(blue);
  border-radius: 3px;
  color: white;

  &::before {
    content: '';
  }

  &:focus, &:hover {
    box-shadow: 0 0 0 1px color(blue, .3);
  }

  &--big {
    padding: 12px 24px;
  }

  > .c-icon {
    margin-right: 6px;
  }
}
```

## Variables

- Define all local variables at the top of the file after the imports.
- Namespace local variables with the filename.
  - e.g. `alert-box.scss` →`$alert-box-font-size: 14px;`
- Variables should be lowercase.


## Color

- Use shorthand hex values where available, e.g., `#fff` instead of `#ffffff`.
- Lowercase all hex values, e.g., `#fff`. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
- Limit alpha values to a maximum of two decimal places. Don’t use a leading zero.

Example:

```scss
/* Bad */
.c-link {
  color: #007ee5;
  border-color: #FFF;
  background-color: rgba(#FFF, 0.0625);
}

/* Good */
.c-link {
  color: color(blue);
  border-color: #fff;
  background-color: rgba(#fff,.06);
}
```

## Don't use `@import`

Compared to `<link>`s, `@import` is slower, adds extra page requests, and can cause other unforeseen problems. Avoid them and instead opt for an alternate approach:

- Use multiple `<link>` elements
- Compile your CSS with a preprocessor into a single file
- Concatenate your CSS files

## Nesting

Avoid unnecessary nesting. Just because you can nest, doesn't mean you always should. Consider nesting only if you must scope styles to a parent and if there are multiple elements to be nested.

Try to only nest pseudo-classes with &, as this is all one element and class, just with different states.

```scss
.button {
  &:hover,
  &:focus { }

  &:active { }
}
```

## Media Queries

- Media queries should be within the CSS selector as per SMACSS. Don't bundle them all in a separate stylesheet or at the end of the document. Doing so only makes it easier for folks to miss them in the future.
- Don’t abstract media queries with Sass mixins, keep it as readable as it can be.
- Use variables for frequently used breakpoints to keep it consistent and maintainable.

```scss
.selector {
  float: left;

  @media only screen and (max-width: 767px) {
    float: none;
  }
}
```

### Create variables for frequently used breakpoints

```scss
$screen-sm-max: "max-width: 767px";

.selector {
  float: left;

  @media only screen and ($screen-sm-max) {
    float: none;
  }
}
```

## Single declarations

In instances where a rule set includes **only one declaration**, consider removing line breaks for readability and faster editing. Any rule set with multiple declarations should be split to separate lines.

The key factor here is error detection—e.g., a CSS validator stating you have a syntax error on Line 183\. With a single declaration, there's no missing it. With multiple declarations, separate lines is a must for your sanity.

```scss
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

/* Multiple declarations, one per line */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}

.icon           { background-position: 0 0; }
.icon-home      { background-position: 0 -20px; }
.icon-account   { background-position: 0 -40px; }
```

## Shorthand notation

Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:

- `padding`
- `margin`
- `font`
- `background`
- `border`
- `border-radius`

Often times we don't need to set all the values a shorthand property represents. For example, HTML headings only set top and bottom margin, so when necessary, only override those two values. Excessive use of shorthand properties often leads to sloppier code with unnecessary overrides and unintended side effects.

The Mozilla Developer Network has a great article on [shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties) for those unfamiliar with notation and behavior.

```scss
/* Bad */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

## Operators

For improved readability, wrap all math operations in parentheses with a single space between values, variables, and operators.

```scss
/* Bad */
.element {
  margin: 10px 0 @variable*2 10px;
}

/* Good */
.element {
  margin: 10px 0 (@variable * 2) 10px;
}
```

## Comments

Code is written and maintained by people. Ensure your code is descriptive, well commented, and approachable by others. Great code comments convey context or purpose. Do not simply reiterate a component or class name.

Be sure to write in complete sentences for larger comments and succinct phrases for general notes.

```scss
/* Bad */
/* Modal header */
.modal-header {
  ...
}

/* Good */
/* Wrapping element for .modal-title and .modal-close */
.modal-header {
  ...
}
```

### Commenting style

Stick to the following two variations:

```scss
/* Basic comment */

/**
 * Multiline comment title
 *
 * Documentation, explanation
 */
```

Always insert two lines before a new component style.

## BEM

Block: Unique, meaningful names for a logical unit of style. Avoid excessive shorthand.

- Good: `.alert-box` or `.recents-intro` or `.button`
- Bad: `.feature` or `.content` or `.btn`

Element: styles that only apply to children of a block. Elements can also be blocks themselves. Class name is a concatenation of the block name, two underscores and the element name. Examples:

- `.alert-box__close`
- `.expanding-section__section`

Modifier: override or extend the base styles of a block or element with modifier styles. Class name is a concatenation of the block (or element) name, two hyphens and the modifier name. Examples:

- `.alert-box--success`
- `.expanding-section--expanded`

```scss
/* Block component */
.block {}
.alert-box {}

/* Element that depends upon the block */ 
.block__element {}
.alert-box__close {}

/* Modifier that changes the style of the block */
.block--modifier {}
.alert-box--succes {} 

/* Modifier that changes the style of the element */
.block__element--modifier {}
.alert-box__close--succes {} 
```

### BEM best practices

Don't `@extend` block modifiers with the block base.
- Good: `<div class="my-block my-block--modifier">`
- Bad: `<div class="my-block--modifier">`

Don't create elements inside elements. If you find yourself needing this, consider converting your element into a block.
- Bad: `.alert-box__close__button`

Choose your modifiers wisely. These two rules have very different meaning:

```scss
.block--modifier .block__element { color: red; }
.block__element--modifier { color: red; }
```

### BEM nesting with &

Nesting for BEM—or any other flavor CSS architecture—is helpful at first, but comes as a cost.
  
```scss
.block {
  &--modifier { /* compiles to .block--modifier */
    text-align: center;
  }

  &__element { /* compiles to .block__element */
    color: red;

    &--modifier { /* compiles to .block__element--modifier */
      color: blue;
    }
  }
}
```

While you’re saving a few bytes in your source file by not writing .block each time, you lose out on the ability search for your classes.

Instead, keep it simple and just write it all out:

```scss
.block__element { }
.block--modifier { }
```

Easy to read, easy to search, easy to refactor, and with no sacrifice to your compiled CSS.

### Making exceptions

In some cases the strict adherence to BEM conventions is either impractical or impossible. E.g. for our generated .markdown-content, as you shouldn’t have to put a class on every single `<p>` tag for example.

Extend your base style with a tag selector for the generated .markdown-content instead.

```scss
.markdown-content p { }
.markdown-content a { }
```

## Selector naming

- Don't use ids in selectors. `#header` is overly specific compared to, for example `.header` and is much harder to override.
- Always use BEM-based naming for your class selectors [when it makes sense](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/).
  - When using modifier classes, always require the base/unmodified class is present.
  
## Namespaced classes

There are a few reserved namespaces for classes to provide common and globally-available abstractions.

- `.u-` for helpers and utilities. Utility classes are usually single-purpose and have high priority. Things like floating elements, trimming margins, etc. Don’t overuse them, e.g. if you’ve an utility class to center text, don’t use it as a shortcut to style components. Aim for autonomous components that can be understood without having to review the html markup.
- `._` for hacks. Classes with a hack namespace should be used when you need to force a style with `!important` or increasing specificity, should be temporary, and should not be bound onto.
- `.t-` for theme classes. Pages with unique styles or overrides for any objects or components should make use of theme classes.
- Never reference `.js-` prefixed class names from CSS files. `.js-` are used exclusively from JS files. Use `.is-, .has-` for state rules that are shared between CSS and JS, a la [SMACSS](https://smacss.com/book/type-state).

## File organization

I prefer a 5-1 pattern, which is abstracted from the [7-1 pattern](https://sass-guidelin.es/#architecture). 5 folders, 1 file to compile them all in a single CSS file.

```bash
styles/
|
|- utilities/            # Configuration and helpers
|   |- _variables.css    # Global variables
|   |- _functions.css
|   |- _mixins.css
|   …
|
|- vendor/               # Third-party CSS
|   |- _normalize.css
|   …
|
|- base/                 # Boilerplate code
|   |- _reset.css
|   |- _base.css
|   |- _typography.css
|   …
|
|- layout/               # Global wireframe (macro)
|   |- _header.css
|   |- _navigation.css
|   |- _sidebar.css
|   |- _footer.css
|   …
|
|- components/           # Modules (micro)
|   |- _buttons.css
|   |- _cards.css
|   |- _tables.css
|   …
|
`- main.css              # Main file to import everything
```

### Naming conventions

File names should always be lowercase and hyphen-delimited.

## Editor preferences

Set your editor to the following settings to avoid common code inconsistencies and dirty diffs:

- Use soft-tabs set to two spaces.
- Trim trailing white space on save.
- Set encoding to UTF-8.
- Add new line at end of files.

---

## Credits

- Inspired by [Dropbox (S)CSS Style Guide](https://github.com/dropbox/css-style-guide).
- Inspired by [@mdo’s Code Guide](http://codeguide.co), which is heavily inspired by [Idiomatic CSS](https://github.com/necolas/idiomatic-css).
- Inspired by [GitHub’s Primer Guidelines](http://primercss.io/guidelines/).
- Inspired by [Hugo Giraudel’s Sass Guidelines](https://sass-guidelin.es/#architecture).
- Compiled and expanded by [Michael Xander](http://michaelxander.com).

---

##### Additional resources

- [BEM 101](https://css-tricks.com/bem-101/)
- [The Sass Way](http://thesassway.com)
- [Nesting in Sass and Less](http://markdotto.com/2015/07/20/css-nesting/)
- [Side Effects in CSS](https://philipwalton.com/articles/side-effects-in-css/)