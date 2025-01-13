---
layout: post
categories: software
tags: pixinsight
title: RenameImages
description: >
    Automated script for renaming and closing auto-generated image files 
image:
    path: https://nightphotons.s3.amazonaws.com/assets/img/software/rename_images/thumbnail_x1,0.jpg
    srcset: 
        1920w: https://nightphotons.s3.amazonaws.com/assets/img/software/rename_images/thumbnail_x1,0.jpg
        960w: https://nightphotons.s3.amazonaws.com/assets/img/software/rename_images/thumbnail_x0,5.jpg
        480w:  https://nightphotons.s3.amazonaws.com/assets/img/software/rename_images/thumbnail_x0,25.jpg
sitemap: true
excerpt_separator: <!--more-->
---
<!--more-->

- _
{:toc}

## Introduction
<div class="rightside">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/software/rename_images/screenshot.png" alt="Graphical interface"/>
</div>

The RenameImages script provides a fast way to quickly rename and close automated outputs from most any integration routine to your preference. This speeds up the very beginning of your processing routine, especially for mosaics where you may have many files that will all need to be named differently. It uses regular expressions to target and either rename or close all views that are open in the current PixInsight instance.

<hr>

## Documentation

RenameImages provides two main functions; renaming and closing images according to regular expressions. Both functions can be independently enabled or disabled with the checkboxes next to each respective group. The default expressions are compatible with WBPP and FBPP outputs, but can be easily modified to support any range of auto-generated output names from any software.

It is recommended that you use this script at the beginning of your process to minimize the risk of accidental closure or loss of future history.
{:.note}

<!-- REGULAR EXPRESSIONS -->
### Regular Expressions
{:style="margin-top:2.5rem;"}
Regular expressions, or "regex", allow for finding, extracting, and manipulating text data. When the script is executed, the identifier of every image open in PixInsight is compared against the user-defined regular expression. If it matches the regex, the specified action will be performed, either renaming or closing the image. Within each regular expression, there are also "groupings", notated with parentheses. These captured groups are accessible when writing name patterns, allowing you to manipulate sections of, or the entirety of the existing identifier.  
{:style="margin-left:0.5rem; margin-bottom:1.5rem;"}

