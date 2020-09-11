---
Wyoming COVID-19 Cases
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown

The following code will provide basic statistical analysis and visualizations detailing COVID-19 cases in the State of Wyoming. 

```{r, warning=FALSE,message=FALSE}
library(readr)
library(tibble)
library(dplyr)
library(tidyverse)
library(ggplot2)
library(lubridate)
library(rmarkdown)
```

## Code

```{r, echo=TRUE}
dataset_url = "https://covidtracking.com/data/state/wyoming"
wyo <- read_csv("C:/Users/Student/Downloads/wyoming-history.csv")

## dataset dimensions
dim(wyo)

## dataset info
vec_col <- colnames(wyo)
head(wyo)
glimpse(wyo)
nrow(wyo)
ncol(wyo)

## remove NA columns/rows
wyo_clean <- wyo[, colSums(is.na(wyo)) != nrow(wyo)]
wyo_clean$positive[is.na(wyo_clean$positive)] <- 0
wyo_clean$negative[is.na(wyo_clean$negative)] <- 0
head(wyo_clean)

## change date to utilizable format
wyo_clean$date <- ymd(wyo_clean$date)
head(wyo_clean)
```

## Plots
```{r message=FALSE}


plot_2 <- ggplot(data = wyo_clean, aes(x = date)) +
  geom_line(aes(y = positive), col="red", lwd = 1) +
  geom_line(aes(y = negative), col="green", lwd = 1,
  linetype="solid") + scale_x_date(date_breaks = "2 week",
  date_labels = "%b-%d")  + ggtitle("Positive vs. Negative Cases") + 
  xlab("Date") + ylab("Cases") + theme(axis.text.x =
  element_text(face="bold", color="black",  size=7, angle=30)) +   
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 
  20)) + theme(axis.title.x = element_text(size=15)) +
  theme(axis.title.y = element_text(size=15)) +
  scale_y_continuous(breaks=seq(0,80000,4000))
plot_2
ggsave("myplot.png", plot = plot_2)
```
