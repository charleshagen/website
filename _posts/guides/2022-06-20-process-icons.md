---
layout: post
categories: guides
tags: basic
title: Process Icons
description: >
    The basics of creating and using process icons.
# hide_description: true
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/thumbnail_x1,0.jpg
    srcset: 
        2400w: https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/thumbnail_x1,0.jpg
        1200w: https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/thumbnail_x0,5.jpg
        600w:  https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/thumbnail_x0,25.jpg
sitemap: true
---

0. _
{:toc}

## What are Process Icons?

<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/process_icons.jpg" alt="Graphical interface"/>
    <p class="figcaption" style="margin-top: 0.25rem;">Process icons Example</p>
</div>

PixInsight is a notoriously unintuitive piece of software and unfortunately, process icons, a critical component of my processing flow are criminally underused. While on the surface, they do not appear to provide much utility, they will add an element of efficiency to your processing workflow that you can not even imagine. These simple icons allow you quick, organized access to your most frequently used processes and scripts and allow you to use multiple instances of customized processes across multiple sessions with saved and constant settings. In this short article, I will show you the basics of creating and using these process icon sets to improve your PixInsight experience!

<hr>

### Designing a Process Icon Set
While creating a process icon set is a process that is very unique to each individual, there are some tips that I can share. 

- Create your process icon set in descending order. If you’re anything like me, organization is everything, and this remains true with my process icons. I keep the icons I use first at the very top of my process icon set, organized laterally by process type, for example, DBE and ABE are in a pair because they both deal with background removal and they are on the same row near the top because they are done early on in the process. 
- Block out sections. In my example to the right, I have 5 primary blocks, the top-right block being my primary process flow, the top-left being my identifier icons, the mid-left being my Relinearization workflow, the bottom-left being my Manual DBE section, and the bottom-right being my color combinations section. While these blocks work for me, however, they may not work for you! Experiment and find a layout that is comfortable and efficient for you.
- Save your settings. While it can be easy to overlook and may sound trivial, saving your common settings can be a massive time-saver when you are processing. Before you save an icon, take the time now to enter your most common settings. For example, with Photometric Color Calibration, enter your focal length, pixel size, and approximate date to avoid having to re-enter those same values every time.
- Hoard icons! While my example may be a bit extreme, feel free to continue adding and iterating your process icon set as your skills and preferred methods in PixInsight change! Icons take up little space on your desktop and you can always cut them down later if you need. 

<hr>

### Creating a Process Icon Set
After you have an idea of the flow of your process icon set, creating the set is very simple!

- If a process or script is able to be loaded as a process icon, there will be a blue triangle in the bottom left corner. (Keep in mind that some scripts can be applied directly to images however some will need to be opened which you can do by double-clicking the icon) 
- Simply drag the blue triangle onto the “Desktop” in PixInsight, this will create the icon. Then feel free to move the icon around into your desired position. 
- Once you’ve organized the icons to your liking, it’s time to save. Click and drag over all of the icons, right-click, and select “Save Selected Icons…”, this will prompt you to select a directory where your .xpsm file will be saved, make sure this is a location that is easily accessible! I recommend saving this to your desktop and using it to launch PixInsight.

<hr>

### Loading Process Icon Set(s)

<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/guides/process_icons/example.jpg" alt="Graphical interface"/>
</div>

Loading one or multiple process icons is also very simple!
- To load a single process icon set, simply right-click on the PixInsight “Desktop” and select Process Icons > Load Process Icons, then locate your xpsm file.
- To load multiple process icons, right-click on the PixInsight “Desktop” and select Process Icons > Merge Process Icons, then locate your xpsm files. This will merge multiple process icon sets together in the same workspace whereas “Load Process Icons” will replace the existing set. 

<hr>

Please feel free to contact me with any questions!
{:style="font-weight:bold; font-size:1.25rem; margin-bottom:0.5rem"}
See my [Contact](/contact) page for info on how to contact me!
{:style="margin-left:1rem;"}

<style>
.rightside {
  width: 45%;
  float: right;
  margin-left: 1rem;
  text-align: center;
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