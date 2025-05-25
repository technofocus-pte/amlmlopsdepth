# Labortorio 01- Preparar el dataset, entrenar e implementar un modelo de clasificación utilizando Azure Machine Learning Studio.

**Objetivo**

Este laboratorio se centra en guiarlo a través del proceso de
configuración de un entorno de Azure Machine Learning, carga, acceso y
exploración de datos, así como el entrenamiento y la implementación de
un modelo de clasificación de imágenes utilizando Azure Machine Learning
Studio.

Tiempo estimado - 45 minutos

## Ejercicio 1: Configuración del espacio de trabajo de Azure Machine Learning

### Tarea 1: Sincronice el reloj de la máquina virtual (VM)

1.  Después de iniciar sesión en la VM, haga clic con el botón derecho
    del ratón en el reloj de la esquina inferior derecha de la pantalla.

2.  Seleccione **Adjust date and time.**

&nbsp;

3.  En la pantalla de configuración que se abre, haga clic en **Sync
    now** en Additional settings. ![A screenshot of a computer
    Description automatically generated](./media/image1.png)

4.  Esto se encarga de sincronizar la hora en caso de que la
    sincronización automática no funcione.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image2.png)

### Tarea 2: Preparación de los recursos de Azure

Esta tarea se centra en la creación de un espacio de trabajo de Azure
Machine Learning. Se mostrará cómo configurar un espacio dedicado para
organizar y gestionar proyectos de aprendizaje automático de manera
eficaz. Este espacio de trabajo funciona como un hub central para la
colaboración, la experimentación y la implementación.

#### Tarea 2.1: Registre los proveedores de recursos necesarios

1.  Navegue hasta la **suscripción** asignada desde la página de inicio
    del portal Azure.

2.  Seleccione Resource Providers en **Settings** en el panel izquierdo.

3.  Busque +++Microsoft.StreamAnalytics+++ y seleccione los tres puntos
    junto al nombre y haga clic en **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  Repita los pasos para registrar +++Microsoft.Cdn+++ y
    +++Microsoft.PolicyInsights+++.

#### Tarea 2.2: Cree un espacio de trabajo de Azure Machine Learning

1.  Inicie sesión en el portal Azure en +++https://portal.azure.com+++
    utilizando el **Username** y la **Password** de la pestaña
    **Resources**.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

2.  En la página de inicio del portal Azure, seleccione **+ Create a
    resource**.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

3.  En la página **Create a resource**, utilice la barra de búsqueda
    para encontrar +++**Azure Machine Learning**+++ y seleccione **Azure
    Machine Learning.**

> ![A screenshot of a computer Description automatically
> generated](./media/image6.png)

4.  En **Marketplace**, haga clic en **Create dropdown** y seleccione
    **Azure Machine Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

5.  Proporcione la siguiente información para configurar su nuevo
    espacio de trabajo y haga clic en **Review + create**.

    - **Subscription**: Seleccione el **Azure subscription** que se le
      ha asignado.

    - **Resource group**: Seleccione el **Resource Group** que se le ha
      asignado.

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: Seleccione su región más cercana **(North Central US**
  está seleccionado aquí)

&nbsp;

- **Container registry:** Seleccione **Create new.** Ingrese
  **+++azuremlcr@lab.LabInstance.Id+++**

**Nota:** La cifra que se adjunta a los nombres de los recursos
corresponde a su ID de instancia del laboratorio para garantizar la
unicidad. Las capturas de pantalla tendrán un número diferente, ya que
son únicas.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  Una vez que se haya superado la validación, haga clic en **Create**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  Haga clic en **Go to resource**, para ver el nuevo espacio de
    trabajo.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  En la página de **Microsoft.MachineLearningServices | Overview**,
    seleccione **Launch studio** bajo **Work with your model in Azure
    Machine Learning studio.**

![A screenshot of a software update Description automatically
generated](./media/image12.png)

#### Tarea 2.3: Cree un recurso de cómputo

*Esta tarea demuestra la creación de un recurso de cómputo en Azure.
Explorará diferentes opciones de cómputo, como máquinas virtuales o
clústeres de cómputo gestionados, y comprenderá cómo configurar y
aprovisionar recursos para ejecutar cargas de trabajo de aprendizaje
automático de manera eficiente*.

