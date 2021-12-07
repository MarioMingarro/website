---
date: "2016-04-27T00:00:00Z"
external_link: null
image:
  caption: Canary
  focal_point: Smart
summary: Learn about R
tags:
- Demo
- robotics
title: Plotting with R
---

# Differences between crop and mask in R

```{r, warning=FALSE}
# load packages
library(tidyverse)
library(raster)
library(elevatr)

shp <- getData("GADM", country = "ES", level = 4)
shp.can <- shp[shp@data$NAME_1 == "Islas Canarias", ]

elevation <- get_elev_raster(shp.can, z = 8)
```

# Canary islands
```{r, warning=FALSE}
plot(shp.can)
```

# DEM
```{r, warning=FALSE}
plot(shp.can)
plot(elevation)
plot(shp.can, add=TRUE)

```

# Crop vs Mask

```{r}
elev_crop <- crop(elevation, shp.can)
plot(elev_crop)

```


```{r}
elev_mask <- mask(elevation, shp.can)
plot(elev_mask)
```
{{< figure src="/img/canary.png">}}
