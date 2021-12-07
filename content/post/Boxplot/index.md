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
title: Downloading and plotting spanish temperatures with R
---

#  Downloading and plotting spanish temperatures with R

```{r, warning=FALSE}
library(reshape2)
library(tidyverse)
library(PupillometryR)
library(climaemet)
library(ggpubr)

aemet_api_key("YOUR API", 
              install = TRUE)

stations <- aemet_stations() 

station <- "2462" 

data_monthly <- tibble(monht = c(seq(1, 12, 1)))
for (y in c(1960, 2020)){
  data <- aemet_monthly_clim(station, year = y)
  data <- select(data, tm_max)
  names(data) <- paste0("Y_",y)
  data_monthly <- cbind(data_monthly, data[c(1:12),])
}


plot_data <- melt(data_monthly[,c(2,3)])
navacerrada <- ggplot(plot_data,
                      aes(
                        x = variable,
                        y = value,
                        fill = variable,
                        colour = variable
                      )) +
  geom_flat_violin(
    position = position_nudge(x = .1, y = 0),
    trim = FALSE,
    alpha = 0.5,
    colour = NA
  ) +
  geom_point(position = position_jitter(width = .2),
             size = 2,
             shape = 20) +
  geom_boxplot(
    outlier.shape = NA,
    alpha = .5,
    width = .1,
    colour = "black"
  ) +
  scale_y_continuous("ºC",c(-10,-5,0,5,10,15,20,25,30,35), limits = c(-10,35))+
  scale_colour_brewer(palette = "Dark2") +
  scale_fill_brewer(palette = "Dark2") +
  ggtitle("Navacerrada") +
  theme(
    legend.title = element_blank(),
    axis.title.x = element_blank(),
    axis.text.x = element_blank(),
    axis.title.y = element_blank(),
    axis.ticks.x = element_blank()
  )

station <- "5973" 

data_monthly <- tibble(monht = c(seq(1, 12, 1)))
for (y in c(1960, 2020)){
  data <- aemet_monthly_clim(station, year = y)
  data <- select(data, tm_max)
  names(data) <- paste0("Y_",y)
  data_monthly <- cbind(data_monthly, data[c(1:12),])
}


plot_data <- melt(data_monthly[,c(2,3)])
cadiz <-
  ggplot(plot_data,
         aes(
           x = variable,
           y = value,
           fill = variable,
           colour = variable
         )) +
  geom_flat_violin(
    position = position_nudge(x = .1, y = 0),
    trim = FALSE,
    alpha = 0.5,
    colour = NA
  ) +
  geom_point(position = position_jitter(width = .2),
             size = 2,
             shape = 20) +
  geom_boxplot(
    outlier.shape = NA,
    alpha = .5,
    width = .1,
    colour = "black"
  ) +
  scale_y_continuous("ºC",c(-10,-5,0,5,10,15,20,25,30,35), limits = c(-10,35))+
  scale_colour_brewer(palette = "Dark2") +
  scale_fill_brewer(palette = "Dark2") +
  ggtitle("Cadiz") +
  theme(
    legend.title = element_blank(),
    axis.title.x = element_blank(),
    axis.text.x = element_blank(),
    axis.title.y = element_blank(),
    axis.ticks.x = element_blank()
  )

ggarrange(navacerrada, cadiz, common.legend = TRUE, legend = "bottom")


```

{{< figure src="/img/boxplot.jpeg">}}