1.  Una vez abierto **Azure Machine Learning Studio**, haga clic en
    **Compute** bajo **Manage** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

2.  Haga clic en **+ New** en la pantalla **Compute instances.**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  En la pantalla Crear instancia de cálculo, ingrese los siguientes
    datos

    1.  Compute name – +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Seleccione **Standard_E4ds_v4**

> Click on **Review + Create**.

**Nota:** Tome nota de este nombre de cómputo para su uso posterior.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

4.  Haga clic en **Create** en la siguiente pantalla.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

**Nota:** El recurso de cómputo tarda aproximadamente 10 minutos en
llegar al estado de ejecución.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

**Importante:** Una vez que el recurso de cómputo esté en ejecución,
podrá continuar con las siguientes tareas. Sin embargo, si está tomando
un descanso de la ejecución del laboratorio, asegúrese de **detener** la
instancia de cómputo y volver a iniciarla cuando retome después del
descanso.

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

**Resumen del ejercicio:**

El ejercicio familiariza a los participantes con los pasos esenciales
para configurar un entorno de Azure Machine Learning. A través de la
serie de tareas, los participantes han aprendido a crear una cuenta de
almacenamiento, instalar el SDK de Machine Learning, iniciar sesión
utilizando Azure CLI, crear un espacio de trabajo de Azure Machine
Learning y configurar un recurso de cómputo. Al completar este
ejercicio, se ha adquirido el conocimiento fundamental y las habilidades
prácticas necesarias para establecer un entorno funcional de Azure
Machine Learning, lo que le permite comenzar sus proyectos de
aprendizaje automático con confianza.

## Ejercicio 2 – Cargar, acceder y explorar sus datos en Azure Machine Learning

**Objetivos**

En este ejercicio, aprenderá a:

- Cargar sus datos en el almacenamiento en la nube.

- Crear un recurso de datos de Azure Machine Learning.

- Acceder a sus datos en un notebook para el desarrollo interactivo.

- Crear nuevas versiones de los recursos de datos.

El inicio de un proyecto de aprendizaje automático típicamente involucra
análisis exploratorio de datos (EDA), preprocesamiento de datos
(limpieza, ingeniería de características) y la construcción de
prototipos de modelos de aprendizaje automático para validar hipótesis.
Esta fase de prototipado del proyecto es altamente interactiva. Se
presta al desarrollo en un IDE o un notebook de Jupyter, con una consola
interactiva de Python. Este laboratorio describe estas ideas.

Nos encontramos en la etapa de **Datos: Explorar y preparar** dentro del
**flujo de trabajo del proyecto de aprendizaje automático.**

![](./media/image19.png)

### Tarea 1: Prepare los recursos de Azure

**Importante:** Asegúrese de que el recurso de cómputo que creamos en el
ejercicio anterior esté en ejecución. Si está tomando un descanso de la
ejecución del laboratorio, asegúrese de **detenerlo** y volver a
iniciarlo cuando retome después del descanso.

#### Tarea 1.1: Cargue un Notebook

1.  Desde el Azure Machine Learning Studio, una vez que el recurso de
    cómputo esté en ejecución, seleccione la opción **Notebooks** en el
    panel izquierdo.  
    ![](./media/image20.png)

2.  Cierre el cuadro de diálogo **What’s new in Notebooks**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image21.png)

3.  El panel de archivos del Notebook se abre con la estructura, **Users
    -\> \< UserName \>**. Haga clic en los tres puntos junto al nombre
    de usuario y seleccione **Create new folder**.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

4.  Ingrese el nombre de la carpeta como +++**Azuremlnotebooks**+++ y
    haga clic en **Create.**

![A screenshot of a computer Description automatically
generated](./media/image23.png)

5.  Una vez que la carpeta esté creada, haga clic en las **opciones del
    menú** (los tres puntos junto al nombre de la carpeta) de la carpeta
    **Azuremlnotebooks** y seleccione **Upload files**.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

