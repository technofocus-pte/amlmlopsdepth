# **Laboratorio 05 - Pronóstico de la demanda con Automated Machine Learning sin código en Azure Machine Learning Studio**

**Objetivo**

En este laboratorio, aprenderá a crear un modelo de pronóstico de series
temporales sin escribir una sola línea de código, utilizando Automated
Machine Learning en Azure Machine Learning Studio. Este modelo predecirá
la demanda de alquiler para un servicio de bicicletas compartidas.

No se escribirá código en este laboratorio; se utilizará la interfaz de
Studio para realizar el entrenamiento.

Tiempo estimado – 60 minutos

## **Ejercicio 1: Preparar el entorno**

### **Tarea 1: Inicie el espacio de trabajo de AML**

1.  Inicie sesión en el portal de Azure,
    +++**https://portal.azure.com+++** si aún no ha iniciado sesión.

2.  Desde el menú del portal de Azure, seleccione **All resources.**

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Seleccione el espacio de trabajo de Azure Machine Learning
    (**Azuemlws@lab.LabInstanceId**).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Haga clic en **Launch studio**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## **Ejercicio 2: Cree un trabajo de Machine Learning automatizado (Automated ML)**

1.  Desde Azure Machine Learning Studio, haga clic en **Automated ML**
    en la sección **Author** del panel izquierdo.

2.  Seleccione **+ New Automated ML job.**

![](./media/image4.png)

### **Tarea 1: Cree un recurso de datos**

1.  Dé al experimento el nombre +++ **experiment_forecast** +++, acepte
    los demás valores predeterminados y seleccione **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

2.  Seleccione **Select task type** como **Time series forecasting** y
    luego haga cic en**+ Create.**

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  En la página **Create data asset**, facilite los siguientes datos.

    1.  Nombre – +++**bikedata**+++

    2.  Tipo – Tabular

> Haga clic en **Next**.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)

4.  En el panel **Data source**, seleccione **From local files** y haga
    clic en **Next**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  En **Destination storage type**, seleccione el workspaceblob y haga
    clic en **Next.**

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  En la selección **File or folder**, seleccione **Upload files** y
    seleccione **bike-no.csv** desde la carpeta **C:\Labfiles** y haga
    clic en **Next**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  Verifique que el formulario de **Settings and preview** esté
    completado de la siguiente manera y seleccione **Next**.

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  El formulario de **Schema** permite configurar adicionalmente los
    datos para este experimento. Para este ejemplo, seleccione **el
    toggle** y asegúrese de que esté en el estado apagado para:

    1.  **casual** y

    2.  **Registered** columns.

> Haga clic en **Next**.

Estas columnas son un desglose de la columna **cnt**, por lo que no se
incluyen.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

9.  En el formulario de **Review**, verifique la información y haga clic
    en **Create** para completar la creación de su recurso de datos.

> ![](./media/image13.png)

10. En la página **Create a new Automated ML job**, se mostrará un
    mensaje de **éxito** tras la creación del recurso de datos.

11. Seleccione el nuevo recurso de datos **bikedata** y haga clic en
    **Next**.

> **Nota:** **Actualice** el panel del recurso de datos si bikedata no
> se muestra.
>
> ![](./media/image14.png)

### **Tarea 2: Configure el trabajo**

1.  En la página de **Task settings**, proporcione los siguientes
    detalles y seleccione **View additional configuration settings**.

> Target column – **cnt(Integer)**
>
> Time column **– date (Date)**
>
> Desmarque la opción **Autodetect forecast horizon** y proporcione el
> valor como +++**14**+++.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

2.  En el panel de configuración adicional, proporcione los siguientes
    detalles y haga clic en **Save**.

- Primary metric - **Normalized root mean squared error**

- Explain best model – **Habilítelo**

- Blocked algorithms - **Extreme Random Trees**

> Expanda **Additional forecasting settings**

- Autodetect Forecast target lags – **No lo seleccione**

- Autodetect Target rolling window size – **No lo seleccione**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3.  Seleccione **Limits** e ingrese +++**60**+++ en el campo de
    **Experiment timeout(minutes)**.

![A screenshot of a test AI-generated content may be
incorrect.](./media/image17.png)

4.  Seleccione los siguientes valores en la sección **Validate and
    test** y luego seleccione **Next.**

> Validation type – **k-fold cross-validation**
>
> Number of cross validations – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

5.  Seleccione **automl-compute** (el que creamos en el laboratorio
    anterior). Haga clic en **Next.**

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

6.  Revise los detalles y seleccione **Submit training job**.

> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

7.  La página de estado muestra el estado inicial como **Running**. Siga
    actualizando la página para conocer el estado.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8.  Una vez que la capacitación se complete, el estado cambiará a
    **Completed**.

**Nota:** El entrenamiento toma aproximadamente entre 30 y 45 minutos
para completarse.

## **Ejercicio 3: Explore los modelos**

1.  Navegue a la pestaña **Models** para ver los algoritmos (modelos)
    que se han probado. Por defecto, los modelos se ordenan según la
    puntuación de la métrica a medida que se completan.

2.  Para este tutorial, el modelo que obtiene la puntuación más alta
    según la métrica seleccionada **Normalized Root Mean Squared Error**
    estará en la parte superior de la lista.

3.  Mientras espera que todos los modelos del experimento finalicen,
    seleccione el **Algorithm name** de un modelo completado para
    explorar los detalles de su rendimiento.

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

4.  Haga clic en **Overview** y vea los detalles del modelo.

> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

5.  Haga clic en la pestaña **Metrics** y explore los detalles.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image24.png)
>
> **Importante:** Continúe ejecutando el siguiente laboratorio mientras
> se completa este entrenamiento. Regrese a este laboratorio desde aquí
> una vez que el entrenamiento haya finalizado.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image25.png)

## **Ejercicio 4: Identifique el mejor modelo**

El Aprendizaje Automático Automatizado en Azure Machine Learning Studio
le permite implementar el mejor modelo como un servicio web en unos
pocos pasos. La implementación es la integración del modelo para que
pueda predecir sobre nuevos datos e identificar áreas potenciales de
oportunidad.

1.  Una vez que el trabajo esté completo, regrese a la página del
    trabajo principal seleccionando el **job name** en la parte superior
    de la pantalla.

![](./media/image26.png)

2.  En la sección **Best model summary**, el mejor modelo en el contexto
    de este experimento se selecciona en función de la métrica
    **Normalized root mean squared error.**

![A screenshot of a computer Description automatically
generated](./media/image27.png)

3.  Haga clic en el nombre del algoritmo para abrirlo y explorar los
    detalles.

4.  El modelo también puede desplegarse como un servicio web.

**Resumen**

En este laboratorio, se utilizó Automated ML en Azure Machine Learning
Studio para crear un modelo de pronóstico de series temporales que
predice la demanda de alquiler de bicicletas compartidas.
