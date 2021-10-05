---
title: "wRC+ is Dope"
author: "Aldo Diaz Caballero"
date: "10/4/2021"
output: 
  html_document: 
    keep_md: yes
---


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(retrosheet)
```

```
## 
## For Retrosheet data obtained with this package:
## 
## The information used here was obtained free of charge from
## and is copyrighted by Retrosheet. Interested parties may
## contact Retrosheet at "www.retrosheet.org"
```

```r
library(ggplot2)
library(Lahman)
```


### Goal

The objective of this experiment is if Aaron's new stat BBPA has has more predictive power than other traditional stats such as OPS or avg when talking about fantasy points. We will be limiting our analysis Retrosheet play-by-play data base and the modern baseball era. Modern baseball era will be defined to be after and including 2000.

BBPA is defined as:

$$
BBPA = \frac{H + 2*2B + 3*3B + 4*HR + BB + IBB + HBP}{PA}
$$



```r
##  Filter to only include post 2000 baseball 
##  Calculating plate appearances
modern.era <- Batting %>%
  filter(yearID <=2019 & yearID >= 2000) %>%
  mutate(PA = AB + BB + IBB + HBP + SH + SF)
head(modern.era)
```

```
##    playerID yearID stint teamID lgID   G  AB   R   H X2B X3B HR RBI SB CS  BB
## 1 abbotje01   2000     1    CHA   AL  80 215  31  59  15   1  3  29  2  1  21
## 2 abbotku01   2000     1    NYN   NL  79 157  22  34   7   1  6  12  1  1  14
## 3 abbotpa01   2000     1    SEA   AL  35   5   1   2   1   0  0   0  0  0   0
## 4 abreubo01   2000     1    PHI   NL 154 576 103 182  42  10 25  79 28  8 100
## 5 aceveju01   2000     1    MIL   NL  62   1   1   0   0   0  0   0  0  0   1
## 6 adamste01   2000     1    LAN   NL  66   2   0   0   0   0  0   0  0  0   0
##    SO IBB HBP SH SF GIDP  PA
## 1  38   1   2  2  1    2 242
## 2  51   2   1  0  1    2 175
## 3   1   0   0  1  0    0   6
## 4 116   9   1  0  3   12 689
## 5   1   0   0  0  0    0   2
## 6   1   0   0  1  0    0   3
```

