---
date: 2015-04-24T10:05:29-05:00
title: Color management made easy
sub-title: Color management made easy

lede-img: 'pano_lede.png'
lede-attribution: "<a href='http://photoflowblog.blogspot.fr'>Andrea Ferrero</a>"

author: "Andrea Ferrero"
author-bio: "Husband, father, scientist and spare-time photographer"
author-img: ''

layout: article
---

# Introduction
The vast majority of imaging devices that we are commonly using (digital cameras, flatbed scanners, computer screens, inkjet printers) represent the image data in terms of RGB triplets. However, the relation between a given *COLOR* and its *RGB REPRESENTATION* is not unique. Each device will use different RGB values for the same color.

If those differences are not correctly taken into account and COMPENSATED, all sort of weird things can happen... have you ever wondered why your printed photos look totally different from what your image editor is showing on the screen? *THAT'S BECAUSE THEY DON'T TALK THE SAME RGB LANGUAGE!* It is as if your "English" image file was talking with a "French" computer screen and a "German" printer: they don't understand each other, and therefore the message gets "distorted"... unless it gets properly *TRANSLATED* into the native language of each device.

This looks like a complete mess! Fortunately, there is a simple and standardized way out of this "color anarchy": **color management**.
We call *color management* "**the set of tools that guarantee a consistent color reproduction across multiple input/output devices**". For example, color management guarantees that an image shown on the computer screen matches *as closely as possible* a printed version of the same image.

---

I know it's not technical, but I can't help feel that a gentle introduction would help the average reader understand better what they're about to get into... :)

Possibly consider answering these primary questions:

*Why*
Why should the average photographer care about color management?

*What*
What is it in relation to what they're accustomed to?  What does it mean to manage color?  etc...

*Who*
Who should really care about color management?  Everyone (I know *everyone* is a likely answer)?  Practically speaking, who would benefit the most and why (see above)?

These are just the basics, but they're something important as a lede to entice the reader to actually read through the material you're about to hit them over the head with (and believe me - some of them will feel this way).

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