6.  Seleccione **Click to browse and select file(s).** Navegue hasta
    **explore-data.ipynb** en **C:\Labfiles** y haga clic en **Open**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image25.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image26.png)

7.  Seleccione la casilla de verificación, **Open file after upload** y
    **I trust the contents of this file.** Luego haga clic en
    **Upload.**

![A screenshot of a computer Description automatically
generated](./media/image27.png)

8.  Esto abre el Notebook cargado.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

9.  Haga clic en **Authenticate** si el estudio le solicita
    autenticarse, ya que es la primera vez que inicia sesión en el
    estudio.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image29.png)

### Tarea 2: Cargue, acceda y explore sus datos. 

#### Tarea 2.1: Descargue los datos 

1.  En el panel **Files** de **Notebooks**, haga clic en los 3 puntos
    junto al nombre de la carpeta **Azuremlnotebooks** y haga clic en
    **Create new folder**.

![](./media/image30.png)

2.  Escriba el nombre de la carpeta como +++**data**+++ y haga clic en
    **Create**.

![](./media/image31.png)

3.  Una vez que la creación de la carpeta se haya realizado
    correctamente, haga clic en las opciones de menú de los **datos** de
    la carpeta y seleccione **Upload files.**

![A screenshot of a computer Description automatically
generated](./media/image32.png)

4.  Seleccione **Click to browse and select file(s)** y navegue hasta
    **C:\Labfiles** para seleccionar el archivo
    **default_of_credit_card_clients.csv**. Luego haga clic en **Open**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image33.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image34.png)

5.  Un mensaje que indica **File uploaded successfully** se muestra bajo
    las notificaciones una vez que finaliza la carga.

![A close-up of a computer screen Description automatically generated
with low confidence](./media/image35.png)

#### Tarea 2.2: Cree un identificador para el espacio de trabajo

1.  Regrese al notebook (**explore-data**).

2.  Antes de adentrarnos en el código, necesita una forma de hacer
    referencia a su espacio de trabajo. Creará ml_client como un
    identificador para el espacio de trabajo. Luego utilizará ml_client
    para gestionar recursos y trabajos.

3.  En la primera celda debajo de **Create handle to workspace**,
    reemplace los marcadores de posición de \<**SUBSCRIPTION_ID\>,
    \<RESOURCE_GROUP\>** y **\<AML_WORKSPACE_NAME\>.**

4.  Reemplace \< RESOURCE_GROUP\> con el nombre del grupo de recursos
    asignado.

5.  Reemplace \<AML_WORKSPACE_NAME\> con
    [+++**Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  Reemplace \< SUBSCRIPTION_ID \> con el
    +++**@lab.CloudSubscription.Id**+++.

7.  Haga clic en el botón **Run cell** disponible en la parte superior
    izquierda de la celda. Busque una marca de verificación en la parte
    inferior de la celda una vez que la ejecución sea exitosa.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

#### Tarea 2.3: Cargue datos al almacenamiento en la nube

1.  Un recurso de datos de Azure Machine Learning es similar a los
    marcadores (favoritos) en un navegador web. En lugar de recordar
    largas rutas de almacenamiento (URI) que apuntan a los datos más
    utilizados, puede crear un recurso de datos y luego acceder a él con
    un nombre amigable.

2.  La siguiente celda del notebook crea el recurso de datos. El ejemplo
    de código carga el archivo de datos sin procesar al recurso de
    almacenamiento en la nube designado.

3.  Cada vez que crea un recurso de datos, necesita una versión única
    para él. Si la versión ya existe, se generará un error. En este
    código, estamos utilizando el tiempo para generar una versión única
    cada vez que se ejecuta la celda.

4.  Ejecute la siguiente celda haciendo clic en el botón **Run cell**
    ubicado en la parte superior izquierda de la celda.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

5.  **“Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx”** es el resultado que se muestra debajo de la
    celda.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image38.png)

6.  Haga clic en **Data** en el panel izquierdo y seleccione el recurso
    de datos **credit-card** que se creó con la ejecución realizada en
    el paso anterior. Explore los detalles y luego regrese al panel de
    **Notebooks**.

![](./media/image39.png)

