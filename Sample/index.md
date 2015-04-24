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

```
Text that is not separated by a blank line will be consolidated into a single paragraph.
So paragraph breaks are noted through the use of a blank line.

This should be a new paragraph now.
```
