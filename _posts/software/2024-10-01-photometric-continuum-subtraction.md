---
layout: post
categories: software
tags: pixinsight
title: PhotometricContinuumSubtraction
description: >
    Automated photometric routine for continuum subtraction 
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/software/photometric_continuum_subtraction/thumbnail_x1,0.jpg
    srcset: 
        1920w: https://nightphotons.s3.amazonaws.com/assets/img/software/photometric_continuum_subtraction/thumbnail_x1,0.jpg
        960w: https://nightphotons.s3.amazonaws.com/assets/img/software/photometric_continuum_subtraction/thumbnail_x0,5.jpg
        480w:  https://nightphotons.s3.amazonaws.com/assets/img/software/photometric_continuum_subtraction/thumbnail_x0,25.jpg
sitemap: true
---

- _
{:toc}

<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/software/photometric_continuum_subtraction/screenshot.png" alt="Graphical interface"/>
</div>

## Introduction
PhotometricContinuumSubtraction, or PCS for short, is a fully automated routine for continuum subtraction. You provide linear narrowband and broadband data, and using a photometric routine, it will automatically calculate and apply the mathematically optimal weights. This provides a robust and fast method of estimating and subtracting continuum signal from your narrowband data.  
<hr>

## Documentation
The PhotometricContinuumSubtraction script can be configured to run either with or without the dialog. If you use the [Rename Images Script](https://www.nightphotons.com/software/rename-images/) or if you use a consistent naming scheme for each filter's image, the script can be saved off into a process icon and dragged onto any active image to run the script. If you prefer to configure the script each time, you can launch the script dialog under `Scripts > NightPhotons > PhotometriContinuumSubtraction`.

**Original Views**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
This section contains view selectors for `Narrowband` and `Broadband` images. Here, select your star-containing narrow and broad band images respectively. These inputs are mandatory.  
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

<!-- |=====================================================================================| -->

**Generate Starless**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
**Generate Starless Checkbox** - Toggle the starless routine. When enabled, a second image will be produced using the same subtraction weights as the star-containing image.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

{:style="margin-left:0.5rem; margin-bottom:0rem;"}
**View Selectors** - Optionally provide starless images to be subtracted using the weights calculated from the photometric routine. If left blank, the starless routine will generate starless images using the selected `Fallback` method.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

{:style="margin-left:0.5rem; margin-bottom:0rem;"}
**Fallback** - If no views are selected in the starless view selectors, the routine will automatically generate starless images using the provided fallback method. Select from StarNet V2 or StarXTerminator.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

<!-- |=====================================================================================| -->

**Settings**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
**Maximum Stars** - This field controls the maximum number of stars that will be included in the calculation, higher values will include more stars which will take longer, but may provide more accurate results. Default value is 400 stars.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

{:style="margin-left:0.5rem; margin-bottom:0rem;"}
**Maximum Peak** - This field controls the maximum peak value that a star can have to be included in the flux calculation, higher values will be more tolerant of brighter stars and may begin to include saturated stars, which can degrade the performance of the routine. Default value is 0.8.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**Generate Plot** - Plot narrowband flux vs. broadband flux. This can be useful for verifying the calculated fit and troubleshooting in the event of poor subtraction. If the curve is non-linear, consider increasing the maximum number of stars.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

<hr>

<!-- |=====================================================================================| -->

## Installation
The PhotometricContinuumSubtraction script is bundled with several other scripts in the NightPhotons PixInsight repository. To install, navigate to `Resources > Updates > Manage Repositories`, then click the `Add` button and paste the repository link found below. Then, `Resources > Updates > Check for Updates` and restart PixInsight. The scripts should now appear under `Scripts > NightPhotons`.
<!-- TODO: Link out to the pixinsight scripts tag -->
#### Repository
~~~
https://raw.githubusercontent.com/charleshagen/pixinsight/main/updates/
~~~
{:style="margin-left:1rem; margin-right:1rem;"}
NightPhotons PixInsight Repository --- (Do not open this link in your browser, there is no web content there)
{:.figcaption}

#### Troubleshooting
If the scripts do not appear under `Scripts > NightPhotons`, you may need to feature the scripts. Navigate to `Scripts > Feature Scripts`, then press `Add` and select your scripts folder, usually found at `C:\Program Files\PixInsight\src\scripts`. The scripts should now appear in your list.

<hr>


<style>
.rightside {
    padding-top: 1rem;
    padding-left: 2rem;
    width: 45%;
    float: right;
}

.rightside img {
    border-radius: 5px;
    width:100%;
    display:block; 
    margin-left:auto; 
    margin-right:0
}

@media (max-width: 768px) { /* Adjust breakpoint as needed */
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
</style>