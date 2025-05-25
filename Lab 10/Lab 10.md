# **Laboratorio 10: Uso del panel de control de IA responsable para mejorar el rendimiento de los modelos de machine learning**

**Objetivo**

Este laboratorio tiene como objetivo obtener un aprendizaje práctico
sobre cómo usar el panel de inteligencia artificial responsable para
depurar los modelos de aprendizaje automático con el fin de mejorar el
rendimiento del modelo para que sea más justo, inclusivo, seguro,
confiable y transparente.

En este laboratorio exploraremos cómo utilizar la sección de **Model
Overview** del panel de Azure Responsible AI (RAI). Utilizaremos los
cohortes creados a partir del laboratorio de Error Analysis para
investigar por qué el comportamiento del modelo es mejor en un cohorte
en comparación con otro.

Tiempo estimado: 60 minutos

## **Ejercicio 1: Preparación de los recursos**

### Tarea 1: Clone el repositorio para este laboratorio

1.  Desde un navegador, inicie sesión en el portal de Azure en
    <https://portal.azure.com>

2.  Abra **Cloud Shell** haciendo clic en el ícono de Cloud Shell en el
    portal de Azure.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  En el símbolo del sistema de Azure Cloud Shell, clone el repositorio
    de GitHub del proyecto **Diabetes Hospital Readmission** ejecutando
    el siguiente comando.

> **+++git clone**
> [**https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification**](https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification)
> +++
>
> Esto clonará el contenido del repositorio localmente.
>
> ![](./media/image2.png)

4.  Cambie al directorio del proyecto ejecutando el siguiente comando.

**+++cd RAI-Clasificación de reingreso hospitalario por diabetes+++**

### Tarea 2: Inicie sesión mediante Azure CLI

1.  Desde cloud shell, ejecute el siguiente comando:

**az login**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

2.  Abra la URL en la consola y escriba el código en el navegador.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

3.  Seleccione las credenciales **de Azure login**.

> ![A screenshot of a phone Description automatically generated with
> medium confidence](./media/image5.png)

4.  Haga clic en **Continue**.

> ![A screenshot of a computer error Description automatically generated
> with medium confidence](./media/image6.png)

5.  Cierre el navegador y regrese al portal de Azure.

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  Los detalles de inicio de sesión se muestran en cloud shell.

> ![A screenshot of a computer Description automatically
> generated](./media/image8.png)

7.  Establezca su entorno por defecto al **grupo de recursos asignado**.

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **Ejercicio 2: Ejecutar trabajos para entrenar el modelo y crear el panel de control de RAI**

1.  Ejecute el siguiente comando para registrar el **training dataset**
    en el espacio de trabajo de Azure Machine Learning.

> **az ml data create -f cloud/train_data.yml**

Se ha creado el activo de datos y los detalles se muestran en cloud
shell.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  Ejecute el siguiente comando para registrar el **testing dataset**
    en el espacio de trabajo de Azure Machine Learning.

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  Cree una **instancia de cómputo** para ejecutar los trabajos. Luego,
    copie el nombre de la instancia (por ejemplo,
    **compute-xxxxxxxxxxxx**) al final de la ejecución para usarlo más
    adelante.

- Ejecute el siguiente comando para **crear** el **cómputo**.

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

4.  En el menú de Cloud Shell, haga clic en el panel **Open editor { }**
    para editar algunos de los archivos.

> ![Open editor](./media/image13.png)

5.  Haga clic en la carpeta
    **RAI-Diabetes-Hospital-Readmission-classification** para expandir
    el directorio.

![Expand directory](./media/image14.png)

6.  Navegue al archivo **cloud/training_job.yml**. Luego, reemplace el
    marcador de posición del nombre de cómputo con el nombre de su
    **compute instance name**.

![Training job update](./media/image15.png)

