MILK (Markup I LiKe)
====================

MILK is both a markup format and a simple yet capable publishing platform. First it is a markup format similar to Markdown, but with many more features that are all well thought out. MILK is also a publishing platform in that it is designed to produce HTML, LaTeX, PDF and ePub documents with enough professionalism for use a resarch papers and book publishing.

Markdown is great. It is concise, simple and easy to learn. Unfortunately it also is missing too many features. A few superset of Markdown have become popular, most notably GFM (GitHub FLavored Markdown) and MultiMarkdown. And yet there are still very obvious omissions, such as indexing and text color. Other formats only partially compatible with Markdown go to great lengths to offer all the bells and whistles. ACIIdoc and MediaWiki in particular stand out. And yet, after perusing their websites, I can't help feeling like I've just been glammered by a vampire. They are rather overwhelmeing, with seemingly haphazard designs. 

## Structure

MILK documents follow the [Perfect Document Structure](https://github.com/openbohemians/milk/wiki/Perfect-Document-Structure) design pattern. This means all MILK documents are structured as a hierarchy of ordered lists. These tiers are designated by the use of headers. Consider the following example.

```rdoc
= Header 1
== Header 2
Content
```

```html
<article class="document">
<ol><li>
  <h1 class="title">Header 1</h1>
  <ol><li>
    <h2 class="title">Header 2</h2>
    Content
  </li></ol>
</li></ol>
</article>
```

## Inline Markup

### Style

All inline markup is clearly indicated by the use of ` x.content.x ` pattern, where `x` is a special chracter expressing the markup style to use. In addition to Milk's inline styles, Markdown compatable inline markup should be available as a an option.


| Markup                     | Markdown                   | Result                   | Render                 |
|----------------------------|----------------------------|--------------------------|------------------------|
| `*.bold.*`                 | `**bold**`                 | `<b>strong</b>`          | **bold**               |
| `'.italic.'`               | `*italic*'`                | `<i>italic</i>`          | *emphasis*             |
| `".quote."`                | `"quotes"`                 | `<q>quote</q>`           | <q>quote</q>           |
| `_.underline._`            | `_underline_`              | `<u>underline</u>`       | <u>underline</u>       |
| `-.strikethru.-`           | `~strikethru~`             | `<s>strikethru</s>`      | <s>strikethru</s>      |
| `^.superscript.^`          | `^superscript^`            | `<sup>superscript</sup>` | <sup>superscript</sup> |
| `~.subscript.~`            | `,subscript,`              | `<sub>subscript</sub>`   | <sub>subscript</sub>   |
| <tt>\`.monotype.\`</tt>    | <tt>\`monotype\`</tt>      | `<tt>monotype</tt>`      | `monotype`             |
| <tt>\`\`no-markup\`\`</tt> | <tt>\`\`no-markup\`\`</tt> | `no-markup`              | no-markup              |


### Hyperlinks

**TODO** <i>I am little bit hesitant to move away from the traditional wiki-wiki style of `[[foo]]` here, and yet I have always felt it would a good thing if links were more like real references in technical papers. This still needs some work and some thought.</i>


```
Somewhere over the rainbow ^(https://en.wikipedia.org/wiki/Rainbow).
```

If the link label has whitespaces any visibile enclosing brackets will automatically be used.

```
Somewhere over the rainbow (Schomer 2012) <https://en.wikipedia.org/wiki/Rainbow>.
```

Or use backticks for invisible grouping.

```
Rockstar `Alan Turing` (^https://en.wikipedia.org/wiki/Alan_Turing)
```

### References

```
She was somewhere over the rainbow (Baum 1900)

^Baum 1900, https://en.wikipedia.org/wiki/Rainbow
```

### Footnotes

Footnotes can be per-section or per-document.

```
This is not to be taken lightly. (1)

^1 Unless it '*exists*' to be taken lightly.
```

### Indexing

Milk support document indexing. Index tags can be free standing or inline. Free-standing index tags can be placed at the start or end of a paragraph block. If it is the only entry in such a block, no paragraph is rendered. Index markup consists a paranthecial with a `#` sign at the head.


```
(# programming)
```

Or a simple hash tag.

```
#fun_code
```

When using hash tag syntax, undersocres are converted to spaces when redered. To subindex, simply separate the subindex from the main index with a comma.

```
(# programming, Ruby)
```

In HTML this renders as an anchor `<a id="programming:Ruby" class="index">`. In Latex the `\\index` command is used.

If multiple indexes are required simply use the first segment of the index as the indexes name. The rendering engine takes an option to split indexes up base on the first entry. For example, a command line rendering tool could take the following entries:

```
(# programming, Ruby)
(# Examples, programming, Ruby)
```

And render the first index tag in a separate "Examples" appendex, apart from the prior index tag, using a `--index Examples` option. Note, different rendering engines might handle the configuration in some other way, but the same capability will be present.

```
This is an #.example.#. 
```


## Block Markup

### Section Tiers

Section tiers can be *plain* or *enumerated*.

```
= Plain
```

```
# Enumerated
```

While the style of enumeration can be adjusted via stylesheets, the standard format is multi-point. So, the first `#` will be rendered as `1` and its first `##` will be rendered as `1.1`.


### Literals

TODO: Separate Code literals from general literals?

Four spaces of indention (or two tabs if that's how you butter your toast) designates a preformatted *literal* section. This translates into the `<pre>` HTML tag. Preformat is a bit a misnomer --it basically means don't apply markup and display in a monotype.

These are most often used for programming code examples.

```
    for(i=0; i<10; i++) {
      printf "HELLO AGAIN!"
    }
```

Unlike more markup where a special brace must be used to indicate the type of content, which is then used by syntax highlighters, MILK uses a special `fig` addendum.


```
    for(i=0; i<10; i++) {
      printf "HELLO AGAIN!"
    }
    fig. 1 (c++) Classic Hello World
```

### Quotations

Quotations, also kown as *block quotes* translate to the `<blockquote>` HTML tag. They are essentially `div` elements which have a style that sets them apart and helps the reader recognize they are quotes.

```
> This is a
> block quote.
```

### Lists

List can be ordered or unordered. Unorder lists com in two formats, light bullet and heavy bullet.

```
  - Unordered
    - List
```

```
  * Unordered
    * List
```

Ordered list can have a few different forms of enumeration. The most obvious is numeral notation.

```
  1. Ordered
    1. List
    2. And so on
```

Numeral notation can be multi-pointed.

```
  1.0 Ordered
    1.1 List
    1.2 And so on
```

The actual number is not signifficant. If you don't expect anyone to every read the markup document itself, simply use the first number repatedly.

```
  1. Ordered
    1. Will be 1
    1. Will be 2
  1. Also 2
```

This applies to all list formats. 

Besides numeral there is *roman enumeration*.

```
  i. Ordered
    ii. List
```

Roman enumeration can be lower or upper case.

```
  a. Ordered
    b. List
```

Alphabetic enumeration can also be lower or upper case.


### Definition Lists


```
fish:: Descipriot of Fish.
eggs:: Description of Eggs.
```

becomes

```html
<dl>
<dt>fish</dt><dd>Description of fish.</dd>
<dt>eggs</dt><dd>Description of fish.</dd>
</dl>
```

### Tables

Tables come in two forms, simple single-line cell rows and multi-line cell rows.

```
------- -------
Cell A1 Cell B1
Cell A2 Cell B2
------- -------
```

Headers and footers ...

```
------------ ------------
! Header A ! ! Header B !
------------ ------------
Cell A1      Cell B1
Cell A2      Cell B2
------------ ------------
Footer A     Footer B
------------ ------------
```

An optional feature, allows simple spreadsheet formulae to be processed.

```
------- --------
Gas          50
Rent        200
------- --------
TOTAL    =A2+B2
------- --------
```

**Grid Tables**. Arbitary sized cells can be create using *grid tables*, which are distringuished by the use of plus signs in cell corners.

```
+--------+--------+
! Fat    ! And    !
! Header ! Again  !
+--------+--------+
| Big    |        |
|        | Table  |
+--------+--------+
```

## Metadata

Documents can be prefaced or appended with metadata. MILK supports simple YAML and JSON.

When a MILK document is parsed the first procedure is to strip off any front or back matter and store the settings for variable lookup.

**YAML metadata**. 

```
---
key1: value1
key2: value2
...
```

**JSON metadata.**

```
--- !json
{
  "key1": "value1",
  "key2": "value2"
}
...
```

Documents should use either front matter or back matter and not both. But if both are given, then front matter takes precedence over back matter if any key paths conflict.

Using metadata in a document is done using `\(key1)`.

TODO: Use `\(key1)` or `(\key1)` ?


## Customization

### Quick Classes

Most of the time we do not need to get vey fancy with our customizations. A simple extra class will generally do the job. To this end, any text section can be *classified* simply by sarting the text with a `.classname`.

```
.important This is an important paragraph.
```


### Implicit Spans

Using HTML style markup without specific tag defaults to a `<span>`.

```
This is <color=green>green</>.
```


### Tag Augmentation

**IMPORTANT** Customizing markup is not something to be done lightly!!! Use it sparingly and only when you have no other good option. It is much better to simply uses classes and modify a sites stylesheets to produce the desired results. In fact, we debated whether it was even wise to allow this level of customization. Implementors may wish to make it a separate API option which can be turned on or off.

For fancier customizations we can use our trusy macro langauge. Customizing any markup tags is as easy as using *relative tagging*. By relative tagging we simply mean that instead of creating a new element of a given tag, we are referencing a another tag which is to be augmented. 

Referencing the current element is done simply by not using a tag name. This harks back to out previous example, as it is the same result if we enclose the class designation in angle brackets.

```
<.important/> This is an important paragraph.
```

But now we can add an *id* if need be.

```
<.important #here/> This is an important paragraph.
```

Or any attribute,

```
<.important #here color=green/> This is an important paragraph.
```

But what if we want to modify the `ol` element that a section header creates? Perhaps we'd like to hide a section.

```
= Big Header <^ol display=none/> 
```

The `^` symbol indicates a backward lookup of the given tag. If we left the `ol` out if would look to first previous tag, which in this case would be `<li>`, which would have a very different effect. Conversely we can have the tag look ahead using a `+` character.

```
<+ol display=none/>
= Big Header
```

It is even possible to augment all the tags fo a given tag using `<*tag ...>` notation. But these should alwasy be put at the top or bottom of a document, and usually the bottom to keep them out of the way.

**IMPORTANT** The relative taggin notation is still be fleshed out and could very well change.


## Macros

MILK supports a *macro* language, which allows the document a great deal of control over the final output. Macro usage is easily identified becuase more usage is in the form of XML/XHTML. So, for instance `<b>foo</b>` is using a macro, in this example a macro called `b`, and it just so happens that this macro translates verbtim to HTML.

MILK hase a built-in set of macros based on a safe subset of HTML. It also provides a few some additional macros.... Beyond the standard macros, MILK documents can also define custom macros, or gain new macros via plugins.

**Plugins**. Macros can come from of plugins. Plugins are particular to an implementation. Plugins allow for a diverse set of external solutions to be embedded into MILK documents. For example, we can easily imagine a plugin for [figlet](http://www.figlet.org/).

```
<figlet font=slant size=10>
THIS IS HUGE! 
</figlet>
```

Macros can also be written in YAML format.

```
--- !figlet
font: slant
size: 10
   =: THIS IS HUGE!
...
```

YAML format is better when the macro is data driven, where as XML/HTML syntax works well when the macro is content oriented.


## Escape Characters

To escape `<` and `>` simply repeat them, i.e. `<<` becomes `<` and `>>` becomes `>`.

To escape inline markup wrap the content in no-markup markup, i.e. `\`\`*.foo.*```.
