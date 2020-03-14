# Competencia para el curso de Reconocimiento de Patrones
## Clasificación de Péptidos Antimicrobianos

## Universidad Nacional de Colombia - Medellín
## Posgrado en Analítica
## 2020-03-14

## Integrantes

* Juan Manuel Marín Quintero
* Juan Camilo Agudelo Marín
* Lina María Grajales Vanegas
* Jhon Anderson Londoño Herrera
* Juan Pablo Trujillo Alviz

## Introducción

Con el fin de aplicar las técnicas de reconocimiento de patrones aprendidas en el curso impartido en la Universidad Nacional de Colombia, se propone la realización de una clasificación de péptidos antimicrobianos. 

Los péptidos antimicrobianos son parte esencial de todos los organismos vivos y configuran la primera línea de denfensa contra bacterias, microbios y parásitos, es decir, causan la muerte de bacterias y microbios que entran al organismo. 

La identificación de los péptidos antimicrobianos se ha realizado de manera manual, sin embargo, en los últimos años se ha venido realizando importantes avances en lo que respecta al uso de los algoritmos de inteligencia artificial y clasificación de patrones. Esto puede traer reducción en tiempo y costo empleado en el proceso de búsqueda manual. 

De esta manera, utilizando una muestra de péptidos antimicrobianos y no antimicrobianos con información de un poco más de 1700 descrpitores como carga eléctrica, hidrofobicidad, punto isoeléctrico, entre otras, se propone aplicar algunas de las diferentes técnicas de clasificación vistas en clase, con el fin de clasificar con el mayor nivel de certeza posible si un péptido es o no antimicrobiano.

## Solución

Se propone realizar la clasificación en dos partes: 

* En la primera se hará una exploración de las técnicas *sin selección* de variables, es decir, no se realizarán análisis para crear nuevas variables, así como tampoco técnicas como análisis de componentes principales (PCA, por sus siglas en inglés). En esta parte se entrenarán 3 técnicas de clasificación diferentes utilizando validación cruzada, además, se ajustarán sus hiperparámetros.
* En la segunda se hará una exploración de variables en las que se seleccionarán, transformarán o extraerán las mejores características del conjunto de datos, luego se reentrenarán las mismas técnicas con el nuevo conjunto de datos.

### Parte 1: clasificación sin selección