7.  Haga clic derecho en cualquier parte del archivo y luego seleccione
    la opción **Save** para guardar el archivo.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8.  A continuación, navegue hasta el archivo
    **cloud/rai_dashboard_pipeline.yml**. Actualice el marcador de
    posición del nombre de cómputo con el **compute instance name** que
    copió anteriormente.

![Rai pipeline update](./media/image17.png)

9.  Haga clic derecho en cualquier parte del archivo y luego seleccione
    la opción **Save** para guardar el archivo.

10. Haga clic derecho en cualquier parte del archivo y luego seleccione
    la opción **Quit** para cerrar la ventana del editor.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image18.png)

11. De vuelta en el símbolo del sistema de Cloud Shell, envíe el trabajo
    para entrenar el modelo. Espere a que el trabajo actualice su estado
    de ejecución a **Completed** durante el entrenamiento. Copie el
    siguiente bloque de código para hacerlo.

> **run_id=$(az ml job create --name my_training_job -f
> cloud/training_job.yml --query name -o tsv)**
>
> **\# wait for job to finish while checking for status**
>
> **if \[\[ -z "$run_id" \]\]**
>
> **then**
>
> **echo "Job creation failed"**
>
> **exit 3**
>
> **fi**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **if \[\[ -z "$status" \]\]**
>
> **then**
>
> **echo "Status query failed"**
>
> **exit 4**
>
> **fi**
>
> **running=("Queued" "Starting" "Preparing" "Running" "Finalizing")**
>
> **while \[\[ ${running\[\*\]} =~ $status \]\]**
>
> **do**
>
> **sleep 8**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **echo $status**
>
> **done**
>
> **Nota:** Si este script no se pega correctamente, cópielo y péguelo
> manualmente.
>
> **Nota:** La ejecución de este script debería tardar entre 3 y 5
> minutos.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

12. Opcionalmente, puede verificar el estado del trabajo en ejecución
    desde **Azure Machine Learning Studio (** <https://ml.azure.com/>
    **)** -\> **Jobs**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image21.png)

13. Una vez finalizado el entrenamiento, registre el modelo en el área
    de trabajo de Azure Machine Learning. Ejecute el siguiente comando
    para ello.

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> Este comando registra el modelo en el espacio de trabajo de AML y
> proporciona los detalles en Cloud Shell, como se muestra en las
> capturas de pantalla a continuación.
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

14. Envíe el flujo de trabajo para crear el **RAI dashboard**. Ejecute
    el siguiente comando para ello.

az ml job create --file cloud/rai_dashboard_pipeline.yml

Este comando envía el trabajo y Cloud Shell se llena con la etapa
inicial de la canalización, que es el estado **Preparing.**

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

15. Inicie sesión en **Azure Machine Learning Studio** en
    <https://ml.azure.com/> para supervisar el trabajo de canalización
    para crear el panel de RAI.

16. Seleccione **Pipelines**. Para ver el progreso del trabajo de
    canalización que crea el panel de control de RAI, haga clic en el
    **Display name** del trabajo.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

17. El experimento estará en estado **Running**.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

18. El estado cambia a **Completed** una vez que se realiza y se crea el
    panel de RAI.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

19. Haga clic en la pestaña **Models** en el panel de navegación
    izquierdo. Luego, haga clic en el nombre del modelo para abrir la
    página de detalles.

> ![](./media/image29.png)

20. Seleccione la opción **Responsible AI** en el menú superior.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

21. Ahora ya está listo para comenzar a utilizar el **panel de control
    de RAI**.

## **Ejercicio 3: Análisis de errores:**

La sección Análisis de Errores del panel RAI proporciona una
distribución de errores según los grupos de características que
contribuyen a la tasa de error del modelo. Dado que los errores no
suelen distribuirse de forma uniforme entre los distintos subgrupos de
datos, esta sección permite identificar aquellas características
asociadas con las tasas de error más elevadas.

### Tarea 1: Encuentre los errores del modelo:

En esta tarea, exploraremos cómo utilizar el análisis de errores para
identificar los errores en el modelo entrenado y determinar su
ubicación. Además, aprenderemos a crear cohortes de datos para
investigar las razones detrás del bajo rendimiento del modelo en algunas
cohortes en comparación con otras.

