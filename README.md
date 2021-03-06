# Navigation objects cheatsheet for t4
*Snippets and notes on using t4 nav objects*

*Full details can be found on the [t4 extranet](https://community.terminalfour.com/product-info/terminalfour/80/assets/navigation/)*

- - -

## Optional output for section link OR text link

This is for when you want to give the user the option to use a text url or a t4 Section Link.

```html
<a href="<t4 type="content" name="Section Link" output="linkurl" modifiers=""  /><t4 type="content" name="URL Link" output="normal" modifiers=""  />">  <!-- any content you wish to surround --> </a>
```

## Selective Output for Images and Files

Selective output does not currently work with file or image elements. (Nov 2017, v7).

> **NOTE** that if outputting a path to an image (File element) then you could use an alt field for the selective output and then place the path to the image and the surrounding img tag etc within it. $value would be the alt.

> **NOTE** that you *can* use e.g. a checkbox for selective output and place a normal file or image tag within that e.g.

```html
<t4 type="content" output="selective-output" process-format="true" modifiers="medialibrary, nav_sections" name="Show further information document link" format="<a href=&quot;<t4 type=&quot;content&quot; name=&quot;Further Information document&quot; output=&quot;file&quot; modifiers=&quot;&quot; />&quot;>Supplementary Information</a>" />
```

## Selective Output Contains T4 tags

The format attribute can contain other T4 tags.

To do this, the "process-format" attribute must be set to true, and the quote symbols in the nested t4 tag must be replaced with &quot;
```html
<t4 type="content" output="selective-output" process-format="true" modifiers="" name="Name" format="<p><t4 type=&quot;lang-var&quot; default-language=&quot;en&quot; en=&quot;Name&quot; ga=&quot;Irish Name&quot; fr=&quot;French Name&quot; /> : $value</p>" />
```

## Selective output example using paths

Useful for when you want an optional output for something like a picture/srcset element

```html
<t4 type="content" output="selective-output" process-format="true" modifiers="medialibrary, nav_sections" name="Header image 1600x600" format="<div class=&quot;module banner align-centre&quot;>
			<picture>
				<source srcset=&quot;<t4 type=&quot;content&quot; name=&quot;Header image 1600x600&quot; output=&quot;normal&quot; modifiers=&quot;&quot; formatter=&quot;path/*&quot; />&quot;>
				<img src=&quot;<t4 type=&quot;content&quot; name=&quot;Header image 1600x600&quot; output=&quot;normal&quot; modifiers=&quot;&quot; formatter=&quot;path/*&quot; />&quot; alt=&quot;<t4 type=&quot;content&quot; name=&quot;Header image alt text&quot; output=&quot;normal&quot; modifiers=&quot;&quot;  />&quot;>
			</picture>
			<div class=&quot;inner&quot;>

			</div>
	</div>" />
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

## Outputting paths

You can use the path/* to output path. This is useful e.g. if you want to use the picture elemenet for responsive image sizes..

```html
<picture>
<source srcset="T4 TAG PATH FOR IMAGE">
<img src="T4 TAG PATH FOR DEFAULT IMAGE" alt="T4 TAG FOR ALT TEXT">
</picture>
```

> Don't forget, `<picture>` needs a polyfill for some browsers.

For a Media element, you can use: 
```html
<t4 type="content" name="Media Image" output="normal" modifiers="" formatter="path/*" />
```

For a File element, you can use (note that you output it as a file but then there's no need for a formatter becuase the default layout is path anyway):
```html
<t4 type="content" name="Image uploaded by user directly to content type" output="file" modifiers="" />
```

You would then need to enter the alt text in a separate field though i.e.
```html
<t4 type="content" name="Alt text" output="normal" modifiers=""  />
```

## Linking between formatters

If you want to do this-- make an A-Z from a selection of sections/content but use a different formatter for the A-Z list, do this:

- create a formatter on the content type e.g. 'text/az'
- create a nav object that pulls in the content. Important, you must untick the 'ignore data ordering' as that only works if you're looking at one section. If you want to pull in from a branch, it must be unticked. You must also enter 'Last Modified' for the 'Content Type element for date'. Note that you'll need to sort the A-Z manually using JS or similar.
- IMPORTANT: in the text/az, you must add a 'Return to Index' navigation object e.g. ```<!-- navigation object : UG 2018 Return to Index --><t4 type="navigation" id="355"/>```


Another much messier method used for a while in the 2018 A-Z in development:

```html
<a href="<t4 type="content" output="fulltext" formatter="text/html" modifiers="nav_sections" name="Course Title" />"> Link </a>
```
Note that output is 'fulltext' but the formatter is also specified. Unfortunately, this will give a .php output rather than folder url. It may be possible to remove the php filename though some other way, post publish.

## Top Content

Top Content navigation object only works with content that has a date associated with it - usually a date element within the content type. If you updated the object and typed "Last Modified" in the "Content Type element for date" then it will find the last 10 pieces of content based on the "Last Modified" date of the content.
Alternatively, you may be better off using Publish to One File. It displays all content below the section to which it is added, publishing a branch of content on one page. It will follow the TERMINALFOUR Site Manager hierarchy and pull in the content from those sections.

## Publish to One File

See previous comment on Top Content. For News, it may be simpler to just render out the entire branch. It lets you specify the formatter too and also paginates if required.

## Publish lists as arrays

Sometimes you may want to output an array from a t4 list. Commonly done in JSON format. You can do this in two ways

> Programmable layout e.g. 

```js
	var modeOfStudyTag = '<t4 type="content" name="Mode of study" output="normal" modifiers="striptags"  />';

	var modeOfStudy = BrokerUtils.processT4Tags (dbStatement, publishCache, section, content, language, isPreview, modeOfStudyTag);

	// modeOfStudy Array build
	// grab the comma list from t4
	var modeofStudyString = new String(modeOfStudy); // e.g. 'undergradute, postgraduate taught'
	// remove spaces after commas
	var modeofStudyStringCompress = modeofStudyString.replace(/\s*,\s*/g, ","); // e.g. 'undergradute,postgraduate taught'
	// create an array
	var modeOfStudyArray = modeofStudyStringCompress.split(',');
	var modeOfStudyOutput = JSON.stringify(modeOfStudyArray);
```

or if programmable layouts are not available, it may be feasible to hack this way

> Normal formatter

In the list, add quotes to the Name value e.g. row 1 would have 

name: "School of whatever" and value: School of whatever

Then in the output formatter you can output the name instead of the default value e.g.

```html
<t4 type="content" name="Element Name" output="normal" display_field="name" />
```

You would need to surround that with the start and end of an array of course. ```[ t4 tag here ]```

## SectionMetaDescription

This has been enabled globally on the system and set in the Hierarchy Handler 'Metadata Type'.
There is a Content Type under Global called 'SectionMetaDescription'. This houses as many formatters as you like and can handle programmable layouts i.e. menu levels etc.

NOTE that the Content Type has a template_type of either 10 (to edit) but always 30 to act as a system template.

Any field/elements added to the SectionMetaDescription will appear on the Section General tab.

Section meta aproach:

```
SELECT * from template WHERE id=[Put the SectionMeta Description Template ID in here]

UPDATE template SET template_type=10 WHERE id=<TEMPLATE ID>

UPDATE template SET template_type=30 WHERE id=<TEMPLATE ID>
```

> Note that once an element has been added to the SectionMeta Description, the name can not be changed later. You must remove and add a new one.

## Getting a unique id from a piece of content

Useful to tag several instances of a Content Type on a page as unique

```
unique<t4 type="meta" meta="content_id" />
```

would output e.g. `unique37584`. This would be used as a CSS class or ID for example.

## Adding an item to a t4 list using SQL

So to add a new value to the list mentioned:

If you hover over the edit button of the list, it should mention what id it is.
Note lid=5

Alternatively. database lookup of list:

`select * from predefined_list WHERE name like '%CRSE%'`

Found id = 128

(Just substitute CRSE with your search)

Lookup of elements in current list:

 `SELECT * FROM predefined_list_entry WHERE list_id = 128 ORDER BY entry_id ASC`
Based on returned set, I took the highest entry_id and added 1 = 638

Prototype - not run on database

Following order of columns in table from previous select query
`INSERT INTO predefined_list_entry  (list_id, entry_id, name, default_selected, sub_list_id, language, value) `

Note: Name, language and value are strings so we surround their values with quotes '.
Default selected: Is this element the default value of the list? 1 if yes, otherwise 0.
Sub_list_id: Is this element a list of another list? If so, add id for of the sub list, otherwise 0.
Final Insert Statement

`INSERT INTO predefined_list_entry VALUES (128, 638, '3 years / 4 years with international or placement year', 0, 0, 'en', '3 years / 4 years with international or placement year')`