Whether you have lots of experience with regular expressions, or none at all, [Regex101](https://regex101.com/){:.faded} is an excellent tool for drafting and testing your regex code. It has syntax highlighting, explanations of all tokens, and makes drafting regex very easy.
{:.faded style="margin-left:0.5rem; margin-bottom:2.5rem;"}

**Script Specifics:**
{:style="margin-left:0.5rem"}
Group zero is reserved for the entire matching pattern (which may or may not be the entire original ID). All subsequent groups are sorted by the order they appear in the Regex pattern. There is currently one supported regex flag, being `i`, or the case insensitive flag.
{:style="margin-left:0.5rem; margin-bottom:2.5rem;"}

**Example:**
{:style="margin-left:0.5rem"}
With the following regex and the view named `masterLight_BIN_1_EXPOSURE_300_00s_FILTER_Red_Mono_integration`: 
{:style="margin-left:0.5rem"}
~~~js
masterLight.*EXPOSURE_([0-9]*).*FILTER_([a-zA-Z0-9]*)
~~~
{:style="margin-left:1.5rem"}
**Group 0** = "masterLight_BIN_1_EXPOSURE_300_00s_FILTER_Red"
{:style="margin-left:1.5rem"}
**Group 1** = "300"
{:style="margin-left:1.5rem"}
**Group 2** = "Red"
{:style="margin-left:1.5rem"}

<hr>

<!-- PATTERNING -->
### Patterning
{:style="margin-top:2.5rem;"}
RenameImages implements patterning with support for free text, regex group content, and FITS header values. Substituted values also accept formatting flags. All generated IDs are guaranteed to be unique and valid automatically. It is important to remember that image identifiers in PixInsight can only contain alphanumerics and underscores and must not start with a number. 
{:style="margin-left:0.5rem; margin-bottom:1.5rem;"}

**Free Text**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Any free text can be used, however it will automatically be converted to conform to PixInsight's naming requirements.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**Regex Group** 
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Regex groups can be accessed with the use of curly braces enclosing an index value (eg. `{0}`, `{1}`...) The value of the selected group will substitute the token. The index values are the same as the group numbers as discussed in the [Regular Expressions](#regular-expressions) section.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**FITS Header**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
FITS header values can be inserted by wrapping the header name with dollar signs (eg. `$FILTER$`, `$OBJECT$`, etc.) The value of the first header matching the provided name will substitute the token. It is not currently possible to access comment values.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**Format Flags**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Regex group and FITS header substitutes both accept three formatting flags, `-u`, `-l`, or `-p` for UPPERCASE, lowercase, and Proper Case respectively. These are added to the end of the enclosed value. (eg. `$FILTER-p$`, `{1-u}`)
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

<hr>

<!-- SETTINGS -->
### Settings
{:style="margin-top:2.5rem;"}
Settings apply to both closing and renaming where appropriate.
{:style="margin-left:0.5rem; margin-bottom:1.5rem;"}

**Modify Images with History**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Images containing process history, past or future, will renamed or closed. This setting is disabled by default for safety.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**Iconize after Rename**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Iconize image if it has been renamed. Useful for large datasets or mosaics when image windows can become cumbersome.
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

**Force Close**
{:style="margin-left:0.5rem; margin-bottom:0rem;"}
Do not prompt when closing an image with process history. For sensitive cases or if used late in a project, disable this option. 
{:style="margin-left:1.5rem; margin-bottom:1.5rem;"}

This option will have no effect if `Modify Images with History` is disabled.
{:.note style="margin-left:1.5rem; margin-bottom:1.5rem;"}

<hr>

<!-- EXAMPLES -->
### Examples
Several examples of patterns and expressions that can be used to achieve specific, repeatable, and fast results for different situations.

**WBPP Output**
{:style="margin-bottom:0rem; margin-top:2rem;"}
Weighted batch preprocessing names our lights starting with "masterLight", we can target that in the regular expression to only rename lights. Here we will simply rename the image to the uppercase filter name.
{:style="margin-left:0.5rem; margin-bottom:1rem;"}
~~~js
masterLight // Regex
$FILTER-u$ // Pattern
// -> LUM
~~~
{:style="margin-left:1.5rem"}


**Mosaics**
{:style="margin-bottom:0rem;"}
If you are using weighted batch preprocessing, you can use keyword groupings to rename your images too. In this example, I used "Panel" to separate panels of a mosaic so it is available in the image name.
{:style="margin-left:0.5rem; margin-bottom:1rem;"}
~~~js
masterLight.*PANEL([0-9]*) // Regex
$FILTER-p$_p{1} // Pattern
// -> Ha_p2
~~~
{:style="margin-left:1.5rem"}

**Multiple Iterations**
{:style="margin-bottom:0rem;"}
You can first rename the images by appending the `FOCALLEN` FITS header value, then select and rename based on the new values! 
{:style="margin-left:0.5rem; margin-bottom:1rem;"}
~~~js
.* // Regex
{0}_FL_$FOCALLEN$ // Pattern
// -> masterLight_BIN_1_EXPOSURE_300_00s_FILTER_Red_Mono_integration_FL_350.00
~~~
{:style="margin-left:1.5rem"}

~~~js
FL_350.00 // Regex
$OBJECT-p$_wide // Pattern
// -> Orion_Nebula_wide
~~~
{:style="margin-left:1.5rem"}

~~~js
FL_1249.00 // Regex
$OBJECT-p$_narrow // Pattern
// -> Orion_Nebula_narrow
~~~
{:style="margin-left:1.5rem"}

<hr>

## Installation
The RenameImages script is bundled with several other scripts in the NightPhotons PixInsight repository. To install, navigate to `Resources > Updates > Manage Repositories`, then click the `Add` button and paste the repository link found below. Then, `Resources > Updates > Check for Updates` and restart PixInsight. The scripts should now appear under `Scripts > NightPhotons`.
<!-- TODO: Link out to the pixinsight scripts tag -->
#### Repository
~~~
https://raw.githubusercontent.com/charleshagen/pixinsight/main/updates/
~~~
{:style="margin-left:1.5rem; margin-right:1rem;"}
NightPhotons PixInsight Repository --- (Do not open this link in your browser, there is no web content there)
{:.figcaption}

#### Troubleshooting
If the scripts do not appear under `Scripts > NightPhotons`, you may need to feature the scripts. Navigate to `Scripts > Feature Scripts`, then press `Add` and select your scripts folder, usually found at `C:\Program Files\PixInsight\src\scripts`. The scripts should now appear in your list.
{:style="margin-left:1rem;"}

<hr>

Please feel free to contact me with any questions!
{:style="font-weight:bold; font-size:1.25rem; margin-bottom:0.5rem"}
See my [Contact](/contact) page for info on how to contact me!
{:style="margin-left:1rem;"}

<style>
.rightside {
    width: 50%;
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