1.  Haga clic en el nombre **Diabetes Hospital Readmission.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

2.  Seleccione **Compute**.

![](./media/image32.png)

#### **Tarea 1.1: Identifique y cree una cohorte para la ruta del árbol con mayor cantidad de errores**

Para iniciar el análisis, se puede observar que el nodo raíz muestra que
de un total de 994 datos de prueba, se encontraron 168 predicciones
incorrectas al evaluar el modelo.

1.  Encuentre la ruta del árbol con mayor número de errores. Cuanto más
    oscuro sea el rojo en el nodo, mayor será la tasa de error.

2.  En nuestro caso, la ruta del árbol con el color rojo más oscuro es
    el nodo de la hoja que está segundo desde abajo a la derecha.

![](./media/image33.png)

3.  **Haga doble clic** en este **nodo** para seleccionar la **ruta
    completa** que conduce hasta él. Esto resalta la ruta y muestra el
    estado de cada nodo.

4.  Cree una cohorte a partir de la ruta seleccionada haciendo clic en
    el botón **Save as a new cohort** en la parte superior derecha de la
    sección Análisis de errores.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)

5.  Ingrese el **nombre de la cohorte** como **+++Err: Prior_Inpatient
    \>0; Num_meds \>11.50 & \<= 21.50+++**

**Haga clic en Save.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **Tarea 1.2: Identifique y cree una cohorte para la ruta del árbol con menos errores**

Para fines de contraste, cree otro cohorte con la ruta del árbol que
tenga el menor número de errores para ver si podemos obtener insights
sobre por qué el modelo tiene un mejor rendimiento en un cohorte en
comparación con otro. El **leaf node** con la condición de
característica **num_lab_procedures ≤ 56.50**, en el extremo izquierdo
del árbol, es la ruta del árbol con menos errores.

1.  **Haga doble clic** en el nodo.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

2.  Haga clic en **Save as a new cohort**. El **filtro** de este
    conjunto de datos es: num_lab_procedures \<= 56.50, number_diagnoses
    \<= 6.50, prior_inpatient \<= 0.00.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

3.  **Nombre** la cohorte: **+++Prior_Inpatient = 0; num_diagnoses \<=
    6.50; lab_procedures \<= 56.50+++** y haga clic en **Save**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **Tarea 1.3: Utilice la lista de características para identificar la característica principal que contribuye a los errores del modelo**

1.  Haga clic en **Feature list**.

![](./media/image39.png)

2.  La lista se ordena según la contribución de las características a
    los errores. Cuanto más arriba esté una característica en la lista,
    mayor será su importancia para los errores del modelo.

3.  En nuestro modelo de readmisión hospitalaria por diabetes, la
    **Feature list** indica que las siguientes características se
    encuentran entre las principales contribuyentes a los errores del
    modelo.

    - Age

    - num_medications

    - medicare

    - time_in_hospital

    - num_procedures

    - insulin

    - discharge_destination

### Tarea 2: Encuentre los errores usando el mapa de calor

De la lista de características, **la edad** fue uno de los principales
factores de error. Por lo tanto, usaremos la pestaña Heat map para
explorar qué grupo de edad de los pacientes está causando un rendimiento
deficiente del modelo.

1.  Seleccione **Heat map** en **Error Analysis**.

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)

2.  En la pestaña **Heat Map**, seleccione **Age** en el menú
    desplegable **Rows: Feature 1** para ver qué papel juega en los
    errores del modelo**.**

3.  Luego de seleccionar la **Age**, podemos ver como el dashboard tiene
    una inteligencia incorporada para dividir la característica en
    diferentes celdas con las posibles condiciones.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

2.  **Pase** el ratón sobre cada celda, podrá ver el número de
    predicciones correctas vs incorrectas, la cobertura de errores y la
    tasa de errores para el grupo de datos representado en la celda.

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)

