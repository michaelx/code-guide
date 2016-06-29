# Liquid Styleguide

Best practices and guidelines for writing Liquid with approachable formatting, syntax, and more.

## Syntax

- Use soft-tabs with two spaces. Spaces are the only way to guarantee code renders the same in any person’s environment.
- Put one space after the opening Liquid markup `{{ ` and `{% `, as well as one space before the closing markup ` }}` and ` %}}`.
- Put one space before and after each filter statement, e.g. `{{ 5 | plus: 1 | divided_by: 2 }}`.
- Include one space after the colon `:` of filter parameters.
- Comma-separated parameter values should include a space after each comma, e.g. `{{ "barbar" | replace_first: "bar", "foo" }}`.
- Use double-quoted strings. Escaped characters will always work without a delimiter change, and ' is a lot more common than " in string literals.
- Don’t quote integers, booleans or nil.
- Put one space before and after operators.

Example:

```liquid
{% comment %}Bad{% endcomment %}
{{'barbar'|replace_first:'bar','foo'}}

{% comment %} Good {% endcomment %}
{{ "barbar" | replace_first: "bar", "foo" }}
```

## Variables

- Define all local variables at the top of the file.
- Use lowercase alphanumeric characters and underscores.
- Always start with a letter.
- No leading sigil (that is, they look like var_name, not $var_name).

## Comments

Code is written and maintained by people. Ensure your code is descriptive, well commented, and approachable by others. Great code comments convey context or purpose. Do not simply reiterate a component or class name.

Be sure to write in complete sentences for larger comments and succinct phrases for general notes.

Always use the Liquid comment tag for Liquid code, rather than HTML comments.

```liquid
<!-- Bad -->
{{'barbar'|replace_first:'bar','foo'}}

{% comment %} Replaces the first occurrence of "bar" with "foo" {% endcomment %}
{{ "barbar" | replace_first: "bar", "foo" }}
```

---

## Credits

- Compiled and expanded by [Michael Xander](http://michaelxander.com).

---

##### Additional resources

- [Liquid template engine](https://github.com/Shopify/liquid)