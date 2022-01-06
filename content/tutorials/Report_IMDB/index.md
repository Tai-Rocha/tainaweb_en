---
title: "Internet Movie Database report - Relatório do Banco de Dados de Filmes da Internet"
description: |
  This is the final report of the Data Science R  II course presented to curso-r to obtain the certification. Este é o relatório final do   cursor R para Ciência de Dados II apresntado para curso-r para obtenção do certificado.
author: Tainá Carreira da Rocha
date: 2021-12-27
output: rmarkdown::gitthub_document
---

This is the final report of the Data Science R  II course presented to curso-r to obtain the certification. The course was promoted by [curso-r](https://curso-r.com/), a Brazilian company specialized in teaching Data Science using the R language.  The [course](https://curso-r.github.io/202111-r4ds-2/) happened in November 2021 and was ministered by Caio Castro and Amanda Amorim |<br/>
<div style="text-align: justify"> Este é o relatório final do   cursor R para Ciência de Dados II apresentado para curso-r para obtenção do certificado.O curso foi promovido pela curso-r, empresa brasileira especializada no ensino de Ciência de Dados na linguagem R. O curso aconteceu em Novembro de 2021 e foi ministrado por Caio Leite e Amanda Amorim. 


### The IMDB dataset | A base de dados IMDB 

Internet Movie Database (IMDB) is an online database of information related to films, television series, home videos, video games, and streaming content online – including cast, production crew and personal biographies, plot summaries, trivia, ratings, and fan and critical reviews.|<br/>
IMDB um banco de dados online de informações relacionadas a filmes, séries de televisão, vídeos caseiros, videogames e streaming de conteúdo online - incluindo elenco, equipe de produção e biografias pessoais, resumos de enredo, curiosidades, classificações e análises de fãs e críticas
</div>

The report respond follow questions: | Este relatório responde as seguintes perguntas :

1. Which month of the year has the most movies? And what day of the year?| Qual o mês do ano com o maior número de filmes? E o dia do ano?

2. Which top 5 countries with most films in the dataset ?| Qual o top 5 países com mais filmes na base?

3. List all currency in `orcamento` and  `receita` columns of `imdb_completa` dataset.| Liste todas as moedas que aparecem nas colunas `orcamento` e `receita` da base `imdb_completa`.

4. Considerando apenas orçamentos e receitas em dólar ($), qual o gênero com maior lucro? E com maior nota média?

5. Dentre os filmes na base `imdb_completa`, escolha o seu favorito. Então faça os itens a seguir:

a) Quem dirigiu o filme? Faça uma ficha dessa pessoa: idade (hoje em dia ou data de falecimento), onde nasceu, quantos filmes já dirigiu, qual o lucro médio dos filmes que dirigiu (considerando apenas valores em dólar) e outras informações que achar interessante (base `imdb_pessoas`).

b) Qual a posição desse filme no ranking de notas do IMDB? E no ranking de lucro (considerando apenas valores em dólar)?

c) Em que dia esse filme foi lançado? E dia da semana? Algum outro filme foi lançado no mesmo dia? Quantos anos você tinha nesse dia?

d) Faça um gráfico representando a distribuição da nota atribuída a esse filme por idade (base `imdb_avaliacoes`).



Load the libraries. If it's not installed use command _install.packages("pckg name")_ 


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)
```

Datasets




1. Which month of the year has the most movies? And what day of the year?| Qual o mês do ano com o maior número de filmes? E o dia do ano?


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)

## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)


## Month

month_movies <- imdb |>  
  mutate(data_lancamento_2 = as.Date(ymd(data_lancamento))) |> # Convert from character to calendar date
  mutate(month = month(data_lancamento_2)) |> # Get/set months component of a date-time
  filter(across (c(ano, month), ~ !is.na(.))) |>  # Remove NA in ano, month columns using forcats
  mutate(year_month = paste(ano, month, sep = "/")) |> # Union of year and Month in column 
  group_by(year_month) |> # Take Column year_month and...
  mutate(count_movies = n_distinct(titulo)) |>  # Count unique values
  ungroup() |> 
  nest_by(year_month, count_movies) |> 
  arrange(desc(count_movies)) |>
  head(1)
month_movies  
```
## Day


