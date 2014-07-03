MILK (Markup I LiKe)
====================

Makrdown is great. It is concise, simple and easy to learn. Unfortunately it also is missing too many features. A few superset of Markdown have become popular, most notably GFM (GitHub FLavored Markdown) and MultiMarkdown. And yet there are still very obvious omissions, such as text color.

Other formats only partially compatible with Markdown go to great lenghts to offer all the bells and whistles. Pandoc and ASCIIdoc stand out. And yet, after perusing their websites, I can't help feeling like I've just been glammered by a vampire. They are rather overwhelmeing, ... haphazard design. 

## Structure

All MILK documents are structured as a hierarchy of ordered lists. These teirs are designated by the use of headers. Consider the following example.

```
= Header 1
== Header 2
Content
```

```
<ol class="no-enum"><li>
  <h1 class="title">Header 1</h1>
  <ol><li>
    <h2 class="title">Header 2</h2>
    Content
  </li></ol>
</li></ol>
```

## Inline Markup

All inline markup is clearly indicated by the use of ` x.content.x ` pattern, where `x` is a special chracter expressing the markup style to use.

| Markup            | Result                    |
|-------------------|---------------------------|
| `*.strong.*`      | `<strong>strong</strong>` |
| `'.emphasis.*     | `<em>emphasis</em>`       |
| `_.underline._`   | `<u>underline</u>`        |
| `-.strikethru.-`  | `<s>strikethru</s>        |
| `^.superscript.^` | `<sup>superscript</sup>`  |
| `~.subscript.~`   | `<sub>subscript</sub>`    |
| ``.monotype.``    | `<tt>monotype</tt>`       |



## Macros

MILK supports a *macro* language, which allows the document writer full control over the final output.

For example imagine a MILK plugin for figlet.

```
<figlet font=slant size=10>
THIS IS HUGE! 
</figlet>
```

TODO: Should the macro language be XML/XHTML-subset or a HAML like syntax?

As you can see the macro language is just XML/HTML. In fact, the macro langauge is, at it's base, simply a safe subset of HTML.


## Implicit Spans

```
This is <color=green>green</>.
```


## Classifying Markup

Most of the time we do not need to get vey fancy with our customizations. A simple extra class will generally do the job. To this end, any text section can be *classified* simply by sarting the text with a `.classname`.

```
.important This is an important paragraph.
```


## Customizing Markup

**IMPORTANT** Customizing markup is not something to be doen lightly!!! Use it sparingly and only when you have to other good option. It is much better to simply uses classes and modify a sites stylesheets to produce the desired results. In fact, we debated whether it was even wise to allow this level of customization. Implementors may wish to make it a separate API option which can be turned on or off.

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

The `^` symbol indicates a backward lookup of the given tag. If we left the `ol` out if would look to first previous tag, which in this case would be <li>, which would have a very different effect. Conversely we can have the tag look ahead using a `+` character.

```
<+ol display=none/>
= Big Header
```

It is even possible to augment all the tags fo a given tag using `<*tag ...>` notation. But these should alwasy be put at the top or bottom of a document, and usually the bottom to keep them out of the way.

**IMPORTANT** The relative taggin notation is still be fleshed out and could very well change.


## Escape Characters

To escape `<` and `>` simply repeat them, i.e. `<<` becomes `<` and `>>` becomes `>`.

To escape inline markup wrap the content in no-markup markup, i.e. ```*.foo.*```.