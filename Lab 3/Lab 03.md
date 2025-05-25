# Laboratorio 03 - Desarrollar y registrar un conjunto de características con un almacén de características gestionado y entrenar modelos utilizando características

Este laboratorio describe cómo crear una especificación de conjunto de
características con transformaciones personalizadas. Posteriormente, se
utiliza dicho conjunto para generar datos de entrenamiento, habilitar la
materialización y realizar un reabastecimiento. La materialización
calcula los valores de las características para una ventana de
características y los almacena en un almacén de materialización. Todas
las consultas de características pueden utilizar esos valores
almacenados.  
Sin materialización, una consulta de conjunto de características aplica
las transformaciones al código fuente en tiempo real para calcular las
características antes de devolver los valores. Este proceso es adecuado
en la fase de prototipado. Sin embargo, para operaciones de
entrenamiento e inferencia en un entorno de producción, se recomienda
materializar las características para garantizar mayor fiabilidad y
disponibilidad.

Tiempo estimado – 50 minutos

## Ejercicio 1: Asigne los roles requeridos:

1.  En la página principal del portal de Azure, seleccione el **Resource
    group** asignado en la pestaña **Resources**. En el panel izquierdo,
    seleccione **Access control(IAM)**. Haga clic en el menú desplegable
    junto a **Add** y seleccione **Add role assignment**.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  Busque +++ **AzureML Data Scientist** +++ y selecciónelo. Haga clic
    en **Next.**

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  En la pestaña Members, haga clic en **+ Select members**, busque su
    **User name,** +++@ lab.CloudPortalCredential (User1).Username+++.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  Seleccione su **Username** y luego haga clic en el botón **Select.**

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  Haga clic en **Review + assign** en las siguientes 2 pantallas.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  El mensaje de asignación de rol agregado se obtiene una vez
    realizada la asignación.

7.  Repita el mismo conjunto de pasos para agregar los roles
    +++**Storage Blob Data Reader**+++ y +++**Storage Blob Data
    Contributor**+++.

## Ejercicio 2: Desarrolle un conjunto de características y regístrelo en el almacén de características administrado

Este tutorial es la primera parte de la serie sobre el almacén de
características administrado. En él, aprenderá a:  
  
• Crear un nuevo recurso de almacén de características mínimas.  
• Desarrollar y probar localmente un conjunto de características con
capacidad de transformación.  
• Registrar una entidad de almacén de características en el almacén de
características.  
• Registrar el conjunto de características desarrollado en el almacén de
características.  
• Generar un DataFrame de entrenamiento de muestra utilizando las
características creadas.  
• Habilitar la materialización sin conexión en los conjuntos de
características y completar los datos de características.

### Tarea 1: Prepare el entorno

1.  En el panel izquierdo de Azure Machine Learning Studio, seleccione
    **Notebooks** en **Authoring**. Haga clic en los tres puntos junto
    al nombre de usuario y seleccione **Upload folder**.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2.  Busque y seleccione la carpeta **featuresstore** en **C:\Labfiles**
    y haga clic en **Upload**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  Vaya a **featurestore-\> notebooks-\>sdk_and_cli** y abra el
    notebook 1.Develop-feature-set-and-register.ipynb

![](./media/image8.png)

4.  Seleccione **Serverless Spark Compute** en **Compute**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5.  Seleccione **Configure session** para configurar la sesión con los
    requisitos previos.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6.  Seleccione **Python packages -\> Upload Conda file**. Haga clic en
    **Browse** y seleccione **conda.yml** en **C:\Labfiles.** Luego,
    seleccione **Apply**.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

7.  **Ejecute** la primera celda del notebook. Esto instalará todas las
    **dependencias** y completará su ejecución. El proceso tomará
    aproximadamente **10 minutos**.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Una vez que se inicia la sesión de Spark, reemplace el **User name**
    con su nombre de usuario y ejecute la siguiente celda.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9.  Ejecute las siguientes 3 celdas para configurar Azure CLI.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10. En la siguiente celda, siga los pasos del **output** para iniciar
    sesión en **Azure** .

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### Tarea 2: Cree un almacén de características mínimas

1.  **Ejecute** la **primera** celda para establecer el nombre, la
    ubicación y otros valores para el almacén de características**.**

![A screenshot of a computer program Description automatically
generated](./media/image19.png)

2.  **Ejecute** la celda **creates the feature store**.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

3.  Ejecute la siguiente celda **initializes AzureML feature store core
    SDK client**.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

### Tarea 3: Prototipe y desarrolle un conjunto de funciones para la agregación continua de transacciones en este notebook.

