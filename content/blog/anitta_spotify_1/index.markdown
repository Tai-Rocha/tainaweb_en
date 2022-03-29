---
title: "Anitta- Spotify Top 1"
subtitle: ""
excerpt: "Spotify Top 1 - March 2022"
date: 2022-03-26
author: "Tainá Rocha"
draft: false
images:
series:
tags:
categories:
layout: single
---

{{< here >}}



## Welcome to the first blog section post!

Last week was marked by an important event, especially for the Brazilian artistic community, but also for the whole of Brazil!  The Anitta singer reached the first position on Spotify with "Envolver" track music, one of the most popular worldwide audio streaming platforms! Anitta is the first Latin Womann and Brazilian to reach this position in Spotify's global ranking.

Therefore, in this post, I decided to explore very basic way (for now) tools for analyzing musical data using R, to identify the popularity trajectory of  Anitta's songs over time, and what factors may be related to the popularity (but don't forget: correlation is not causality). This analysis was only possible because this data is available by [Spotify for developers](https://developer.spotify.com/), Spotify's API, where we can request the data and so receive them through JSON metadata with the artist data, album tracks, directly from Spotify Data Catalog. You can also get user data such as playlists and songs that the user saves in the library. In future posts, in the tutorials section, I plan to do a step-by-step on how to create an account in Spotify for developers and generate the credentials. And also another complete tutorial on how to do these analyzes through R.

#### Analyzing Spotify's "This Is Aniita" Playlist






The following table summarizes some popularity score statistics (0 to 100) that help us qualify the variation pattern.


```
## # A tibble: 1 × 4
##   Média Mediana `Desvio Padrão` `Coeficiente de Variação`
##   <dbl>   <dbl>           <dbl>                     <dbl>
## 1  57.5      59            14.7                     0.255
```

Since the popularity score ranges from 0 to 100, the average reveals good popularity above 50 (57.5).

#### Let's see the score in a histogram chart


<div class="figure">
<img src="{{< blogdown/postref >}}index_files/figure-html/fig-1.png" alt="Histogram of Popularity Score counting" width="672" />
<p class="caption">Figure 1: Histogram of Popularity Score counting</p>
</div>

The histogram reveals more details than the mean. Most songs have a popularity score in the 60s. Few songs (from 0 to 4) scored 0 and less than 4 songs score in the 90s. 

#### Most popular songs currently 
Considering a score greater than 80

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> Música </th>
   <th style="text-align:right;"> Popularidade </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Envolver </td>
   <td style="text-align:right;"> 96 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Boys Don't Cry </td>
   <td style="text-align:right;"> 86 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NO CHÃO NOVINHA </td>
   <td style="text-align:right;"> 85 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Envolver Remix </td>
   <td style="text-align:right;"> 83 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Faking Love (feat. Saweetie) </td>
   <td style="text-align:right;"> 81 </td>
  </tr>
</tbody>
</table>

#### Popularity of songs over the years 
<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

The overall popularity score increased over time. However, between 2015 and 2016 some songs hit the score in the 60s, followed by a drop and rising again after 2017.

#### Relationships among factors such as:
Speed (speechiness), acoustic music (acousticness), and others. Ranging from -1 to 1, where at 0 there are no evident correlations and at -1 or 1 there are correlations. Always good to remember that: correlation does not imply causality. :blush: 

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

The popularity score does not show strong correlations with the other variables.

## So let's go to music?



<iframe width="560" height="315" src="https://www.youtube.com/embed/q5R5XZgkXWA" frameborder="0" allowfullscreen></iframe>

