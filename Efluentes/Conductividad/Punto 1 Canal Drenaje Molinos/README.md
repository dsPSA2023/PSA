### Modelo de Identificación de Alta Conductividad en Canal de Drenaje de Molinos

El presente proyecto pretende generar un modelo que ayude a predecir la presencia de material azucarado en el canal de drenaje y así reducir las pérdidas de producto por motivo de derrames. Con apoyo de los ingenieros de producción se realizó una selección de las variables de proceso que pueden incidir en la conductividad del canal de drenaje, las variables tomadas en cuenta son las siguientes: 

- Flujo de Asepcia para Patio y Molinos Tandem A y B
- Niveles de los chutes de Molinos Tandem A y B
- Niveles de todos los evaporadores del efecto A al E
- Conductividad del Canal de Drenaje de Molinos 

El conjunto de datos cuenta con 53 variables y alrededor de 2000 observaciones tomadas como promedio cada hora durante los primeros 80 días de la presente zafra.  Se definio como variable objetivo la medición de conductividad del Canal de Drenaje. 


![image](https://github.com/dsPSA2023/PSA/assets/161398218/40e437a8-b1c8-4ebe-ba60-f1a5d0f95336)


#### Regresión Logistica

La idea principal del proyecto es determinar la presencia alta conductidad en el canal de drenaje, esto se realizó mediante la aplicacion del algoritmo de regresión logisitica, el cual requiere que la variable objetivo se categorica. Para esto se aplica una *binarización* a la variable de conductividad. La binarización consiste en *transformar* la variable continua de conductividad a una variable binaria de *presencia* o *no presencia* de conductividad. Esto porque no toda conductividad se debe necesariamente a trazas de azúcar, y en la práctica es más útil definir un límite superior para esta variable. 

La binarización se implementa de forma que: Una *alta conductividad es una clase POSITIVA* y una *baja conductividad es una clase NEGATIVA*.

![Regresion Logistica](https://github.com/dsPSA2023/PSA/assets/161398218/aafdb0f4-6cd2-418b-99af-2558f3ed1ff6)

#### Determinación de Umbral Óptimo

Adicionalmente, debido a la distribución *sesgada* de la variable de conductividad, es posible que una mala selección del umbral produzca un imbalance en el conjunto de entrenamiento y por lo tanto, métricas de detección del algoritmo que no son óptimas.

#### K-Means Clustering
Para poder distinguir entre clases, se ejecuta un algoritmo de clustering no-supervisado, con el propósito de encontrar *"regiones de operación"* en los datos, y luego verificar si estas regiones tienen en común algún rango de valores de conductividad.