```r
library(dplyr)
library(forcats)
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(knitr)
library(tibble)
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x lubridate::as.difftime() masks base::as.difftime()
## x lubridate::date()        masks base::date()
## x dplyr::filter()          masks stats::filter()
## x lubridate::intersect()   masks base::intersect()
## x dplyr::lag()             masks stats::lag()
## x lubridate::setdiff()     masks base::setdiff()
## x lubridate::union()       masks base::union()
```

```r
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
day_movies <- imdb |>  
  mutate(data_lancamento_2 = as.Date(ymd(data_lancamento))) |>   
  mutate(month = month(data_lancamento_2)) |>  
  mutate(day = day(data_lancamento_2)) |>  
  filter(across(c(day, month, ano), ~ !is.na(.))) |>  
  mutate(year_month_day = paste(ano, month, day, sep = "/")) |>  
  group_by(year_month_day) |>  
  mutate(count_movies = n_distinct(titulo)) |>  
  ungroup() |> 
  nest_by(year_month_day, count_movies) |>  
  arrange(desc(count_movies)) |>  
  head(1)
```

```
## Warning: 4563 failed to parse.
```

```r
day_movies
```

```
## # A tibble: 1 × 3
## # Rowwise:  year_month_day, count_movies
##   year_month_day count_movies                data
##   <chr>                 <int> <list<tibble[,24]>>
## 1 2018/10/26               46           [46 × 24]
```


2. Which top 5 countries with most films in the dataset ?| Qual o top 5 países com mais filmes na base?


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)

## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)


top_5 <- imdb |> 
  filter(!is.na(pais))  |> 
  group_by(pais) |> 
  count(pais, sort = TRUE) |> 
head(5)
top_5
```

3. List all currency in `orcamento` and  `receita` columns of `imdb_completa` dataset.| Liste todas as moedas que aparecem nas colunas `orcamento` e `receita` da base `imdb_completa`.



```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
currency <- imdb |> 
  filter(across(c(orcamento, receita), ~!is.na(.)))  |> 
  summarise(unique(across(
    .cols = c(orcamento, receita),
    .fns = ~ str_remove((.x), pattern = '[0-9]+')))) 
currency
```

```
## # A tibble: 62 × 2
##    orcamento receita
##    <chr>     <chr>  
##  1 "$ "      "$ "   
##  2 "NOK "    "$ "   
##  3 "GBP "    "$ "   
##  4 "DEM "    "$ "   
##  5 "FRF "    "$ "   
##  6 "SEK "    "$ "   
##  7 "ITL "    "$ "   
##  8 "JPY "    "$ "   
##  9 "RUR "    "$ "   
## 10 "AUD "    "$ "   
## # … with 52 more rows
```

4. Considerando apenas orçamentos e receitas em dólar ($), qual o gênero com maior lucro? E com maior nota média?

Maior Lucro por Gênero


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
gen_dolar <- imdb |> 
filter(across(c(orcamento, receita), ~!is.na(.)))  |>
 mutate(across(starts_with(c("orcamento", "receita")), ~gsub("\\$", "", .))) |> 
 mutate(across(.cols = c(orcamento, receita), .fns = ~ as.numeric(.x))) |> 
 mutate(lucro = c(receita - orcamento)) |> 
 filter(across(c(lucro, genero), ~!is.na(.))) |> 
 mutate(genero = str_split(genero, ", ")) |>  
 unnest(genero) |> 
 nest_by(genero, lucro) |> 
 arrange(desc(lucro)) |> 
 head(1)
```

```
## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion
```

```r
gen_dolar
```

