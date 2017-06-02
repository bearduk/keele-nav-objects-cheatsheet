# Navigation objects cheatsheet for t4
*Snippets and notes on using t4 nav objects*

*Full details can be found on the [t4 extranet](https://community.terminalfour.com/product-info/terminalfour/80/assets/navigation/)*

- - -

## Optional output for section link OR text link

This is for when you want to give the user the option to use a text url or a t4 Section Link.

```html
<a href="<t4 type="content" name="Section Link" output="linkurl" modifiers=""  /><t4 type="content" name="URL Link" output="normal" modifiers=""  />">  <!-- any content you wish to surround --> </a>
```

## Selective Output Contains T4 tags

The format attribute can contain other T4 tags.

To do this, the "process-format" attribute must be set to true, and the quote symbols in the nested t4 tag must be replaced with &quot;
```html
<t4 type="content" output="selective-output" process-format="true" modifiers="" name="Name" format="<p><t4 type=&quot;lang-var&quot; default-language=&quot;en&quot; en=&quot;Name&quot; ga=&quot;Irish Name&quot; fr=&quot;French Name&quot; /> : $value</p>" />
```

## Output linktext or linkurl

### output name only of section / content link
```html
<t4 type="content" name="element" output="linktext" />
```
### output url only of section / content link
```html
<t4 type="content" name="element" output="linkurl" />
```

## Using linkurl outputs only the link for section links.

Useful when you don't want the default section name and surrounding ahref. Note the href is the t4 linkurl tag (method described elsewhere in this document) with " converted to &quot;
```html
<t4 type="content" output="selective-output" process-format="true" modifiers="" name="Optional Section link" format="<div class=&quot;classname&quot;><a href=&quot;<t4 type=&quot;content&quot; name=&quot;Optional Section link&quot; output=&quot;linkurl&quot; />&quot;>Read more <i class=&quot;fa fa-arrow-right&quot;></i></a></div>" />
```

## Contains quotes

```html
<t4 type="content" output="selective-output" modifiers="" name="Element name" format="<p class=&quot;phone&quot;>Phone: $value</p>" />
```


## Note on outputing media

For occassions when the output needs to be a path rather than a standard img media content layout e.g. when using `data-src` for lazy loading images i.e. loader would be standard src and data-src would be the image path:

```
<!-- Media element formatter -->

By default, Media elements are formatted using the Media content layout on the Media Content type, but if a specific Media content layout is required, it is possible to add the formatter attribute to the T4 tag to specify the layout to use.

<t4 type="content" name="Image" output="normal" modifiers="" formatter="path/*" />

In the example above, the "Image" element, which is a Media element, is formatted using the "path/*" layout on the Media Content type.

```

## Linking between formatters

Example taken from the 2018 A-Z in development:

```html
<a href="<t4 type="content" output="fulltext" formatter="text/html" modifiers="nav_sections" name="Course Title" />"> Link </a>
```
Note that output is 'fulltext' but the formatter is also specified. Unfortunately, this will give a .php output rather than folder url. It may be possible to remove the php filename though some other way, post publish.