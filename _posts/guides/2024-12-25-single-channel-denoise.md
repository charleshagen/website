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
[DeepSNR](https://www.deepsnrastro.com/), created by Mikita Misiura, the creator of StarNet, is one of the most powerful neural network tools that we astrophotographers have access to. It has revolutionized the way we denoise our data, providing exceptionally clean results, far exceeding any other tool. The only caveat, however, is that in order for DeepSNR to effectively remove noise from an image, the source image must be a 3-channel combined RGB image inside of Pixinsight. This is no problem at all for broadband applications but is a limitation for single or dual channel data. This article will go over a powerful, yet simple technique for working around the three-channel limitation without the need to collect additional data. 

<hr>

<!-- Insert image -->

<!-- <div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 1854/1264; margin-bottom: 30px; width:100%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/old_advanced_narrowband/NoAddition.jpg" alt="Image 1">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/old_advanced_narrowband/HaCloudAddition.jpg" alt="Image 2" style="opacity:0;">
</div> -->

## The Problem
While it may be tempting to perform certain processes to fake having a color image for DeepSNR to run, there are caveats and downsides to most of the trivial approaches one might think of. I will detail two techniques for effectively denoising single channel images with DeepSNR. 


#### Why Can’t I Convert the Image to Color?
{:style="margin-left:1rem;"}
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
LRGB data presents another special edge case - oftentimes it is not necessary to treat luminance like a single channel as there are steps that can be taken in post-processing to achieve a very similar result. See the [Luminance](#background) section for more details.
{:style="margin-left:1.5rem;"}

<!-- Include something about LRGB and what to do with that? -->


## Solutions
The two solutions both involve manipulating the available data in such a way as to form RGB images with non-correlated noise profiles. This requires some forethought as this can only be done in the pre-processing stages. As such, the two methods only diverge in the RGB image creation stage. The process is identical from that point on.

#### 1. Triple Integration
{:style="margin-left:1rem;"}
The most straight forward approach is to simply divide each integration into three equal size stacks which can be assigned to red, green and blue.
{:style="margin-left:1.5rem;"}

#### 2. Drizzle Hack
{:style="margin-left:1rem;"}
The second approach is less straight forward, but is easier to do in practice, exchanging some processing time for convenience. In short, we will generate a single drizzled image and abuse SuperPixel debayering to produce an RGB image:
{:style="margin-left:1.5rem;"}

1. Drizzle 2x
For each channel, drizzle using a scale factor of *at least* 2 times. 
2. Test something
{:style="margin-left:2rem;"}

Low drop shrink values create the least correlation between the noise, yielding a better result.
{:.note style="margin-left:1.5rem;"}

#### Processing RGB


<!-- Mouseover Swap -->
<style>
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
</style>

<script>
document.getElementById('_pushState').addEventListener('hy-push-state-after', function() {
    initMouseover();
});
initMouseover();

function initMouseover() {
    const mouseovers = document.querySelectorAll(".mouseover-swap");
    mouseovers.forEach(pair => {
        const img2 = pair.querySelector("img:last-child");
        
        pair.addEventListener("mouseover", function() {
            img2.style.opacity = 1;
        });
        
        pair.addEventListener("mouseout", function() {
            img2.style.opacity = 0;
        });
    });
}
</script>