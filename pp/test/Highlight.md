!import(RUN-TESTS.pp)

# Highlight PP-Macros Test

This is a run test of the GFM-TaskList pp-macros set.

    macros module: Highlight.pp
    related files: code-example.pb

    Macros list:
!raw{
-   `!HighlightFile(FILE)(LANG)[(OPTIONS)]` — imports and syntax-highlights and external file.
-   `!HighlightInlineTheme(THEME)` — retrives a Highlight theme and injects its CSS into the documents.
}

# Add Highlight Color Theme

For these examples we'll import Highlight's `edit-purebasic` theme into the document via the !raw(!HighlightInlineTheme) macro:

```
!raw(!HighlightInlineTheme(edit-purebasic))
```

this macro emits the following raw html:

``` html
!HighlightInlineTheme(edit-purebasic)
```

!HighlightInlineTheme(edit-purebasic)

Now the CSS definition for PureBASIC code blocks (via Highlight) is available document-wide and you'll see it aplied to the examples in this document.

# Highlighting an External File

The `!raw(!HighlightFile)` macro takes an external source code file, passes it to Highlight tool for syntax coloring, and injects into the document the raw HTML results as a `<pre><code>` block.

The syntax is:

!raw{!HighlightFile(FILE)(LANG)[(OPTIONS)]}

... taking the following parameters:

- `FILE` (mandatory) — The source file to syntax-color.
- `LANG` (mandatory) — The language of the source (eg: HTML, Python, etc.).
- `OPTIONS` (facultative) — Further options to pass to Highlight during invocation.

## Basic Example

We import the PureBASIC example file "`code-example.pb`" without passing any extra options to Highlight:

!def(TEST)(!HighlightFile(code-example.pb)(purebasic))

!RUNTEST

## Example Using Options

Now we import the same file, this time passing some extra options to Highlight in order to show line numbers:

!HighlightFile(code-example.pb)(purebasic)(--line-numbers --line-length=1)

--------------

!def(TEST)(!HighlightFile(code-example.pb)(purebasic)(--line-numbers --line-length=1))

!RUNTEST

# Highlighting a Code Block

The `!raw(!Highlight)` macro takes an block of source code and passes it to Highlight tool for syntax coloring, and injects into the document the raw HTML results as a `<pre><code>` block.

It's similar to the `!raw(!HighlightFile)` macro, except it relies on source code defined inside the current document instead of an external file. So we won't show the raw html emitted in the following examples, just the macro used and the final result.

The syntax is:

    !raw{!Highlight(LANG)(OPTIONS)
    ~~~~~
    CODE
    ~~~~~
    }

... taking the following parameters:

- `LANG` (mandatory) — The language of the source (eg: HTML, Python, etc.).
- `OPTIONS` (can be empty) — Further options to pass to Highlight during invocation. If none desired, just pass empty value.
- `CODE` (mandatory) — The block of source code to syntax-color.

> __NOTE 1__: The `CODE` parameter is passed between lines of tildas instead of brackets. From PP's documentation:
> 
> > The last argument can be enclosed between lines of tildas or backquotes (of the same length) instead of parenthesis, brackets or braces and. This is useful for literate programming, diagrams or scripts (see examples). Code block arguments are not stripped: spaces and blank lines are preserved.

<!-- -->

> __NOTE 2__: This macro creates and deletes a temporary file (named "`_pp-tempfileX.tmp`", where `X` is a numeric counter) in the macros folder (`/pp/macros/`) for each macro call in the document, to temporarily store the code to highlight. The `X` counter is reset at each PP invocation.

## Basic Example

We now define a block of PureBASIC code and pass it to Highlight without any extra options:

```
!raw{!Highlight(purebasic)()
~~~~~~~~~~
; PureBASIC 5.60
For i=1 To 10
  Debug("Counting " + Str(i))
Next
~~~~~~~~~~
)}
```

... which produces:

!Highlight(purebasic)()
~~~~~~~~~~
; PureBASIC 5.60
For i=1 To 10
  Debug("Counting " + Str(i))
Next
~~~~~~~~~~
)

## Example Using Options

We now define a block of PureBASIC code and pass it to Highlight, this time providing some extra options to Highlight in order to show line numbers:

```
!raw{!Highlight(purebasic)(--line-numbers --line-length=1)
~~~~~~~~~~
; PureBASIC 5.60
For i=1 To 10
  Debug("Counting " + Str(i))
Next
~~~~~~~~~~
)}
```

... which produces:

!Highlight(purebasic)(--line-numbers --line-length=1)
~~~~~~~~~~
; PureBASIC 5.60
For i=1 To 10
  Debug("Counting " + Str(i))
Next
~~~~~~~~~~
)