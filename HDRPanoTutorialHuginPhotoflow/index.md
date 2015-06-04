---
date: 2015-04-24T10:05:29-05:00
title: TITLE 
sub-title: A pithy sub-title

lede-img: 'pano_heading.jpg'
lede-attribution: "<a href='http://blog.patdavid.net'>Pat David</a>"

author: "Pat David"
author-bio: "A little bit about myself"
author-img: ''

layout: article
---

This post is more a draft of a possible tutorial than a forum thread. The goal is to show how to create a panorama using only free and open source tools.
Through this tutorial I will use the [Hugin](http://hugin.sourceforge.net/) suite for aligning and stitching the shots, and the [PhotoFlow RAW editor](https://github.com/aferrero2707/PhotoFlow) to prepare the initial images and to finalize the processing of the assembled panorama.

<img src="/uploads/default/145/62a03e24e825a19f.png" width="690" height="322"> 

To explain my workflow I will use the image above as an example. It is assembled from six sets of images, each set consisting of three bracketed shots to record the full dynamic range of the scene. The three exposures are assembled separately, and then blended together in the final post-processing.
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

<img src="/uploads/default/136/630af83d91f41735.png" width="384" height="412"> <img src="/uploads/default/137/428b13fe967a0db0.png" width="383" height="246"> 

<img src="/uploads/default/138/0dd071d18e1872fb.png" width="384" height="246"> <img src="/uploads/default/139/3176ba7b18868939.png" width="383" height="247"> 

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

<img src="/uploads/default/140/60ce61b66f5b7251.png" width="667" height="500"> 

1. Click on *Add images* and load all the tiff files included in your panorama. Hugin should automatically determine the lens focal length and the exposure values from the EXIF data embedded in the tiff files. 
2. Click on *Create control points* to let hugin determine the anchor points that will be used to align the images and to determine the lens correction parameters so that all shots overlap perfectly. If the scene contains a large amount of clouds that have likely moved during the shooting, you can try setting the feature matching algorithm to *cpfind+celeste* to automatically exclude non-reliable control points in the clouds.
3. Set the geometric parameters to *Positions and Barrel Distortion* and hit the *Calculate* button.
4. Set the photometric parameters to *High dynamic range, fixed exposure* (since we are going to stitch bracketed shots that have been taken with fixed exposures), and hit the *Calculate* button again.

At this point we can have a first look at the assembled panorama. Hugin provides an OpenGL-based previewer that can be opened by clicking on the on the *GL* icon in the top toolbar (marked with the arrow in the above screenshot). This will open a window like this:

<img src="/uploads/default/141/de380d7c86ab9eb9.png" width="689" height="417"> 

If the shots have been taken handheld and are not perfectly aligned, the panorama will probably look a bit "wavy" like in my example. This can be easily fixed by clicking on the *Straighten* button (at the top of the *Move/Drag* tab). Next, the image can be centered in the preview area with the *Center* and *Fit* buttons.

If the horizon is still not straight, you can further correct it by dragging the center of the image up or down:

<img src="/uploads/default/142/4da68208de9f4bdc.png" width="690" height="417"> 

At this point, one can switch to the *Projection* tab and play with the different options. I usually find the *Cylindrical* projection better than the *Equirectangular* that is proposed by default (the vertical dimension is less "compressed"). For architectural panoramas that are not too wide, the *Rectilinear* projection can be a good option since vertical lines are kept straight.

<img src="/uploads/default/144/8e5a3191572672f9.png" width="690" height="398"> 

If the projection type is changed, one has to click once more on the *Center* and *Fit* buttons.

Finally, you can switch to the *Crop* tab and click on the *HDR Autocrop* button to determine the limits of the area containing only valid pixels.

We are now done with the preview window; it can be closed and we can go back to the main window, in the *Stitcher* tab. Here we have to set the options to produce the output images the way we want. The idea is to blend each bracketed exposure into a separate panorama, and then use **enfuse** to create the final exposure-blended version. The intermediate panoramas, which will be saved along with the enfuse output, are already aligned with respect to each other and can be combined using different type of masks (luminosity, gradients, freehand, etc...).

The *Stitcher* tab has to be configured as in the image below, selecting *Exposure fused from any arrangement* and *Blended layers of similar exposure, without exposure correction*. I usually set the output format to *TIFF* to avoid compression artifacts.

<img src="/uploads/default/143/085b7a1eed618aa0.png" width="592" height="500"> 

The final act starts by clicking on the *Stitch!* button. The input images will be distorted, corrected for the lens vignetting and blended into seamless panoramas. The whole process is likely to take quite long, so it is probably a good opportunity for taking a pause...

At the end of the processing, few new images should appear in the output directory: one with an "_blended_fused.tif" suffix containing the output of the final enfuse step, and few with an "_exposure_????.tif" suffix that contain intermediate panoramas for each exposure value.

#Conclusion
At this point one has all the ingredients for the final post-processing. Since this post is already quite long, I've decided to stop here  and keep the post-processing part for a second episode...
Also, I prefer to get some feedback on this first part before writing the next one. **Is it too basic? Or too detailed?**

By the way, I'm not a native English speaker, so do not hesitate to correct/improve the wording and phrasing in the text!