---
title: "GridDER"
subtitle: "Grid Detection and Evaluation in R"
excerpt: " GridDER is a tool for identifying biodiversity records that have been designated locations on widely used grid systems. Our tool also estimates the degree of environmental heterogeneity associated with grid systems, allowing users to make informed decisions about how to use such occurrence data in global change studies. We show that a significant proportion (~13.5%; 261 million) of records on GBIF, largest aggregator of natural history collection data, are potentially gridded data, and demonstrate that our tool can reliably identify such records and quantify the associated uncertainties.The package can serve as a tool to not only screen for gridded points, but to quantify the geographic and environmental uncertainties associated with these records, which can be used to inform models and analyses that utilize these data, including those pertaining to global change"
date: 2022-05-23
author: "Feng X, Rocha T, Thammavong HT, Tulaiha R, Chen X, Xie Y, Park D"
draft: false
tags:
  - hugo-site
categories:
  - R
  - GridDER
  - Biodiversity
  - Spatial Analysis
  
# layout options: single or single-sidebar
layout: single
links:
- icon: file
  icon_pack: fas
  name: EcoEvoRxiv
  url: https://ecoevorxiv.org/6qy5u/
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/BiogeographyLab/gridder
---

{{< here >}}

![Package logo](featured-hex.png)

## GridDER: Grid Detection and Evaluation in R


---

### The project

Observations and collections of organisms form the basis of our understanding of Earth’s biodiversity and are an indispensable resource for global change studies. Geographic information is key, serving as the link between organisms and the environments they reside in. However, the geographic information associated with these records is often inaccurate, thus limiting their efficacy for research. Along these lines multiple solutions for identifying erroneous coordinate data and records georeferenced to centroids or landmarks have been developed. Another prominent, but less discussed and documented source of inaccuracies arises due to the use of gridded survey systems in many regions of the world. Here we present GridDER, a tool for identifying biodiversity records that have been designated locations on widely used grid systems. Our tool also estimates the degree of environmental heterogeneity associated with grid systems, allowing users to make informed decisions about how to use such occurrence data in global change studies. We show that a significant proportion (~13.5%; 261 million) of records on GBIF, largest aggregator of natural history collection data, are potentially gridded data, and demonstrate that our tool can reliably identify such records and quantify the associated uncertainties. GridDER can serve as a tool to not only screen for gridded points, but to quantify the geographic and environmental uncertainties associated with these records, which can be used to inform models and analyses that utilize these data, including those pertaining to global change.