3.  La celda con **más de 60 años** tiene **536** predicciones correctas
    y **126 incorrectas. La cobertura de error es del 73,81 %** y la
    tasa de error **del 18,79 %.**

4.  La celda de **30 a 60 años** tiene **273** predicciones correctas y
    **25 incorrectas. La cobertura de error es del 25,60 %** y la tasa
    de error **del 13,61 %**.

5.  La celda con* ***30 años o menos*** *tiene **17** predicciones de
    modelo correctas y **1 incorrecta.**

> Dado que nuestra observación muestra que **Age** juega un papel
> importante en las predicciones erróneas del modelo, vamos a crear
> cohortes para cada grupo de edad para un análisis más detallado en el
> próximo laboratorio.

#### ***Tarea 2.1: Cree cohortes basadas en los grupos de edad***

1.  Haga clic en el porcentaje de la celda **Over 60 years**. Verá un
    borde azul alrededor de la celda cuadrada.

2.  Haga clic en **Save as a new cohort**.

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

3.  En el cuadro de diálogo **Save as a new cohort**, ingrese:

    - Cohort name - **+++Age==Over 60 year+++**

Haga clic en **Save**.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

4.  Repita los pasos 2 y 3 para crear un cohorte para cada una de las
    otras dos celdas de Age.

- **Cohort \#4:** Name - **+++Age == 30–60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### Tarea 3: Visualice las listas de cohortes

1.  Haga clic en el icono de engranaje en **Settings** en la esquina
    superior derecha de la sección Error Analysis.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)

2.  Esto abrirá un panel de **Configuración de Cohorte** con la lista de
    todos los cohortes que ha creado.

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## Ejercicio 4: Uso de RAI para realizar análisis de modelos

En este laboratorio exploraremos cómo utilizar la sección de **Model
Overview** del panel de Azure Responsible AI (RAI). Utilizaremos los
cohortes creados a partir del laboratorio de Error Analysis para
investigar por qué el comportamiento del modelo es mejor en un cohorte
en comparación con otro.

## **Ejercicio 4.1: Descripción general del modelo**

### Tarea 1: Revise y compare la tabla de métricas de rendimiento del modelo

1.  Desplácese hacia abajo, debajo de Error Analysis, para encontrar la
    sección de Model Overview.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

2.  Bajo Model Overview, seleccione el panel **Dataset Cohorts**. Esto
    muestra los diferentes cohortes creados en una tabla con las
    métricas del modelo.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

3.  Compare la cohorte con más errores **Err: Prior_Inpatient \> 0;
    Num_Meds \> 11 and ≤ 21.50** frente a la menor cantidad de errores
    **Prior_inpatient = 0; num_diagnose ≤ 6.50; lab_procedures \<
    56.50.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4.  Pase el mouse sobre la línea del diagrama de caja en el gráfico para
    ver los detalles de la medición.

![A screenshot of a computer Description automatically
generated](./media/image50.png)

5.  Observe que el puntaje de precisión para el **cohorte erróneo** es
    0.806, lo cual es bajo. La tasa de **falsos positivos** es **muy
    baja** y el valor de **falsos negativos** es **alto**. Esto
    significa que la mayoría de los pacientes que el modelo predice
    tienen una alta tasa de predicción de pacientes que no serán
    readmitidos como readmitidos en 30 días en el hospital.

> ![A red line in a white sheet Description automatically
> generated](./media/image51.png)

6.  A continuación, observe las métricas de la **cohorte** con **menos
    errores**: su puntuación de precisión es de 0,94, mucho mejor que la
    puntuación de precisión general del modelo con todos los datos. Sin
    embargo, esta cohorte también presenta una baja tasa **de falsos
    positivos : 0**.

