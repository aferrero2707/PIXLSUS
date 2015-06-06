---
date: 2015-04-24T10:05:29-05:00
title: Creating HDR panoramas with Hugin and PhotoFlow
sub-title: A pithy sub-title

lede-img: 'pano_heading.jpg'
lede-attribution: "<a href='http://blog.patdavid.net'>Pat David</a>"

author: "Andrea Ferrero"
author-bio: "Husband, father, scientist and spare-time photographer"
author-img: ''

layout: article
---

The goal of this tutorial is to show how to create a sort-of-HDR panoramic image using only Free and Open Source tools. To explain my workflow I will use the image below as an example. 
This panorama was obtained from the combination of six views, each consisting of three bracketed shots at -1EV, 0EV and +1EV exposure. The three exposures are stitched together with the [Hugin](http://hugin.sourceforge.net/) suite, and then exposure-blended with [enfuse](). The [PhotoFlow RAW editor](https://github.com/aferrero2707/PhotoFlow) is used to prepare the initial images and to finalize the processing of the assembled panorama.
The final result of the post-processing is anticipated below (click to compare with the simple +1EV exposure).

<figure>
<img src="pano_final.png" data-swap-src="pano_+1EV.png" alt="Final result" width="690" height="322"> 
<figcaption>
Final result of the panorama editing (click to compare to simple +1EV exposure)
<\figcaption>
<\figure>

In this case I have used the brightest image for the foreground, the darkest one for the sky and clouds, and and exposure-fused one for a seamless transition between the two.

The rest of the post will show how to get there...

Before we continue, let me advise you that I'm not a pro, and that the tips and "recommendations" that I'll be giving in this post are mostly derived from trial-and-error and common sense. Feel free to correct/add/suggest anything... **we are all here to learn**! 

# Taking the shots
Shooting a panorama requires a bit of preparation and planning, to make sure that one can get the best out of Hugin when stitching the shots together. Here is my personal "checklist":

* set the camera to manual focus, so that the focus plane is the same for all shots
* make sure that each frame has sufficient overlap with the previous one (something between 1/2 and 1/3 of the total area), so that hugin can find enough control points to align the images and determine the lens correction parameters
* when taking the shots, try to follow as much as possible a straight line (keeping for example the horizon at the same height in your viewfinder); if you have a tripod, use it!
* to maximize the angle of view, frame vertically for an horizontal panorama (and vice-versa for a vertical one)
* frame the shots a bit wider than needed, to avoid bad surprises when cropping the stitched panorama
* take all shots with a fixed exposure (manual or locked) to avoid luminance variations that might not be fully compensated by hugin
* if you shoot during a sunny day, the brightness might vary significantly across the whole panorama; in this case, take three or more bracketed exposures for each view (we will see later how to blend them in the post-processing)

# Processing the RAW files
If you plan to create the panorama starting from the in-camera Jpeg images, you can safely skip this section. On the other hand, if you are shooting RAW you will need to process and prepare all the input images for Hugin. In this case it is important to make sure that the RAW processing parameters are exactly the same for all the shots. The best is to adjust the parameters on one reference image, and then batch-process the rest of the images using those settings.

Loading and processing a RAW file is rather easy:

1. click the "Open" button and choose the appropriate RAW file from your hard disk; the image preview area will show at this point a grey and rather dark image
2. add a "RAW developer" layer; a configuration dialog will show up which allows to access and modify all the typical RAW processing parameters (white balance, exposure, color conversion, etc... see screenshots below).

<img src="pf_raw_wb.png" width="384" height="412" style="float:left; margin-right:5px;" > 
<img src="pf_raw_exposure.png" width="383" height="246" style="float:left; margin-right:5px;" > 

<img src="pf_raw_demo.png" width="384" height="246" style="float:left; margin-right:5px;"> 
<img src="pf_raw_output.png" width="383" height="247" style="float:left; margin-right:5px;"> 

More details on the RAW processing in PhotoFlow can be found in [this tutorial](http://photoflowblog.blogspot.fr/2014/09/tutorial-how-to-process-raw-image-in.html).

Once the result is ok the RAW processing parameters need to be saved into a preset. This can be done following a couple of simple steps:

1. select the "RAW developer" layer and click on the "Save" button below the layers list widget (at the bottom-right of the photoflow's window)
2. a file chooser dialog chooser dialog will pop-up, where one has to choose an appropriate file name and location for the preset and then click "Save"; **the preset file name must have a ".pfp" extension**

The saved preset needs then to be applied to all the RAW files in the set. Under Linux, PhotoFlow comes with an handy script that automates the process. The script is called *pfconv* and can be found [here](https://github.com/aferrero2707/PhotoFlow/blob/master/tools/pfconv). It is a wrapper around the *pfbatch* and *exiftool* commands, and is used to process and convert a bunch of files to TIFF format. Save the script in one of the folders included in your `PATH` environment variable (for example `/usr/local/bin`) and make it executable:

    sudo chmod u+x /usr/local/bin/pfconv

Processing all RAW files of a given folder is quite easy. Assuming that the RAW processing preset is stored in the same folder under the name `raw_params.pfp`, run this commands in your preferred terminal application:

    cd panorama_dir
    pfconv -p raw_params.pfp *.NEF

Of course, you have to change `panorama_dir` to your actual folder and the `NEF` extension to the one of your RAW fles.

Now go for a cup of coffee, and be patient... a panorama with three or five bracketed shots for each view can easily have more than 50 files, and the processing can take half an hour or more. Once the processing completed, there will be one tiff file for each RAW image, an the fun with Hugin can start!

**TODO: add instructions for batch-processing in Darktable and RawTherapee.**

# Stitching the shots into a seamless panorama with Hugin
Hugin is a powerful and free software suite for stitching multiple shots into a seamless panorama, and more. Under Linux, Hugin can be usually installed through the package manager of your distribution. In the case of Ubuntu-based distros it can be usually installed with

    sudo apt-get install hugin

If you are running Hugin for the first time, I suggest to switch the interface type to **Advanced** in order to have full control over the available parameters. 

The first steps have to be done in the *Photos* tab:

<img src="hugin_1.png" width="667" height="500"> 

1. Click on *Add images* and load all the tiff files included in your panorama. Hugin should automatically determine the lens focal length and the exposure values from the EXIF data embedded in the tiff files. 
2. Click on *Create control points* to let hugin determine the anchor points that will be used to align the images and to determine the lens correction parameters so that all shots overlap perfectly. If the scene contains a large amount of clouds that have likely moved during the shooting, you can try setting the feature matching algorithm to *cpfind+celeste* to automatically exclude non-reliable control points in the clouds.
3. Set the geometric parameters to *Positions and Barrel Distortion* and hit the *Calculate* button.
4. Set the photometric parameters to *High dynamic range, fixed exposure* (since we are going to stitch bracketed shots that have been taken with fixed exposures), and hit the *Calculate* button again.

At this point we can have a first look at the assembled panorama. Hugin provides an OpenGL-based previewer that can be opened by clicking on the on the *GL* icon in the top toolbar (marked with the arrow in the above screenshot). This will open a window like this:

<img src="hugin_2.png" width="689" height="417"> 

If the shots have been taken handheld and are not perfectly aligned, the panorama will probably look a bit "wavy" like in my example. This can be easily fixed by clicking on the *Straighten* button (at the top of the *Move/Drag* tab). Next, the image can be centered in the preview area with the *Center* and *Fit* buttons.

If the horizon is still not straight, you can further correct it by dragging the center of the image up or down:

<img src="hugin_3.png" width="690" height="417"> 

At this point, one can switch to the *Projection* tab and play with the different options. I usually find the *Cylindrical* projection better than the *Equirectangular* that is proposed by default (the vertical dimension is less "compressed"). For architectural panoramas that are not too wide, the *Rectilinear* projection can be a good option since vertical lines are kept straight.

<img src="hugin_4.png" width="690" height="398"> 

If the projection type is changed, one has to click once more on the *Center* and *Fit* buttons.

Finally, you can switch to the *Crop* tab and click on the *HDR Autocrop* button to determine the limits of the area containing only valid pixels.

We are now done with the preview window; it can be closed and we can go back to the main window, in the *Stitcher* tab. Here we have to set the options to produce the output images the way we want. The idea is to blend each bracketed exposure into a separate panorama, and then use **enfuse** to create the final exposure-blended version. The intermediate panoramas, which will be saved along with the enfuse output, are already aligned with respect to each other and can be combined using different type of masks (luminosity, gradients, freehand, etc...).

The *Stitcher* tab has to be configured as in the image below, selecting *Exposure fused from any arrangement* and *Blended layers of similar exposure, without exposure correction*. I usually set the output format to *TIFF* to avoid compression artifacts.

<img src="/uploads/default/143/085b7a1eed618aa0.png" width="592" height="500"> 

The final act starts by clicking on the *Stitch!* button. The input images will be distorted, corrected for the lens vignetting and blended into seamless panoramas. The whole process is likely to take quite long, so it is probably a good opportunity for taking a pause...

At the end of the processing, few new images should appear in the output directory: one with an "_blended_fused.tif" suffix containing the output of the final enfuse step, and few with an "_exposure_????.tif" suffix that contain intermediate panoramas for each exposure value.

#Manual exposure blending with PhotoFlow
*Very often, photo editing is all about getting **what your eyes have seen** out of **what your camera has captured**.* 

The image that will be edited through this tutorial is no exception: the human vision system can "compensate" large luminosity variations and can "record" scenes with a wider dynamic range than your camera sensor. In the following I will attempt to restore such large dynamics by combining under- and over-exposed shots together, in a way that does not produce unpleasing halos or artifacts. Nevertheless, I have intentionally pushed the edit a bit "over the top" in order to better show how far one can go with such a technique. 

This second part introduces a certain number of quite general editing ideas, mixed with details specific to their realization in PhotoFlow. Most of what is described here can be reproduced in GIMP with little extra effort, but without the ease of non-destructive editing.

The steps that I followed to go from one to the other can be more or less outlined like that:

1. take the foreground from the +1EV version and the clouds from the -1EV version; use the exposure-blended Hugin output to improve the transition between the two exposures
2. apply an S-shaped tonal curve to increase the overall brightness and add contrast. 
3. apply a combination of the *a* and *b* channels of the CIE-Lab colorspace in **overlay** blend mode to give more "pop" to the green and yellow regions in the foreground

The image below shows side-by-side three of the output images produced with Hugin at the end of the first part. The left part contains the brightest panorama, obtained by blending the shots taken at +1EV. The right part contains the darkest version, obtained from the shots taken at -1EV. Finally, the central part shows the result of running the **enfuse** program to combine the -1EV, 0EV and +1EV panoramas. 

<img src="/uploads/default/original/1X/4f0a582a69c8fd4d662f0415824cfa559b5af27a.png" width="690" height="322">
pano_exp_comp.png

## Exposure blending in general
In scenes that exhibit strong brightness variations, one often needs to combine different exposures in order to compress the dynamic range so that the overall contrast can be further tweaked without the risk of loosing details in the shadows or highlights.

In this case, the name of the game is "seamless blending", i.e. combining the exposures in a way that looks natural, without visible transitions or halos.
In our specific case, the easiest thing would be to simply combine the +1EV and -1EV images through some smooth transition, like in the example below.

<img src="/uploads/default/original/1X/df860834755d759bbd7debfd0210421372dabcd9.png" width="690" height="322"> 
pano_+1EV_-1EV_blend.png

The result is not too bad, however it is very difficult to avoid some brightening of the bottom part of the clouds (or alternatively some darkening of the hills), something that will most likely look artificial even if the effect is subtle (our brain will recognize that something is wrong, even if one cannot clearly explain the reason...). We need something to "bridge" the two images, so that the transition looks more natural. 

At this point it is good to recall that the last step performed by Hugin was to call the **enfuse** program to blend the three bracketed exposures. The enfuse output is somehow intermediate between the -1EV and +1EV versions, however a side-by-side comparison with the 0EV image reveals the subtle and sophisticated work done by the program: the foreground hill is brighter and the clouds are darker than in the 0EV version. And even more importantly, this job is done without triggering any alarm in your brain! Hence, the enfuse output is a perfect candidate to improve the transition between the hill and the sky.

<img src="/uploads/default/original/1X/71735d05ae085d287dd8744c7e7bf64af60524ef.png" width="690" height="322"> 
<img src="/uploads/default/original/1X/f52c16ab8c52684acf82df1814ab54b626d5d2f2.png" width="690" height="322"> 
pano_enfuse.png
pano_0EV.png
*Caption: Enfuse output (click to see 0EV version)*

## Exposure blending in PhotoFlow
It is time to put all the stuff together. First of all, we should open **PhotoFlow** and load the +1EV image. Next we need to add the enfuse output on top of it: for that you first need to add a new layer and choose the *Open image* tool from the dialog that will open up (see below).

<img src="/uploads/default/original/1X/d895f96511e8ac18eb75996280ef71fd44f1d6af.png" width="690" height="415"> 
pf_add_layer_edit.png

After clicking the "OK" button, a new layer will be added and the corresponding configuration dialog will be shown. There you can choose the name of the file to be added; in this case, choose the one ending with "_blended_fused.tif" among those created by Hugin:

<img src="/uploads/default/original/1X/83c1321c552937ba8b5022b6f787885e5ba4955d.png" width="576" height="348"> 
pf_open_image_edit.png

## Layer masks: theory (a bit) and practice (a lot)

For the moment, the new layer completely replaces the background image. This is not the desired result: instead, we want to keep the hills from the background layer and only take the clouds from the "_blended_fused.tif" version. In other words, we need a **layer mask**.

To access the mask associated to the "enfuse" layer, double-click on the small gradient icon next to the name of the layer itself. This will open a new tab with an initially empty stack, where we can start adding layers to generate the desired mask.

<img src="/uploads/default/original/1X/f51b9344fe539b9895b9e9ffa00d386587807de6.png" width="690" height="417"> 
pf_enfuse_before_blend_edit.png

In PhotoFlow, masks are edited the same way as the rest of the image: through a stack of layers that can be associated to most of the available tools. In this specific case, we are going to use a combination of gradients and curves to create a smooth transition that follows the shape of the edge between the hills and the clouds. The technique is explained in detail in [this screencast](https://www.youtube.com/watch?v=kapppq-PbTk). To avoid the boring and lengthy procedure of creating all the necessary layers, you can download  [this preset file](http://aferrero2707.github.io/PhotoFlow/data/presets/gradient_modulation.pfp) and load it as shown below:

<img src="/uploads/default/original/1X/89db61f9c049c91dac7d03e9a8b5b14f00f93178.png" width="690" height="327"> 
pf_enfuse_mask_initial.png

The mask is initially a simple vertical linear gradient. At the bottom (where the mask is black) the associated layer is completely transparent and therefore hidden, while at the top (where the mask is white) the layer is completely opaque and therefore replaces anything below it. Everywhere in between, the layer has a degree of transparency equal to the shade of gray in the mask.

In order to show the mask, activate the "show active layer" radio button below the preview area, and then select the layer that has to be visualized. In the example above, I am showing the output of the topmost layer in the mask, the one called "transition". Double-clicking on the name of the "transition layer allows to open the corresponding configuration dialog, where the parameters of the layer (a [**curves** adjustment](http://photoflowblog.blogspot.fr/2014/09/tutorial-using-curves-tool-in-photoflow.html) in this case) can be modified. The curve is initially a simple diagonal: output values exactly match input ones.

If the rightmost point in the curve is moved to the left, and the leftmost to the right, it is possible to modify the vertical gradient and the reduce the size of the transition between pure black and pure white, as shown below:

<img src="/uploads/default/original/1X/1a51348409d8bedfbe6d3ee9d1695ba085d0cd0e.png" width="690" height="417"> 
pf_transition_example.png

We are getting closer to our goal of revealing the hills from the background layer, by making the corresponding portion of the mask purely black. However, the transition we have obtained so far is straight, while the contour of the hills has a quite complex curvy shape... this is where the second **curves** adjustment, associated to the "modulation" layer, comes into play.

As one can see from the screenshot above, between the bottom gradient and the "transition" curve there is a group of three layers: an **horizontal** gradient, a modulation curve and **invert** operation. Moreover, the group itself is combined with the bottom vertical gradient in [**grain merge**](http://docs.gimp.org/en/gimp-concepts-layer-modes.html) blending mode.

Double-clicking on the "modulation" layer reveals a tone curve which is initially flat: output values are always 50% independently of the input. Since the output of this "modulation" curve is combined with the bottom gradient in **grain merge** mode, nothing happens for the moment. However, something interesting happens when a new point is added and dragged in the curve: the shape of the mask matches exactly the curve, like in the example below.

<img src="/uploads/default/original/1X/129554243292bd9a31f92ff217ed4b57ed63694a.png" width="690" height="417"> 
pf_modulation_example.png

## The sky/hills transition
The technique introduced above is used here to create a precise and smooth transition between the sky and the hills. As you can see, with a sufficiently large number of points in the modulation curve one can precisely follow the shape of the hills:

<img src="/uploads/default/original/1X/1d80438c888a895410ecd404cc846fa59c327777.png" width="690" height="311"> 
pf_enfuse_mask.png

The result of the blending looks like that (click the image to see the initial +1EV version):

<img src="/uploads/default/original/1X/6c5f2133f045bb407c47217566702c38fd5cbb7b.png" width="690" height="328"> 
pano_enfuse_blended.png
<img src="/uploads/default/original/1X/a3c20fbfe940618f76a57f89c7d726b4405baead.png" width="690" height="328"> 
pano_+1EV.png
*Caption: enfuse output blended with the +1EV image (click image to see the initial +1EV version)*

The sky looks already much denser and saturated in this version, and the clouds have gained in volume and tonal variations. However, the -1EV image looks even better, therefore we are going to take the sky and clouds from it. 

To include the -1EV image we are going to follow the same procedure done already in the case of the enfuse output:

1. add a new layer of type "Open image" and load the -1EV Hugin output (I've named this new layer "sky")
2. open the mask of the newly created layer and add a transition that reveals only the upper portion of the image

Fortunately we are not obliged to recreate the mask from scratch. PhotoFlow includes a feature called **layer cloning**, which allows to **dynamically** copy the content of one layer into another one. Dynamically in the sense that the pixel data gets copied *on the fly*, such that the destination always reflects the most recent state of the source layer.

After activating the mask of the "sky" layer, add a new layer inside it and choose the "clone layer" tool (see screenshot below).

<img src="/uploads/default/original/1X/ecd966ed66053d6b4e32a13ea8f2db245a6a1765.png" width="661" height="500"> 
pf_clone_layer.png

In the tool configuration dialog that will pop-up, one has to choose the desired source layer among those proposed in the list under the label "Layer name". The generic naming scheme of the layers in the list is "[root group name]/root layer name/OMap/[mask group name]/[maks layer name]", where the items inside square brackets are optional. 

<img src="/uploads/default/original/1X/1d030a38af7cca9143e7a1f2da995cc402557aad.png" width="470" height="398"> 
pf_sky_mask_clone_layer.png

In this specific case, I want to apply a smoother transition curve to the same base gradient already used in the mask of the "enfuse" layer. For that we need to choose "enfuse/OMap/gradient modulation (blended)" in order to clone the output of the "gradient modulation" group **after the *grain merge* blend**, and then add a new **curves** tool above the cloned layer:

<img src="/uploads/default/original/1X/fa068f813e689e4f26224b889876e7b223d2111b.png" width="690" height="296"> 
pf_sky_mask.png

The result of all the efforts done up to now is shown below; it can be compared with the initial starting point by clicking on the image itself:

<img src="/uploads/default/original/1X/ef30f1c8eda320bbca5aa5f0cd8446e8b3d0f038.png" width="690" height="322"> 
<img src="/uploads/default/original/1X/a3c20fbfe940618f76a57f89c7d726b4405baead.png" width="690" height="328"> 
pano_sky_blended.png
pano_+1EV.png
Caption: edited image after blending the upper portion of the -1EV version through a layer mask. Click to see the initial +1EV image.

We are not quite done yet, as the image is still a bit too dark and flat, however this version will "tolerate" some contrast and luminance boost much better than a single exposure. In this case I've added a **curves** adjustment at the top of the layer's stack, and I've drawn an S-shaped RGB tone curve as shown below:

<img src="/uploads/default/original/1X/c1caee94f739a0fdf6c1341031d7b324400003e4.png" width="348" height="500"> 
pf_tone_curve_edit.png

The effect of this tone curve is to increase the overall brightness of the image (the middle point is moved to the left) and to compress the shadows and highlights without modifying the black and white points (i.e. the extremes of the curve). This curve definitely gives "pop" to the image (click to see the version before the tone adjustment):

<img src="/uploads/default/original/1X/a9f421cba5b9fd10cbfce0a974e487f8490fc2c6.png" width="690" height="322"> 
<img src="/uploads/default/original/1X/ef30f1c8eda320bbca5aa5f0cd8446e8b3d0f038.png" width="690" height="322"> 
pano_contrast.png
pano_sky_blended.png
Caption: result of the S-shaped tonal adjustment (click the image to see the version before the adjustment)

However, this comes at the expense of an overall increase in the color saturation, which is a typical side effect of RGB curves. While this saturation boost looks quite nice in the hills, the effect is rather disastrous in the sky. The blue as turned electric, and is far from what a nice, saturated blue sky should look like!

We are going to fix the colors and add a bit more "pop" to the foreground in the third, and last part of the tutorial...

#Conclusion