```
## # A tibble: 1 × 3
## # Rowwise:  genero, lucro
##   genero      lucro                data
##   <chr>       <dbl> <list<tibble[,20]>>
## 1 Action 2553439092            [1 × 20]
```


Título

```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
gen_mean <- imdb |>  
  left_join(imdb_avaliacoes, by = "id_filme", copy = TRUE) |> 
  filter(across(c(genero, nota_media), ~!is.na(.))) |>  
  mutate(genero = str_split(genero, ", ")) |>  
  unnest(genero) |>  
  nest_by(genero, nota_media) |>  
  arrange(desc(nota_media)) |>  
  head(1) 
gen_mean
```

```
## # A tibble: 1 × 3
## # Rowwise:  genero, nota_media
##   genero nota_media                data
##   <chr>       <dbl> <list<tibble[,37]>>
## 1 Crime         9.8            [1 × 37]
```


5. Dentre os filmes na base `imdb_completa`, escolha o seu favorito. Então faça os itens a seguir:

a) Quem dirigiu o filme? Faça uma ficha dessa pessoa: idade (hoje em dia ou data de falecimento), onde nasceu, quantos filmes já dirigiu, qual o lucro médio dos filmes que dirigiu (considerando apenas valores em dólar) e outras informações que achar interessante (base `imdb_pessoas`).

Direction

```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
esdto_director <- imdb |>  
  filter(str_detect(id_filme, pattern = "tt1305806$")) |>  
  select("Diretor Name's"= direcao)

esdto_director
```

```
## # A tibble: 1 × 1
##   `Diretor Name's`    
##   <chr>               
## 1 Juan José Campanella
```


General Infos


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
general_infos <- imdb_pessoas |> 
 filter(str_detect(nome, pattern = "Juan José Campanella")) |> 
  #filter(str_detect(titulo, pattern = "tt1305806$", negate = TRUE)) %>% 
  #summarise(n_filmes = n_distinct(titulo))
  mutate(across(.cols = data_nascimento, .fns = ~ as.Date(ymd(.)))) |> 
  mutate(idade = Sys.Date() - data_nascimento) |> 
  mutate(across(.cols = idade, .fns = ~ as.numeric(.x))) |> 
  mutate(idade = idade/365) |> 
  select(idade, local_nascimento, local_falecimento, data_nascimento, data_falecimento)
  
  general_infos
```

```
## # A tibble: 1 × 5
##   idade local_nascimento       local_falecimen… data_nascimento data_falecimento
##   <dbl> <chr>                  <chr>            <date>          <date>          
## 1  62.5 Buenos Aires, Argenti… <NA>             1959-07-19      NA
```

N filmes dirigidos 



```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
juan_all_movies  <- imdb |> 
  left_join(imdb_avaliacoes, by = "id_filme", copy = TRUE) |>
  filter(str_detect(direcao, pattern = "Juan José Campanella")) |> 
  filter(str_detect(id_filme, pattern = "tt1305806$", negate = TRUE)) |>  
  summarise(n_filmes = n_distinct(id_filme))

  juan_all_movies  
```

```
## # A tibble: 1 × 1
##   n_filmes
##      <int>
## 1        7
```

Lucros médios dos filmes dirigidos 


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
lucro_medio <- imdb_completa %>% 
  filter(str_detect(direcao, pattern = "Milos Forman")) %>% 
  filter(across(c(receita, orcamento), ~!is.na(.), str_detect((.x), pattern = "^.[[:blank:][:digit:]]"))) %>% 
  mutate(across(
    .cols = c(orcamento, receita),
    .fns = ~ (str_extract((.x), pattern = ".[[:digit:]]+!*")))) %>% 
  mutate(across(
    .cols = c(orcamento, receita),
    .fns = ~ as.numeric(.x))) %>% 
  mutate(lucro = receita - orcamento) %>% 
  summarise(lucro_medio = mean(lucro))
