---
date: 2015-04-24T10:05:29-05:00
title: A FL/OSS panorama
sub-title: Creating panoramas with Hugin and PhotoFlow

lede-img: 'pano_lede.png'
lede-attribution: "<a href='http://photoflowblog.blogspot.fr'>Andrea Ferrero</a>"

author: "Andrea Ferrero"
author-bio: "Husband, father, scientist and spare-time photographer"
author-img: ''

layout: article
---

# RGB is not a Color

Show examples of the same colors in different color spaces, and show why ICC profiles are needed to correctly "interpret" RGB values

Further reading: http://www.cambridgeincolour.com/tutorials/color-spaces.htm

# The GAMUT: how much color do you need?

Explain what the GAMUT of a given colorspace is, why a wider gamut is sometimes better and sometimes dangerous.

Make practical examples of which colorspace to use when?

# Middle gray is not middle density: the origin of gamma encoding

Explain why gamma encoding was introduced to reflect the way the human vision system perceive dark and light tones.

Show the impact of different gamma encodings in different colorspaces on the practical aspects of photo editing. For example, the same RGB curve does not give the same output in sRGB and ProPhotoRGB colorpsaces...

Further reading: http://www.cambridgeincolour.com/tutorials/gamma-correction.htm

# From color to grayscale: the influence of the RGB colorspace on grayscale conversion

Show how individual RGB channels, usually mixed with varying proportions to convert the image to grayscale, look quite different in various colorspaces. Therefore, the result of the grayscale conversion depends on the input RGB colorspace...