![A picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### Tarea 2: Examine el gráfico de distribución de probabilidad

1.  Desplácese hacia abajo para ver la **distribución de probabilidad**.

2.  El gráfico de distribución de probabilidad muestra la probabilidad
    del modelo de predecir si los pacientes de las cohortes serán
    readmitidos o no al hospital dentro de los 30 días.

3.  Compare la probabilidad de que los pacientes no sean readmitidos
    para las tres cohortes.

4.  Verá que la cohorte de datos **All data** los datos con el conjunto
    de datos de prueba de todos los pacientes muestra que la mayoría de
    los pacientes no serán readmitidos nuevamente al hospital dentro de
    los 30 días, con una probabilidad media de pacientes no readmitidos
    de 0,854 y un cuartil superior de 0,986, lo cual es bueno.

5.  A continuación, la cohorte con la tasa de error más alta: ***Err:
    Prior_Inpatient \>0; Num_meds \>11.50 & \<= 21.50***, muestra una
    probabilidad ligeramente menor de 0.89 y una mediana de 0.719.

6.  Por último, la cohorte con menor tasa de error: ***Prior_Inpatient =
    0*; *num_diagnoses \<= 6.50*; *lab_procedures \<= 56.50***, muestra
    una probabilidad de pacientes no readmitidos con una mediana de 0.90
    y un cuartil superior de 0.986.

> ![A screenshot of a computer Description automatically
> generated](./media/image53.png)

7.  Para cambiar el gráfico para mostrar la probabilidad de que los
    pacientes sean readmitidos para las 3 cohortes, haga clic en el
    botón **Choose Label** en el eje x.

8.  Seleccione el botón **Probability: Readmitted** en la ventana
    emergente.

9.  Luego haga clic en el botón **Apply**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

10. Compare la probabilidad de que los pacientes sean readmitidos para
    las 3 cohortes.

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image55.png)

9.  Se observa que las tres cohortes tienen una probabilidad de
    readmisión inferior a 0,55. La cohorte con el menor número de
    errores de modelo tiene la probabilidad más baja, 0,179. La cohorte
    con la mayor cantidad de errores tiene la probabilidad más alta,
    0,543.

### Tarea 3: Revise el gráfico de visualización de métricas

Ahora obtengamos una comprensión más profunda del rendimiento del modelo
cambiando al panel de visualizaciones de métricas.

1.  Haga clic en la pestaña **Metric visualizations**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

2.  Para elegir otra métrica, haga clic en **Choose metric** en el eje x
    para seleccionar **Precision score** de la lista de otras métricas
    disponibles. Luego, haga clic en el botón **Apply.**

> **Nota**: Dado que el modelo entrenado es un problema de
> clasificación, el panel de RAI solo mostrará métricas de
> clasificación.
>
> ![](./media/image57.png)

3.  Al revisar el gráfico, verá que el rendimiento del modelo para todos
    los cohortes de datos de prueba y el cohorte erróneo es correcto
    aproximadamente el 70% del tiempo.

4.  La tasa de **puntuación de precisión** para la **cohorte menos
    errónea** es **de 0,94** para pacientes sin hospitalización previa y
    el número de diagnósticos es inferior a 7. Esto es coherente con la
    puntuación de precisión.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image58.png)

5.  Por último, cambie la métrica a **Recall** para ver qué tan bien el
    modelo pudo predecir correctamente que los pacientes en las cohortes
    serán readmitidos nuevamente al hospital en 30 días.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6.  El recall muestra que la **predicción del modelo** fue **correcta
    menos del 25%** de las veces para todos los cohortes de pacientes
    que serán readmitidos. Esto revela que las predicciones del modelo
    no son correctas la mayoría de las veces cuando se intenta predecir
    qué pacientes serán readmitidos dentro de los 30 días.

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### Tarea 4: Observe la matriz de confusión

La Matriz de Confusión es útil para comprobar la tasa de acierto del
modelo en la predicción. Esto revelará su capacidad de aprendizaje en
los casos en que el paciente es readmitido en el hospital dentro de los
30 días frente a los casos en que no lo es.

1.  Haga clic en la pestaña **Confusion matrix**.

&nbsp;

