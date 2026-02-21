---
layout: post
categories: guides
tags: basic
title: Linear Luminance
description: >
    A simple, more reliable  alternative to the standard LRGB combination method which yields better results. 
hide_description: true
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/linear_lum/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/linear_lum/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/linear_lum/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/linear_lum/thumbnail_x0,25.jpg
sitemap: true
---
0. _
{:toc}

## Background

Luminance application is traditionally performed using a nonlinear color space; in the case of Pixinsight, this is `CIE Lab`. Here, the `CIE L*` channel in `Lab` color is replaced with the nonlinear (already stretched) Luminance filter data. While this does work, it can result in unexpected changes in color, saturation and brightness due both to the nonlinear color space used as well as the fact that Luminance needs to be stretched completely independently from the RGB data. This can be mitigated with certain processing strategies such as matching Histograms during stretching, but remains fundamentally problematic. Here I will present a simple procedure for applying Luminance in a linear state which will better preserve saturation and takes guesswork out of the equation.

### Procedure 

1. Take the average of your linear color calibrated RGB image using this PixelMath to create a new grayscale image called `Average`:
    ~~~js
    /*file: "RGB/k"*/Avg($T[0],$T[1],$T[2])
    ~~~
1. At this point, fully denoise your RGB image as to avoid bringing chrominance noise into the LRGB image. 
1. LinearFit your linear Luminance image using `Average` as a reference.
1. Apply this PixelMath to the RGB image to apply linear Luminance:
    ~~~js
    /*file: "RGB/k"*/Lum * ($T / Avg($T[0],$T[1],$T[2]))
    ~~~
1. Apply a stretch of your choice and enjoy the simplicity of processing a well-matched LRGB image.
