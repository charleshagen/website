---
layout: post
categories: guides
tags: advanced
title: Single Channel DeepSNR
description: >
    Exploring a simple, yet powerful technique for denoising individual monochrome channels using DeepSNR without additional data collection. 
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/thumbnail_x0,25.jpg
sitemap: true
excerpt_separator: <!--more-->
---
<!--more-->

- _
{:toc}

## Background 

[DeepSNR](https://www.deepsnrastro.com/), created by Mikita Misiura, the creator of StarNet, is one of the most powerful neural network tools that we Astrophotographers have access to. It has revolutionized the way we denoise our data, providing exceptionally clean results, far exceeding any other tool. The only caveat, however, is that in order for DeepSNR to effectively remove noise from an image, the source image must be a 3-channel combined RGB image inside of PixInsight. This is no problem at all for broadband applications but is a limitation for single or dual channel data. This article will go over two powerful, yet simple techniques for working around the three-channel limitation without the need to collect additional data. 

<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 2000/1240; margin-bottom: 20px; margin-top: 2rem; width:75%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/no_denoise.jpg" alt="Not Denoised">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/denoise.jpg" alt="Denoised" style="opacity:0;">
  <span class="center button right icon-arrow-right2"></span>
  <span class="center button left icon-arrow-left2"></span>
</div>

Before and after single channel DeepSNR
{:.figcaption}

<hr>

## The Problem
While it may be tempting to perform certain processes to fake having a color image for DeepSNR to run, there are caveats and downsides to most of the trivial approaches one might think of.


#### Why Can’t I Convert the Image to Color?
{:style="margin-left:1rem;"}

<div class="rightside">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/deepsnr/pockmarks.jpg" alt="Image with deepSNR artifacts">
  <p class="figcaption" style="margin-top: 0.25rem;">Performing DeepSNR on an HOO image hallucinates stars, creating distinct pockmark-like artifacts</p>

</div>

As a result of DeepSNR being trained on color images only, it requires three channels that all contain distinct noise patterns. The technical term for this is 'non-correlated' noise, meaning no channel's noise pattern depends on any of the others. It is able to use the difference in these noise patterns to get a “best guess” as to what the underlying signal might have been.  When you convert a grayscale image to RGB, you only have one set of noise across all three channels, meaning it is strongly correlated, which will result in hallucinations. 
{:style="margin-left:1.5rem;"}

#### Why Can't I Add Noise?
{:style="margin-left:1rem;"}
While adding synthetic noise to an image to denoise it will allow DeepSNR to run with few hallucinations, adding noise will degrade the quality of the image post-denoise significantly. While the result may appear to work on the surface, adding more noise to the data causes the model to have to 'guess' more as to the true nature of the structure. If some part of the data were only on the edge of detectability before, adding more noise will destroy that signal. To retain the integrity of the data, we must explore other options.
{:style="margin-left:1.5rem;"}

<hr>

## When is it necessary?
It is important to note that it is only necessary to perform these techniques when you do not have appropriate data for DeepSNR's normal operating conditions. For example, with RGB data alone, this is not necessary as you can just denoise an RGB combined image. However, there are several common cases where additional steps are necessary:

#### Captured Less than Three Channels
{:style="margin-left:1rem;"}
In any case where you capture less than three channels total, you will need to use one of the techniques outlined below. This is because DeepSNR strictly requires three non-correlated noise profiles, otherwise it will hallucinate stars and create obvious artifacts. 
{:style="margin-left:1.5rem;"}


#### Channels do not Share Similar Structure
{:style="margin-left:1rem;"}
DeepSNR relies on all the channels sharing some amount of structure. If you combined three images with completely different structure and ran it through DeepSNR, you would notice 'bleeding' between the channels. This means that even if you have three channels of data, you may still want to denoise them individually. For example, supernova remnants will oftentimes have wildly different structures between Sii, Ha, and Oiii - this would be a good candidate for single channel noise reduction whereas a target like The Rosette nebula, may not be, as many of the structures are similar between the SHO channels. 
{:style="margin-left:1.5rem;"}

#### Capturing LRGB Images
{:style="margin-left:1rem;"}
LRGB data presents another special edge case - oftentimes it is not necessary to treat luminance like a single channel as it can be combined with two of the other color channels to form a color image which can itself be denoised. eg. creating an RLB image (Where Lum is assigned to the Green channel), denoising that image, and then extracting the Green channel as the denoised Luminance.
{:style="margin-left:1.5rem;"}

<!-- Include something about LRGB and what to do with that? -->

<hr>

## Solutions
The two solutions both involve manipulating the available data in such a way as to form RGB images with non-correlated noise profiles. This requires some forethought as this can only be done in the pre-processing stages. As such, the two methods only diverge in the RGB image creation stage. The process is identical from that point on.

### Method 1 --- Drizzle Hack
<!-- {:style="margin-left:1rem;"} -->
The drizzle approach takes the least amount of effort and can be mostly automated. We will leverage the fact that drizzle can increase the pixel resolution of an image while retaining non-correlated noise profile to our advantage by abusing the Debayer process. The procedure is as follows:

1. **Drizzle Integration**<br>
    For each channel, drizzle using a scale factor of *at least* 2 times. The resolution of the stack should be 2 times the final expected resolution. 
    - Low drop shrink values create the least correlation between the noise and should be used, even if artifacts are created. Most drizzle artifacts will become invisible once downsampled in later steps. 
2. **Debayer**<br>
    On the drizzled master images, run the Debayer process with pattern set to `RGGB`, and the method set to `SuperPixel`. This will create an RGB image with half the pixel resolution of the grayscale image.
    - Superpixel debayering is effectively a 2x integer downsample that extracts color information. With RGGB, the top left pixel will be assigned to red, the top right and bottom left to green, and the bottom right to blue, forming one 'SuperPixel' with color data. Because the noise in the drizzled data was non-correlated, so too are the channels of color in the new RGB image. 
{:style="margin-left:0.5rem;"}

<br>
That's it! Now it's time to move to the [Processing RGB](#processing-rgb) section to complete the process.

<hr>

### Method 2 --- Triple Integration

In situations where the drizzle approach is not possible due to limitations of the data or in cases where there are too many subframes to drizzle in a reasonable amount of time, the triple integration method works just as well, but takes a bit more work. We will form three stacks from subsets of the original raw data and form those into a color image to be run through DeepSNR. 

1. **Preparing the Data**<br>
    For each channel, divide the calibrated and registered subframes into three roughly even groups. It is easiest to do this with three folders in the registered file directory.
    - For example, if I had 119 Hydrogen-alpha images, I would put 40 images in a folder called “Integration 1”, 40 into “Integration 2”, and 39 into “Integration 3”. 
2. **Integrate**<br>
    For each of the folders, integrate the images as your normally would, remembering to enable all appropriate subframe weighting and rejection parameters.
3. **Combine RGB**<br>
    You now have three mono integrations composed from equal subsets of the original data. Using Pixelmath or ChannelCombination, assign integrations 1, 2, 3 to RGB respectively to form a color image. 
{:style="margin-left:0.5rem;"}

You may now proceed to the [Processing RGB](#processing-rgb) section!
    


### Processing RGB
With the RGB color image created, we will finish up by performing the denoise and getting back a grayscale image.

1. **DeepSNR**<br>
    On the new RGB image, apply DeepSNR at 100% with the `Linear data` option checked. The image will appear over-denoised, but this will be addressed in step three.
    - Alternatively, you can apply DeepSNR at your desired strength now, however it may be hard to judge exactly what strength is right for the data and running many iterations can become lengthy.
    - If when the STF is applied and you see a dominant hue, change the stretch preview to ‘unlinked’ by ctrl-clicking the nuclear button in the top right, or disabling the chain link icon in the STF process. The unlinked stretch should look approximately neutral in color.
2. **PixelMath**<br>
    Again to the RGB image, apply the following PixelMath with `Create new Image` checked, `Color space` set to Grayscale, and the identifier set to `NR`. This averages the three color channels, creating a single gray output.
    ~~~js
    /*file: "RGB/k"*/Avg($T[0],$T[1],$T[2])
    ~~~
3. **Blending**<br>
    In step one, DeepSNR was set to 100%, we will blend this back with the original to create a natural noise profile. If you used the drizzle method, apply an IntegerResample set to 2x downsample and name this image `Org`. If you used the Triple Integration method, bring in the stack containing all data and name it `Org`. Then in PixelMath, use the following expression to quickly iterate denoise amounts.<br><br>
    ~~~js
    /*file: "RGB/k"*/(Org * F) + (NR * ~F)
    ~~~
    ~~~js
    /*file: "Symbols"*/F = 0.7
    ~~~
    - Adjust the value of `F` on the range of 0 to 1 in `Symbols` to achieve a pleasing noise profile.
{:style="margin-left:0.5rem;"}
    
<hr>

Please feel free to contact me with any questions!
{:style="font-weight:bold; font-size:1.25rem; margin-bottom:0.5rem"}
See my [Contact](/contact) page for info on how to contact me!
{:style="margin-left:1rem;"}

<!-- Mouseover Swap -->
<style>
.rightside {
  width: 40%;
  float: right;
  margin-left: 1rem;
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