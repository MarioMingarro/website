---
date: "2021-12-06T00:00:00Z"
external_link: null
image:
  caption: Canary
  focal_point: Smart
summary: In this post we will try to explain the difference between two functions of the raster package that often lead to confusion.
tags:
- Plot
- Raster
title: Differences between crop and mask in R
---

In this post we will try to explain the difference between two functions of the raster package that often lead to confusion.

Load R packages
```{r, warning=FALSE}
library(tidyverse)
library(raster)
library(elevatr)
```
Obtain the data to work with. In this case we are going to focus on the Canary Islands.

```{r, warning=FALSE}
shp <- getData("GADM", country = "ES", level = 4)
shp.can <- shp[shp@data$NAME_1 == "Islas Canarias", ]
elevation <- get_elev_raster(shp.can, z = 8)
```

Plotting Canary islands shapefile
```{r, warning=FALSE}
plot(shp.can)
```
{{< figure src="/img/posts/canary_shp.shp">}}

Plotting Digital Elevation Model
```{r, warning=FALSE}

plot(elevation)
plot(shp.can, add=TRUE)

```
{{< figure src="/img/posts/canary_elev.shp">}}

Now let's see what we have come to see in this post.

Crop function

```{r}
elev_crop <- crop(elevation, shp.can)
plot(elev_crop)

```
{{< figure src="/img/posts/crop.shp">}}

Mask function

```{r}
elev_mask <- mask(elevation, shp.can)
plot(elev_mask)
```
{{< figure src="/img/posts/mask.shp">}}

However, it is very important to emphasise that when working with large rasters the mask function is not well implemented and often takes a long time. Therefore, I always recommend using the crop function before the mask function.


```{r}
elev_crop <- mask(crop(elevation,
                       shp.can),
                  shp.can)
```
I hope this advice will help you and that you won't spend hours staring at your computer screen waiting for the mask function to run....