1.  **Ejecute** la primera celda de esta sección para explorar los datos
    de origen **de las transacciones.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  Ejecute la segunda celda para **desarrollar un conjunto de
    características de transacciones** localmente.

![A screenshot of a computer code Description automatically
generated](./media/image23.png)

3.  Ejecute la siguiente celda para generar un **marco de datos Spark**
    a partir de la especificación del conjunto de características.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  Para registrar la especificación del conjunto de características con
    el almacén de características, debe guardarse en un formato
    específico. Inspeccione las transacciones generadas
    **FeaturesetSpec**: Abra este archivo desde el árbol de archivos
    para ver la especificación:
    **featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml**.  
    Ejecute la siguiente celda para exportarlo como especificación del
    conjunto de características.

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

### Tarea 4: Registre una entidad en el almacén de características

1.  La entidad facilita la implementación de la práctica recomendada de
    utilizar las mismas definiciones de clave de unión en todos los
    conjuntos de características que emplean las mismas entidades
    lógicas. Ejecute la celda para registrar una entidad en el almacén
    de características.

> ![A screen shot of a computer Description automatically
> generated](./media/image26.png)

### Tarea 5: Registre el conjunto de características de transacciones con el almacén de características

1.  Desde el portal de Azure(+++https://portal.azure.com+++), navegue
    hasta la **Storage account** que comienza con **featureset** en su
    grupo de recursos asignado**.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  En el panel izquierdo, seleccione Access Control(IAM). Seleccione
    **Add** -\> **Add role assignment**.

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  Busque y seleccione +++**Storage Blob Data Reader**+++.

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

4.  Complete la asignación de roles similar a las que hicimos en el
    ejercicio 1.

5.  De manera similar, agregue el rol +++**Storage Blob Data
    Contributor**+++.

6.  Regrese a Azure Machine Learning Studio.

7.  Registra un conjunto de características en el almacén de
    características para compartirlo y reutilizarlo con otros. También
    obtiene funciones administradas como el control de versiones y la
    materialización. El conjunto de características hace referencia a la
    especificación del conjunto de características que creó
    anteriormente y a propiedades adicionales como la configuración de
    la versión y la materialización.

8.  **Ejecute** la siguiente celda para **registrar el conjunto de
    características de transacción** con el almacén de características.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### Tarea 6: Explore la interfaz de usuario del almacén de características

1.  Abra una nueva pestaña en el navegador y navegue a la página de
    inicio global de Azure ML en +++https://ml.azure.com/home+++.

2.  Haga clic en **Feature stores** en la navegación izquierda.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

3.  Haga clic en **featurestore**.

**Nota:** La creación y actualización de activos del almacén de
características, como conjuntos de características y entidades, solo
puede realizarse mediante el SDK y la CLI. La interfaz de usuario está
disponible para la búsqueda y exploración del almacén de
características.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

### Tarea 7: Genere un marco de datos de entrenamiento utilizando las características registradas

1.  Se comienza explorando los datos de observación, que suelen
    constituir la base para los datos de entrenamiento e inferencia.
    Posteriormente, se combinan con los datos de características para
    generar el conjunto completo de entrenamiento. Los datos de
    observación corresponden a información capturada durante el evento;
    en este caso, incluyen datos clave de la transacción, como el ID de
    transacción, el ID de cuenta y el importe. Al estar destinados al
    entrenamiento, también incorporan la variable objetivo (*is_fraud).*

2.  **Ejecute** la celda y observe los datos de salida.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  **Ejecute** la siguiente celda para obtener el **conjunto de
    características registradas** y **enumerar sus características** .

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

4.  **Ejecute** la siguiente celda para **imprimir** los **valores de
    muestra** .

![A screenshot of a computer Description automatically
generated](./media/image35.png)

5.  **Ejecute** la siguiente celda. En este paso, seleccionaremos **las
    características** que queremos que formen parte de **los datos de
    entrenamiento** y usaremos el SDK del almacén de características
    para generarlos.

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

6.  Ejecute la siguiente celda para generar un marco de datos de
    entrenamiento utilizando datos de características y datos de
    observación.

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

### Tarea 8: Habilite la materialización sin conexión en el conjunto de características de transacciones

Una vez habilitada la materialización en un conjunto de características,
es posible realizar tareas de relleno o programar trabajos de
materialización de forma recurrente.

1.  Ejecute la siguiente celda para configurar
    spark.sql.shuffle.partitions en el archivo YAML, en función del
    tamaño de los datos de características.

2.  La configuración de Spark spark.sql.shuffle.partitions es un
    parámetro opcional que puede influir en la cantidad de archivos
    Parquet generados por día durante la materialización del conjunto de
    características en el almacén sin conexión. El valor predeterminado
    es 200. Se recomienda evitar la creación de múltiples archivos
    Parquet pequeños, ya que pueden ralentizar la recuperación de
    características sin conexión. En caso de bajo rendimiento, se
    sugiere revisar la carpeta correspondiente en el almacén para
    verificar si la causa es una cantidad excesiva de archivos pequeños,
    y ajustar este parámetro según sea necesario.

**Nota:** Dado que los datos utilizados en este notebook son de tamaño
reducido, el parámetro se establece en 1 en el archivo
featureset_asset_offline_enabled.yaml.

> ![A screenshot of a computer Description automatically
> generated](./media/image38.png)

3.  La materialización consiste en calcular los valores de las
    características para una ventana de tiempo específica y almacenarlos
    en un almacén de materialización. Este proceso incrementa la
    fiabilidad y disponibilidad de las características. Todas las
    consultas posteriores utilizarán los valores almacenados en dicho
    almacén.

> En este paso se realizará un **reabastecimiento único** para una
> ventana de características de 18 meses.

4.  La celda de código a continuación ejecutará la materialización de
    los datos cuyo estado actual sea **None** o **Incomplete** para la
    ventana definida. **Ejecútela** para iniciar el proceso.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

5.  **Imprima datos de muestra** del conjunto de características en la
    siguiente celda. Ejecútela.En la información de salida se puede
    observar que los datos se recuperaron del almacén de
    materialización.

> El método get_offline_features(), que se utiliza para recuperar datos
> de entrenamiento o inferencia, también usará por defecto el almacén de
> materialización.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## Ejercicio 3: Experimente y entrene modelos usando características

En este notebook aprenderá a:

- Prototipe una nueva especificación de conjunto de características para
  cuentas, utilizando valores precomputados existentes como
  características. Luego, registre la especificación local del conjunto
  de características como un conjunto de características en el almacén
  de características. Este proceso difiere del primer tutorial, donde se
  creó un conjunto con transformaciones personalizadas.

- Seleccione características para el modelo a partir de los conjuntos de
  características de transacciones y cuentas, y guárdelas como una
  especificación de recuperación de características.

- Ejecute una canalización de entrenamiento que utilice la
  especificación de recuperación de características para entrenar un
  nuevo modelo. Esta canalización emplea el componente integrado de
  recuperación de características para generar los datos de
  entrenamiento

### Tarea 1: Configure el entorno

1.  Desde el panel Notebooks, abra el notebook **Experiment and train
    models using features**.

2.  Haga clic en **Configure session** y cargue el archivo
    **conda.yaml**, de forma similar a como se realizó en el notebook
    anterior.

3.  **Ejecute** la **primera celda** para iniciar la sesión. Tardará
    unos 10 minutos.

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4.  En la siguiente celda, reemplace el marcador de posición
    **\<your_user_alias\>** con su **nombre de usuario** en la
    estructura de la carpeta y **ejecute** la celda.

![A screenshot of a computer program Description automatically
generated](./media/image42.png)

5.  **Ejecute** las siguientes **3** celdas para **configurar CLI** .

6.  La siguiente celda es initializes the project workspace variables.
    **Ejecútela** para **inicializar las variables** .

![A screenshot of a computer Description automatically
generated](./media/image43.png)

7.  La siguiente celda es initializes the feature store variables.
    Ejecútela.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

8.  Ejecute la siguiente celda **Initialize the feature store
    consumption client** para inicializar el cliente de consumo del
    almacén de características.

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

### Tarea 2: Cree un conjunto de características de cuentas localmente a partir de datos precalculados

Para incorporar características precalculadas, puede crear una
especificación de conjunto de características sin escribir código de
transformación. Esta especificación permite desarrollar y probar un
conjunto de características en un entorno de desarrollo completamente
local sin conectarse a ningún almacén de características . En este paso,
creará la especificación de conjunto de características localmente y
muestreará sus valores.

1.  Ejecute la siguiente celda para **explorar los datos de origen de
    las cuentas.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  Ejecute la siguiente celda para **crear la especificación del
    conjunto de características de cuentas** localmente a partir de
    estas características precomputadas.

![A screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3.  **Ejecute** la siguiente celda para generar un **marco de datos
    Spark** a partir de la especificación del conjunto de
    características.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  Para registrar la especificación del conjunto de características en
    el almacén de características, debe guardarse en un formato
    específico. Acción: Después de ejecutar la siguiente celda,
    inspeccione las cuentas generadas FeatureSetSpec : Abra este archivo
    desde el árbol de archivos para ver la especificación: featurestore
    / featuresets /accounts/spec/ FeatureSetSpec . **Ejecute** la
    siguiente celda.![A screenshot of a computer program Description
    automatically generated](./media/image50.png)

### Tarea 3: Experimente con funciones no registradas localmente y regístrese en la tienda de funciones cuando esté listo

Al desarrollar funciones, se recomienda realizar pruebas y validaciones
locales antes de registrarlas en el almacén de características o
ejecutar procesos de entrenamiento en la nube. En este paso, se
generarán datos de entrenamiento para el modelo de aprendizaje
automático combinando características de un conjunto local no registrado
(cuentas) con un conjunto registrado en el almacén de características
(transacciones).

1.  **Ejecute** la siguiente celda para **select features** for
    **model.**

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

2.  **Ejecute** las siguientes 2 celdas para **generar datos de
    entrenamiento** localmente.

![A close-up of a computer code Description automatically
generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

3.  **Ejecute** la siguiente celda para **registrar el conjunto de
    características de cuentas** en el almacén de características. Una
    vez que haya experimentado con diferentes definiciones de
    características localmente y las haya probado, podrá registrarlas en
    el almacén de características. Para ello, registrará una definición
    de activo del conjunto de características en el almacén de
    características.

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  **Ejecute** las siguientes 2 celdas para obtener el conjunto de
    características registrado y la prueba de cordura.

![A screenshot of a computer Description automatically
generated](./media/image55.png)

### Tarea 4: Ejecute el experimento de entrenamiento

1.  Ejecute la siguiente celda para descubrir características del SDK.

![A screenshot of a computer Description automatically
generated](./media/image56.png)

2.  En los pasos anteriores, seleccionó características de una
    combinación de conjuntos de características no registrados y
    registrados para experimentación y pruebas locales. Ahora está listo
    para experimentar en la nube. Guardar las características
    seleccionadas como una especificación de recuperación de
    características y utilizarla en el flujo de MLOps/CICD para
    entrenamiento/inferencia aumenta su agilidad en la entrega de
    modelos.

3.  **Ejecute** la siguiente celda para **seleccionar características
    para el modelo** .

![A screenshot of a computer program Description automatically
generated](./media/image57.png)

4.  **Ejecute** la siguiente celda y exporte las características
    seleccionadas como una **especificación de recuperación de
    características**.

![A screenshot of a computer program Description automatically
generated](./media/image58.png)

### Tarea 5: Entrene en la nube utilizando pipelines y registre el modelo si es satisfactorio

En este paso, activará manualmente el pipeline de entrenamiento. En un
escenario de producción, esto podría ser activado por una canalización
CI/CD basada en cambios en la especificación de recuperación de
características en el repositorio de origen.

1.  **Ejecute** la siguiente celda para **ejecutar el pipeline de
    entrenamiento.**

![A screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2.  En el panel izquierdo del estudio, haga clic derecho en **Jobs** y
    ábralo en una nueva pestaña. Seleccione el experimento
    **training_on_fraud_model**.

![A screenshot of a computer Description automatically
generated](./media/image61.png)

3.  Haga clic en **training job** y explore los detalles. El experimento
    debería tardar entre 5 y 15 minutos en completarse.

![A screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4.  Espere a que se complete. Una vez completado, seleccione **Models**
    en el panel izquierdo. Seleccione **fraud_model** en la lista. Este
    es el modelo creado.

![A screenshot of a computer Description automatically
generated](./media/image64.png)

5.  Seleccione la pestaña **Feature sets**. Aquí podrá ver tanto **las
    transacciones** como **las cuentas.** conjuntos de características
    de los que depende este modelo.

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6.  Abra **feature store UI** en +++https://ml.azure.com/home+++.
    Seleccione **Feature stores** -\> **featurestore**.

![A screenshot of a computer Description automatically
generated](./media/image66.png)

7.  Seleccione **Feature sets** en el panel izquierdo y luego seleccione
    cualquiera de los **feature sets**.

![A screenshot of a computer Description automatically
generated](./media/image67.png)

8.  Haga clic en la pestaña **Models** . Puede ver la lista de modelos
    que utilizan los conjuntos de características (determinados a partir
    de la especificación de recuperación de características al registrar
    el modelo).

![A screenshot of a computer Description automatically
generated](./media/image68.png)

Resumen:

En este laboratorio, aprendimos a desarrollar y registrar un conjunto de
características en un almacén de características administrado, así como
a entrenar modelos utilizando dichas características.
