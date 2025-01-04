---
layout: post
categories: guides
tags: advanced
title: Continuum Subtraction
description: >
    New and improved advanced narrowband processing guide
hide_description: true
sitemap: false
---
0. _
{:toc}

## Introduction
Combining narrowband and broadband astrophotography is a fantastic way to 
First step into 'advanced processing'
Elevates your astrophotography
Advanced technique that allows for better control of different 


## Continuum Subtraction

### What is Continuum Subtraction?
  In an ideal world, narrowband filters would have an arbitrarily narrow bandpass, isolating exactly and only the target emission line. However, due to manufacturing limitations, optical constraints, and cosmological redshift, astrophotographers often use filters only as narrow as about 3 nm for deep sky imaging. While these filters are narrow, they don't completely isolate the target emission line. Some unwanted broadband signal, or 'continuum,' still passes through. The continuum subtraction technique aims to approximate and remove this continuum signal from narrowband integrations using broadband data that cover a similar spectral range. This process improves the contrast of the emission signal and allows for completely separate processing of broadband and narrowband data.

### Spectral Considerations

* **Hα**
  * Subtraction of Ha is straight forward as Ha is a heavily dominant emission band on most targets. For other bands, that is not necessarily the case. The two other most common bands require a bit more consideration when selecting a subtraction reference. 

* **O-III**
  * For Oiii data, it is recommended to use only Green data, as this still contains the Oiii line but normally does not contain significant H-beta signal, unlike Blue data. If you use blue data as a subtraction reference on a target that has significant H-beta emission and very weak Oiii signal, you may over-subtract signal that is not present in the Oiii. 

* **S-II**
  * With Sii data, you will often find that there is a significant amount of Ha emission in the broadband signal as Ha is far more prevalent in most emission nebulae than Sii. In attempting to remove continuum signal, you will likely find that there are regions correlating with the presence of Ha that are over subtracted, yielding an image that has unpleasant “holes”. 
  * The solution is to first continuum subtract your Ha signal, then remove the Ha emission from the broadband red image, isolating mostly the continuum structure, then using that resulting image as the new continuum reference image.

## Independent Processing

## Recombination

With both sets of images fully separated and independently processed, it's time to recombine. 

Continue with [Noise](noise-sources.md){:.heading.flip-title}
{:.read-more}