#### Tarea 2.4: Acceda a sus datos en un notebook

1.  Regrese al notebook y ejecute la celda con el comando **%pip** para
    instalar la biblioteca Python **azureml-fsspec** en su kernel de
    **Jupyter**.

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image40.png)

2.  Ejecute la siguiente celda para acceder al archivo CSV en
    **Pandas**.

3.  Obtendrá el **Data asset URI** impreso en la parte inferior de la
    celda y los datos también se mostrarán.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image41.png)

#### Tarea 2.5: Cree una nueva versión del recurso de datos

> 1\. Puede que haya notado que los datos necesitan una pequeña limpieza
> para que estén listos para entrenar un modelo de aprendizaje
> automático. Estos presentan:
>
> a\. Dos encabezados
>
> b\. Una columna de ID de cliente; no usaríamos esta característica en
> el aprendizaje automático
>
> c\. Espacios en el nombre de la variable de respuesta

2.  Además, en comparación con el formato CSV, el formato de archivo
    **Parquet** es una mejor opción para almacenar estos datos. Parquet
    ofrece compresión y mantiene el esquema. Por lo tanto, para limpiar
    los datos y almacenarlos en Parquet, ejecute la siguiente celda.

3.  Asegúrese de que la ejecución sea exitosa observando la marca de
    verificación en la parte inferior de la celda.

![](./media/image42.png)

4.  Esta tabla muestra la estructura de los datos en el archivo original
    **default_of_credit_card_clients.csv** descargado en un paso
    anterior. Los datos cargados contienen 23 variables explicativas y 1
    variable de respuesta, como se muestra aquí:

[TABLE]

5.  Ejecute la siguiente celda para crear una nueva versión del recurso
    de datos (los datos se cargarán automáticamente en el almacenamiento
    en la nube).

6.  Tras una ejecución exitosa, aparecerá un resultado que indica:
    **Data asset created. Name: credit_card, version:
    YYYY.MM.DD.xxxxxx_cleaned** después de la celda.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image43.png)

![A screenshot of a computer Description automatically generated with
low confidence](./media/image44.png)

**Importante:**

Esta celda de código en Python establece los valores de **nombre** y
**versión** para el recurso de datos que crea. Como resultado, el código
en esta celda fallará si se ejecuta más de una vez, sin un cambio en
estos valores. Los valores fijos para el **nombre** y la **versión**
ofrecen una forma de pasar valores que funcionan para situaciones
específicas, sin preocuparse por los valores generados automáticamente o
aleatoriamente.

7.  El archivo Parquet limpio es la última versión del recurso de datos.
    El código en la siguiente celda muestra primero el conjunto de
    resultados de la versión CSV, luego la versión Parquet al
    ejecutarse.

8.  Ejecute la siguiente celda y verifique el resultado debajo.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image45.png)

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image46.png)

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image47.png)

![A picture containing text, screenshot, number, display Description
automatically generated](./media/image48.png)

9.  Busque los datos depurados en **Data**.

> ![](./media/image49.png)

**Importante:** Puede continuar con el siguiente ejercicio desde este
punto. Sin embargo, si se interrumpe la ejecución del laboratorio, se
recomienda **detener** la instancia de cómputo y volver a iniciarla al
reanudar la actividad.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image50.png)

**Resumen del ejercicio**

En este ejercicio, se ha aprendido a cargar datos en el almacenamiento
en la nube, crear un recurso de datos en Azure Machine Learning, acceder
a los datos desde un notebook para desarrollo interactivo y crear nuevas
versiones de recursos de datos.

## Ejercicio 3 – Entrene y desplegue un modelo de clasificación de imágenes en Azure Machine Learning studio

**Objetivo**

En este ejercicio, se aprenderá a:

1.  Conectarse al espacio de trabajo y configurar un recurso de cómputo
    mediante la interfaz de Notebooks en Azure Machine Learning Studio.

2.  Importar los datos y prepararlos para el entrenamiento.

3.  Entrenar un modelo para clasificación de imágenes.

4.  Visualizar y analizar las métricas para optimizar el modelo.

5.  Desplegar el modelo en línea y probarlo.

