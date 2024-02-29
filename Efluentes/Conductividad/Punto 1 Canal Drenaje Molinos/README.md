### Modelo de Identificación de Alta Conductividad en Canal de Drenaje de Molinos.

El presente proyecto pretende generar un modelo que ayude a predecir la presencia de material azucarado en el canal de drenaje del Área de Molinos y así reducir las pérdidas de producto por motivo de derrames. Con apoyo de los ingenieros de producción, se realizó una selección de las variables de proceso que pueden incidir en la conductividad del canal de drenaje. Las variables tomadas en cuenta son las siguientes: 

  - Flujo de asepsia para patio y Molinos Tándem A y B.
  - Niveles de los chutes de Molinos Tándem A y B.
  - Niveles de todos los evaporadores del efecto A al E.
  - Conductividad del canal de drenaje de Molinos. 

El conjunto de datos cuenta con 53 variables y con 1920 observaciones tomadas como promedio cada hora durante los primeros 80 días de la presente zafra.  Se definió como variable objetivo la medición de conductividad del canal de drenaje; a continuación se muestra el comportamiento de la conductividad en el tiempo. 


![image](https://github.com/dsPSA2023/PSA/assets/161398218/40e437a8-b1c8-4ebe-ba60-f1a5d0f95336)

El valor de la conductividad comenzó a disminuir alrededor de la observación 550 (día de zafra 23) y vuelve a presentar valores altos cerca de la observación 1400 (día de zafra 59), alcanzando valores por arriba de 4000 uS.  

La conductividad presenta una distribución sesgada a la derecha, alcanzando valores arriba de 8000 uS.     

![image](https://github.com/dsPSA2023/PSA/assets/161398218/0b7be937-21fc-4389-ac02-2df28f52bfc0)

Estadísticas descriptivas conductividad 

  - Mínimo:         5.15 uS
  - Máximo:      8273.00 uS 
  - Media:        460.22 uS
  - Mediana:      269.84 uS 

La idea principal del proyecto es determinar la presencia de alta conductividad en el canal de drenaje, esto se realizó mediante la aplicación del algoritmo de regresión logística. Para lo cual se realizó una *transformación* de la variable continua de conductividad a una variable binaria de *presencia* o *no presencia* de alta conductividad. 


#### Regresión Logística

La regresión logística es un tipo de modelo de regresión utilizado para predecir la probabilidad de que una observación pertenezca a una de dos o más categorías. A pesar de su nombre, la regresión logística se utiliza comúnmente para problemas de clasificación, no de regresión. La regresión logística se basa en el concepto de la función logística, que es una función *sigmoide* que transforma los valores continuos en un rango de 0 a 1. Esta función se utiliza para modelar la probabilidad de que una observación pertenezca a una clase específica, en nuestro caso una *alta conductividad es una clase POSITIVA* y una *baja conductividad es una clase NEGATIVA*.


![Regresión Logística](https://github.com/dsPSA2023/PSA/assets/161398218/aafdb0f4-6cd2-418b-99af-2558f3ed1ff6)

Se realizaron 2 análisis al conjunto de datos, el primero sin tomar en cuenta el impacto del día de Zafra y el segundo tomándolo en cuenta.  

El procedimiento realizado en ambos casos se detalla a continuación:

  - Agrupamiento de Datos: Se ejecutó un algoritmo de *clustering no-supervisado*, con el propósito de encontrar agrupaciones en los datos que permitan identificar *"regiones de operación"*, y luego verificar si estas regiones tienen en común algún rango de valores de conductividad.
    
  - Búsqueda del umbral óptimo: Se aplicó una *binarización* a la variable de conductividad. La binarización se implementó de forma que: Una *alta conductividad es una clase POSITIVA* y una *baja conductividad es una clase NEGATIVA*. Adicionalmente, debido a la distribución *sesgada* de la variable de conductividad, es posible que una mala selección del umbral produzca un desbalance en el conjunto de datos y, por lo tanto, las métricas de detección del algoritmo de regresión logística no sean óptimas.  Para explorar el umbral de conductividad, se implementa un algoritmo que explora *distintos umbrales de conductividad* y grafica las métricas de detección para cada selección:
  
        - *Precision:* Habilidad del Clasificador de minimizar Falsos Positivos.
        - *Accuracy:* Habilidad del Clasificador de Detectar Verdaderos Positivos y Verdaderos Negativos.
        - *Recall:* Habilidad del Clasificador de Detectar Verdaderos Positivos.
        - *F1:* Media Ponderada de la Precision y el Recall.
        - *Overall:* Media Aritmética de todas las métricas anteriores.
        - *Clase Positiva (%):* Porcentaje de clase positiva presente en el conjunto de datos.
  
- Generacion de modelo de regresion logistica con el umbral optimo: El criterio para seleccionar el umbral se propone sea el del "cima". En este método se selecciona un umbral a partir del cual las métricas caen por debajo de un nivel "aceptable", de esta forma, se obtiene una *propuesta de umbral óptima*. La consideración principal es el costo de un error en la clasificación, en nuestro caso tiene más impacto un *Falso Negativo* que un *Falso Positivo*, clasificar que no tendremos alta conductividad cuando la hay, representa la posibilidad de no prevenir un derrame de material azucarado, por lo tanto, una perdida de producto. La métrica más relevante para este caso de estudio es Recall, también conocido como sensibilidad o tasa de verdaderos positivos, el recall indica la capacidad del clasificador para detectar verdaderos positivos. Es fundamental en situaciones donde los falsos negativos tienen un alto costo o impacto.

![image](https://github.com/dsPSA2023/PSA/assets/161398218/af30ce16-ced5-4bbc-bd5d-f8b1292743d6)

  
- Evaluacion del modelo metrica finales

![image](https://github.com/dsPSA2023/PSA/assets/161398218/0b48e666-c335-40e1-8bdb-2464026ad73b)

- 
- Variables de importancias por control y permutacion
- Variables de importancia por... 
