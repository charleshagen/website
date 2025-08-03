---
layout: post
categories: guides
tags: advanced
title: HDR Mapping
description: >
    An effective method for revealing both bright and dim structures within a high dynamic range image.
hide_description: true
sitemap: true
---
0. _
{:toc}

## Background


- Astro data is very high dynamic range
- Stretching compresses dynamic range
- Contrast is finite when using a one-to-one stretch
    - Different stretch methods produce different stretch profiles, but all have the same total amount of contrast. You can only *distribute* the contrast to different portions of the histogram.
        - Histogram Transformation sacrifices contrast in the highlights in exchange for good contrast in the darker parts of the image
        - Arcsinh sacrifices contrast in the darker portions of the image while preserving some contrast in the highlights. This is why you can see detail in the brightest sections of the image, but it looks washed out.
- By using a nonlinear method for determining the stretch, you can increase the 'total' contrast in the image.



Astro data produces extremely high dynamic range images and the aim of stretching is to compress the dynamic range to that of a display. Stretching, however, is not always sufficient for very high dynamic range targets like bright nebulae and galaxies, especially when imaged in fields with faint nebulosity. 

Stretching is limited to a set amount of contrast 



Oftentimes there are large bright objects in the same field as very diffuse, faint objects. This is most commonly an issue when imaging galaxies in fields of delicate IFN, but can happen with nebulae as well. M42 is a great example, but many reflection nebulae have the same issue. 

Masking and curves can sometimes be used to recover a bit of contrast in these very bright areas, but often imagers resort to tools like Pixinsight's HDRMT which often produces less than ideal results. This article will detail a simple and effective way to produce pleasing HDR images with lots of control over the final result with the use of Pixelmath.

HDR processing is a style of processing where very bright structures and very faint structures are presented within the same image at similar brightness values. Specifically, it refers to the approach where the final brightness of any given pixel is not found using a direct one-to-one relationship from the input pixel value, but instead depends on the context of other pixel values surrounding it. This processing style mimics the human eye's response to bright scenes, wherein our eyes become less sensitive to bright regions, effectively dimming them in our perception. 

## Overview

The method relies on the Midtones Transfer Function (MTF). This is the same mathematical expression that is found in the ScreenTransferFunction and HistogramTransformation processes in Pixinsight. The fundamental concept is that in a stretched image, brighter regions need to be de-stretched more than dimmer regions to equalize the contrast across many brightness ranges. To do this, we will construct a map of where the brighter regions are, then we will use that map to de-stretch them in Pixelmath using the MTF function. This allows us to use a continuous set of values to de-stretch the image without having to run multiple masked iterations. The result is a very fast and highly iterable HDR method.

### Procedure 

- Ensure that your image has already been stretched such that the darkest features in the image are already sufficiently bright and at the right contrast level.
- Create a monochrome image to form the 'map'. If you are applying the HDR to a single channel, you will use the channel itself. Otherwise, for a color image, you may choose to use the Average value of all three channels, the extracted Luminance channel, or any individual channel. 
  - For example, if there is some feature in the image that is particularly bright in one channel such as a bright blue reflection nebula, you may want to use the blue channel. This may take some experimentation. If this is your first time performing HDR, I suggest using either the extracted luminance or the average of the RGB channels.
- Rename this new map image to `X`. This will be used in PixelMath in a later step.
- On the `X` image, using MMT, set the algorithm to Median-wavelet transform, then disable all small scale layers, iterating until your features of interest are no longer present in the map image.
  - These features of interest are the features in the image in which you want to enhance contrast. For example, in most galaxy images, you will want to enhance the contrast in the dark dust lanes and spiral arms, thus it is important that you disable all scales smaller than and including those features so their contrast is preserved after the process.
- Again on the `X` image, apply a CurvesTransformation instance wherein you decrease the brightness of the darker portions of the image to be nearly or completely black. You may also need to subtly destretch the `X` image to better control where the HDR is applied.
- Finally, on the original image, apply the following pixelmath, changing the `0.8` value (on the range of 0 to 1) to vary the strength of the HDR. 
  - Remember, the `X` image is *not* a mask, you should have no masks applied when applying this pixelmath.

    ~~~js
    /*file: "RGB/k"*/mtf(0.5+(X*0.8)/2, $T)
    ~~~

### How it works

For those who are unfamiliar with PixelMath, this may look like black magic, but in reality this expression is very simple. I will break down the components of the PixelMath and explain what they do, hopefully resulting in a better understanding of what exactly is happening in the process.

The body of the function uses the `mtf(arg1, arg2)` function. This mtf function (midtones transfer function) is the same mathematical operation that is found in the ScreenTransferFunction and HistogramTransformation processes in Pixinsight. It takes two arguments (inputs), `arg1` being the midtones value, the exact same as the middle slider in HT, and the second being the target image. In the pixelmath above, we are giving it the expression `0.5 + (X*0.8)/2` for the midtones value and `$T`, the syntax for the active target image, for the image. The midtones value is calculated by adding some value to `0.5` - this is important because 0.5 is the identity value for the mtf. That means that with a value of 0.5, when applied to an image, the resulting image will be identical to the input. 

Every pixel value in Pixinsight is represented on the range of 0 to 1 where black pixels are 0 and white pixels are 1. This is why we add `(X*0.8)/2` to the identity value; where the image `X` is close to black, the MTF function will do nothing to the image. Where the `X` is close to 1, the first argument of mtf will be `0.5 + (0.8 * ~1) / 2` which will de-stretch the image substantially. 



Here I use a strength value of 0.8. Where the `X` image is bright, values will be close to 1. Thus, the first argument in the MTF function, which controls the midtones value, will be high, decreasing the stretch of the image.