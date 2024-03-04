## Identificación de Alta Conductividad o Trazas en Efluentes del Área Industrial.

El día a día de la operación de una planta industrial se centra en mantener el control de las variables del proceso para maximizar la producción y obtener la calidad deseada del producto final, durante la operación es posible tener la presencia de derrames y fugas de productos en proceso, derivado de altos niveles en tanques o fallas en equipos. Estos dos tipos de eventos impactan de manera negativa en la producción, ya que generan pérdidas de producto a través de los efluentes de las distintas áreas de la planta. 

Una manera habitual de determinar la presencia de producto en los efluentes, es la medición de conductividad y trazas. Normalmente, se suelen definir puntos de referencia para dar seguimiento a dichas mediciones en las distintas áreas del proceso. Estos puntos de monitoreo sirven para determinar si existe o existió algún tipo de derrame de producto, a través del aumento de la conductividad o las trazas. En PSA se tienen definidos los siguientes puntos de monitoreo:  

 ![image](https://github.com/dsPSA2023/PSA/assets/161398218/b1dc3507-540b-4d28-873d-2e1af99efdaa)  

En algunos casos se realizan las 2 mediciones en el mismo punto de monitoreo, Conductividad (Medición del Sistema de Control) y Trazas (Medición de Laboratorio),  la diferencia principal consiste en la cantidad de datos disponibles en cada medición.  La conductividad cuenta con un registro histórico en el sistema de control que se puede consultar a demanda con la frecuencia que se desee, mientras que la medición de trazas solo tiene registros cada 4 horas, según programa de laboratorio.   

El presente proyecto pretende generar un modelo, que ayude a predecir la presencia material azucarado, por medio de la medición de conductividad o de trazas en el punto de referencia definido, con la finalidad de reducir las pérdidas de producto por motivo de derrames. Con apoyo de los ingenieros de producción, se realizó una selección de las variables de proceso que pueden impactar en dichas mediciones, la mayoría son sensores de nivel de tanques abiertos y medidores de flujo en estaciones de bombeo de producto.

El conjunto de datos, en cada caso, cuenta con diferente cantidad de variables (columnas) según la experiencia de los ingenieros de proceso y de observaciones (filas) según la medición que se está analizando:  

 - Conductividad:  Variables del sistema de control definidas por el ingeniero de proceso. Observaciones como promedio cada hora desde inicio de zafra.
 - Trazas: Variables del sistema de control definidas por el ingeniero de proceso, incluyendo la medición de conductividad en caso, aplique. Observaciones como promedio 5 minutos antes y después de la estampa de tiempo que tenga cada medición de trazas desde inicio de zafra. 

Inicialmente, se realiza una exploración de las diferentes variables del conjunto de datos, se define la variable objetivo en cada uno de los casos (Conductividad o Trazas). Se muestra el comportamiento de la variable objetivo en el tiempo y su distribución en un histograma de frecuencias.  

La idea principal del proyecto es determinar la presencia de alta conductividad o trazas en el punto de monitoreo en estudio, esto se llevo a cabo mediante la generación de un modelo de regresión logística. Para lo cual se realizó una *transformación* de la variable objetivo, inicialmente continua a una variable binaria de *presencia* o *no presencia* de alta conductividad o trazas. 

#### Regresión Logística

La regresión logística es un tipo de modelo del algoritmo de regresión, utilizado para predecir la probabilidad de que una observación pertenezca a una de dos o más categorías. A pesar de su nombre, la regresión logística se utiliza comúnmente para problemas de clasificación, no de regresión. La regresión logística se basa en el concepto de la función logística, que es una función *sigmoide* que transforma los valores continuos en un rango de 0 a 1. Esta función se utiliza para modelar la probabilidad de que una observación pertenezca a una clase específica, en nuestro caso una *alta conductividad o trazas es una clase POSITIVA* y una *baja conductividad o trazas es una clase NEGATIVA*.


  ![Regresión Logística](https://github.com/dsPSA2023/PSA/assets/161398218/aafdb0f4-6cd2-418b-99af-2558f3ed1ff6)

Al conjunto de datos de cada punto de monitoreo de conductividad o trazas, se le realizaron 2 análisis, el primero sin tomar en cuenta el impacto del día de Zafra y el segundo tomándolo Aen cuenta.  

El procedimiento realizado en ambos casos se detalla a continuación:

  - Agrupamiento de Datos: Se ejecutó un algoritmo de *clustering no-supervisado*, con el propósito de encontrar agrupaciones en los datos que permitan identificar *"regiones de operación"*, y luego verificar si estas regiones tienen en común algún rango de valores de conductividad.
    
  - Búsqueda del umbral óptimo: Se aplicó una *binarización* a la variable de conductividad. La binarización se implementó de forma que: Una *alta conductividad es una clase POSITIVA* y una *baja conductividad es una clase NEGATIVA*. Adicionalmente, debido a la distribución *sesgada* de la variable de conductividad, es posible que una mala selección del umbral produzca un desbalance en el conjunto de datos y, por lo tanto, las métricas de detección del algoritmo de regresión logística no sean óptimas.  Para explorar el umbral de conductividad, se implementa un algoritmo que explora *distintos umbrales de conductividad* y grafica las métricas de detección para cada selección:
  
     - *Precision:* Habilidad del Clasificador de minimizar Falsos Positivos.
     - *Accuracy:* Habilidad del Clasificador de Detectar Verdaderos Positivos y Verdaderos Negativos.
     - *Recall:* Habilidad del Clasificador de Detectar Verdaderos Positivos.
     - *F1:* Media Ponderada de la Precision y el Recall.
     - *Overall:* Media Aritmética de todas las métricas anteriores.
     - *Clase Positiva (%):* Porcentaje de clase positiva presente en el conjunto de datos.
  
- Generacion de modelo de regresion logistica con el umbral optimo: El criterio para seleccionar el umbral se propone sea el del "cima". En este método se selecciona un umbral a partir del cual las métricas caen por debajo de un nivel "aceptable", de esta forma, se obtiene una *propuesta de umbral óptima*. La consideración principal es el costo de un error en la clasificación, en nuestro caso tiene más impacto un *Falso Negativo* que un *Falso Positivo*, clasificar que no tendremos alta conductividad cuando la hay, representa la posibilidad de no prevenir un derrame de material azucarado, por lo tanto, una perdida de producto. La métrica más relevante para este caso de estudio es Recall, también conocido como sensibilidad o tasa de verdaderos positivos, el recall indica la capacidad del clasificador para detectar verdaderos positivos. Es fundamental en situaciones donde los falsos negativos tienen un alto costo o impacto, se calcula como el número de predicciones positivas correctas dividido por el número total de positivos.

  
- Evaluación de Metricas Finales:  Con el valor de umbral optimo definido, se entrena un modelo de clasificacion y se evaluan las metricas finales para determinar su capacidad de prediccion. Para este fin se utilizan la matriz de confusion y la Curva PR.

    - Matriz de Confusión: La matriz de confusión es una herramienta que se utiliza para evaluar el rendimiento de un modelo de clasificación. Es una tabla que muestra la cantidad de aciertos y errores de un modelo en la clasificación de instancias en cada una de las clases.
  
    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/31991dda-7d3b-406e-8db1-03b66178cc1f)
  
    - Curva PR: La gráfica de precisión y recall es una herramienta de evaluación comúnmente utilizada en problemas de clasificación binaria, especialmente cuando hay un desbalance significativo entre las clases, que es una representación gráfica de la relación entre la tasa de verdaderos positivos (Recall) y la tasa de falsos positivos (Precision) para diferentes umbrales de clasificación. En general, la curva de precisión y recall nos permite visualizar cómo cambia la precisión a medida que se ajusta el recall y viceversa. Esto puede ser útil para comprender el compromiso entre precisión y recall en diferentes puntos de corte y para seleccionar el umbral de decisión óptimo para nuestro problema específico.
   
    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/49c1f416-8d71-4761-9ac1-87dca0f77f1a)

     
