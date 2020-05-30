---
title: "Reto Analisis Cócteles"
date: 2018-01-28
tags: [Analytics, BI, EDA]
header:
excerpt: "Analytics, BI, EDA"
mathjax: "true"
---

_Respondiendo al reto de [Daniel Jimenez](https://www.danieljimenezm.com), hare un analisis exploratorio de los datos de cócteles [data](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-05-26/cocktails.csv). 

<br>


<iframe width="950" height="600" src="https://app.powerbi.com/view?r=eyJrIjoiZTdlMTdkMTUtMzk2ZC00MTI1LTllNWMtMWFlZGUzMDg1OTc4IiwidCI6IjcxOGE2MTYzLWE5YzYtNDdlMi1iYzRjLTZmMjRmMGJjMjYyYyJ9" frameborder="0" allowFullScreen="true"></iframe>

<br>


_Conclusiones Pestaña "Ingredientes":

* _De las 545 bebidas que hay en esta tabla, el 87.71% de ellas son alcoholicas, el 10,54% no alcoholica y el 1.65% son con opcion de alcohol.
* _Asi mismo el top 5 de ingredientes que en general se usan mas para hacer cócteles son el vodka, ginebra, azucar, jugo de naranja y el jugo de limon.
- _Los ingredientes mas usados para bebidas no alcoholicas de este conjunto son:
   - _Azucar
   - _Agua
   - _Leche
   - _Jugo de piña
   - _Hielo
   - _Yogurth

Here's a numbered list:
1. First
2. Second
3. Third

Python code block:
```python
    import numpy as np

    def test_function(x, y):
      z = np.sum(x,y)
      return z
```

R code block:
```r
library(tidyverse)
df <- read_csv("some_file.csv")
head(df)
```

Here's some inline code `x+y`.

Here's an image:
<img src="{{ site.url }}{{ site.baseurl }}/images/perceptron/linsep.jpg" alt="linearly separable data">

Here's another image using Kramdown:
![alt]({{ site.url }}{{ site.baseurl }}/images/perceptron/linsep.jpg)

Here's some math:

$$z=x+y$$

You can also put it inline $$z=x+y$$
