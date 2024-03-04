## Modelo de Identificación de Altas Trazas en Canal de Drenaje de Molinos.

El presente proyecto pretende generar un modelo que ayude a predecir la presencia de material azucarado en el canal de drenaje del Área de Molinos y así reducir las pérdidas de producto por motivo de derrames. Con apoyo de los ingenieros de producción, se realizó una selección de las variables de proceso que pueden incidir en las trazas del canal de drenaje. Las variables tomadas en cuenta son las siguientes: 

  - Flujo de asepsia para patio y Molinos Tándem A y B.
  - Niveles de los chutes de Molinos Tándem A y B.
  - Niveles de todos los evaporadores del efecto A al E.
  - Conductividad del canal de drenaje de Molinos. 

Se utilizan los datos de Laboratorio sobre trazas en el canal de molinos. Dado que la muestra corresponde a un instante determinado, para poder correlacionar valores en el sistema de control se toma el valor promedio de la variable del sistema en el período entre 5 minutos antes de la muestra y 5 minutos después de la muestra, de esta manera se tiene un valor más representativo de la distribución de las variables al momento de medir la traza. 

El conjunto de datos cuenta con 53 variables y con 580 observaciones de los primeros 80 días de la presente zafra.  Se definió como variable objetivo la medición de trazas del canal de drenaje; a continuación se muestra el comportamiento de las trazas en el tiempo. 


![image](https://github.com/dsPSA2023/PSA/assets/161398218/d2b6d9e8-0dad-453b-87dc-b0499206ddad)


El valor de las trazas comenzó a disminuir alrededor de la observación 150 (día de zafra 23) y vuelve a presentar valores altos cerca de la observación 565 (día de zafra 95), alcanzando valores por arriba de 2000 mg/L.  

La conductividad presenta una distribución sesgada a la derecha, alcanzando valores arriba de 8000 mg/L.     

![image](https://github.com/dsPSA2023/PSA/assets/161398218/96e5a241-f6c8-4286-a190-1ba9d8aa927b)


Estadísticas descriptivas conductividad 

  - Mínimo:         8.00 mg/L
  - Máximo:      9249.00 mg/L 
  - Media:       1743.00 mg/L
  - Mediana:     1327.00 mg/L 

La idea principal del proyecto es determinar la presencia de altas trazas en el canal de drenaje, esto se realizó mediante la aplicación del algoritmo de regresión logística. Para lo cual se realizó una *transformación* de la variable continua de trazas a una variable binaria de *presencia* o *no presencia* de altas trazas. 


Se realizaron 2 análisis al conjunto de datos, el primero sin incluir el día de zafra y el segundo incluyéndolo, para evaluar el impacto que tiene el día de zafra en la variable en estudio.
 
## Analisis sin Dia de Zafra.
[Notebook](https://github.com/dsPSA2023/PSA/blob/168615c0c0a49e31a6aad6a195d04aee52d71e81/Efluentes/Trazas/Punto%201%20Canal%20Drenaje%20Molinos/Trazas_Canal_Molinos.ipynb)

- Agrupamiento de Datos: De las metricas obtenidas por el modelo de Clustering se determina que 3 agrupaciones es la opción mas adecuada. Al identificar cada una de las 3 agrupaciones se observa que los datos de altas trazas de inicio de zafra pertenecen a 3 agrupaciones distintas por lo que no es posible identificar regiones de operación que determinen la presencia de Altas Trazas en el canal de drenaje.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/57d42879-fc81-4f44-8604-ff853c7a5122)


- Busqueda del Umbral Optimo: Al evaluar la metricas de los modelos con diferentes umbrales se observa que el umbral con valor de 1250 mg/L muestra metricas por encima de 64% con un Overall de 67% y una proporcion balanceada de clases, teniendo 53% de presencia de Clase Positiva.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/d3392ac3-5364-47a8-94d1-0368d898d009)


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/c7a1bc9c-772f-47ba-af75-5651df0e011a)


  
- Generacion de modelo de regresion logistica con el umbral optimo: Se elije un umbral de 1250 mg/L y se entrena el modelo de regresion logistica.   

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/8cc446e2-89b9-4805-b646-e18c49c082ad)



- Evaluación de Metricas Finales: Las metricas finales con el conjunto de datos de prueba muestran un desepeño aceptable de clasificacion para el modelo con el umbral determinado.  
  - Matriz de Confusion:  

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/09c49c46-ac91-4d51-8939-0504c745e4fa)



  - Curva PR:

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/c1cfd7ab-0a9c-4823-ae99-79c704fe9561)


 
- Determinación de Factores de Importancia:
  - Importancia por Magnitud de Factor:  El modelo muestra las siguientes variables como mas importantes por Magnitud:
      - Nivel de Chute del Molino 3 Tándem A (STD)
      - Nivel del Vaso A1 (AVG)
      - Nivel del Vaso B4 (AVG)
      - Nivel del Vaso D1 (STD)
      - Nivel del Vaso A6 (STD) 

     ![image](https://github.com/dsPSA2023/PSA/assets/161398218/ddb638c4-ccbf-4a1c-9b23-a56184c8007d)


  - Importancia por Permutacion de Factor: El modelo muestra las siguientes variables como mas importantes por Control:
      - Nivel de Chute del Molino 3 Tándem A (STD)
      - Nivel del Vaso C5 (AVG)
      - Flujo de Agua de Asepsia a Molinos Tándem A (STD)
      - Nivel del Vaso E4 (STD)
      - Nivel del Vaso D2 (STD)

  
    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/3002edeb-c65e-4338-9fee-d9a99a4f8861)