Nos encontramos en la etapa de **Train & validate model** dentro de
**Machine Learning project workflow**.![A picture containing text, font,
number, screenshot Description automatically
generated](./media/image51.png)Tarea 1: Cargue el notebook

1.  En la página de **Notebooks** de Azure Machine Learning Studio, haga
    clic en las opciones del menú de la carpeta **AzureMLnotebooks** y
    seleccione **Upload files.**

> ![A screenshot of a computer Description automatically
> generated](./media/image52.png)

2.  Seleccione **Click to browse and select file(s)**, navegue a
    **C:\Labfiles** y seleccione el archivo
    **azureml-getting-started-studio** (un archivo fuente de Jupyter).

> ![A screenshot of a computer screen Description automatically
> generated with medium confidence](./media/image53.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

3.  Seleccione la casilla **Open file after upload** y, a continuación,
    haga clic en **Upload**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image55.png)

4.  Una vez que el archivo se ha cargado correctamente, se abre en el
    Studio y se conecta automáticamente al recurso Compute
    (cpu-cluster-fs) que se encuentra en estado **Running**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

### Tarea 2: Conecte al espacio de trabajo de Azure Machine Learning

Antes de sumergirnos en el código, será necesario conectarse a su
espacio de trabajo. El espacio de trabajo es el recurso de nivel
superior en Azure Machine Learning, proporcionando un lugar centralizado
para trabajar con todos los artefactos que se crean al utilizar Azure
Machine Learning.

Estamos utilizando **DefaultAzureCredential** para obtener acceso al
espacio de trabajo. **DefaultAzureCredential** debería ser capaz de
manejar la mayoría de los escenarios.

*\# Handle to the workspace*

**from** azure.ai.ml **import** MLClient

*\# Authentication package*

**from** azure.identity **import** DefaultAzureCredential

credential **=** DefaultAzureCredential()

*\# Get a handle to the workspace. You can find the info on the
workspace tab on ml.azure.com*

ml_client **=** MLClient(

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  En el código anterior (primer celda del notebook), reemplace los
    valores de los marcadores de posición **SUBSCRIPTION_ID,
    RESOURCE_GROUP** y **AML_WORKSPACE_NAME** con los valores que
    guardamos en el ejercicio anterior.

2.  Su primera celda en el notebook debe verse ahora de esta manera.
    Haga clic en el botón **Run** en la parte superior izquierda de la
    primera celda.

![A screenshot of a computer Description automatically
generated](./media/image57.png)

1.  Asegúrese de que la celda se haya ejecutado correctamente al
    verificar su estado en la parte inferior de la celda.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image58.png)

In \[ \]:

### Tarea 3: Suba los datos

Para ejecutar un trabajo de entrenamiento en Azure Machine Learning,
necesitará un entorno.

En este laboratorio, utilizará un entorno preconfigurado llamado
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest, que contiene todas las
bibliotecas necesarias (Python, MLflow, numpy, pip, etc.).

1.  Ejecute el código en la siguiente celda para cargar los datos.

2.  Asegúrese de que se muestre un mensaje que indique **Data asset
    created** como salida de la celda.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image59.png)

###  Tarea 4: Cree el trabajo de comando para entrenar

Ahora que tiene todos los recursos necesarios para ejecutar su trabajo,
es hora de construir el trabajo utilizando el SDK de Python v2 de Azure
ML. Vamos a crear un command job.

Un AzureML command job es un recurso que especifica todos los detalles
necesarios para ejecutar su código de entrenamiento en la nube: entradas
y salidas, el tipo de hardware a utilizar, el software a instalar y cómo
ejecutar su código. El command job contiene la información necesaria
para ejecutar un solo comando.

#### Task 4.1 : Cree un script de entrenamiento

1.  Comencemos creando el script de entrenamiento: el archivo
    **main.py** de Python.

2.  Ejecute la siguiente celda y asegúrese de que se ejecuta
    correctamente.

![A picture containing text, font, line, screenshot Description
automatically generated](./media/image60.png)

