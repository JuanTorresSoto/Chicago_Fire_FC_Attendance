# Chicago_Fire_FC_Attendance
Home &amp; Away attendance (2008-2022)
Project 1

---
author: "Juan Torres Soto"
date: "5/31/2022"
output: html_document
---

## Home & Away Attendance

This project includes data from a Major League Soccer Data Set. To view the origin of the data that is found in Kaggle click here [link](https://www.kaggle.com/datasets/josephvm/major-league-soccer-dataset). It contains:

* Player
* Game
* Event
* Table

data from the MLS. For the purpose of this project, I am only analyzing the matches.csv table from game data. 


## Setting up my enviroment
Notes: setting up my R environment by loading the 'tidyverse' package.

* library(tidyverse)
* library(readr)
* library(dplyr)
* library(ggplot2

```{r loading packages, warning=FALSE, include=FALSE}
library(tidyverse)
library(readr)
library(dplyr)
library(ggplot2)
```


### Home Attendance Average
Notes:

* Load the data
* Clean out the data 

so we are only looking at the average home recorded attendance vs. other MLS teams. 


```{r loading and cleaning up the home data, warning=FALSE, include=FALSE}
ecfc <- read_csv("matches.csv")
View(ecfc) 

cfc <- dplyr::filter(ecfc, home == "Chicago Fire FC")

cfc.h=filter(cfc, attendance!="") %>% 
  group_by(home, away) %>%
  summarize(attendance, .groups="drop")

mean.h <- aggregate(x=cfc.h$attendance,
                  by=list(cfc.h$home,cfc.h$away),
                  FUN=mean) 
```

```{r home average table}
mean.h
```

#### Home Average Visual 
Notes: this will show a chart with the average game attendace per team. 

```{r home data viz, echo=FALSE}
ggplot(mean.h, aes(x = Group.2, y = x)) +
  geom_col() +
  scale_x_discrete(label = function(x) stringr::str_trunc(x, 12)) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5)) +
  geom_bar(stat = "identity", fill="#1b98e0", colour="red") +
  xlab("Away Teams") +
  ylab("Attendance") + 
  ggtitle("Chicago Fire FC Home")
```





### Away Attendance Average  
Note: 
* We will repeat the steps for Home Attendance Average.
* This will show the average match attendance when Chicago Fire are athe visitng team. 

```{r cleaning the away data, warning=FALSE, include=FALSE}
cf <- dplyr::filter(ecfc, away == "Chicago Fire FC")

cfc.a=filter(cf, attendance!="") %>%
  group_by(home, away, date, year) %>%
  summarize(attendance, .groups="drop")

mean.a <- aggregate(x=cfc.a$attendance,
          by=list(cfc.a$home,cfc.a$away),
          FUN=mean) 
```


```{r away average table}
mean.a
```

#### Away Average Visual 

```{r away average visual, echo=FALSE, warning=FALSE}
ggplot(mean.a, aes(x = Group.1, y = x)) +
  geom_col() +
  scale_x_discrete(label = function(x) stringr::str_trunc(x, 12)) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5)) +
  geom_bar(stat = "identity", fill="#1b98e0", colour="red") +
  xlab("Home Teams") +
  ylab("Attendance") + 
  ggtitle("Chicago Fire FC Away")
```


