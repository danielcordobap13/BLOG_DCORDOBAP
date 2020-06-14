---
title: "Reto Analisis Cócteles"
date: 2018-01-28
tags: [Analytics, BI, EDA,PowerBI,Multiple correspondence Analysis (MCA)]
header:
excerpt: "Analytics, BI, EDA, Multiple correspondence Analysis (MCA), PowerBI"
mathjax: "true"
---

_Respondiendo al reto de [Daniel Jimenez](https://www.danieljimenezm.com), hare un analisis exploratorio de los datos de cócteles [data](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-05-26/cocktails.csv)_. 

<br>


<iframe width="950" height="600" src="https://app.powerbi.com/view?r=eyJrIjoiZTdlMTdkMTUtMzk2ZC00MTI1LTllNWMtMWFlZGUzMDg1OTc4IiwidCI6IjcxOGE2MTYzLWE5YzYtNDdlMi1iYzRjLTZmMjRmMGJjMjYyYyJ9" frameborder="0" allowFullScreen="true"></iframe>

<br>


### _Conclusiones Pestaña "Ingredientes"_:

* _De las 545 bebidas que hay en esta tabla, el 87.71% (478) de ellas son alcoholicas, el 10,54% (58) no alcoholica y el 1.65% (9) son con opcion de alcohol_.
* _Asi mismo el top 5 de ingredientes que en general se usan mas para hacer cócteles son el vodka, ginebra, azucar, jugo de naranja y el jugo de limon_.

- _Los ingredientes mas usados para bebidas alcoholicas de este conjunto de datos son_:
   - _Vodka_
   - _Ginebra_
   - _Azucar_
   - _Jugo de naranja_
   - _Jugo de limon_
   - _Ron suave_

- _Los ingredientes mas usados para bebidas no alcoholicas de este conjunto de datos son_:
   - _Azucar_
   - _Agua_
   - _Leche_
   - _Jugo de piña_
   - _Hielo_
   - _Yogurth_
 

- _Los ingredientes mas usados para bebidas que pueden ser alcoholicas o no de este conjunto de datos son_:
   - _Azucar_
   - _Jugo de naranja_
   - _Jugo de limon_
   - _Jugo de piña_
   - _Leche_
   - _Yogurth_
 
 <br>


### _Conclusiones Pestaña "Composicion Bebidas"_:

_En este analisis solo nos enfocaremos en las categorias "Ordinary Drink", "Cocktail", "Punch" y "Shot", ya que ellas almacenan la mayoria de cocteles de esta tabla_:

- _Ordinary Drink_:
   - _Ginebra_ _(6.51%)_
   -  _Vodka_ _(3.81%)_
   - _Jugo de limon_ _(3.77%)_
   - _Ron ligero_ _(3.21%)_
   - _Limon_ _(3.11%)_
   - _Azucar en polvo_ _(2.83%)_
   - _Cascara de limon_ _(2.64%)_
   - _Jugo de Naranja_ _(2.55%)_
   - _Granada_ _(2.45%)_
   - _Amaretto_ _(2.36%)_

- _Cocktail_:
   - _Vodka_ _(7.2%)_
   -  _Ginebra_ _(6.36%)_
   - _Granada_ _(4.66%)_
   - _Jugo de Naranja_ _(4.66%)_
   - _Jugo de limon_ _(4.66%)_
   - _Jugo de arandano_ _(3.81%)_
   - _Cascara de piña_ _(2.54%)_
   - _Absolut citron_ _(2.54%)_
   - _Angostura bitters_ _(2.12%)_
   - _Vermount seco_ _(2.12%)_

- _Shot_:
   - _Vodka_ _(6.58%)_
   -  _Bailey's_ _(5.92%)_
   - _Kahlua_ _(5.92%)_
   - _Amaretto_ _(4.61%)_
   - _Jugo de Arandano_ _(4.61%)_
   - _Midori melon licor_ _(3.95%)_
   - _151 proof ron_ _(3.29%)_
   - _Raspberry licor_ _(3.29%)_
   - _Goldschlager_ _(3.29%)_
   - _Sambuca_ _(3.29%)_

- _Punch_:
   - _Jugo de Naranja_ _(6.58%)_
   -  _Azucar_ _(5.92%)_
   - _Vino rojo_ _(5.92%)_
   - _Vodka_ _(4.61%)_
   - _Hielo_ _(4.61%)_
   - _Limon_ _(3.95%)_
   - _Jugo de piña_ _(3.29%)_
   - _Ron_ _(3.29%)_
   - _Clavos_ _(3.29%)_
   - _Jugo de Limon_ _(3.29%)_

   

 <br>


### _Conclusiones Pestaña "Vasos y Medidas"_:

_Acorde a los dos histogramas que hay en esta pestaña, los vasos más usados para bebidas alcoholicas son_:
   - _Vaso cocktel_ 
   - _Vaso Collins_
   - _Highball Vaso_
   - _Vaso "Old Fashion"_ 

_Las medidas más usadas en bebidas alcoholicas son :_
   - _1 oz_
   - _1/2 oz_
   - _2 oz_

_Para las bebidas alcolohicas los vasos más usados son_:
   - _Highball Vaso_ 
   - _Coffe mug_ 
   - _Vaso Collins_

_En el caso de las bebidas que pueden o no ser alcoholicas, los vasos mas usados son_:
   - _Vaso Collins_
   - _Punch bowl_
   
   
<br>

### _Analisis Multivariado:_
_Una vez se hizo un analisis descriptivo univariado, se procede a realizar un analisis multivariados para encontrar la combinacion entre atributos que generan los cockteles, para ello utilizaremos R_:


   


R code block:
```r
library(tidylo)
library(dplyr)
library(SensoMineR)
library(FactoMineR)
library(explor)



cocktails <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-05-26/cocktails.csv')


MC1=MCA(cocktails[,c("alcoholic","ingredient","category")])
explor(MC1)
```


Aqui los resultados:

<img width="950" height="600" src="/images/perceptron/rsa_2.jpg" allowFullScreen="true">
