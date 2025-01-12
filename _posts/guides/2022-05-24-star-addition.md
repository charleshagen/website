---
layout: post
categories: guides
tags: advanced
title: Independent Starless Processing
description: >
    A comprehensive guide to separating, independently processing, generating synthetic narrowband stars, and introducing stars into any image.
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/thumbnail_x0,25.jpg
sitemap: true
excerpt_separator: <!--more-->
---
<!--more-->

- _
{:toc}

## Background 

<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/magenta_stars.jpg" alt="Graphical interface"/>
    <p class="figcaption" style="margin-top: 0.25rem;">The Hubble Palette “Magenta Stars” - A common problem with the SHO color combination</p>
</div>

When processing astronomical images, especially when using unusual dynamic narrowband palettes, shooting in areas of high star density, or when the data has significant differences in signal, it can be advantageous to process the image with the stars separated from the primary target signal or to simply replace the stars altogether. With traditional methods, this would mean generating countless masks, tweaking, and generally limiting your processing potential for each individual image with a high likelihood that the stars will still have aberrations post-combination. 

This article details a very simple and effective method to properly separate and later reintroduce stars into any image without artifacts while being able to process the separated images independently. It will also discuss how to generate synthetic “natural” stars from a narrowband dataset which can vastly improve the look of narrowband stars. 

<hr>

<h3 style="margin-top:1.5rem">Process Icons</h3>
<a class="btn btn-primary" href="https://nightphotons.s3.amazonaws.com/assets/process_icons/star_addition.xpsm">Download</a>

<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 1500/1130; margin-bottom: 20px; margin-top: 40px; width:90%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/after.jpg" alt="after">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/before.jpg" alt="before" style="opacity:0;">
  <span class="center button right icon-arrow-right2"></span>
  <span class="center button left icon-arrow-left2"></span>
</div>

Before and After single channel indpendent starless processing
{:.figcaption}

<hr>

## What is Relinearization?
The “relinearization” technique, as I call it, is the core of artifact-free star addition.  The idea of linearization is to take nonlinear data, apply a stretch such that it is in a pseudo-linear state, make any adjustments, and bring it back into the nonlinear space with the inverse of the initial stretch.  This allows the user to use Pixelmath to add certain datasets to one another while retaining the detail of both and avoid clipping or having to deal with complex, finicky masks. Keep in mind, though, this technique does not bring your data back to a linear state, it simply compresses the histogram such that images can be added to one another without clipping. Any adjustments that need to be done in a linear state like Deconvolution will not be able to make use of this technique.

<hr>

## Creating Synthetic “Natural” Stars

Creating natural-looking stars from narrowband is very simple,  for the purposes of this guide we will use Ha and Oiii data, the two most commonly used narrowband filters, but feel free to play around with different combinations! This process is used to create a natural-looking “Star Only” image that is separate from your Starless main target image. Keep in mind that you can also use different combinations for both the Stars and Starless images!

- The first step is to combine your linear images in an HOO combination with Hydrogen-alpha data being mapped to Red and Oxygen III data being mapped to the Blue and Green channels. This can be done in PixelMath or ChannelCombination. 
- Next, extract linear stars from your data, this can be done very simply with StarNet 2 which has a linear checkbox as well as with other alternatives like StarXterminator. With StarNet 2, make sure you check the “Create Starmask” box and be sure to keep the Starless images so you can process them separately.  If your chosen star-extracting tool doesn’t support linear star removal or your data is already in a nonlinear state, see the “Extracting Linear Stars“ section below for details on how you can do this manually. 
- Now, open the HistogramTransformation Process and adjust the midpoint slider of the RGBK tab to stretch the image until the stars become visible. It is very important that you do not adjust the black point as the background of the star-only images are already clipped, any further adjustment would lead to faint stars being lost and other fainter details around the stars being clipped. If you notice that the star colors are not balanced or there is some cast that is either red or blue, adjust the midpoint slider in the R tab accordingly to achieve a balanced look. 
*( Tip: Remember that you can also stretch your stars less as a form of star reduction! )*
- Once you are happy with your stretch, open the SCNR process and apply the default settings at full strength to the image. Then, invert the image, and apply it one more time, but this time at reduced strength. I would recommend 30-50%.  Then, invert the image one more time to get it back to its original state. 
- Finally, use the CurvesTransformation and ColorSaturation processes to refine the look of your stars and you are done! For next steps, continue on to the “Combining Stars with the Starless Via Relinearization” section.

<hr>

<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 1186/896; margin-bottom: 20px; margin-top: 40px; width:90%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/hoo_stars.jpg" alt="after">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/synth_stars.jpg" alt="before" style="opacity:0;">
  <span class="center button right icon-arrow-right2"></span>
  <span class="center button left icon-arrow-left2"></span>
</div>

HOO stars vs. "Natural" stars
{:.figcaption}

The HOO stars are green and red whereas the processed stars are blue, white, yellow and red as you would expect from RGB stars.
{:.note}

## Using PixelMath for Relinearization
While this method can be performed manually with the HistogramTransformation process, there is an easier, faster, and more effective way to do so which I would personally recommend. This section will detail the same PixelMath equations that are found in the Process Icon set which is available for download at the beginning of the article.  Once you have your nonlinear images, they must be named in accordance with the formulas. Remember, this is case sensitive so the names must match exactly (ex. “Starless”, “Original”, or “Stars”) With that, the following formulas can be used for Combination and Extraction and are labeled accordingly. Ensure that when using these PixelMath formulas, “Use a single RGB/K expression” and “Create new image” are both checked.

### Combining Stars
  ~~~js
  /*file: "RGB/k"*/mtf(~m,(mtf(m,Starless)+mtf(m,Stars)))
  ~~~
  ~~~js
  /*file: "Symbols"*/m=0.999,
  ~~~

### Extracting Stars
  ~~~js
  /*file: "RGB/k"*/mtf(~m,(mtf(m,Original)-mtf(m,Starless)))
  ~~~
  ~~~js
  /*file: "Symbols"*/m=0.999,
  ~~~

<hr>

## Extracting Broadband Stars 

<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/ht.jpg" alt="Graphical interface"/>
</div>

While this process is very straightforward, it is still very important that when using the star-extracting tool of your choice, you ensure that the Stars extracted are linear. Extracting nonlinear stars will fill the star cores with noise that will be brought into your image, defeating the point of the linearization technique altogether. Once you have extracted your linear stars, use the HistogramTransformation Process to stretch the image, ensuring that you only use the “Midtones” slider to make adjustments to the image as outlined earlier in the guide. At this point, you can now process your stars and starless images separately to achieve any desired appearance.

Note: If your chosen star-extracting tool doesn’t support linear star removal, see the “Extracting Linear Stars“ section below for details on how you can do this manually. 

<hr>

## Combining Stars with the Starless Via Relinearization (Manually)
Once you have your nonlinear (already stretched) Star-Only image and your nonlinear processed Starless image, it is time to combine! In short, we will be “relinearizing” both images, combining them in PixelMath, and stretching them back.  This can be done manually as described or it can be done very simply using the PixelMath process, “AddStars” in the Process icon set linked at the top of the guide and as detailed in the “Using PixelMath for Relinearization” section.

- To begin, rename your images “Starless” and “Stars” accordingly. 
- Open the HistogramTransformation Process and reset it to it’s default settings. Then, in the “Midtones” box, type the value 0.999. Apply this process to both your “Stars” and “Starless” images. This will bring both images to a pseudo-linear state.
- Now, open PixelMath and use the formula " Stars + Starless “ with PixelMath set to “Create new image”.  
- With this new image, open the HistogramTransformation Process once again and set the “Midtones” value to 0.001. Apply this to the new image. 
- That’s it! You should now have a new image that contains both the target and the stars with no artifacts!

<hr>

## Extracting Linear Stars (Manually)

If you have found yourself in a position where you are not able to extract clean, linear stars “out of the box”, don’t worry. There is a very easy process that can be used to extract linear stars with any star-extracting tool. For this process you will need a nonlinear image containing both the target and the stars as well as a nonlinear starless image. This can be done manually as described or it can be done using the PixelMath process, “ExtractStars” in the above Process icon set and as detailed in the “Using PixelMath for Relinearization” section. 

- Apply your tool of choice to a copy of your nonlinear dataset that you would like to extract “clean” stars from so that you have one image that contains both the target and the stars, and another image that contains just the target and no stars. 
- Now, open the HistogramTransformation Process and reset it to it’s default settings. Then, in the “Midtones” box, type the value 0.999. Apply this to both images. 
- Next, open the PixelMath Process and use the formula “ [Image containing stars] - [starless image] “ with the process set to “Create new image”. The output image will be your “linear stars” image. At this point, you can carry on to the “Combining Stars with the Starless Via Relinearization” section. 
- Finally, if you wish to make further nonlinear adjustments to your “Stars” image, open the HistogramTransformation Process once again, and set the “Midtones” value to 0.001. Apply this to the new image. to get back to nonlinear “Clean” stars. 

<div class="split">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/star_nonlinear.jpg" alt="Nonlinear Stars">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/starless_processing/star_linear.jpg" alt="Linear Stars">
</div>

<hr>

Please feel free to contact me with any questions!
{:style="font-weight:bold; font-size:1.25rem; margin-bottom:0.5rem"}
See my [Contact](/contact) page for info on how to contact me!
{:style="margin-left:1rem;"}

<!-- Mouseover Swap -->

<style>
.btn {
  background-color: rgba(128,128,128,0.3);
  border: none;
}

.split {
  display: flex;
}

.split img {
  padding: 10px;
  width: 50%;
}

.rightside {
  width: 45%;
  float: right;
  margin-left: 1rem;
  text-align: center;
}

.rightside img {
  border-radius: 5px;
  width:100%;
  display:block; 
  margin-left:auto; 
  margin-right:0
}

@media (max-width: 768px) {
  .rightside {
    width: 100%;
    float: none;
    display: inline-block; 
  }
  .rightside img {
    width: 85%;
    margin-left:auto; 
    margin-right:auto;
  }
}

.mouseover-swap {
  position: relative;
  display: inline-block;
}

.mouseover-swap img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%; 
  height: 100%;
  transition: .25s ease-in-out;
}

.mouseover-swap .center {
  margin: 0;
  position: absolute;
  top: 50%;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%); 
}

.mouseover-swap .right {
  margin-right: 5px;
  right: 0;
}

.mouseover-swap .left {
  margin-left: 5px;
  left: 0;
}

.mouseover-swap .button {
  font-size: 1.5em;
  border-radius: 5px;
  background-color: rgba(30,30,30,0.6);
  padding: 8px;
  cursor: pointer;
}
</style>

<script>
initMouseover();

function initMouseover() {
  const mouseovers = document.querySelectorAll(".mouseover-swap");
  mouseovers.forEach(pair => {
    const img2 = pair.querySelectorAll("img")[1];
    const buttons = pair.querySelectorAll(".button");
    buttons.forEach(button => {
      button.addEventListener("click", function() {
        if (img2.style.opacity == 0) {
          img2.style.opacity = 1;
        } else {
          img2.style.opacity = 0;
        }
      });
    })

    /*pair.addEventListener("mouseover", function() {
        img2.style.opacity = 1;
    });
    
    pair.addEventListener("mouseout", function() {
        img2.style.opacity = 0;
    });*/
  });
}
</script>