## Modelo de Identificación de Alta Conductividad en Canal de Drenaje de Molinos.

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




 
## Analisis sin Dia de Zafra.
[Notebook](https://github.com/dsPSA2023/PSA/blob/4e0f66fee5051fb58333692f7bdd920614a5e90c/Efluentes/Conductividad/Punto%201%20Canal%20Drenaje%20Molinos/Trazas_Canal_Molinos.ipynb)

- Agrupamiento de Datos: De las metricas obtenidas por el modelo de Clustering se determina que 3 agrupaciones es la opción mas adecuada. Al identificar cada una de las 3 agrupaciones se observa que los datos de alta conductividad de inicio de zafra pertenecen a 2 agrupaciones distintas por lo que no es posible identificar regiones de operación que determinen la presencia de Alta Conductividad en el canal de drenaje.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/4687c1e4-a763-4004-aa15-6ad34eb7196f)

- Busqueda del Umbral Optimo: Al evaluar la metricas de los modelos con diferentes umbrales se observa que el umbral con valor de 250 uS muestra metricas por encima de 75% con un Overall de 80% y una proporcion balanceada de clases, teniendo 51% de presencia de Clase Positiva.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/37e38427-b005-4a19-87f9-e4fa2880fa61)

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/a9147c55-0ab4-4844-b5ee-153f6c726851)
  
- Generacion de modelo de regresion logistica con el umbral optimo: Se elije un umbral de 250 uS y se entrena el modelo de regresion logistica.   

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/0fe95fa8-9c69-4f0c-8924-ee3a7a2e1440)


- Evaluación de Metricas Finales: Las metricas finales con el conjunto de datos de prueba muestran un desepeño aceptable de clasificacion para el modelo con el umbral determinado.  
  - Matriz de Confusion:  

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/b9d01cea-7238-4d7c-8ad5-32f1dbeee071)

  - Curva PR:

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/80a62613-8844-4175-8a77-a6b69b80a0a6)
 
- Determinación de Factores de Importancia:
  - Importancia por Magnitud de Factor:  El modelo muestra las siguientes variables como mas importantes por Magnitud:
      - Flujo de Agua de Asepsia a Patio Tándem A (AVG)
      - Flujo de Agua de Asepsia a Patio Tándem B (STD)
      - Flujo de Agua de Asepsia a Molinos Tándem A (STD)
      - Nivel de Chute del Molino 5 Tándem B (AVG)
      - Nivel de Chute del Molino 4 Tándem B (STD)
      - Nivel de Chute del Molino 1 Tándem B (AVG)  

     ![image](https://github.com/dsPSA2023/PSA/assets/161398218/779fbeab-4967-4263-906a-38971d3941c9)

  - Importancia por Permutacion de Factor: El modelo muestra las siguientes variables como mas importantes por Control:
      - Flujo de Agua de Asepsia a Molinos Tándem B (STD)
      - Flujo de Agua de Asepsia a Patio Tándem A (AVG)
      - Flujo de Agua de Asepsia a Patio Tándem B (AVG)
      - Nivel de Chute del Molino 5 Tándem B (AVG)
      - Nivel de Chute del Molino 1 Tándem A (AVG)  

  
    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/086ec61c-b684-4dbf-9932-5e30dd96cfd5)

- Probabilidad de Clase Positiva (Alta Conductividad): El modelo muestra las siguientes variables como mas importantes para el incremento de la Probabilidad de obtener una clase Positiva:

  - Flujo de Agua de Asepsia a Patio Tándem A (AVG): 7% por unidad de aumento. 
  - Nivel Tanque Jugo Filtrado Tándem B (STD): 5% por unidad de aumento.
  - Flujo de Agua de Asepsia a Molinos Tándem B (AVG): 3% por unidad de aumento.
  - Nivel de Chute del Molino 4 Tándem B (STD): 3% por unidad de aumento.
  - Flujo de Agua de Asepsia a Molinos Tándem A (STD): 2% por unidad de aumento.


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/cbf4c6a5-95ca-4a6c-adca-f4cab039d958)