2.  Observará que el **modelo** funciona **mejor** con pacientes que
    **no son readmitidos** en comparación con **los readmitidos** .

3.  El número de falsos negativos debe ser menor que el de verdaderos
    negativos. Esto significa que, de todos los datos de pacientes, el
    modelo solo pudo predecir correctamente el reingreso hospitalario de
    24 pacientes en menos de 30 días.

- El número de Verdaderos Positivos (VP) es: **802**

- El número de falsos negativos (FN) es: **159**

- El número de falsos positivos (FP) es: **9**

- El número de Verdaderos Negativos (VN) es: **24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **Ejercicio 2: Cohorte de características**

Dado que la cohorte con el mayor error incluye pacientes con un número
de pacientes *hospitalizados (Prior_Inpatient) \> 0* días y un número de
medicamentos entre 11 y 22, donde el modelo presentó una mayor tasa de
error, analizar con más detalle *Prior_Inpatient* y *Num_medications*
ayudará a identificar los problemas. En este laboratorio, solo
analizaremos *Prior_Inpatient*.

1.  Haga clic en la pestaña **Feature Cohorts**.

2.  En el menú desplegable **Feature(s),** desplácese por la lista y
    seleccione la casilla **prior_inpatient**. Esto mostrará tres
    cohortes de características diferentes y las métricas de rendimiento
    del modelo.

> ![A screenshot of a computer Description automatically
> generated](./media/image62.png)

3.  El cohorte **prior_inpatient \< 3** tiene un tamaño de muestra de
    **943**. Esto significa que la mayoría de los pacientes en los datos
    de prueba fueron hospitalizados menos de 3 veces en el pasado. **La
    tasa de precisión del modelo** para este cohorte es **0.838**, lo
    cual es bueno.

4.  Solo 39 pacientes de los datos de prueba pertenecen al cohorte
    **prior_inpatient ≥ 3 y \< 6**. La tasa de precisión del modelo es
    **0.692**, lo cual no es bueno.

5.  Por último, solo 12 pacientes de los datos de prueba tienen una
    hospitalización previa de 6 días o más. La **precisión del modelo**
    de **0,75** para esta cohorte es aceptable.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

### Tarea 1: Distribución de probabilidad de características

De manera similar a la cohorte del conjunto de datos, tiene la
posibilidad de ver la Probability Distribution.

1.  Se puede observar que cuanto menor sea el número de
    hospitalizaciones previas del paciente diabético, mayor será la
    probabilidad de que el paciente no sea readmitido en 30 días.

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### Tarea 2: Visualizaciones de métricas de características

1.  Seleccione **Metrics visualization**. En el eje X, haga clic en el
    botón **Choose metric**. A continuación, seleccione la métrica
    **Precision score.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image65.png)

2.  Se observe que el puntaje de precisión para los pacientes con
    **prior_inpatient \< 3** es 0.40, lo cual es muy bajo. Esto
    significa que de todas las predicciones que hizo el modelo, solo el
    40% fueron correctas para este cohorte.

> ![A blue and white bar graph Description automatically
> generated](./media/image66.png)

3.  La puntuación de precisión de las otras dos cohortes es buena.

4.  A continuación, seleccione la métrica **Recall score** para el eje
    x.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image67.png)

5.  Por el contrario, verá que el puntaje de recall para los pacientes
    con **prior_inpatient \< 3** es 0.013. Esto significa que, para la
    mayoría de los pacientes en los datos de prueba, el modelo tiene
    dificultades para predecir correctamente si el paciente será
    readmitido dentro de los 30 días o no.

> ![A picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **Resumen**
>
> Este laboratorio muestra cómo las métricas tradicionales de
> rendimiento del modelo (por ejemplo, precisión, recall, matriz de
> confusión, etc.) siguen siendo muy importantes. Al combinar los
> insights de RAI y las métricas tradicionales de rendimiento, el panel
> nos proporciona una herramienta integral para analizar y depurar el
> modelo a un nivel más granular.
