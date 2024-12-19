---
layout: post
categories: guides
title: Noise Sources 1
description: >
    An introduction to noise sources on CMOS cameras.
sitemap: false
hide_last_modified: true
excerpt_separator: <!--more-->
---
<!--more-->

- this unordered seed list will be replaced by toc as unordered list
{:toc}

## Explanation of SNR
Signal is defined as the mean photon flux from the target. 
Noise is simply the deviation from the true average flux that our detectors observe. This can be found statistically.


### Noise Definition
Photons are a poisson distribution due to their random and discrete nature.
Poisson distribution says that the mean, , is equal to its variance. Meaning the standard deviation of a poisson distribution is .
In this case, the RMS noise of a poisson distribution of photons is equal simply to the standard deviation.
Noise adds in quadrature - Sqrt(Sum(sources^2))
We aren't going to go through the process of deriving this for the time being, but it is just a statistical property that we can use to evaluate noise sources and their contribution
This is important, as 1e-RMS + 1e-RMS ≠ 2e-RMS! A fundamental idea in evaluating noise sources. (It’s 1.41e-)
If you think of a signal with some noise source, sometimes the noise will constructively interfere, sometimes it will destructively interfere. 

* * *


### Noise sources
 There are dozens of noise sources, most of which we do not need to care about. There are a couple composite noise sources which are useful abstractions of the underlying noise sources that encompass all of the significant noise sources present in our images. These sources are shot noise, dark noise, and read noise.

We will focus on temporal noise only as calibration frames and dithering can largely mitigate spatial noise.
- **Shot noise**
    - Sky background
    - Target photons
- **Read noise**
    - Noise from quantization error, random telegraph signal noise, amplifier noise, and some thermal effects (resistor noise).
- **Dark noise** 
    - Also a form of shot noise, same random distribution of discrete particles, but from primarily thermally dependent sources.
    - It’s important to note that dark current is not the same thing as dark noise. Just as with photon shot noise, it’s the square root of the dark current.

* * *


### Putting it all together (The CCD Equation)
CCD equations with signal, sky background noise, target noise, read noise, dark noise.
How can we use this in practice?
Some examples - identifying the dominant noise source
High light pollution broadband
High light pollution narrowband
Low light pollution broadband
Low light pollution narrowband
Some fundamentals
The colder your sensor, the better. If you have a camera that does not suffer from amp glow, there’s no reason not to cool it as much as possible. Thermoelectric coolers are designed to run continuously and will generally only degrade if exposed to temperatures higher than their rated maximum operating temperature. Run your camera cold!
(I use NINA to automatically determine the coldest temperature that my camera will be able to reach on a given night given the ambient temperature, so it’s always running at peak performance)
More longer exposures are better, especially for fainter objects in the frame. Disproportionately so. If you are imaging something really faint, use as long of exposures as possible.
Where possible, you should try to minimize read noise by selecting an appropriate gain, especially for narrowband. 
Gain settings
Unity Gain is meaningless and rarely coincides with useful ranges of the sensor. Unity simply means 1 e- results in 1 DN which has no impact on performance.
Look for high gain conversion drops, this is evident in a drop in read noise and usually a spike in Dynamic Range.
Dynamic range isn't perfect, noise does not scale linearly. For broadband, this might be fine.
On-paper specifications are important, but testing is important. Look at your linear images to see what’s saturating the sensor. If you cant get sub lengths you want with a certain gain setting, it might be time to change it.
Conclusions:
When shooting broadband in high light pollution or if using fast optics in lower light pollution, there are benefits to reducing gain in the event that your exposure time is limited by clipping at some higher gain. 0 Gain on most astro cameras will have a better dynamic range than the high conversion gain switch point which will, in some cases, provide very small benefits to noise, but will undoubtedly massively reduce the number of total subs you need to collect, saving on storage obligation and stacking times.

When shooting with narrowband filters, even in relatively high light pollution, read noise can make a difference. This depends on the filter in question and the intensity of the target. For bright targets where there is plenty of signal, read noise isnt that big of a deal. For faint targets where the photon shot noise is a much lower proportion, you can see greta benefits from longer subs.

