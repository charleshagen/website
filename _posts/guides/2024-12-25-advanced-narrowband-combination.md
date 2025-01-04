---
layout: post
categories: guides
tags: advanced
title: Advanced Narrowband Combination
description: >
    Managing and incorporating separate narrowband signal into broadband images.
# hide_description: true
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/thumbnail_x0,25.jpg
sitemap: false
---

0. _
{:toc}

## Background 
Narrowband and Broadband are often thought of as completely independent fields of astrophotography; generally, for this reason, a target will be imaged in either one or the other, but this is far from the true reality of the situation. Nearly every target in the sky has some component of both narrowband and broadband signal, much of which is often overlooked. In this article, I will be walking you through the process of narrowband combination with M31 - a target that has extremely complex and dynamic narrowband signal that is rarely included. While this is an extreme example, the concepts can be easily applied to any target. Furthermore, I would encourage you to look for narrowband signal in places that you would not normally expect, you might be surprised at what you find!

<!-- Insert image -->

<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 1854/1264; margin-bottom: 30px; width:100%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/NoAddition.jpg" alt="Image 1">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/HaCloudAddition.jpg" alt="Image 2" style="opacity:0;">
</div>


## Continuum Subtraction

For continuum subtraction, I will detail two possible methods, one being a more traditional method that uses a scale factor and PixelMath subtraction and another which uses a synthetic color workflow technique that outputs perfect continuum-subtracted image without any guessing and checking. While most of the narrowband integration process will be done in the later non-linear stages of the process, this stage does require linear data. In preparation, you will need your linear narrowband data and any of the nearest corresponding broadband filters which will be used for subtraction.  For example, For Hydrogen and Sulfur data, broadband red is used for the continuum subtraction, and for Oxygen data, Green. Whichever broadband channel you select, in this article, we will simply refer to it as the “Continuum” image.

Local Normalization can be destructive for continuum subtraction! In this case, I recommend avoiding Local Normalization for this application.
{:.note}

### Method 1 - Synthetic Color Flow Method (preferred)

This method relies on a few key principles of emission. If a theoretical filter existed that had a perfect narrowband bandpass, nearly no stars would show up in the data. Unfortunately, these filters do not exist, so stars are still present. This method relies on the fact that these stars are almost entirely continuum signal and thus if the continuum signal were to have the same relative intensity as the narrowband, could be perfectly subtracted.  The most effective way of normalizing the intensities of these two datasets that I have found is using a synthetic color image and a standard color calibration procedure.

- To begin, remove any gradients using DBE or ABE depending on which is most applicable. This is very important as any gradients in either image will transfer to the other. 

- Use ChannelCombination or PixelMath to create an image where the Narrowband image is assigned to the Red channel and the continuum image is assigned to both the Blue and Green channels. Ensure that the output is a color image.  For the purposes of this article, this image will be called “HRR” because we are assigning Ha to Red and Red to Green and Blue. (Keep in mind, for Oiii this will be different! Most likely “OGG”)

- Create a preview around an area in the background of the HRR image where there is minimal signal. To properly view the image at this stage, use an unlinked STF stretch, this will give the best idea of the signal in both images. 

- Open BackgroundNeutralization and use this new preview, HRR->Preview01, as the background reference image. After applying, use a linked STF stretch.

- Open ColorCalibration with default settings and use the default <target image> as the white reference and HRR->Preview01 as the background reference image. The image should now be completely neutral, all of the stars should be white and only the narrowband signal should have any color. 

    - If you find that the white balance is not correct, Create a preview of the HRR image that contains only a large patch of stars. Use this as a white reference.

        - At this point, you can optionally remove the stars from the image. Be cautious that star removal does not remove any structure from the narrowband image. This is **very** common with galaxy narrowband signal. Star extraction will not likely be necessary for narrowband addition as any artifacts will be obscured by stars, but for standalone narrowband images, and in some extreme cases it may be necessary.

- Using PixelMath, apply the following formula to the ‘HRR’ image to subtract the calibrated Continuum data from the Narrowband data. Create a new grayscale image, this will be your continuum-subtracted narrowband image. 

    - Rename this to “NB” for future steps.

```
RGB/k:
$T[0] - ($T[1] - med( $T[1]))
```

<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 1854/1264; margin-bottom: 30px; width:100%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/RGB.jpg" alt="Image 1">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/RGB_BN_CC.jpg" alt="Image 2" style="opacity:0;">
</div>

<hr>

### Method 2 - Traditional Method

- The first step in this process is optional. If you wish, this is the point where you will remove the stars from the linear images.  This will not likely be necessary for narrowband addition as any artifacts will be obscured by stars, but for standalone narrowband images, and in some extreme cases it may be necessary.

- Remove any gradients using DBE or ABE depending on which is most applicable. This is very important as any gradients in either image will transfer to the other.

- Once the stars have been removed from all images, a simple PixelMath formula can be used for the subtraction.

    - At this stage, I would also recommend denoising your continuum image. Any noise left in the continuum will be transferred into the narrowband, making the already faint signal even noisier.  You can use any linear noise removal method of your choice, although be sure not to remove any fine details or structures if possible as it is critical that everything is removed from the narrowband image. 

    - In the PixelMath below, there is a symbol f which will be used as a factor to determine how much of the continuum signal is removed. As a rough approximation, you can use the ( bandpass of NB * subexposure length of NB ) / ( bandpass of BB * subexposure length of BB ) . This is only an approximation, however, and it will still likely need visual tweaking. Apply this to your narrowband image and replace it. 

- After running the above PixelMath, you should notice a change in your image. If the image has gone too dark, the continuum data has been over-subtracted and you need to lower your f value. If there is still any obvious continuum signal remaining, you will need to increase your f value and run the process again. Undo the PixelMath, make the adjustment to the f value, and adjust until the data has no remaining continuum signal and no overcorrected areas.

```
RGB/k:
Narrowband - f * (Continuum- med(Continuum))

Symbols:
f = [ input value of f here ]
```

<img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/f0.14.jpg">


<hr>


## Bright Structure Addition
- This part of the process is the primary addition method for narrowband signal, it is no different from the process that will be used to add fainter signals, with the exception of the masks and preparation that will be used for the faint data. The combination relies on the Relinearization technique discussed in the Independent Starless Processing article.

- To begin, stretch both the narrowband and broadband signal to a nonlinear state with your preferred method. Although this process can be performed anywhere in the nonlinear phase, I recommend this addition be done after most of the primary broadband processing. (The broadband image can have stars or can be starless, for this process it does not have an effect)

- While preparing the Narrowband image, be mindful that the pixel values will directly correlate to the brightness values added to the final image. The black point should be pushed extremely far, perhaps even slightly clipped. Curves transformation can help tremendously with this by compressing the dark signal in the image to near zero as shown.

- Open PixelMath and simply apply the following function to the broadband image to add the narrowband signal.

    - Depending on the look that you’d like, adjust the B and G values, these are simple percentages. If you prefer a more pink tone rather than a red, add more blue, etc. (This works the same way for other bands as well. for Oiii, I recommend weights of 0, 0.75, 1.0 for RGB respectively)

```
Red:
$T * ~R + R * mtf(~m, (mtf(m, $T) + mtf(m, NB)))

Green:
$T * ~G + G * mtf(~m, (mtf(m, $T) + mtf(m, NB)))

Blue:
$T * ~B + B * mtf(~m, (mtf(m, $T) + mtf(m, NB)))

Symbols:
R = 1.0,
G = 0.0,
B = 0.05,
m = 0.999,
```



<hr>

<img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/ha_addition_old/HaAddition.jpg">


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