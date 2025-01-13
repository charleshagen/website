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
## Introduction
PhotometricContinuumSubtraction, or PCS for short, is a fully automated routine for continuum subtraction. You provide linear narrowband and broadband data, and using a photometric routine, it will automatically calculate and apply the mathematically optimal weights. This provides a robust and fast method of estimating and subtracting continuum signal from your narrowband data.  

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

## Usage