Sources:
https://uasal.github.io/spacehardwarehandbook-public/reports/CMOS/CMOS_Noise_Sources.pdf
Outline of the Presentation:
Explanation of SNR. 
Signal is defined as the mean photon flux from the target. 
Noise is simply the deviation from the true average flux that our detectors observe. This can be found statistically.


Noise Definition
Photons are a poisson distribution due to their random and discrete nature.
Poisson distribution says that the mean, , is equal to its variance. Meaning the standard deviation of a poisson distribution is .
In this case, the RMS noise of a poisson distribution of photons is equal simply to the standard deviation.
Noise adds in quadrature - $$ \sqrt{\Sigma(sources^2)} $$
We aren't going to go through the process of deriving this for the time being, but it is just a statistical property that we can use to evaluate noise sources and their contribution
This is important, as 1e-RMS + 1e-RMS ≠ 2e-RMS! A fundamental idea in evaluating noise sources. (It’s 1.41e-)
If you think of a signal with some noise source, sometimes the noise will constructively interfere, sometimes it will destructively interfere. 

Noise sources - There are dozens of noise sources, most of which we do not need to care about. There are a couple composite noise sources which are useful abstractions of the underlying noise sources that encompass all of the significant noise sources present in our images. These sources are shot noise, dark noise, and read noise.

We will focus on temporal noise only as calibration frames and dithering can largely mitigate spatial noise.
Read noise
Noise from quantization error, random telegraph signal noise, amplifier noise, and some thermal effects (resistor noise).
Dark noise 
Also a form of shot noise, same random distribution of discrete particles, but from primarily thermally dependent sources.
It’s important to note that dark current is not the same thing as dark noise. Just as with photon shot noise, it’s the square root of the dark current.
Photon shot noise
Sky background
Target photons
Putting it all together (The CCD Equation)
CCD equations with signal, sky background noise, target noise, read noise, dark noise.
How can we use this in practice?
Some examples - identifying the dominant noise source
High light pollution broadband
High light pollution narrowband
Low light pollution broadband
Low light pollution narrowband
Some fundamentals
The colder your sensor, the better. If you have a camera that does not suffer from amp glow, there’s no reason not to cool it as much as possible. Thermoelectric coolers are designed to run continuously and will generally only degrade if exposed to temperatures higher than their rated maximum operating temperature. Run your camera cold!
(I use NINA to automatically determine the coldest temperature that my camera will be able to reach on a given night given the ambient temperature, so it’s always running at peak performance)
More longer exposures are better, especially for fainter objects in the frame. Disproportionately so. If you are imaging something really faint, use as long of exposures as possible.
Where possible, you should try to minimize read noise by selecting an appropriate gain, especially for narrowband. 
Gain settings
Unity Gain is meaningless and rarely coincides with useful ranges of the sensor. Unity simply means 1 e- results in 1 DN which has no impact on performance.
Look for high gain conversion drops, this is evident in a drop in read noise and usually a spike in Dynamic Range.
Dynamic range isn't perfect, noise does not scale linearly. For broadband, this might be fine.
On-paper specifications are important, but testing is important. Look at your linear images to see what’s saturating the sensor. If you cant get sub lengths you want with a certain gain setting, it might be time to change it.
Conclusions:
When shooting broadband in high light pollution or if using fast optics in lower light pollution, there are benefits to reducing gain in the event that your exposure time is limited by clipping at some higher gain. 0 Gain on most astro cameras will have a better dynamic range than the high conversion gain switch point which will, in some cases, provide very small benefits to noise, but will undoubtedly massively reduce the number of total subs you need to collect, saving on storage obligation and stacking times.

When shooting with narrowband filters, even in relatively high light pollution, read noise can make a difference. This depends on the filter in question and the intensity of the target. For bright targets where there is plenty of signal, read noise isnt that big of a deal. For faint targets where the photon shot noise is a much lower proportion, you can see greta benefits from longer subs.

Sources:
https://uasal.github.io/spacehardwarehandbook-public/reports/CMOS/CMOS_Noise_Sources.pdf
