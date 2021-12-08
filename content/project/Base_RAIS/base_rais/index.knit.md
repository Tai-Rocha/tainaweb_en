---
title: "Bibliometrix package"
author: "Tainá Rocha"
date: "2021-08-09"
output: rmarkdown::github_document
---



#### A package for quantitative research in scientometrics and bibliometrics that includes all the main bibliometric methods of analysis. 

#### 1. Get the dataset

I obtained the data from SCOPUS and Web of Science repositories (WOS). I searched for articles using a generic query for test purposes. It is important to emphasize that building a correct query requires [some steps](https://guides.library.illinois.edu/c.php?g=980380&p=7089538)  

Generic query just to test package: Reforestation * Temperate forest 

Years: from 2000 to 2021

Finally, select all items of search to export. In this tutorial I exported as ".bib" file

**SCOPUS**


**WOS**

#### 2. The package 

##### 2.1 To install

```r
#install.packages("bibliometrix")
```

##### 2.2 Shiny

The package presents a Shiny version, which makes it possible to run analyses in a point and click interface, without write code. This is a really cool possibility for people who want to use the tool but don't want to write code in R. To use biblioshiny, you need to run  the below code without # :


```r
# bibliometrix::biblioshiny()
```

##### 2.3 Despite Shiny it's really cool, writing the code it's a better way for reproducibility in future analyses. Also, It's easy to use codes for this package.

Loading some libraries (make sure that all libraries are installed)


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
library(bibliometrix)
```

```
## To cite bibliometrix in publications, please use:
## 
## Aria, M. & Cuccurullo, C. (2017) bibliometrix: An R-tool for comprehensive science mapping analysis, 
##                                  Journal of Informetrics, 11(4), pp 959-975, Elsevier.
##                         
## 
## https://www.bibliometrix.org
## 
##                         
## For information and bug reports:
##                         - Send an email to info@bibliometrix.org   
##                         - Write a post on https://github.com/massimoaria/bibliometrix/issues
##                         
## Help us to keep Bibliometrix free to download and use by contributing with a small donation to support our research team (https://bibliometrix.org/donate.html)
## 
##                         
## To start with the shiny web-interface, please digit:
## biblioshiny()
```

##### 2.4 The next step is to import the data. The import function is bibliometrix::convert2df(). It is important to inform as an argument the path of .bib file, the data source (dbsource) and the file format.

SCOPUS

















