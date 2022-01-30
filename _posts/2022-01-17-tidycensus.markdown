---
title: "Mapping census data in R with tidycensus"
layout: post
date: 2022-01-17 22:44
image: https://walker-data.com/tidycensus/logo.png
headerImage: true
tag:
- R
- dataviz
star: true
category: blog
author: tom-pham
description: Markdown summary with different options
---

### Introduction

The Census Bureau contains a rich source of demographic and economic data in the US.

Today I will be sharing a really cool package called [`tidycensus`](https://walker-data.com/tidycensus/index.html) that allows users to interface with a select number of the US Census Bureau's data APIs and return tidyverse-ready data frames. This package extends functionality further by allowing you to retrieve [`sf`](https://r-spatial.github.io/sf/) (simple feature) geometry alongside the data, which allows for a simplified workflow to produce maps. 

### Basic usage for tidycensus

Get a [Census API key](https://api.census.gov/data/key_signup.html)

**Search for data**. There are two main sources: the deccenial Census data and the American Community Survey data. Each specific dataset is identified by a variableID, and has a particular unit of geography. There are a wide range of geographies but some common ones include: the whole country, state, county, census tract, and census block group. You can read more detailed documentation [here](https://walker-data.com/tidycensus/articles/basic-usage.html#searching-for-variables-1). 

**Retrieve data**. There are two main functions you can use to retrieve data. [`get_acs()`](https://walker-data.com/tidycensus/reference/get_acs.html) and [`get_decennial()`](https://walker-data.com/tidycensus/reference/get_decennial.html). At minimum you must provide the geography and variable (dataset ID).

### Make a map

```r
library(sf)
library(tidyverse)
library(tidycensus)
library(viridis)

census_api_key(YOUR API KEY GOES HERE)

# Get data with geometry
# B19013_001 = MEDIAN HOUSEHOLD INCOME IN THE PAST 12 MONTHS 
# (IN 2017 INFLATION-ADJUSTED DOLLARS)
ca_income <- get_acs(geography = "county",
                     variables = "B19013_001",
                     state = "CA",
                     geometry = TRUE)

# Create map
ggplot() +
  geom_sf(
    data = ca_income, 
    mapping = aes(fill = estimate), 
    color = "white", size = 0.1, 
    show.legend = TRUE
  ) +
  labs(
    title = "Income in California",
    fill = "Income"
  ) +
  scale_fill_viridis() +
  theme_void()
```
![](/assets/images/ca_household_income_county_map.png)