3.  El script en la siguiente celda maneja el preprocesamiento de los
    datos, dividiéndolos en datos de prueba y de entrenamiento. Luego,
    utiliza estos datos para entrenar un modelo basado en árboles y
    devolver el modelo de salida.
    [MLFlow](https://mlflow.org/docs/latest/tracking.html) se utilizará
    para registrar los parámetros y métricas durante la ejecución de
    nuestro pipeline.

4.  Ejecute la celda y asegúrese de que se ejecuta correctamente con la
    salida,

**Writing ./src/main.py**

> ![A screenshot of a computer program Description automatically
> generated with low confidence](./media/image61.png)
>
> ![A screenshot of a computer program Description automatically
> generated with medium confidence](./media/image62.png)

5.  Como puede ver en este script, una vez que el modelo está entrenado,
    el archivo del modelo se guarda y se registra en el espacio de
    trabajo. Ahora puede utilizar el modelo registrado en los endpoints
    de inferencia.

#### Tarea 4.2: Configure el comando

Ahora que tiene un script que puede realizar las tareas deseadas,
utilizará el comando de propósito general que puede ejecutar acciones de
línea de comandos. Esta acción de línea de comandos puede ser una
llamada directa a comandos del sistema o ejecutar un script.

1.  Aquí, utilizará los datos de entrada, la proporción de división, la
    tasa de aprendizaje y el nombre del modelo registrado como variables
    de entrada.

2.  Desde el panel izquierdo, seleccione **Data** y luego seleccione
    **credit-card-data.**

![A screenshot of a computer Description automatically
generated](./media/image63.png)

3.  En la sección de **Data sources**, busque el valor de **Datastore
    URI** y cópielo. Guárdelo para usarlo en el siguiente paso.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image64.png)

4.  En la siguiente celda, reemplace lo siguiente:

> a\. El valor de **path** con el **Datastore URI** guardado en el paso
> anterior.
>
> b\. El valor de **compute** con
> +++**cpu-cluster-fs@lab.LabInstance.Id**+++ (el nombre del clúster que
> guardamos en el Lab 1).

5.  Haga clic en **Run**. Asegúrese de que la celda se ejecute
    correctamente.

![A screenshot of a computer program Description automatically
generated](./media/image65.png)

### Tarea 6: Present eel trabajo

Ahora es el momento de enviar el trabajo para que se ejecute en AzureML.
**El trabajo tardará de 2 a 3 minutos en ejecutarse**. Podría tardar más
(hasta 10 minutos) si la instancia de cómputo ha sido reducida a cero
nodos y el entorno personalizado aún se está construyendo.

1.  Ejecute la celda con el siguiente comando para enviar el trabajo.

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  Haga clic en **Run**. Asegúrese de que la ejecución sea exitosa y
    que haya un enlace a los resultados en la columna **Details Page.**

**Nota**: Tardará unos 2 minutos en completarlo.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image66.png)

3.  Abra el enlace disponible en la columna **Details Page** del
    resultado, en una nueva pestaña.

### Tarea 7: Revise los resultados del trabajo de entrenamiento

1.  Puede ver el resultado de un trabajo de formación de la siguiente
    manera **clicking the URL generated after submitting a job**.

> ![A screenshot of a computer Description automatically
> generated](./media/image67.png)

2.  Alternativamente, también puede hacer clic en **Jobs** en el menú de
    navegación izquierdo. Un trabajo es un grupo de muchas ejecuciones
    de un script o fragmento de código especificado. La información de
    la ejecución se guarda bajo ese trabajo.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image68.png)

3.  La página de **Overview** muestra primero el **estado** en el panel
    de **propiedades** como **Running**.

4.  El estado cambia a **Completed** una vez que está listo.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image69.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image70.png)

5.  Seleccione el panel **Metrics** para ver las métricas.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image71.png)

6.  Seleccione la pestaña **Images** para ver la matriz de confusión del
    entrenamiento, la curva de precisión-recall y la curva ROC.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image72.png)

1)  **Resumen** es donde puede ver el estado del trabajo**.**

2)  **Métricas** muestra diferentes visualizaciones de las métricas que
    especificó en el script.

3)  **Imágenes** es donde puede ver cualquier artefacto de imagen que
    haya registrado con MLflow.

