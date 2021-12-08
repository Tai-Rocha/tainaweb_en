---
title: "Base RAIS"
description: |
  Semana de Data Science na Prática 
author: "Tainá Rocha"
date: "2021-12-07"
output: html_document
editor_options: 
  markdown: 
    wrap: 72
---




"Quanto ganha um cientisat de Ddos "

# Acessando dados RAIS
 
Vamos utilizar o [datalake da inciativa base dados](https://basedosdados.org/) 

Usando o robo


```r
library(bigrquery)
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
conection <- dbConnect(
  bigquery(),
  project = "basedosdados",
  dataset = "br_me_rais",
  billing = "Semana Data Science" # Google Cloud Project 
)
conection
```

```
## <BigQueryConnection>
##   Dataset: basedosdados.br_me_rais
##   Billing: Semana Data Science
```





Abixo está o código que carrega as primeiras 5 linhas de microdados 
 
Ler de um arquivo 

```r
tabela_normal <- read.csv("https://raw.githubusercontent.com/curso-r/main-r4ds-1/master/dados/imdb.csv")

head(tabela_normal)
```

```
##                                      titulo  ano           diretor duracao
## 1                                   Avatar  2009     James Cameron     178
## 2 Pirates of the Caribbean: At World's End  2007    Gore Verbinski     169
## 3                    The Dark Knight Rises  2012 Christopher Nolan     164
## 4                              John Carter  2012    Andrew Stanton     132
## 5                             Spider-Man 3  2007         Sam Raimi     156
## 6                                  Tangled  2010      Nathan Greno     100
##     cor                                                   generos pais
## 1 Color                           Action|Adventure|Fantasy|Sci-Fi  USA
## 2 Color                                  Action|Adventure|Fantasy  USA
## 3 Color                                           Action|Thriller  USA
## 4 Color                                   Action|Adventure|Sci-Fi  USA
## 5 Color                                  Action|Adventure|Romance  USA
## 6 Color Adventure|Animation|Comedy|Family|Fantasy|Musical|Romance  USA
##         classificacao orcamento   receita nota_imdb likes_facebook       ator_1
## 1 A partir de 13 anos 237000000 760505847       7.9          33000  CCH Pounder
## 2 A partir de 13 anos 300000000 309404152       7.1              0  Johnny Depp
## 3 A partir de 13 anos 250000000 448130642       8.5         164000    Tom Hardy
## 4 A partir de 13 anos 263700000  73058679       6.6          24000 Daryl Sabara
## 5 A partir de 13 anos 258000000 336530303       6.2              0 J.K. Simmons
## 6               Livre 260000000 200807262       7.8          29000 Brad Garrett
##             ator_2               ator_3
## 1 Joel David Moore            Wes Studi
## 2    Orlando Bloom       Jack Davenport
## 3   Christian Bale Joseph Gordon-Levitt
## 4  Samantha Morton         Polly Walker
## 5     James Franco        Kirsten Dunst
## 6     Donna Murphy          M.C. Gainey
```