```

```
## Error in filter(., str_detect(direcao, pattern = "Milos Forman")): object 'imdb_completa' not found
```


b) Qual a posição desse filme no ranking de notas do IMDB? E no ranking de lucro (considerando apenas valores em dólar)?

Grade Ranking 


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
ranking_grade <- imdb |>  
  left_join(imdb_avaliacoes, by = "id_filme", copy = TRUE) |>
  group_by(nota_media) |> 
  arrange((desc(nota_media))) |> 
  rowid_to_column(var = "Ranking_number") |> 
  filter(str_detect(id_filme, pattern = "tt1305806$"))
 
ranking_grade
```

```
## # A tibble: 1 × 40
## # Groups:   nota_media [1]
##   Ranking_number id_filme  titulo  titulo_original    ano data_lancamento genero
##            <int> <chr>     <chr>   <chr>            <dbl> <chr>           <chr> 
## 1           1867 tt1305806 Il seg… El secreto de s…  2009 2010-06-04      Drama…
## # … with 33 more variables: duracao <dbl>, pais <chr>, idioma <chr>,
## #   orcamento <chr>, receita <chr>, receita_eua <chr>, nota_imdb <dbl>,
## #   num_avaliacoes.x <dbl>, direcao <chr>, roteiro <chr>, producao <chr>,
## #   elenco <chr>, descricao <chr>, num_criticas_publico <dbl>,
## #   num_criticas_critica <dbl>, num_avaliacoes.y <dbl>, nota_media <dbl>,
## #   nota_mediana <dbl>, nota_media_ponderada <dbl>,
## #   nota_media_idade_0_18 <dbl>, num_votos_idade_0_18 <dbl>, …
```


Lucro Ranking 


```r
library(dplyr)
library(forcats)
library(lubridate)
library(knitr)
library(tibble)
library(tidyverse)
library(stringr)

imdb <- basesCursoR::pegar_base("imdb_completa")
head(imdb)
```

```
## # A tibble: 6 × 21
##   id_filme  titulo   titulo_original    ano data_lancamento genero duracao pais 
##   <chr>     <chr>    <chr>            <dbl> <chr>           <chr>    <dbl> <chr>
## 1 tt0000009 Miss Je… Miss Jerry        1894 1894-10-09      Roman…      45 USA  
## 2 tt0000574 The Sto… The Story of th…  1906 1906-12-26      Biogr…      70 Aust…
## 3 tt0001892 Den sor… Den sorte drøm    1911 1911-08-19      Drama       53 Germ…
## 4 tt0002101 Cleopat… Cleopatra         1912 1912-11-13      Drama…     100 USA  
## 5 tt0002130 L'Infer… L'Inferno         1911 1911-03-06      Adven…      68 Italy
## 6 tt0002199 From th… From the Manger…  1912 1913            Biogr…      60 USA  
## # … with 13 more variables: idioma <chr>, orcamento <chr>, receita <chr>,
## #   receita_eua <chr>, nota_imdb <dbl>, num_avaliacoes <dbl>, direcao <chr>,
## #   roteiro <chr>, producao <chr>, elenco <chr>, descricao <chr>,
## #   num_criticas_publico <dbl>, num_criticas_critica <dbl>
```

```r
##  Get IMDB People dataset
imdb_pessoas <- basesCursoR::pegar_base("imdb_pessoas")
head(imdb_pessoas)
```

```
## # A tibble: 6 × 15
##   pessoa_id nome   nome_nascimento altura bio   data_nascimento local_nascimento
##   <chr>     <chr>  <chr>            <dbl> <chr> <chr>           <chr>           
## 1 nm0000001 Fred … Frederic Auste…    177 "Fre… 1899-05-10      Omaha, Nebraska…
## 2 nm0000002 Laure… Betty Joan Per…    174 "Lau… 1924-09-16      The Bronx, New …
## 3 nm0000003 Brigi… Brigitte Bardot    166 "Bri… 1934-09-28      Paris, France   
## 4 nm0000004 John … John Adam Belu…    170 "Joh… 1949-01-24      Chicago, Illino…
## 5 nm0000005 Ingma… Ernst Ingmar B…    179 "Ern… 1918-07-14      Uppsala, Uppsal…
## 6 nm0000006 Ingri… Ingrid Bergman     178 "Ing… 1915-08-29      Stockholm, Swed…
## # … with 8 more variables: data_falecimento <date>, local_falecimento <chr>,
## #   razao_falecimento <chr>, nome_conjuges <chr>, num_conjuges <dbl>,
## #   num_divorcios <dbl>, num_filhos <dbl>, num_conjuges_com_filhos <dbl>
```