- Determinación de Factores de Importancia: Para la determinación de los factores de importancia se presentan dos perspectivas: Importancia por Magnitud del Factor e Importancia por Control del Factor.

  - Importancia por Magnitud del Factor: Esta importancia es el peso relativo que tiene este factor respecto a los demás factores, para contribuir positivamente o negativamente a la conductividad. Si la contribución es positiva, a mayor magnitud del factor, mayor probabilidad de detectar la clase positiva (mayor probabilidad de alta conductividad). Si la contribución es negativa, a mayor magnitud del factor, mayor probabilidad de detectar la clase negativa (menor probabilidad de alta conductividad).
  
  - Importancia por Control del Factor: Esta importancia se calcula a partir del efecto que tiene aleatorizar la variable (manteniendo todas las demás constantes) sobre la conductividad. Esto es especialmente importante para identificar *el efecto que tiene la precisión del control de la variable sobre la conductividad final*.

- Probabilidad de Clase Positiva (Alta Conductividad): El modelo permite conocer cómo cada factor impacta la probabilidad de obtener la **clase positiva** (*alta conductividad*). Para ello se presentan los *incrementos en probabilidad*: *Si el factor tiene un incremento unitario, cuánto aumenta la probabilidad de tener una alta conductividad*.

- Conclusiones:
Dados los factores de importancia identificados, las recomendaciones son las siguientes:
  - Para los *factores con mayor importancia por magnitud* se recomienda controlar con un *SP menor* (si la importancia por magnitud es *positiva*) o con un *SP mayor* (si la importancia por magnitud es *negativa*) para *reducir la probabilidad de tener alta conductividad*.
  - Para conocer *qué tanto incrementar o decrementar el SP del factor*, se verifica su contribución a la *Probabilidad de Clase Positiva*: Si la contribución es positiva, se recomienda reducir el SP. Si la contribución es negativa, se recomienda aumentar el SP. Ambas acciones *reducirán la probabilidad de ocurrencia de la clase positiva* (alta conductividad).
  - Para los *factores con mayor importancia por control* se recomienda *reducir la varianza de la variable* (reducir su variación si es posible por medios manuales o ajustes de los lazos de control involucrados). En el caso de *importancias negativas*, significa que los algoritmos *no encontraron evidencia de que el control de esta variable importe para la estabilidad de la conductividad*.