- Probabilidad de Clase Positiva (Alta Conductividad): El modelo muestra las siguientes variables como mas importantes para el incremento de la Probabilidad de obtener una clase Positiva:

  - Nivel del Vaso A6 (STD): 11% por unidad de aumento. 
  - Nivel del Vaso A6 (STD): 11% por unidad de aumento.
  - Nivel de Chute del Molino 3 Tándem A (STD): 6% por unidad de aumento.
  - Nivel del Vaso B2 (STD): 5% por unidad de aumento. 
  - Nivel del Vaso E4 (STD): 5% por unidad de aumento.


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/fd6c30f2-e093-4f43-9a12-a942adb632a5)



## Analisis con Dia de Zafra.
[Notebook](https://github.com/dsPSA2023/PSA/blob/168615c0c0a49e31a6aad6a195d04aee52d71e81/Efluentes/Trazas/Punto%201%20Canal%20Drenaje%20Molinos/Trazas_Canal_Molinos_Dia_Zafra.ipynb)

- Agrupamiento de Datos: De las metricas obtenidas por el modelo de Clustering se determina que 3 agrupaciones es la opción mas adecuada. Al identificar cada una de las 3 agrupaciones se observa que los datos de altas trazas de inicio de zafra pertenecen a 3 agrupaciones distintas por lo que no es posible identificar regiones de operación que determinen la presencia de Altas Trazas en el canal de drenaje.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/6166f4db-9674-406c-97f5-b4ce0b50ac51)


- Busqueda del Umbral Optimo: Al evaluar la metricas de los modelos con diferentes umbrales se observa que el umbral con valor de 1250 mg/L muestra metricas por encima de 65% con un Overall de 69% y una proporcion balanceada de clases, teniendo 53% de presencia de Clase Positiva.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/43ed6ea3-c27b-43bc-a1ad-3203e960bd1d)




   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/62914c55-69fc-4ebf-a232-b45666f8b9e7)



  
- Generacion de modelo de regresion logistica con el umbral optimo: Se elije un umbral de 1250 mg/L y se entrena el modelo de regresion logistica.   

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/355f8031-231a-46c4-9329-722d45c497fa)





- Evaluación de Metricas Finales: Las metricas finales con el conjunto de datos de prueba muestran un desepeño aceptable de clasificacion para el modelo con el umbral determinado.  
  - Matriz de Confusion:  

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/09c49c46-ac91-4d51-8939-0504c745e4fa)



  - Curva PR:

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/c1cfd7ab-0a9c-4823-ae99-79c704fe9561)


 
- Determinación de Factores de Importancia:
  - Importancia por Magnitud de Factor:  El modelo muestra las siguientes variables como mas importantes por Magnitud:
      - Nivel del Vaso A1 (AVG)
      - Nivel de Chute del Molino 3 Tándem A (STD)
      - Nivel del Vaso B4 (AVG)
      - Nivel del Vaso D1 (STD)
      - Nivel del Vaso E3 (AVG) 

     ![image](https://github.com/dsPSA2023/PSA/assets/161398218/c0d0cc71-56b8-410a-9de5-9a86508a94f2)


  - Importancia por Permutacion de Factor: El modelo muestra las siguientes variables como mas importantes por Control:
      - Nivel del Vaso C5 (AVG)
      - Nivel de Chute del Molino 3 Tándem A (STD)
      - Nivel del Vaso E4 (STD)
      - Nivel de Chute del Molino 4 Tándem A (AVG)
      - Flujo de Agua de Asepsia a Molinos Tándem A (STD)

  
    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/87d5a77e-d067-4308-bbce-a2970e7d6389)


- Probabilidad de Clase Positiva (Alta Conductividad): El modelo muestra las siguientes variables como mas importantes para el incremento de la Probabilidad de obtener una clase Positiva:

  - Nivel del Vaso A6 (STD): 12% por unidad de aumento. 
  - Nivel del Vaso D1 (STD): 11% por unidad de aumento.
  - Nivel de Chute del Molino 3 Tándem A (STD): 6% por unidad de aumento.
  - Nivel de Chute del Molino 2 Tándem A (STD): 6% por unidad de aumento. 
  - Nivel del Vaso E4 (STD): 5% por unidad de aumento.


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/f583d1c9-03e2-480c-8a14-92210c266a44)