```r
## Get IMDB assessments
imdb_avaliacoes <- basesCursoR::pegar_base("imdb_avaliacoes")
head(imdb_avaliacoes)
```

```
## # A tibble: 6 × 19
##   id_filme  num_avaliacoes nota_media nota_mediana nota_media_ponderada
##   <chr>              <dbl>      <dbl>        <dbl>                <dbl>
## 1 tt0000009            154        5.9            6                  5.9
## 2 tt0000574            589        6.3            6                  6.1
## 3 tt0001892            188        6              6                  5.8
## 4 tt0002101            446        5.3            5                  5.2
## 5 tt0002130           2237        6.9            7                  7  
## 6 tt0002199            484        5.8            6                  5.7
## # … with 14 more variables: nota_media_idade_0_18 <dbl>,
## #   num_votos_idade_0_18 <dbl>, nota_media_idade_18_30 <dbl>,
## #   num_votos_idade_18_30 <dbl>, nota_media_idade_30_45 <dbl>,
## #   num_votos_idade_30_45 <dbl>, nota_media_idade_45_mais <dbl>,
## #   num_votos_idade_45_mais <dbl>, nota_media_top_1000_avaliadores <dbl>,
## #   num_votos_top_1000_avaliadores <dbl>, nota_media_eua <dbl>,
## #   num_votos_eua <dbl>, nota_media_fora_eua <dbl>, num_votos_fora_eua <dbl>
```

```r
ranking_lucro <-imdb |> 
 filter(across(c(orcamento, receita), ~!is.na(.)))  |>
 mutate(across(starts_with(c("orcamento", "receita")), ~gsub("\\$", "", .))) |> 
 mutate(across(.cols = c(orcamento, receita), .fns = ~ as.numeric(.x))) |> 
 mutate(lucro = c(receita - orcamento)) |> 
 filter(across(c(lucro, genero), ~!is.na(.))) |> 
 group_by(lucro) |> 
  arrange(desc(lucro)) |> 
  rowid_to_column(var = "Ranking_Lucro") |> 
  filter(str_detect(id_filme, pattern = "tt1305806$"))
```

```
## Warning in mask$eval_all_mutate(quo): NAs introduced by coercion
```

```r
ranking_lucro
```

```
## # A tibble: 1 × 23
## # Groups:   lucro [1]
##   Ranking_Lucro id_filme  titulo   titulo_original   ano data_lancamento genero 
##           <int> <chr>     <chr>    <chr>           <dbl> <chr>           <chr>  
## 1          2001 tt1305806 Il segr… El secreto de …  2009 2010-06-04      Drama,…
## # … with 16 more variables: duracao <dbl>, pais <chr>, idioma <chr>,
## #   orcamento <dbl>, receita <dbl>, receita_eua <chr>, nota_imdb <dbl>,
## #   num_avaliacoes <dbl>, direcao <chr>, roteiro <chr>, producao <chr>,
## #   elenco <chr>, descricao <chr>, num_criticas_publico <dbl>,
## #   num_criticas_critica <dbl>, lucro <dbl>
```



c) Em que dia esse filme foi lançado? E dia da semana? Algum outro filme foi lançado no mesmo dia? Quantos anos você tinha nesse dia?


d) Faça um gráfico representando a distribuição da nota atribuída a esse filme por idade (base `imdb_avaliacoes`).