El cuaderno puede ser explorado en [este enlance](https://github.com/Juapatral/reconocimiento_patrones_2020/blob/master/competencia/competencia_crp.ipynb).

#### Lectura de datos

Primero se realiza la lectura de los conjuntos de datos y un análisis previo. Se observa que los conjuntos con los que se va a trabajar tienen aproximadamente 22.000 registros y cuentan con 1.760 columnas.

Además, se elimina una columna que contiene el índice de los datos ya que no es necesaria y se visualiza si existen desbalanceos o no en la variable objetivo. Se observa que la variable objetivo tiene una distribución aproximada de 60-40, es decir, cerca del 60% de las observaciones corresponden a péptidos no antimicrobianos. Con este nivel de distribución y en vista de que el objetivo de este trabajo es aplicar las técnicas de clasificación, no se realizará ajustes de datos desbalanceados.

#### Estandarización

Siguiendo este orden de ideas, el conjunto de datos se divide en entrenamiento y pruebas, este último con el 20 % de los datos. Luego, se realiza una estandarización de variables utilizando una transformación normal. Este ajuste se calcula sobre el conjunto de datos de entrenamiento y se aplica sobre ambos conjuntos, el de entrenamiento y pruebas.

#### Modelación

Se propone realizar el ajuste de los siguientes tres modelos:

* Regresión logística.
* Árbol de Clasificación.
* XGBoost.

##### Regresión logística

Para la [regresión logistica](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) se deciden ajustar dos hiperparámetros, la función de regularización, *L1* o *L2*, y el coeficiente inverso de regularización, *C*. El primer hiperparámetro ayuda a que el modelo no se sobreajuste con coeficientes distantes, mientras que el segundo parámetro indica qué tanto peso tiene la regularización.

Después de realizar validación cruzada para obtener los mejores hiperparámetros, se encuentra que los mejores parámetros para estos datos son *L2* y *C=0.01* 

Se entrena el modelo utilizando validación cruzada con 5 particiones, en promedio se demoró 5.9 segundos el entrenamiento de este modelo. La precisión, el recall y el f1 sobre el conjunto de pruebas son 0.86, 0.86 y 0.86, respectivamente.

##### Árbol de Clasificación

Para entrenar el [árbol de clasificación](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html?highlight=decisiontreecl#sklearn.tree.DecisionTreeClassifier) se propone ajustar los siguientes parámetros:

* Máxima profundidad: máxima cantidad de ramas o divisiones a generar.
* Máximas características: cantidad máxima de características a utilizar para clasificar.
* Mínimo de muestras por hoja: cantidad mínima de observaciones que debe clasificar un nodo final.
* Mínimo de muestras por división: cantidad mínima de observaciones para que un nodo parcial se pueda dividir.
* Criterio: función a medir, *gini*, impureza, o *entropia*, ganancia de información.

Después de realizar validación cruzada para obtener los mejores hiperparámetros, se encuentra que los mejores parámetros para estos datos son 12, 13, 9, 2 y entropía, respectivamente.

Se entrena el modelo utilizando validación cruzada con 5 particiones, en promedio se demoró 0.7 segundos el entrenamiento de este modelo. La precisión, el recall y el f1 sobre el conjunto de pruebas son 0.77, 0.77 y 0.77, respectivamente.

##### XGBoost

Un XGBoost es un [*Extreme Gradient Boosting*](https://xgboost.readthedocs.io/en/latest/tutorials/model.html), es un método de ensamble de diferentes árboles de clasificación. Se propone ajustar los siguientes parámetros:

* Peso mínimo de nodo: si la suma de los pesos de las observaciones de un nodo no alcanzan este mínimo, no se parte el nodo.
* Gamma: reducción de pérdida mínima requerida para partir un nodo.
* Submuestreo: porcentaje de datos que se usarán para el submuestreo al momento de iniciar el entrenamiento y así prevenir sobreajustes.
* Submuestra de columnas por árbol: porcentaje de la columna usada para entrenar cada árbol del ensamble.
* Máxima profundidad: máxima cantidad de ramas o divisiones a generar.

Después de realizar validación cruzada para obtener los mejores hiperparámetros, se encuentra que los mejores parámetros para estos datos son 1, 2, 0.6, 0.6 y 5, respectivamente.

Se entrena el modelo utilizando validación cruzada con 5 particiones, en promedio se demoró 13.8 minutos el entrenamiento de este modelo. La precisión, el recall y el f1 sobre el conjunto de pruebas son 0.90, 0.89 y 0.89, respectivamente.

##### Conclusiones modelado

Modelo|Tiempo (segundos)|Precisión|Recall|F1
---|---|---|---|---
Regresión Logística|5.9|0.86|0.86|0.86
Árbol de Clasificación|0.7|0.77|0.77|0.77
XGBoost|830|0.90|0.89|0.89

De acuerdo con esta tabla, la mejor predicción la genera el modelo del XGBoost, sin embargo, se demora casi 138 veces más que la regresión logística y casi 1.186 veces más que un único árbol de clasificación. Por otro lado, a pesar de que el árbol de clasificación es el más rápido, también tiene las métricas más bajas de predicción. De esta forma, la regresión logística genera muy buenos resultados (similares al XGBoost) en un tiempo considerable (menos de 6 segundos), por lo que puede ser el mejor modelo de los 3.

### Parte 2: clasificación con selección

## Método de Selección 

Se utiliza la técnica de bosques aleatorios, con todas las variables del dataset, se entrena con un tamaño de entrenamiento del 80%. Se toma como decisión considerar importante las variables que tuvieran que en el feature_importance fueran mayores a 0. Las variables importantes que dieron como resultado fueron charge, charge_density, isoelectric_point, K, M, KK, SolventAccessibiliyC1, SolventAccessibiliyC3, ChargeC1, ChargeC3, ChargeT23, SolventAccessibilityD1001, SolventAccessibilityD3001, SolventAccessibilityD3100, ChargeD3001, ChargeD3025, ChargeD3075, ChargeD3100, GearyAuto_AvFlexibility4, GearyAuto_FreeEnergy5, QSOgrant13, QSOSW13.

## Método de Extracción 

Como método de extracción utilizaremos la técnica del PCA (Análisis de Componentes Principales) con el objetivo de poder determinar si el uso de las componentes en el modelo mejorarán su precisión.

## Regresión Logística con Método de Selección de Variables

El mejor modelo logístico que se logro fue con los parámetros *L2* y *C=0.01*. Por tanto utilizaremos este mismo modelo con las variables que nos dio el método de selección.

## Regresión Logística con Método de Extracción de Variables
El mejor modelo logístico que se logro fue con los parámetros *L2* y *C=0.01*. Por tanto utilizaremos este mismo modelo con las 448 componentes y analizar su nueva predicción.

## Árbol de Clasificación con Método de Selección de Variables

El mejor modelo de arbol de clasificación que se realizo por medio de parametrización dio como resultado maxima profundidad de 12, Máximas Características de 13, Mínimo de muestra de hoja de 9, Mínimo de muestra de subdivisión de 2 y eligiendo como criterio la entropía. Por tanto con esta estructura del arbol lo apicaremos sobre las variables que recomienda el método de selección. 

## Árbol de Clasificación con Método de Extracción de Variables

El mejor modelo de arbol de clasificación que se realizo por medio de parametrización dio como resultado maxima profundidad de 12, Máximas Características de 13, Mínimo de muestra de hoja de 9, Mínimo de muestra de subdivisión de 2 y eligiendo como criterio la entropía. Utilizaremos este mismo modelo con las 448 componentes y analizar la nueva predicción. 

## Xgboost con Método de Selección de Variables

El mejor modelo de Xgboost que se realizo por medio de parametrización dio como resultado peso mínimo de nodo de 1, Gamma de 2, submuestreo 0.6, submuestra de columnas de 0.6 y Máxima de Profundidad de 5.  Por tanto se utilizará esta estructura de Xgboost sobre las variables que recomienda el método de selección.

## Xgboost con Método de Extracción de Variables

El mejor modelo de Xgboost que se realizo por medio de parametrización dio como resultado peso mínimo de nodo de 1, Gamma de 2, submuestreo 0.6, submuestra de columnas de 0.6 y Máxima de Profundidad de 5. Se utilizará este mismo modelo con las 448 componentes y analizar la nueva predicción

## Comparativos de los Modelos 

Modelo|Tiempo (segundos)|Precisión|Recall|F1
---|---|---|---|---
Regresión Logística Todas las Variables|5.9|0.86|0.86|0.86
Regresión Logística Método de Selección|0.07|0.83|0.83|0.83
Regresión Logística Método de Extracción|15.37|0.86|0.86|0.86
Árbol de Clasificación Todas las Variables|0.7|0.77|0.77|0.77
Árbol de Clasificación Método de Selección|0.17|0.82|0.82|0.82
Árbol de Clasificación Método de Extracción|15.37|0.67|0.68|0.67
XGBoost Todas las Variables|830|0.90|0.89|0.89
XGBoost Método de Selección|15.37|0.86|0.86|0.86
XGBoost Método de Extracción|15.37|0.85|0.85|0.84


En los modelos de Regresión Logística y XGBoost dan un mejor rendimiendo en precisión con todas las variables, sin embargo el XGboost se demoro mucho en el ajuste con 830 segundos a comparacion de 5.9 segundos de la regresión logística. En el método de los árboles de decisión ocurrió que al realizarlo con la selección de variables obtuvo un mejor desempeño que con todas las variables y extracción. Si tuvieramos que elegir el mejor modelo con respecto a la precisión, recall y f1 se tomaría la decisión de elegir el XGBoost con todas las variables alcanzón 0.90, 0.89 y 0.89 respectivamente. Sin embargo si tendríamos restricciones computacionales preferiríamos utilizar el XGBoost pero utilizando solo las variables del método de selección a pesar que en precisión, recall y f1 bajaríamos a 0.86, 0.86 y 0.86 respectivamente, disminuriamos mucho el tiempo de entrenamiento bajando de 830 segundos a 15.37 segundos. Como reto fue muy interesante, la parte más retadora fue encontrar los parametros óptimos en cada modelo, sin embargo la tarea se vuelve un poco accesible utilizando hypertunnings aunque se demora mucho el proceso vale la pena.


