---
layout: page
title:  Gear
---


<div class="telescope-grid" markdown="1">
  <div class="grid-table" markdown="1">

| Type          | Name                                 |
|:--------------|:-------------------------------------|
| Telescope     | Esprit 100ed                         |
| Mount         | Astro-Physics 900gto w/ CP3v2        |
| Camera        | QHY 268M Photographic                |
| Filters       | Antlia V series LRGB + 3nm SHO       |
| Filter Wheel  | QHY CFW3 M                           |
| Focuser       | Moonlite Nitecrawler WR25            |
| Observatory   | Sonoran Desert Skies Observatory     |

  </div>

  <div class="grid-img-small">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/gear/observatory.jpg" alt="SDSO Observatory with MilkyWay">
  </div>

  <div class="grid-img-tall">
    <img src="https://nightphotons.s3.amazonaws.com/assets/img/gear/telescope.jpg" alt="Telescope">
  </div>
</div>

## Current Allsky:
<img src="https://sdso-allsky.alexander-helms.workers.dev/api/image">

<style>
.telescope-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto;
  gap: 15px;
  grid-template-areas: 
    "table tall-img"
    "small-img tall-img";
}

.grid-table { 
  grid-area: table; 
}

.grid-img-small { 
  grid-area: small-img; 
}

.grid-img-tall { 
  grid-area: tall-img;
  min-height: 0; 
}

.grid-img-small img, 
.grid-img-tall img {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}

@media (max-width: 768px) {
  .telescope-grid {
    grid-template-areas: 
      "table"
      "small-img"
      "tall-img";
    grid-template-columns: 1fr;
  }
}
</style>