4)  **Trabajos secundarios** contiene trabajos secundarios si los
    agregó**.**

5)  **Salidas + registros** contiene los archivos de registro necesarios
    para la solución de problemas u otros fines de monitoreo.

6)  **Código** contiene el script/código utilizado en el trabajo**.**

7)  **Explicaciones y equidad** se utilizan para ver cómo se desempeña
    su modelo frente a los estándares de IA responsable. Son
    características en vista previa y requieren instalaciones
    adicionales de paquetes.

8)  **Monitoreo** es donde puede ver las métricas para el rendimiento de
    los recursos de cómputo.

### Tarea 8: Implemente el modelo como un endpoint en línea

Después de entrenar un modelo de machine learning, es necesario
**implementarlo** para que otros puedan usarlo para inferencias. Para
este propósito, Azure Machine Learning permite crear endpoints y agregar
implementaciones a ellos.  
Un endpoint, en este contexto, es una ruta HTTPS que proporciona una
interfaz para que los clientes envíen solicitudes (datos de entrada) a
un modelo entrenado y reciban los resultados de inferencia
(calificación) del modelo. Un endpoint proporciona:  
  
• Autenticación mediante autenticación basada en "clave o token"  
• Terminación TLS(SSL)  
• Una URI de calificación estable
(endpoint-name.region.inference.ml.azure.com)  
Una implementación es un conjunto de recursos necesarios para alojar el
modelo que realiza la inferencia.

#### Tarea 8.1: Cree un endpoint en línea

1.  Ahora implemente su modelo de aprendizaje automático como un
    servicio web en la nube de Azure, un endpoint en línea.

2.  Seleccione **Endpoints** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image73.png)

3.  Seleccione **Create** para Real-time endpoints.

![A screenshot of a computer Description automatically
generated](./media/image74.png)

4.  Seleccione **credit_defaults_model** y luego haga clic en
    **Select.**

![A screenshot of a computer Description automatically
generated](./media/image75.png)

5.  Seleccione **Standard_E4s_v3** en la máquina virtual. Indique el
    número de instancias como **1.**

> Acepte los demás valores predeterminados de un **Endpoint name** único
> y el **Endpoint name**, y luego seleccione **Deploy**.

![A screenshot of a computer Description automatically
generated](./media/image76.png)

**Nota:** La creación del endpoint tarda unos 20 minutos.

6.  Una vez completado, el estado de Aprovisionamiento cambia a
    **Succeeded**.

![A screenshot of a computer Description automatically
generated](./media/image77.png)

#### Tarea 8.2: Pruebe con una consulta de ejemplo

1.  En la página del endpoint, seleccione la pestaña **Test**.

2.  Copie y pegue el siguiente archivo de solicitud de ejemplo en el
    campo **Input data to test real-time endpoint** sustituyendo el
    código ya presente en él.

> **{**
>
> **"input_data": {**
>
> **"columns":
> \[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22\],**
>
> **"index": \[0, 1\],**
>
> **"data": \[**
>
> **\[20000,2,2,1,24,2,2,-1,-1,-2,-2,3913,3102,689,0,0,0,0,689,0,0,0,0\],**
>
> **\[10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10,
> 9, 8\]**
>
> **\]**
>
> **}**
>
> **}**

3.  Seleccione **Test** y vea el resultado en **Test result**.

> ![A screenshot of a computer Description automatically
> generated](./media/image78.png)

### Tarea 9: Elimine el Endpoint

1.  Desde el panel izquierdo, seleccione los **Endpoints**. Seleccione
    el endpoint que creamos y haga clic en **Delete.**

![A screenshot of a computer Description automatically
generated](./media/image79.png)

2.  Haga clic en **Delete** en el cuadro de diálogo de confirmación.

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image80.png)

3.  Busque una notificación sobre la eliminación correcta.

![A picture containing text, screenshot, font, line Description
automatically generated](./media/image81.png)

**Resumen**

En este laboratorio, ha aprendido a entrenar un modelo de clasificación
de imágenes en Azure Machine Learning Studio y a implementarlo como un
servicio web.
