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


Se realizaron 2 análisis al conjunto de datos, el primero sin incluir el día de zafra y el segundo incluyéndolo, para evaluar el impacto que tiene el día de zafra en la variable en estudio.
 
## Analisis sin Dia de Zafra.
[Notebook](https://github.com/dsPSA2023/PSA/blob/bcdcfd4117b736d63e56fb5427a066d5777b22c2/Efluentes/Conductividad/Punto%201%20Canal%20Drenaje%20Molinos/Conductividad_Canal_Molinos.ipynb)

- Agrupamiento de Datos: De las metricas obtenidas por el modelo de Clustering se determina que 3 agrupaciones es la opción mas adecuada. Al identificar cada una de las 3 agrupaciones se observa que los datos de alta conductividad de inicio de zafra pertenecen a 2 agrupaciones distintas por lo que no es posible identificar regiones de operación que determinen la presencia de Alta Conductividad en el canal de drenaje.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/4687c1e4-a763-4004-aa15-6ad34eb7196f)

- Busqueda del Umbral Optimo: Al evaluar la metricas de los modelos con diferentes umbrales se observa que el umbral con valor de 250 uS muestra metricas por encima de 78% con un Overall de 80% y una proporcion balanceada de clases, teniendo 51% de presencia de Clase Positiva.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/27e3052e-91ad-4e1a-b992-95d8730fa107)


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/ae5520d5-0f10-4338-9fc2-5fe63bb6bddb)

  
- Generacion de modelo de regresion logistica con el umbral optimo: Se elije un umbral de 250 uS y se entrena el modelo de regresion logistica.   

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/2321fe82-ede6-4e1f-8c86-e7290f37bfb8)


- Evaluación de Metricas Finales: Las metricas finales con el conjunto de datos de prueba muestran un desepeño aceptable de clasificacion para el modelo con el umbral determinado.  
  - Matriz de Confusion:  

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/85281bb2-e3ff-4296-83fa-babd7020eb41)


  - Curva PR:

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/fe7bca8d-b00c-414d-838f-eb58f4740393)

 
- Determinación de Factores de Importancia:
  - Importancia por Magnitud de Factor:  El modelo muestra las siguientes variables como mas importantes por Magnitud:
      - Flujo de Agua de Asepsia a Patio Tándem A (AVG)
      - Flujo de Agua de Asepsia a Patio Tándem B (STD)
      - Flujo de Agua de Asepsia a Molinos Tándem A (STD)
      - Nivel de Chute del Molino 5 Tándem B (AVG)
      - Nivel de Chute del Molino 4 Tándem B (STD) 

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


## Analisis Con Dia de Zafra
[Notebook](https://github.com/dsPSA2023/PSA/blob/bcdcfd4117b736d63e56fb5427a066d5777b22c2/Efluentes/Conductividad/Punto%201%20Canal%20Drenaje%20Molinos/Conductividad_Canal_Molinos_Dia_Zafra.ipynb).

- Agrupamiento de Datos: De las metricas obtenidas por el modelo de Clustering se determina que 3 agrupaciones es la opción mas adecuada. Al identificar cada una de las 3 agrupaciones se observa que los datos de alta conductividad de inicio de zafra pertenecen a 2 agrupaciones distintas por lo que no es posible identificar regiones de operación que determinen la presencia de Alta Conductividad en el canal de drenaje.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/445e09d3-dbe4-4357-a31d-7442b02a6013)


- Busqueda del Umbral Optimo: Al evaluar la metricas de los modelos con diferentes umbrales se observa que el umbral con valor de 250 uS muestra metricas por encima de 78% con un Overall de 80% y una proporcion balanceada de clases, teniendo 51% de presencia de Clase Positiva.  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/c98abd24-ca89-47d8-9429-c72f12776c8a)


   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/2eb74b3b-da1f-4657-8a32-31e03f53a151)

  
- Generacion de modelo de regresion logistica con el umbral optimo: Se elije un umbral de 250 uS y se entrena el modelo de regresion logistica.   

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/df933527-ebf8-40d4-ad35-d050a9bbdec9)



- Evaluación de Metricas Finales: Las metricas finales con el conjunto de datos de prueba muestran un desepeño aceptable de clasificacion para el modelo con el umbral determinado.  
  - Matriz de Confusion:  

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/d6496a72-9800-4757-902c-42766b808a19)


  - Curva PR:

    ![image](https://github.com/dsPSA2023/PSA/assets/161398218/03cf9b9a-1235-4eaf-92ec-1c7e1441a1de)

 
- Determinación de Factores de Importancia:
  - Importancia por Magnitud de Factor:  El modelo muestra las siguientes variables como mas importantes por Magnitud:
      - Flujo de Agua de Asepsia a Molinos Tándem A (STD)
      - Flujo de Agua de Asepsia a Patio Tándem B (STD)
      - Flujo de Agua de Asepsia a Patio Tándem A (AVG)
      - Nivel de Chute del Molino 4 Tándem B (STD)
      - Nivel de Chute del Molino 1 Tándem B (AVG)

     ![image](https://github.com/dsPSA2023/PSA/assets/161398218/216555f9-2c77-4d8c-b322-6647bc82af22)


  - Importancia por Permutacion de Factor: El modelo muestra las siguientes variables como mas importantes por Control:
      - Flujo de Agua de Asepsia a Molinos Tándem B (STD)
      - Dia de Zafra
      - Nivel de Chute del Molino 1 Tándem A (AVG)
      - Flujo de Agua de Asepsia a Patio Tándem B (STD)
      - Nivel de Chute del Molino 5 Tándem B (AVG)
      - Nivel de Chute del Molino 6 Tándem A (AVG)
        
  
      ![image](https://github.com/dsPSA2023/PSA/assets/161398218/05a34dd1-c3a5-4278-994d-a75a1758976c)


- Probabilidad de Clase Positiva (Alta Conductividad): El modelo muestra las siguientes variables como mas importantes para el incremento de la Probabilidad de obtener una clase Positiva:

  - Flujo de Agua de Asepsia a Patio Tándem A (AVG): 5% por unidad de aumento. 
  - Nivel Tanque Jugo Filtrado Tándem B (STD): 5% por unidad de aumento.
  - Flujo de Agua de Asepsia a Molinos Tándem A (STD): 3% por unidad de aumento.
  - Flujo de Agua de Asepsia a Molinos Tándem B (AVG): 3% por unidad de aumento.
  - Nivel de Chute del Molino 4 Tándem B (STD): 3% por unidad de aumento.
  

   ![image](https://github.com/dsPSA2023/PSA/assets/161398218/985f438f-8fa3-4d75-b64f-182dfb2c9d48)

