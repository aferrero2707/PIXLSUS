---
date: 2015-04-24T10:05:29-05:00
title: TITLE 
sub-title: A pithy sub-title

lede-img: 'Into the Fog.jpg'
lede-attribution: "<a href='http://blog.patdavid.net'>Pat David</a>"

author: "Pat David"
author-bio: "A little bit about myself"
author-img: ''

layout: article
---

This is what a markdown file will look like for [PIXLS.US](https://pixls.us).
If you're just starting out, I recommend that you simply copy this file into your own sub-folder for your article and modify it as needed.



## Front Matter
At the top of this markdown file is some "front-matter" contained between a triplet of dashes.  This is the metadata for your post.

<pre>
---
title:
sub-title:

lede-img:
lede-attribution:

author:
author-bio:
author-img:
---
</pre>

These parameters *should* be mostly self-explanatory.  The `lede-img` is the background image across the top of a post, and the `lede-attribution` is the text that will appear below and to the right of the image for attribution.  The `lede-attribution` can contain html elements inside quotes:

`lede-attribution: "<a href='http://blog.patdavid.net'>Pat David</a>"` 


## Markdown
Some simple markdown for reference.

<pre>
Text that is not separated by a blank line will be consolidated.
Paragraph breaks are noted through the use of a blank line.
Newlines will not break it up.
These are all one paragraph.

This should be a new paragraph now.
</pre>

Will render as this:

Text that is not separated by a blank line will be consolidated.
Paragraph breaks are noted through the use of a blank line.
Newlines will not break it up.
These are all one paragraph.

This should be a new paragraph now.

Some simple formatting rules for markdown,

<pre>
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
</pre>
all render as:
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5

Formatting is straightforward:

`*Emphasis*` renders as: *Emphasis*

`**Even more**` renders as: **Even more**

`[Link Text](https://pixls.us)` renders as [Link Text](https://pixls.us)

`[Link Text](https://pixls.us "PIXLS.US is neat!")` renders as [Link Text](https://pixls.us "PIXLS.US is neat!") with a title.

`> Block quote offset text` will render as:

> Block quote offset text.

Which is just a fancier looking blockquote.


### HTML
Markdown will also just pass normal HTML markup straight through without a problem, so it's possible to mix writing straight html markup along with markdown.  This is something we use in the next section.



## PIXLS.US Specific Things
There are a couple of things that I have setup for the site specifically.
Particularly with images.

### Images
Images are actually wrapped inside a `<figure>` element, so an image would look like this:

<pre>
&lt;figure>
&lt;img src="Into the Fog.jpg" alt="Into the Fog"/>
&lt;/figure>
</pre>

Which would render as:

<figure>
<img src="Into the Fog.jpg" alt="Into the Fog"/>
</figure>

To add a caption to an image, simply add the `<figcaption>` tag:

<pre>
&lt;figure>
&lt;img src="Into the Fog.jpg" alt="Into the Fog"/>
&lt;figcaption>Into the Fog by Pat David&lt;/figcaption>
&lt;/figure>
</pre>

Which would produce:

<figure>
<img src="Into the Fog.jpg" alt="Into the Fog"/>
<figcaption>Into the Fog by Pat David</figcaption>
</figure>

#### Bigger Images
There is also a method for increasing an image size to give it more prominence on the page.  Adding the class `big-vid` to the figure element will increase the image size:

<pre>
&lt;figure class="big-vid">
&lt;img src="Into the Fog.jpg" alt="Into the Fog"/>
&lt;figcaption>Into the Fog by Pat David&lt;/figcaption>
&lt;/figure>
</pre>

Will render as:

<figure class="big-vid">
<img src="Into the Fog.jpg" alt="Into the Fog"/>
<figcaption>Into the Fog by Pat David</figcaption>
</figure>

#### Image Sizes
There are three main image sizes used on the site:

* The smallest version has a max width of 640px.
* The big-vid version has a max width of 960px.
* Full-size images (preferably between 1650px to 2048px wide).

When in doubt, simply include the full-size images (in case a larger size is wanted during editing).


### Commands
When commands need to be referenced, there is a special class used called `Cmd` to help differentiate the command from the surrounding text:

&lt;span class="Cmd">Colors → Desaturate…&lt;/span>

Which will look like:

<span class="Cmd">Colors → Desaturate…</span>



### Aside
The same is true for any 'asides' that need to be offset from the actual text:

&lt;p class="aside">This is some section of text that needs to be offset from the rest in some way.&lt;p>

<p class="aside">This is some section of text that needs to be offset from the rest in some way.</p>
