
---

#### Title: "Interpretando un Algoritmo de ML Enfocado a un Score de credito"

#### Date: 2020-09-09

#### Tags: Analytics, Credit Risk, R, Machine Learning Interpretability, H2O


#### Excerpt: "Analytics,H2O, Credit Risk, Machine Learning Interpretability"

#### Packages: TeachingSampling, h2o:3.26.0.2, iml, DALEX, dplyr

---

<br>
En esta práctica se mostrará como desarrollar e interpretar un algoritmo de machine learning, enfocado al riesgo de crédito.

Para esta práctica se van a utilizar los datos del siguiente reto [CREDIT_RISK](https://www.kaggle.com/c/home-credit-default-risk/data?select=HomeCredit_columns_description.csv), el dataset desarrollado será subido a este repositorio.

<br>

### Paso 1:

Lo primero que se hace despues de haber unido las tablas para desarrollar el modelo, es seleccionar las variables que se usaran dentro del algoritmo.

Para ello, se utilizo una tecnica llamada RFE ([recursive feature elimination](http://www.feat.engineering/recursive-feature-elimination.html)):


```R
load(file="D:/2020_1/MCH_Interpretability/DATA_PROJECT_02222020.RData") ## Data set 
```


```R
dim(DATA_PROJECT) ## Dimension de la tabla
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>685900</li><li>162</li></ol>



Dado que son 162 variables, la idea será reducirlas y solo trabajar con las características que aporten información significativa.

Para no tener problemas con el tiempo de procesamiento, se hará un muestreo estratificado con respecto a la variable "TARGET" del 10%. Con esta muestra se realizará el proceso.


```R
table(DATA_PROJECT$TARGET)

```


    
         0      1 
    651573  34327 



```R
defaultW <- getOption("warn") 

options(warn = -1) 
library(TeachingSampling)

Nh = c(651573,34327) ## El tamaño de cada estrato
nh <- c(6515,500) ## El tamaño de la distribucion de la muestra

res<-S.STSI(DATA_PROJECT$TARGET, Nh, nh)

DATA_SAM = DATA_PROJECT[res,]
```


```R
dim(DATA_SAM) ## Tamaño de la muestra
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>66874</li><li>162</li></ol>




```R
head(DATA_SAM)
```


<table>
<caption>A data.frame: 6 × 162</caption>
<thead>
	<tr><th></th><th scope=col>SK_ID_CURR</th><th scope=col>TARGET</th><th scope=col>NAME_CONTRACT_TYPE</th><th scope=col>CODE_GENDER</th><th scope=col>FLAG_OWN_CAR</th><th scope=col>FLAG_OWN_REALTY</th><th scope=col>CNT_CHILDREN</th><th scope=col>AMT_INCOME_TOTAL</th><th scope=col>AMT_CREDIT</th><th scope=col>AMT_ANNUITY.x</th><th scope=col>...</th><th scope=col>AMT_RECIVABLE</th><th scope=col>AMT_TOTAL_RECEIVABLE</th><th scope=col>CNT_DRAWINGS_ATM_CURRENT</th><th scope=col>CNT_DRAWINGS_CURRENT</th><th scope=col>CNT_DRAWINGS_OTHER_CURRENT</th><th scope=col>CNT_DRAWINGS_POS_CURRENT</th><th scope=col>CNT_INSTALMENT_MATURE_CUM</th><th scope=col>NAME_CONTRACT_STATUS</th><th scope=col>SK_DPD</th><th scope=col>SK_DPD_DEF</th></tr>
	<tr><th></th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>...</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>21</td><td>Active</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>13</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>22</td><td>Active</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>14</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>22</td><td>Active</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>15</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>22</td><td>Active</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>17</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>22</td><td>Active</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>40</th><td>100145</td><td>0</td><td>Cash loans</td><td>F</td><td>Y</td><td>Y</td><td>1</td><td>202500</td><td>260725.5</td><td>16789.5</td><td>...</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>22</td><td>Active</td><td>0</td><td>0</td></tr>
</tbody>
</table>




```R
DATA_SAM = DATA_SAM[,2:162]
DATA_SAM$TARGET = as.factor(as.character(DATA_SAM$TARGET)) 

library(doParallel)
library(caret)
cl <- makePSOCKcluster(8)
registerDoParallel(cl)
ctrl <- rfeControl(functions =rfFuncs,number = ifelse(method %in% c("cv",
  "repeatedcv"), 1, 5))

set.seed(1)
lmProfile2 <- rfe(DATA_SAM[,2:161], DATA_SAM$TARGET,
                  sizes = c(10,25,30,40), rfeControl = ctrl)  ## el metodo iterara con 10, 25, 30 ,35 y 40 variabes.



stopCluster(cl)
```

<br>
El resultado del código anterior podrá ser encontrado en el repositorio

<br>


```R
load(file="D:/2020_1/MCH_Interpretability/FTL_RES.RData") ## Objeto.
```

<br>
Del objeto solo se tomarán los dataframes ("results","variable"), ya que allí se encuentran los resultados que se necesitan para continuar.
<br>

<br>

#### Cantidad de variables optimas:


```R
defaultW <- getOption("warn") 
A_1 =  lmProfile2$results

A_1 = A_1[order(-A_1$Accuracy,-A_1$Kappa) ,]

head(A_1)
```


<table>
<caption>A data.frame: 4 × 5</caption>
<thead>
	<tr><th></th><th scope=col>Variables</th><th scope=col>Accuracy</th><th scope=col>Kappa</th><th scope=col>AccuracySD</th><th scope=col>KappaSD</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>2</th><td>25</td><td>0.9993940</td><td>0.9954358</td><td>0.0007256914</td><td>0.005376993</td></tr>
	<tr><th scope=row>3</th><td>30</td><td>0.9993630</td><td>0.9951932</td><td>0.0007151947</td><td>0.005298739</td></tr>
	<tr><th scope=row>4</th><td>40</td><td>0.9993630</td><td>0.9951932</td><td>0.0007151947</td><td>0.005298739</td></tr>
	<tr><th scope=row>1</th><td>10</td><td>0.9992556</td><td>0.9943629</td><td>0.0009080693</td><td>0.006791870</td></tr>
</tbody>
</table>



<br>
La idea es escoger el conjunto de variables que tenga el Accuracy y Kappa más altos. En este caso el mejor grupo de variables es el de "25".


<br>

#### Variables dentro del grupo optimo:


```R
A_2 = lmProfile2$variables
A_2 = A_2[A_2$Variables ==25,1:5]
head(A_2)
```


<table>
<caption>A data.frame: 6 × 5</caption>
<thead>
	<tr><th></th><th scope=col>0</th><th scope=col>1</th><th scope=col>Overall</th><th scope=col>var</th><th scope=col>Variables</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>71</th><td>23.30000</td><td>23.30000</td><td>23.30000</td><td>EXT_SOURCE_1          </td><td>25</td></tr>
	<tr><th scope=row>72</th><td>23.05532</td><td>23.05532</td><td>23.05532</td><td>EXT_SOURCE_2          </td><td>25</td></tr>
	<tr><th scope=row>73</th><td>22.96995</td><td>22.96995</td><td>22.96995</td><td>ORGANIZATION_TYPE     </td><td>25</td></tr>
	<tr><th scope=row>74</th><td>17.18718</td><td>17.18718</td><td>17.18718</td><td>DAYS_LAST_PHONE_CHANGE</td><td>25</td></tr>
	<tr><th scope=row>75</th><td>15.33767</td><td>15.33767</td><td>15.33767</td><td>OCCUPATION_TYPE       </td><td>25</td></tr>
	<tr><th scope=row>76</th><td>14.25884</td><td>14.25884</td><td>14.25884</td><td>EXT_SOURCE_3          </td><td>25</td></tr>
</tbody>
</table>



<br>
Como para desarrollar el rfe se usó resampling, se escogerán las variables que más fueron muestreadas dentro del algoritmo.
<br>


```R
VAES = data.frame(table(A_2$var)) 
VAES = VAES[order(-VAES$Freq) ,]
head(VAES,25)
```


<table>
<caption>A data.frame: 25 × 2</caption>
<thead>
	<tr><th></th><th scope=col>Var1</th><th scope=col>Freq</th></tr>
	<tr><th></th><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>AMT_INCOME_TOTAL          </td><td>25</td></tr>
	<tr><th scope=row>2</th><td>APARTMENTS_AVG            </td><td>25</td></tr>
	<tr><th scope=row>3</th><td>APARTMENTS_MEDI           </td><td>25</td></tr>
	<tr><th scope=row>10</th><td>DAYS_LAST_PHONE_CHANGE    </td><td>25</td></tr>
	<tr><th scope=row>11</th><td>DAYS_REGISTRATION         </td><td>25</td></tr>
	<tr><th scope=row>14</th><td>EXT_SOURCE_1              </td><td>25</td></tr>
	<tr><th scope=row>15</th><td>EXT_SOURCE_2              </td><td>25</td></tr>
	<tr><th scope=row>16</th><td>EXT_SOURCE_3              </td><td>25</td></tr>
	<tr><th scope=row>22</th><td>LIVINGAREA_AVG            </td><td>25</td></tr>
	<tr><th scope=row>23</th><td>LIVINGAREA_MEDI           </td><td>25</td></tr>
	<tr><th scope=row>24</th><td>LIVINGAREA_MODE           </td><td>25</td></tr>
	<tr><th scope=row>25</th><td>NAME_FAMILY_STATUS        </td><td>25</td></tr>
	<tr><th scope=row>26</th><td>NAME_TYPE_SUITE           </td><td>25</td></tr>
	<tr><th scope=row>27</th><td>OCCUPATION_TYPE           </td><td>25</td></tr>
	<tr><th scope=row>28</th><td>ORGANIZATION_TYPE         </td><td>25</td></tr>
	<tr><th scope=row>29</th><td>OWN_CAR_AGE               </td><td>25</td></tr>
	<tr><th scope=row>30</th><td>REG_CITY_NOT_LIVE_CITY    </td><td>25</td></tr>
	<tr><th scope=row>31</th><td>REGION_POPULATION_RELATIVE</td><td>25</td></tr>
	<tr><th scope=row>32</th><td>WEEKDAY_APPR_PROCESS_START</td><td>25</td></tr>
	<tr><th scope=row>33</th><td>YEARS_BUILD_MODE          </td><td>25</td></tr>
	<tr><th scope=row>4</th><td>BASEMENTAREA_AVG          </td><td>23</td></tr>
	<tr><th scope=row>13</th><td>ENTRANCES_MEDI            </td><td>19</td></tr>
	<tr><th scope=row>19</th><td>HOUR_APPR_PROCESS_START   </td><td>17</td></tr>
	<tr><th scope=row>12</th><td>ENTRANCES_AVG             </td><td>14</td></tr>
	<tr><th scope=row>5</th><td>BASEMENTAREA_MEDI         </td><td>13</td></tr>
</tbody>
</table>



<br>
Estas fueron las 25 variables más significativas dentro del ejercicio.

Ellas serán  usadas para poder desarrollar un algoritmo que prediga a la variable "TARGET".



<br>

## Desarrollo del Score financiero:

<br>
Como ya se escogieron las variables, se filtran dichas características dentro del primer data set para crear el modelo.


```R
variables= c("TARGET",as.vector(VAES[1:25,"Var1"]))

```


```R
DATA_PROJECT_MODEL = DATA_PROJECT[,variables]

DATA_PROJECT_MODEL$EXT_SOURCE_1 = round(DATA_PROJECT_MODEL$EXT_SOURCE_1*1000,0) 
DATA_PROJECT_MODEL$EXT_SOURCE_2= round(DATA_PROJECT_MODEL$EXT_SOURCE_2*1000,0) 
DATA_PROJECT_MODEL$EXT_SOURCE_3= round(DATA_PROJECT_MODEL$EXT_SOURCE_3*1000,0) 



```

La transformación anterior se hizo ya que estas variables son puntajes financieros que se encuentran en una escala decimal.


```R
dim(DATA_PROJECT_MODEL)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>685900</li><li>26</li></ol>



<br>

#### Desarrollo modelo final


```R
defaultW <- getOption("warn") 
library(h2o)

h2o.init(max_mem_size = "10G",nthreads=-1)
```

    Warning message:
    "package 'h2o' was built under R version 3.6.3"
    ----------------------------------------------------------------------
    
    Your next step is to start H2O:
        > h2o.init()
    
    For H2O package documentation, ask for help:
        > ??h2o
    
    After starting H2O, you can use the Web UI at http://localhost:54321
    For more information visit http://docs.h2o.ai
    
    ----------------------------------------------------------------------
    
    
    Attaching package: 'h2o'
    
    The following objects are masked from 'package:stats':
    
        cor, sd, var
    
    The following objects are masked from 'package:base':
    
        %*%, %in%, &&, ||, apply, as.factor, as.numeric, colnames,
        colnames<-, ifelse, is.character, is.factor, is.numeric, log,
        log10, log1p, log2, round, signif, trunc
    
    

     Connection successful!
    
    R is connected to the H2O cluster: 
        H2O cluster uptime:         1 days 15 hours 
        H2O cluster timezone:       America/Bogota 
        H2O data parsing timezone:  UTC 
        H2O cluster version:        3.30.0.1 
        H2O cluster version age:    5 months and 5 days !!! 
        H2O cluster name:           H2O_started_from_R_ASUS_rgl042 
        H2O cluster total nodes:    1 
        H2O cluster total memory:   9.83 GB 
        H2O cluster total cores:    8 
        H2O cluster allowed cores:  8 
        H2O cluster healthy:        TRUE 
        H2O Connection ip:          localhost 
        H2O Connection port:        54321 
        H2O Connection proxy:       NA 
        H2O Internal Security:      FALSE 
        H2O API Extensions:         Amazon S3, Algos, AutoML, Core V3, TargetEncoder, Core V4 
        R Version:                  R version 3.6.1 (2019-07-05) 
    

    Warning message in h2o.clusterInfo():
    "
    Your H2O cluster version is too old (5 months and 5 days)!
    Please download and install the latest version from http://h2o.ai/download/"

    
    


```R

DATA_1 = as.h2o(DATA_PROJECT_MODEL)
DATA_1$TARGET = as.factor(as.character(DATA_1$TARGET))

```

    
    H2O is not running yet, starting it now...
    
    Note:  In case of errors look at the following log files:
        C:\Users\ASUS\AppData\Local\Temp\RtmpcxVgYS\file2cc012e677c/h2o_ASUS_started_from_r.out
        C:\Users\ASUS\AppData\Local\Temp\RtmpcxVgYS\file2cc04ac33ad8/h2o_ASUS_started_from_r.err
    
    
    Starting H2O JVM and connecting: . Connection successful!
    
    R is connected to the H2O cluster: 
        H2O cluster uptime:         9 seconds 169 milliseconds 
        H2O cluster timezone:       America/Bogota 
        H2O data parsing timezone:  UTC 
        H2O cluster version:        3.30.0.1 
        H2O cluster version age:    5 months and 3 days !!! 
        H2O cluster name:           H2O_started_from_R_ASUS_rgl042 
        H2O cluster total nodes:    1 
        H2O cluster total memory:   10.67 GB 
        H2O cluster total cores:    8 
        H2O cluster allowed cores:  8 
        H2O cluster healthy:        TRUE 
        H2O Connection ip:          localhost 
        H2O Connection port:        54321 
        H2O Connection proxy:       NA 
        H2O Internal Security:      FALSE 
        H2O API Extensions:         Amazon S3, Algos, AutoML, Core V3, TargetEncoder, Core V4 
        R Version:                  R version 3.6.1 (2019-07-05) 
    

    Warning message in h2o.clusterInfo():
    "
    Your H2O cluster version is too old (5 months and 3 days)!
    Please download and install the latest version from http://h2o.ai/download/"

    
      |======================================================================| 100%
    

<br>

<br>
Se divide el dataset en tres subconjuntos:

-70% train

-20% valid

-10% test


```R
splits <- h2o.splitFrame(data=DATA_1, ratios=c(0.7,0.2))  
train <- splits[[1]]
valid <- splits[[2]]
test <- splits[[3]]

```


```R
target <- "TARGET"

predictors <- setdiff(names(train), target)


automl_h2o_models <- h2o.automl(
  x = predictors, 
  y = target,
  training_frame= train, nfolds = 0,max_models=20, sort_metric = c("logloss"),balance_classes = TRUE,exclude_algos=c("DeepLearning","StackedEnsemble"))

lb <- h2o.get_leaderboard(automl_h2o_models)

automl_leader <- automl_h2o_models@leader

```

      |                                                                      |   0%
      |======================================================================| 100%
    

<br>
Se usa la función "automl" que compara varios algoritmos dentro de la librería y selecciona el mejor con respecto a la métrica de "logloss". Para este ejercicio no se tuvo en cuenta los modelos "DeepLearning" y "StackedEnsemble", ya que computacionalmente son bastante costosos.



```R
automl_leader@model$validation_metrics
```


    H2OBinomialMetrics: gbm
    ** Reported on validation data. **
    
    MSE:  6.784022e-32
    RMSE:  2.604616e-16
    LogLoss:  1.452516e-17
    Mean Per-Class Error:  0
    AUC:  1
    AUCPR:  1
    Gini:  1
    R^2:  1
    
    Confusion Matrix (vertical: actual; across: predicted) for F1-optimal threshold:
               0    1    Error      Rate
    0      45897    0 0.000000  =0/45897
    1          0 2379 0.000000   =0/2379
    Totals 45897 2379 0.000000  =0/48276
    
    Maximum Metrics: Maximum metrics at their respective thresholds
                            metric threshold        value idx
    1                       max f1  1.000000     1.000000   0
    2                       max f2  1.000000     1.000000   0
    3                 max f0point5  1.000000     1.000000   0
    4                 max accuracy  1.000000     1.000000   0
    5                max precision  1.000000     1.000000   0
    6                   max recall  1.000000     1.000000   0
    7              max specificity  1.000000     1.000000   0
    8             max absolute_mcc  1.000000     1.000000   0
    9   max min_per_class_accuracy  1.000000     1.000000   0
    10 max mean_per_class_accuracy  1.000000     1.000000   0
    11                     max tns  1.000000 45897.000000   0
    12                     max fns  1.000000     0.000000   0
    13                     max fps  0.000000 45897.000000 252
    14                     max tps  1.000000  2379.000000   0
    15                     max tnr  1.000000     1.000000   0
    16                     max fnr  1.000000     0.000000   0
    17                     max fpr  0.000000     1.000000 252
    18                     max tpr  1.000000     1.000000   0
    
    Gains/Lift Table: Extract with `h2o.gainsLift(<model>, <data>)` or `h2o.gainsLift(<model>, valid=<T/F>, xval=<T/F>)`


<br>
El modelo escogido por la función fue un Gradient Boosting Machine. De igual forma, el archivo del algoritmo será colgado en el repositorio.

<br>

### Interpretacion Global Modelo:

<br>
La primera tarea que se realiza dentro de este segmento es ver el peso de las variables que componen el algoritmo:

<br>


```R
h2o.varimp_plot(automl_leader)
```


<img src="/images/Practica_SCORE_FINANCIERO_47_0.png" alt="MarineGEO circle logo" style="height: 100px; width:100px;"/>




<br>

Dentro de las 5 variables que más pesan se encuentran los puntajes financieros provenientes de otras centrales de riesgo (EXT_SOURCE_1, EXT_SOURCE_2 y EXT_SOURCE_3), seguido por el income total. 

El análisis de la variable "DAYS_LAST_PHONE_CHANGE" se omite ya que dentro de la lógica de negocio no tiene mucho sentido analizarla. Inclusive para una versión 2.0 del modelo, este tipo de características podrían ser removidas.


<br>
Ahora bien, como no se sabe el efecto que tiene cada variable en cuando a la disminución o aumento del riesgo de no pago. Se aplica la técnica de [ALE_PLOTS](https://christophm.github.io/interpretable-ml-book/ale.html) para poder hacer esta descripción.

<br>

## ALE PLOTS:

<br>


```R
defaultW <- getOption("warn") 

library(dplyr)
library(iml)
library(DALEX)
library(TeachingSampling)

# 2. Create a vector with the actual responses
response <- as.numeric(as.vector(DATA_PROJECT_MODEL$TARGET))

# 3. Create custom predict function that returns the predicted values as a
#    vector (probability of purchasing in our example)
pred <- function(model, newdata)  {
  results <- as.data.frame(h2o.predict(model, as.h2o(newdata)))
  return(results[[3L]])
}
```


```R
table(DATA_PROJECT_MODEL$TARGET)

## Sample:
Nh = c(651573,34327)
nh <- c(65158,3433)

res<-S.STSI(DATA_PROJECT_MODEL$TARGET, Nh, nh)

DATA_SAM = DATA_PROJECT_MODEL[res,]

response_1 <- as.numeric(as.vector(DATA_SAM$TARGET))
```


    
         0      1 
    651573  34327 



```R
predictor.gbm <- Predictor$new(
  model = automl_leader, 
  data = DATA_SAM, 
  y = response_1, 
  predict.fun = pred,
  class = "classification"
)

```


#### EXT_SOURCE_1



```R
## Ale plots:


ale = FeatureEffect$new(predictor.gbm, feature = "EXT_SOURCE_1")

ale$plot()
```

      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
    


![im1](/images/Practica_SCORE_FINANCIERO_56_1.png)


Lo que indica la gráfica es que el riesgo de no pago se reduce significativamente cuando una persona tiene un Score de crédito mayor a los 200 puntos en esta central de riesgo.

<br>

#### EXT_SOURCE_2


```R

ale = FeatureEffect$new(predictor.gbm, feature = "EXT_SOURCE_2")

ale$plot()
```

      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
    


![](/images/Practica_SCORE_FINANCIERO_59_1.png)


Lo que indica la gráfica es que el riesgo de no pago se reduce significativamente cuando una persona tiene un Score de crédito mayor a los 400 puntos en esta central de Riesgo.

<br>

#### EXT_SOURCE_3


```R
ale = FeatureEffect$new(predictor.gbm, feature = "EXT_SOURCE_3")

ale$plot()
```

      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
    


![](/images/Practica_SCORE_FINANCIERO_62_1.png)



Lo que indica la gráfica es que el riesgo de no pago se reduce significativamente cuando una persona tiene un Score de crédito mayor a los 150 puntos en esta central de Riesgo.


<br>

#### AMT_INCOME_TOTAL


```R
ale = FeatureEffect$new(predictor.gbm, feature = "AMT_INCOME_TOTAL")

ale$plot()
```

      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
      |======================================================================| 100%
    


![](/images/Practica_SCORE_FINANCIERO_65_1.png)


Lo que indica la gráfica es que el riesgo de no pago se reduce significativamente cuando una persona tiene un Income total mayor a los USD 200000.

En conclusión, se podría definir al candidato optimo con las siguientes características:


- Más de 200 puntos en la central 1
- Más de 400 puntos en la central 2
- Más de 150 puntos en la central 3
- Sus ingresos en total son más de USD 200000.




<br>

##### Nota Aclaratoria:

No seria correcto afirmar que este modelo esta listo para ser puesto en produccion ya que el sobreajuste que tiene es alto.

Adicionalmente, los ejemplos de Interpretacion global se hicieron con una muestra de los datos originales por cuestiones de costo computacional.
