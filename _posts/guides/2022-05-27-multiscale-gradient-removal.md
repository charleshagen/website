---
layout: post
categories: guides
tags: advanced
title: Mulitscale Gradient Removal
description: > 
    The most effective background extraction method which is surprisingly simple.
# hide_description: true
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/thumbnail_x0,25.jpg
sitemap: false
---

0. _
{:toc}

## Background
Multiscale Gradient Removal, or MSGR as it is often referred to, is a simple yet effective way to remove background gradients in an image by using two different data sources at different image scales.  By replacing the large-scale components of an image with the smaller scale components of a wider image, complex and particularly difficult gradients are completely removed leaving only very simple, low function degree gradients which are easily removed with processes like ABE or DBE. This is an adaptation of Vicent Peris’ method.

<h3 style="margin-top:1.5rem">Process Icons</h3>
<a class="btn btn-primary" href="https://nightphotons.s3.amazonaws.com/assets/process_icons/msgr.xpsm">Download</a>

<!-- Image -->
<div class="mouseover-swap" style="display:block; margin-left:auto; margin-right:auto; aspect-ratio: 3818/2509; margin-bottom: 20px; margin-top: 40px; width:95%;">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/rgb_narrow_original.jpg" alt="Before MSGR">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/rgb_narrow_corrected.jpg" alt="After MSGR" style="opacity:0;">
  <span class="center button right icon-arrow-right2"></span>
  <span class="center button left icon-arrow-left2"></span>
</div>

Before and After running MSGR
{:.figcaption} 

<hr>

### How does MSGR work?

<!-- Diagram -->
<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/diagram.jpg" alt="Graphical interface"/>
    <p class="figcaption" style="margin-top: 0.25rem;">Removing a small portion of an image containing complex gradients reveals very simple gradients.</p>
</div>

Multiscale gradient removal is very simple. In principle, by using a small section of a larger image, the complexity of the gradient decreases dramatically making what little gradient remains very simple to correct. MSGR uses the gradient of the wide-field image as a reference to generate a background model which can be subtracted from the original image to remove any residual gradients.  As this method requires multiple datasets, It is recommended that the pixel scale of the wide-field image be at least 1.5 times, but not greater than 6 times the pixel scale of the narrow-field image where possible, although depending on the quality of the data and scale of correction needed this may change. This is a recommendation, not a hard limit.

### Performing Multiscale Gradient Removal

Performing MSGR is a relatively simple task,

- First and foremost, load both the narrow-field and wide-field datasets into Pixinsight. MSGR can be done on monochrome or color images.
- If the wide-field dataset contains any prominent gradients, remove them with Dynamic / Automatic Background Extraction. I recommend keeping the correction simple, low function degree, high smoothing, and/or few points far apart.  
- Create a duplicate of the narrow-field image and name it “Narrow”. Keep the original narrow-field image as it will be needed later for correction.
- On the wide-field image, create a preview that is slightly wider than the field of view of the “Narrow” image. Apply the StarAlign_Narrow Icon or use the StarAlign process. The included process icon set (download above) contains all of the settings for each process.
    - If you use the StarAlign process manually, ensure that the following settings are correct.
        - Reference Image = “Narrow” 
        - Registration Model = Thin Plate Splines
        - ☑ Distortion correction
        - Star Matching > ☑ Restrict to previews
        - Star Matching > ☑ Use scale differences 
    - Run the process on the wide-field image
- With the new registered wide-field image, rename it to “Wide” 
- [Optional but recommended] Use StarNet V2 or StarXterminator in linear mode to remove the stars in both the “Wide” and “Narrow” images 

- Use the Multiscale Median Transform process or the MMT_1 Icon on both the “Wide” and “Narrow” images
    - If using the MMT process manually, disable the first 8 or so MMT layers by double-clicking the green checkmarks. by default only 4 layers will be displayed, this can be changed in the top right corner of the process. 
- On the “Wide” image, apply the LinearFit_Narrow Icon or use the Linear Fit process with “Narrow” as the Reference image.
- Use the CreateModel icon or use Pixelmath to create a new image called “Model” with the following formula:
~~~js
/*file:RGB/k*/Narrow * med(Wide) / Wide
~~~
- Apply Multiscale Median Transform or the MMT_2 Icon to the new “Model” image. This will get rid of any residual fine-scale details introduced in the model creation.
    - If using the MMT process manually, disable one additional layer from the previous step using MMT eg. If 8 layers were removed with the first MMT, remove 9.

- Finally, on your original narrow-field image which was separated in step 2, apply the RemoveModel Icon or use PixelMath with the following formula
~~~js
/*file:RGB/k*/$T * med(Model) / Model
~~~

- Congratulations! Your image should now be free of any gradients!

<hr>

<div style="margin-top: 1.5rem; width: 100% margin-bottom:">
  <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/gradient_removal/model.jpg" alt="MSGR Background Model" style="margin-left: auto; margin-right: auto; width: 85%; display: block;"/>
</div>

MSGR Background Model
{:.figcaption style="margin-top: 0.5rem;"}

<hr>
Please feel free to contact me with any questions!
{:style="font-weight:bold; font-size:1.25rem; margin-bottom:0.5rem"}
See my [Contact](/contact) page for info on how to contact me!
{:style="margin-left:1rem;"}


<style>
.btn {
  background-color: rgba(128,128,128,0.3);
  border: none;
}

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
</style>

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