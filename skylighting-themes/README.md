# Skylighting JSON Themes

    pandoc v2.0.5

Resources for customizing syntax highlighting styles via KDE theme files.

-----

**Table of Contents**

<!-- MarkdownTOC autolink="true" bracket="round" autoanchor="false" lowercase="true" lowercase_only_ascii="true" uri_encoding="true" depth=3 -->

- [Files List](#files-list)
- [Introduction](#introduction)
- [The Default Theme](#the-default-theme)
- [KDE vs Pandoc Themes](#kde-vs-pandoc-themes)
    - [KDE Theme’s Data Excess](#kde-theme%E2%80%99s-data-excess)
- [Theme Boilerplate](#theme-boilerplate)
    - [Usage Guidelines](#usage-guidelines)
        - [The "Normal" Style](#the-normal-style)
        - [The Metadata](#the-metadata)
        - [Base Colors and Line Numbers](#base-colors-and-line-numbers)
        - [Text Styles/Token Types](#text-stylestoken-types)
- [References](#references)
    - [Full List of Token Types](#full-list-of-token-types)
    - [Token-Types to CSS-Classes](#token-types-to-css-classes)
    - [CSS-Classes to Token-Types](#css-classes-to-token-types)

<!-- /MarkdownTOC -->

-----

# Files List

  - [`default.theme`](./default.theme) — output of `--print-default-data-file default.theme` (KDE “Kate” theme, MIT License).
  - [`boilerplate.theme`](./boilerplate.theme) — Highlight-style boilerplate (public domain).
  - [`UNLICENSE`](./UNLICENSE) — Public domain declaration for `boilerplate.theme`

JSON files of pandoc's built-in themes:

- [`builtin-breezedark.theme`](./builtin-breezedark.theme)
- [`builtin-espresso.theme`](./builtin-espresso.theme)
- [`builtin-haddock.theme`](./builtin-haddock.theme)
- [`builtin-kate.theme`](./builtin-kate.theme)
- [`builtin-monochrome.theme`](./builtin-monochrome.theme)
- [`builtin-pygments.theme`](./builtin-pygments.theme)
- [`builtin-tango.theme`](./builtin-tango.theme)
- [`builtin-zenburn.theme`](./builtin-zenburn.theme)

# Introduction

Pandoc v2 introduced a new feature which allows to dynamically load a custom syntax highlighting theme file (JSON) via the `--highlight-style=FILE` option:

> __`--highlight-style=`*STYLE*|*FILE__
> 
> Instead of a *STYLE* name, a JSON file with extension `.theme` may be supplied. This will be parsed as a KDE syntax highlighting theme and (if valid) used as the highlighting style. 
>
>  To generate the JSON version of an existing style, use `--print-highlight-style`.

# The Default Theme

  - <https://github.com/jgm/pandoc/blob/master/data/default.theme>

The default theme used by pandoc can be viewed at the above link, or generated by invoking pandoc with:

    pandoc --print-default-data-file default.theme

You can see the result of the above command in this file (created with pandoc v2.0.5):

  - [`default.theme`](./default.theme) (KDE “Kate” theme, MIT License).

The theme file created is based on the the default theme ("Kate") of the KDE project, with a few additional keys added (`"author"`, `"license"`, `"read-only"`), which can be found at this link:

- https://github.com/KDE/syntax-highlighting/blob/master/data/themes/default.theme

> __NOTE 1__: The generated file is the full KDE syntax highlighting theme used internally by pandoc, and it shouldn't be used as a reference to build custom themes. Use instead the output of `--print-highlight-style`, which is filtered to show only the JSON fields relevant to pandoc! (introduced in [pandoc v2.0.4])

<!-- -->

> __NOTE 2__: The default theme thus generated (as of pandoc v2.0.5) is actually Kate, even though pandoc default style is pygments. (see [#4096])

[pandoc v2.0.4]: https://github.com/jgm/pandoc/releases/tag/2.0.4 "Go to pandoc 2.0.4 release page"

[#4096]: https://github.com/jgm/pandoc/issues/4096 "pandoc Issue #4096: Default Theme is Kate not Pygments"

# KDE vs Pandoc Themes

Pandoc uses KDE syntax highlighting theme files, but it’s worth noting that KDE themes contain more data than what is actually needed for pandoc usage. If you’ve found a ready-made KDE theme that you want to use for pandoc syntax highlighting, than the data excess isn’t a concern to you. On the other hand, if you’r building a theme from scratch, than you might be better off working with the boilerplate file provided here, which contains only the data needed for pandoc usage.

You can find examples of KDE themes at:

  - [github.com/KDE/syntax-highlighting/data/themes](https://github.com/KDE/syntax-highlighting/tree/master/data/themes)

In this folder you can find the Kate theme both in full KDE format as well as in its pandoc filtered version:

 - [`default.theme`](./default.theme) — Kate theme via ` --print-default-data-file default.theme` (original KDE theme)
 - [`builtin-kate.theme`](./builtin-kate.theme) — Kate theme via `--print-highlight-style` (pandoc theme)

... if you diff-compare these two files, you'll be able to view that the pandoc output version of Kate not only doesn't contain the KDE JSON keys which are not used by pandoc, it also contains additional keys which are pandoc-specific. 

[KDE Kate theme]: https://github.com/KDE/syntax-highlighting/blob/master/data/themes/default.theme "Link to Kate theme source at KDE repository"

## KDE Theme’s Data Excess

The problem with KDE syntax highlighting themes file is that that contain many keys which are not actually used by pandoc for highligthing. (See [Issue \#4096](https://github.com/jgm/pandoc/issues/4096))

For example, let’s look at the “Kate” KDE theme ([`default.theme`](./default.theme)).

Some members of `"text-styles"` have `"selected-text-color"` and `"strike-through"` entries, which don’t affect the ouput of pandoc highlighting:

``` json
    },
    "text-styles": {
        "Normal" : {
            "text-color" : "#1f1c1b",
            "selected-text-color" : "#ffffff",
            "bold" : false,
            "italic" : false,
            "underline" : false,
            "strike-through" : false
        },
        "Keyword" : {
            "text-color" : "#1f1c1b",
            "selected-text-color" : "#ffffff",
            "bold" : true
        },
```

Also, most of the name/value pairs in `"editor-colors"` (ie: 26 out of 28) are specific to the Kate Editor, and will not affect pandoc syntax-highlighting:

``` json
    "editor-colors": {
        "background-color" : "#ffffff",
        "code-folding" : "#94caef",
        "bracket-matching" : "#ffff00",
        "current-line" : "#f8f7f6",
        "icon-border" : "#f0f0f0",
        "indentation-line" : "#d2d2d2",
        "line-numbers" : "#a0a0a0",
        "current-line-number" : "#1e1e1e",
        "mark-bookmark" : "#0000ff",
        "mark-breakpoint-active" : "#ff0000",
        "mark-breakpoint-reached" : "#ffff00",
        "mark-breakpoint-disabled" : "#ff00ff",
        "mark-execution" : "#a0a0a4",
        "mark-warning" : "#00ff00",
        "mark-error" : "#ff0000",
        "modified-lines" : "#fdbc4b",
        "replace-highlight" : "#00ff00",
        "saved-lines" : "#2ecc71",
        "search-highlight" : "#ffff00",
        "selection" : "#94caef",
        "separator" : "#898887",
        "spell-checking" : "#bf0303",
        "tab-marker" : "#d2d2d2",
        "template-background" : "#d6d2d0",
        "template-placeholder" : "#baf8ce",
        "template-focused-placeholder" : "#76da98",
        "template-read-only-placeholder" : "#f6e6e6",
        "word-wrap-marker" : "#ededed"
    }
}
```

# Theme Boilerplate

  - [`boilerplate.theme`](./boilerplate.theme)

For the above mentioned reasons, I’ve created a [theme boilerplate file](././boilerplate.theme), containing only entries that are significant for pandoc usage (the boilerplate is released into the [Public Domain](./UNLICENSE)).

This boilerplate is a more manageable template for building a custom theme.

> **NOTE** — I haven’t been able to find much documentation on how pandoc/skylighting handle themes; so I’ve been relying on testing with theme files and on peeking at skylighting sourcecode.

## Usage Guidelines

Here are some guidelined on how to edit custom theme files.

  - You can safely delete any unneeded entry (ie: name/value pair).
  - You can delete `false`-valued boolean entries (they’ll be `false` by default).
  - Color values must be hex strings in the “`#00ff00`” format (color names like `"red"` are not allowed, nor are RGB color definitions).
  - Using the `null` value will cause the corresponding style to be omitted in the final CSS (eg: `"text-color": null,` will not produce any `color:` property).
  - The order of the entries can be changed.
  - Beware of the commas\! Missing or misplaced commas will cause the theme file to fail.

You might even add comments (or any custom entry) via extra string entries, as long as they are valid JSON — they’ll be just ignored\! Example:

``` json
    "Comment" : "JUST A COMMENT",
    "text-styles": {
        "Normal" : {
            "text-color" : "#ffffff",
```

… but you should avoid this in published theme files; it would only add confusion regarding theme’s standard. But in case you’re using some custom app or scripts to generate pandoc theme files, you should be reliefed to know that you could safely add custom entries for that app/script’s sake, without breaking a pandoc theme.

### The "Normal" Style

Pandoc doesn't actually use the `Normal` token (there is not corresponding class), so all settings for `"Normal"` style will have no effect. In the final CSS, these will be applied to `code span.`:

```css
code span. { color: #ffffff; background-color: #333333; font-weight: bold; font-style: italic; text-decoration: underline; } /* Normal */
```

... which have not effect on any tokens (they are not inherited by other spans). (see [Issue #4176], pandoc 2.0.5)

[Issue #4176]: https://github.com/jgm/pandoc/issues/4176

Since "normal" code is represented outside any `<span>`, it's basic style in the theme file are handled by the definitions outside the `"text-styles"` block. It's therefore safe to assume that the KDE `"Normal"` entry can be omited from custom pandoc themes, so I've removed it from the boilerplate.

### The Metadata

The `"metadata"` section of the theme is entirely optional, you could even remove it and the theme will still work. But if you’re planning to share your theme with others you should consider filling it appropriately.

``` json
{
  "metadata": {
    "name": "theme boilerplate",
    "author": "Tristano Ajmone <tajmone@gmail.com>",
    "license": "Unlicense",
    "revision": 3
  },
```

  - `"name"` — Theme’s name.
  - `"author"` — Your name.
  - `"license"` — Theme’s license.
  - `"revision"` — Theme’s revision number (integer)

> **NOTE** — pandoc simply ignores this data, so you have quite some freedom of action here (you could add custom fields); try nevertheless to stick to the KDE standard as much as you can, for the sake of clarity.

### Base Colors and Line Numbers

Following the `"metadata"` section, and before `"text-styles"`, you'll find the basic styles definitions:

``` json
  },
  "text-color": "#888888",
  "background-color": "#666666",
  "line-number-color": "#0000ff",
  "line-number-background-color": "#00ff00",

  "text-styles": {
```

These entries are rather self-explaining:

  - `"text-color":` — base code foreground color ("normal")
  - `"background-color":` — background color for the whole code block

... these are applied directly to the `<div>` that wraps the whole code block (via `div.sourceCode { … }`)

  - `"line-number-color":` — line numbers foreground color
  - `"line-number-background-color":` — background color for line numbers gutter

... these are applied via `pre.numberSource a.sourceLine::before`. If line numbers background and/or foregraound colors are omitted (or set to `null`), the base code blocks' FG and BG colors will be inherited.

The boilerplate default color values for these entries are mere placeholder (and quite hugly ones too); change them according to needs.

### Text Styles/Token Types

All the members of `"text-styles"` represent [skylighting token types](#full-list-of-token-types), and share some common name/value pairs:

``` json
    "text-styles": {

        "Keyword": {
            "text-color" : "#ffffff",
            "background-color" : "#333333",
            "bold" : true,
            "italic" : true,
            "underline" : true
        },
```

… where `"Keyword"` correspond to the “Keyword” token type, and so on.

I’ve filled all text-styles with the same sample-data set, which is usually more than you’ll ever need. Dummy color values are provided as placeholders. You should edit color values to your needs, and remove any unwanted attributes, including boolean “`false`” values (you can delete the whole entry, as `false` is the defaul value of missing entries).

To temporary disable from the final CSS a specific entry, just set it to `null`. This might be useful during testing, instead of deleting it completely.

The idea is to avoid copy-&-paste operations: everything you might need for any text-style is there, remove what you don’t need. It seemed better to me than having a single entry with the data, and then have to copy and paste over what was needed.

For example, if you wanted Constants to be shown in red (`#ff0000`) and **bold**, you’d only keep:

``` json
        "Constant" : {
            "text-color" : "#ff0000",
            "bold" : true,
        },
```



# References

  - <https://github.com/jgm/skylighting>
  - <https://github.com/KDE/syntax-highlighting>

## Full List of Token Types

The list of skylighting supported token types can be found in the “`Types.hs`” source file:

  - [github.com/jgm/skylighting/blob/master/src/Skylighting/Types.hs](https://github.com/jgm/skylighting/blob/master/src/Skylighting/Types.hs#L170)

Their HTML class-names equivalents can be found in the “`HTML.hs`” source file:

  - [github.com/jgm/skylighting/blob/master/src/Skylighting/Format/HTML.hs](https://github.com/jgm/skylighting/blob/master/src/Skylighting/Format/HTML.hs#L16)

## Token-Types to CSS-Classes

Here is a reference table listing all the token types and their corresponding CSS class-names:

|     TOKEN      | CLASS |
|----------------|-------|
| Normal         | ---   |
| Alert          | `.al` |
| Annotation     | `.an` |
| Attribute      | `.at` |
| BaseN          | `.bn` |
| BuiltIn        | `.bu` |
| Char           | `.ch` |
| Comment        | `.co` |
| CommentVar     | `.cv` |
| Constant       | `.cn` |
| ControlFlow    | `.cf` |
| DataType       | `.dt` |
| DecVal         | `.dv` |
| Documentation  | `.do` |
| Error          | `.er` |
| Extension      | `.ex` |
| Float          | `.fl` |
| Function       | `.fu` |
| Import         | `.im` |
| Information    | `.in` |
| Keyword        | `.kw` |
| Operator       | `.op` |
| Other          | `.ot` |
| Preprocessor   | `.pp` |
| RegionMarker   | `.re` |
| SpecialChar    | `.sc` |
| SpecialString  | `.ss` |
| String         | `.st` |
| Variable       | `.va` |
| VerbatimString | `.vs` |
| Warning        | `.wa` |

## CSS-Classes to Token-Types

Here is the same table as above, but ordered by CSS class-names:

| CLASS |     TOKEN      |
|-------|----------------|
| ---   | Normal         |
| `.al` | Alert          |
| `.an` | Annotation     |
| `.at` | Attribute      |
| `.bn` | BaseN          |
| `.bu` | BuiltIn        |
| `.cf` | ControlFlow    |
| `.ch` | Char           |
| `.cn` | Constant       |
| `.co` | Comment        |
| `.cv` | CommentVar     |
| `.do` | Documentation  |
| `.dt` | DataType       |
| `.dv` | DecVal         |
| `.er` | Error          |
| `.ex` | Extension      |
| `.fl` | Float          |
| `.fu` | Function       |
| `.im` | Import         |
| `.in` | Information    |
| `.kw` | Keyword        |
| `.op` | Operator       |
| `.ot` | Other          |
| `.pp` | Preprocessor   |
| `.re` | RegionMarker   |
| `.sc` | SpecialChar    |
| `.ss` | SpecialString  |
| `.st` | String         |
| `.va` | Variable       |
| `.vs` | VerbatimString |
| `.wa` | Warning        |

