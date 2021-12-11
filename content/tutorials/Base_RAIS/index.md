---
title: "Analisando dados da RAIS"
description: |
  Essa análise teve como objetivo obter os salários na base da RAIS. Esse projeto foi feito na Semana Data Science na Prática da Curso-R.
author: curso-r e Tainá
date: 2021-12-09
output: rmarkdown::github_document
---




Nesse relatório estamos interessados em responder a pergunta:

"Quanto ganha um cientista de dados?"

Para isso vamos utilizar a base da RAIS anonimizada

# Acessando os dados da RAIS

Vamos utilizar [o datalake da iniciativa base dos dados](https://basedosdados.org).

A base de dados que queremos analisar aqui é a base de pessoas que (potencialmente) trabalham com ciência de dados. Existe um Código Brasileiro de Ocupações (CBO), que tem um cadastro de todas as ocupações formais no Brasil. Vamos pegar alguns códigos que são relacionados a ciência de dados e filtrar a base da RAIS para obter os dados dessas pessoas.


```r
library(bigrquery)
library(dplyr)
```

Abaixo está o código que carrega as primeiras 5 linhas da tabela de microdados.



Vamos fazer a mesma coisa utilizando o pipe!


```r
primeiras_cinco_linhas_com_pipe <- tbl(conexao, "microdados_vinculos") |>  
  select(everything()) |>  
  head(5) |>  
  collect()
# atalho: ctrl+shift+M
# antes e atualmente: {magrittr}: %>%
# atualmente (4.1 ou +): |>
primeiras_cinco_linhas_com_pipe
```


Pergunta principal de pesquisa: 
Quem trabalha com ciência de dados ganha quanto?


```r
codigos_cbo <- c(
  "252515", "252525", "211110",
  # pesquisa/cientista
  "211205", "211210","411035",
  "211210", "131120","211215"
  # ocupações estatísticas
)
microdados_tbl <- tbl(conexao, "microdados_vinculos") |>  
  select(everything()) |>  
  filter(
    ano >= 2013,
    cbo_2002 %in% codigos_cbo
  ) |>  
  head(5000)
tabela_microdados_vinculos <- collect(microdados_tbl)
View(tabela_microdados_vinculos)
```

Agora vamos rodar com a base completa!


```r
microdados_tbl <- tbl(conexao, "microdados_vinculos") |>  
  select(everything()) |>  
  filter(
    ano >= 2013,
    cbo_2002 %in% codigos_cbo
  )
tabela_microdados_vinculos <- collect(microdados_tbl)
saveRDS(tabela_microdados_vinculos, "tabela_microdados_vinculos.rds")
```

## Perguntas de pesquisa

- Quanto ganha uma pessoa que trabalha com ciência de dados

Perguntas mais específicas

- Quanto o valor médio varia no tempo?
- Quanto o valor médio varia regionalmente?
- Quanto o valor médio varia por características das pessoas?
    - Gênero
    - Raça/cor
    - Idade

- [Desafio] Qual cargo tem a maior taxa de crescimento dentro daquele setor da economia (CNAE) proporcionalmente a municípios com mais pessoas empregadas naquela CBO

### Como variam os salários médios no tempo?


```r
tabela_microdados_vinculos <- readRDS("tabela_microdados_vinculos.rds")
library(ggplot2)
### Comentários:
## ctrl+shift+c
tabela_medias <- tabela_microdados_vinculos |> 
  group_by(ano) |>  
  summarise(media_salario = mean(valor_remuneracao_media))
## Funções do {dplyr} que vamos usar:
# filter: filtra linhas
# select: seleciona colunas
# mutate: cria colunas
# group_by + summarise: summariza a base
# arrange: ordena a base
ggplot(tabela_medias) +
  aes(x = ano, y = media_salario) +
  geom_col() +
  scale_x_continuous(breaks = 2013:2019)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

Agora vamos ver os números exatos:


```r
library(knitr)
tabela_medias %>% 
  kable()
```



|  ano| media_salario|
|----:|-------------:|
| 2013|      3457.553|
| 2014|      3702.131|
| 2015|      4229.452|
| 2016|      4409.327|
| 2017|      4969.977|
| 2018|      4886.116|
| 2019|      4969.408|

### Quanto o salário médio varia regionalmente?


```r
tabela_media_uf <- tabela_microdados_vinculos %>% 
  group_by(sigla_uf) |>  
  summarise(
    media = mean(valor_remuneracao_media)
  )
```

Essa visualização a princípio é melhor em tabela:


```r
knitr::kable(tabela_media_uf)
```



|sigla_uf |    media|
|:--------|--------:|
|AC       | 2213.220|
|AL       | 1921.249|
|AM       | 4003.298|
|AP       | 2488.921|
|BA       | 3444.450|
|CE       | 3583.833|
|DF       | 6290.945|
|ES       | 2289.896|
|GO       | 3053.561|
|MA       | 1518.701|
|MG       | 3373.300|
|MS       | 2671.339|
|MT       | 2520.127|
|PA       | 4239.167|
|PB       | 1852.823|
|PE       | 4014.886|
|PI       | 1556.301|
|PR       | 2835.612|
|RJ       | 7303.597|
|RN       | 2495.838|
|RO       | 1943.391|
|RR       | 2780.840|
|RS       | 3434.072|
|SC       | 3033.095|
|SE       | 3733.273|
|SP       | 5053.901|
|TO       | 1981.932|

Agora olhando em gráfico:


```r
tabela_media_uf |> 
  ggplot(aes(x = sigla_uf, y = media)) +
  geom_col()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

Esse gráfico é legal até pra colocar na análise explicativa! DF e RJ aparentemente estão muito acima dos demais estados, conforme destaca o gráfico abaixo:


```r
library(forcats)
tabela_media_uf |>  
  mutate(
    sigla_uf_fator = fct_reorder(sigla_uf, media)
  ) |>  
  ggplot(aes(y = sigla_uf_fator, x = media)) + 
  geom_col() +
  labs(y = "Unidade da Federação", x = "Média Salarial (R$)")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />

Será que essa mesma conclusão permanece quando escolhemos a mediana como medida resumo dos salários?


```r
tabela_mediana_uf <- tabela_microdados_vinculos %>% 
  group_by(sigla_uf) %>% 
  summarise(
    mediana = median(valor_remuneracao_media)
  )
tabela_mediana_uf %>% 
  arrange(desc(mediana)) %>% 
  knitr::kable()
```



|sigla_uf |  mediana|
|:--------|--------:|
|RJ       | 7528.560|
|DF       | 4715.790|
|SP       | 3862.090|
|AM       | 3276.510|
|PE       | 2540.750|
|PA       | 2255.790|
|RS       | 2211.380|
|MS       | 2199.530|
|SE       | 2134.990|
|MG       | 2124.070|
|RN       | 2050.670|
|RR       | 2000.000|
|BA       | 1923.050|
|MT       | 1872.510|
|GO       | 1858.930|
|SC       | 1833.685|
|CE       | 1790.210|
|PR       | 1728.040|
|AP       | 1473.145|
|ES       | 1450.890|
|TO       | 1352.080|
|RO       | 1347.035|
|AC       | 1334.445|
|AL       | 1279.175|
|PB       | 1222.690|
|PI       | 1093.530|
|MA       | 1008.330|


```r
tabela_mediana_uf %>% 
  mutate(
    sigla_uf = fct_reorder(sigla_uf, mediana)
  ) %>% 
  ggplot(aes(x = mediana, y = sigla_uf)) +
  geom_col()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="672" />

```r
tabela_media_uf %>% 
  mutate(
    sigla_uf_fator = fct_reorder(sigla_uf, media)
  ) %>% 
  ggplot(aes(y = sigla_uf_fator, x = media)) + 
  geom_col() +
  labs(y = "Unidade da Federação", x = "Média Salarial (R$)")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-2.png" width="672" />

### Os salários variam por sexo?


```r
tabela_resumo_sexo <- tabela_microdados_vinculos |>  
  group_by(sexo) |>  
  summarise(
    media = mean(valor_remuneracao_media),
    mediana = median(valor_remuneracao_media)
  )
```


```r
tabela_resumo_sexo |>  
  knitr::kable()
```



|sexo |    media| mediana|
|:----|--------:|-------:|
|1    | 5331.746| 3905.72|
|2    | 3600.132| 2300.71|

### Os salários variam por Raça/Cor?


```r
tabela_resumo_raca_cor <- tabela_microdados_vinculos |>  
  group_by(raca_cor) |>  
  summarise(
    media = mean(valor_remuneracao_media),
    mediana = median(valor_remuneracao_media)
  )
```


```r
tabela_resumo_raca_cor |>  
  knitr::kable()
```



|raca_cor |    media|  mediana|
|:--------|--------:|--------:|
|1        | 3085.090| 2748.890|
|2        | 4287.588| 2888.915|
|4        | 3014.454| 2072.205|
|6        | 6783.212| 5338.735|
|8        | 2732.073| 1749.345|
|9        | 5656.755| 4888.030|


```r
tabela_resumo_sexo_raca_cor <- tabela_microdados_vinculos |>  
  group_by(cbo_2002, raca_cor, sexo) |>  
  summarise(
    media = mean(valor_remuneracao_media),
    mediana = median(valor_remuneracao_media)
  )
```

```
## `summarise()` has grouped output by 'cbo_2002', 'raca_cor'. You can override using the `.groups` argument.
```


```r
tabela_resumo_sexo_raca_cor |>  
  knitr::kable()
```



|cbo_2002 |raca_cor |sexo |     media|   mediana|
|:--------|:--------|:----|---------:|---------:|
|131120   |1        |1    |  3044.484|  2846.390|
|131120   |1        |2    |  3711.533|  2894.720|
|131120   |2        |1    |  5754.384|  3710.660|
|131120   |2        |2    |  4713.267|  3547.830|
|131120   |4        |1    |  4070.667|  3482.140|
|131120   |4        |2    |  4139.647|  3632.360|
|131120   |6        |1    |  4749.999|  2720.095|
|131120   |6        |2    |  4527.754|  2914.305|
|131120   |8        |1    |  4222.106|  3094.520|
|131120   |8        |2    |  3949.073|  3288.460|
|131120   |9        |1    |  2756.113|  1969.710|
|131120   |9        |2    |  2504.534|  1911.410|
|211110   |1        |1    |  4977.286|  2252.620|
|211110   |1        |2    |  4179.436|  2012.500|
|211110   |2        |1    |  7404.453|  6095.230|
|211110   |2        |2    |  5222.835|  4156.200|
|211110   |4        |1    |  4865.089|  2888.650|
|211110   |4        |2    |  4005.826|  2958.860|
|211110   |6        |1    |  7008.334|  5988.050|
|211110   |6        |2    |  7416.892|  6036.220|
|211110   |8        |1    |  4204.910|  2527.780|
|211110   |8        |2    |  3517.021|  2640.690|
|211110   |9        |1    |  6269.211|  4172.000|
|211110   |9        |2    |  6317.404|  5708.750|
|211205   |1        |1    |  2452.480|  2596.560|
|211205   |1        |2    |  4670.118|  5284.330|
|211205   |2        |1    |  9739.266|  7073.535|
|211205   |2        |2    |  7194.206|  6024.610|
|211205   |4        |1    |  6618.641|  5911.410|
|211205   |4        |2    |  3582.730|  3000.000|
|211205   |6        |1    | 12634.457|  9633.100|
|211205   |6        |2    |  9791.802|  8536.310|
|211205   |8        |1    |  5586.215|  4226.040|
|211205   |8        |2    |  4479.939|  3499.990|
|211205   |9        |1    | 11754.928| 11684.510|
|211205   |9        |2    | 10258.871| 10730.140|
|211210   |2        |1    |  6223.349|  5000.000|
|211210   |2        |2    |  5006.268|  4205.330|
|211210   |4        |1    |  5751.621|  5383.535|
|211210   |4        |2    |  2229.416|  2174.430|
|211210   |6        |1    | 20459.611|  7901.545|
|211210   |6        |2    |  9277.978|  9206.500|
|211210   |8        |1    |  5563.023|  4211.365|
|211210   |8        |2    |  4324.135|  3893.330|
|211210   |9        |1    |  6854.247|  4244.570|
|211210   |9        |2    |  3512.073|  2655.990|
|211215   |2        |1    |  5207.926|  4540.070|
|211215   |2        |2    |  3800.894|  3267.630|
|211215   |4        |1    |  4066.597|  2441.385|
|211215   |4        |2    |  3845.828|  1640.805|
|211215   |6        |2    |  4565.443|  4568.040|
|211215   |8        |1    |  3819.912|  3360.800|
|211215   |8        |2    |  3867.480|  3613.510|
|211215   |9        |1    |  7037.869|  6487.440|
|211215   |9        |2    |  5029.161|  3673.585|
|252515   |1        |1    |  4785.281|  3355.735|
|252515   |1        |2    |  1983.550|  1797.470|
|252515   |2        |1    |  4657.551|  3647.480|
|252515   |2        |2    |  3300.008|  2541.730|
|252515   |4        |1    |  3349.139|  2789.920|
|252515   |4        |2    |  2794.911|  2231.555|
|252515   |6        |1    |  5347.074|  4500.000|
|252515   |6        |2    |  4647.427|  3869.710|
|252515   |8        |1    |  3324.709|  2330.370|
|252515   |8        |2    |  2253.579|  1679.805|
|252515   |9        |1    |  3502.275|  2436.540|
|252515   |9        |2    |  2537.900|  1929.530|
|252525   |1        |1    |  3106.797|  2347.750|
|252525   |1        |2    |  2949.537|  2703.485|
|252525   |2        |1    |  5237.162|  4103.680|
|252525   |2        |2    |  3358.031|  2288.415|
|252525   |4        |1    |  3774.552|  2752.570|
|252525   |4        |2    |  2280.888|  1571.345|
|252525   |6        |1    |  7953.548|  6846.910|
|252525   |6        |2    |  5427.499|  4656.830|
|252525   |8        |1    |  2949.831|  1933.715|
|252525   |8        |2    |  1993.650|  1336.285|
|252525   |9        |1    |  2657.023|  1561.455|
|252525   |9        |2    |  1749.925|  1172.880|
|411035   |1        |1    |  1680.846|  1369.315|
|411035   |1        |2    |  1470.052|  1024.120|
|411035   |2        |1    |  1572.291|  1277.860|
|411035   |2        |2    |  1534.250|  1260.000|
|411035   |4        |1    |  1404.462|  1269.740|
|411035   |4        |2    |  1359.977|  1200.860|
|411035   |6        |1    |  2173.909|  1835.260|
|411035   |6        |2    |  1862.322|  1560.500|
|411035   |8        |1    |  1477.781|  1254.150|
|411035   |8        |2    |  1392.285|  1198.305|
|411035   |9        |1    |  6775.978|  7615.605|
|411035   |9        |2    |  6044.256|  7461.230|


```r
ggplot(tabela_resumo_sexo_raca_cor,
       aes(x = raca_cor, y = media, fill = sexo)) +
  geom_col(position = 'dodge')
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />
