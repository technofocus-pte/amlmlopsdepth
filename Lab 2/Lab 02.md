# Laboratorio 02 - Creación de un conjunto de datos etiquetado utilizando las herramientas de etiquetado de datos de Azure Machine Learning

**Objetivo**

En este laboratorio se aprenderá a utilizar las herramientas de datos de
Azure Machine Learning en Azure Machine Learning Studio para gestionar
colecciones de datos no etiquetados y convertirlos en conjuntos de datos
etiquetados que se ajusten a las clases que serán detectadas por el
modelo de detección de objetos entrenado.

Tiempo estimado - 40 minutos

## **Ejercicio 1: Preparación de los recursos de Azure**

### **Tarea 1: Cree una cuenta de almacenamiento de Azure**

1.  Desde la página principal del **portal de Azure** (+++
    **https://portal.azure.com** +++), escriba +++**storage account**+++
    en la barra de búsqueda y seleccione **Storage accounts**.

![](./media/image1.png)

2.  Seleccione **+Create.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image2.png)

3.  En la página Create a storage account, ingrese los siguientes
    detalles.

> **Detalles del proyecto**

- Subscription – Seleccione su **suscripción.**

- Resource group **–** Seleccione el **grupo de recursos.**

> **Detalles de la instancia**

- Storage account name – +++ **imagestoreacc@lab.LabInstance.Id** +++

- Region: seleccione la **región** en la que creó su **AML Workspace**

- Performance – Seleccionar **Standard**

- Redundancia: Seleccione **Locally-redundant storage(LRS)**

Seleccione **Next.**

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  En la pestaña Advanced, asegúrese de que la opción **Allow
    cross-tenant replication** en la sección **Blob storage** esté
    desmarcada. Acepte los demás valores predeterminados y seleccione
    **Review + create**.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  Una vez pasada la validación, haga clic en **Create**.

> ![A screenshot of a computer error Description automatically
> generated](./media/image5.png)

6.  Una vez completada la implementación, haga clic en **Go to
    resource**.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  Tome nota del nombre de la cuenta de almacenamiento, ya que se
    utilizará en una parte posterior del laboratorio. Permanezca en la
    misma página y continúe con la siguiente tarea.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

### **Tarea 2: Cree un Azure Storage Container**

1.  Desde el menú izquierdo de la página de la cuenta de almacenamiento,
    desplácese hasta la sección **Data Storage** y luego seleccione
    **Containers.**

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  Seleccione **+ Container**. En el panel **New container**, escriba
    el nombre del contenedor como +++imagedata+++ y haga clic en
    **Create.**

![A screenshot of a computer Description automatically
generated](./media/image9.png)

3.  Una vez creado el contenedor, seleccione **Access keys** en
    **Security + networking** en el panel izquierdo. En la página Access
    keys, haga clic en **Show** junto al valor de la clave y luego
    **cópiela** . Guarde el valor copiado en un bloc de notas para
    futuras consultas.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4.  Vuelva a la página de contenedores seleccionando **Containers**
    desde el panel izquierdo.

![](./media/image11.png)

5.  Seleccione el contenedor recién creado, **imagedata.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6.  Haga clic en **Upload**. En el panel **Upload blob**, haga clic en
    **Browse for files** y abra la carpeta **train_img** desde
    **C:\Labfiles.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image13.png)

7.  Seleccione todos los archivos en la carpeta train_img y haga clic en
    **Open**.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

8.  Haga clic en **Upload** en la página Upload blob.

![](./media/image15.png)

9.  Una vez cargado, se mostrará el mensaje **Successfully uploaded
    blob(s);** cierre el panel **Upload blob.**

![](./media/image16.png)

10. Una vez completado, debería ver que las 242 imágenes se han agregado
    al contenedor de almacenamiento de Azure.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **Ejercicio 2: Cree un proyecto de etiquetado de datos de Azure Machine Learning**

1.  Desde la página de inicio de Azure Machine Learning Studio,
    seleccione **Data Labeling** en **Manage** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

2.  Seleccionar **+ Create.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)

3.  En la sección **Project details**, proporcione los siguientes
    detalles.

    1.  **Project name** - +++ **soda** +++

    2.  Media type: **Image**

    3.  **Labeling task type: Object Identification (Bounding Box)** 

Seleccione **Next**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

4.  En la pantalla **Add workforce (optional)**, deje la opción
    deshabilitada y seleccione **Next** para continuar.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image21.png)

5.  En la página **Select or create data,** haga clic en **+ Create.**

> ![](./media/image22.png)

6.  En el panel **Data type** de la página **Create data asset**,
    proporcione los siguientes detalles.

    1.  **Name** – +++**sodaObjects**+++

    2.  **Description –** +++**Image labelling**+++

    3.  **Type –** File

> Haga clic en **Next**.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

7.  En el panel **Data source** de la página **Create data asset**,
    seleccione la opción **From Azure storage** y luego haga clic en
    **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

8.  En el panel **Storage type** de la página **Create data asset**,
    seleccione **Create new datastore.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image25.png)

9.  En el panel **New** **datastore**, proporcione los siguientes
    detalles:  
    a. **Datastore name** – +++**sodadatastore**+++  
    b. **Datastore type** – Seleccione **Azure Blob Storage**  
    c. **Account selection method** – Seleccione **From Azure
    subscription**  
    d. **Subscription ID**– Seleccione su suscripción  
    e. **Storage account** – Seleccione **imagestoreacc**  
    f. **Blob container**– Seleccione **imagedata**  
    g. **Authentication type**– Seleccione **Account Key**  
    h. **Account key** – Ingrese la clave de cuenta guardada
    anteriormente en el ejercicio 1.

> Haga clic en **Create**.
>
> ![](./media/image26.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image27.png)

10. En la página **Select a datastore** se muestra el mensaje de
    **Create success**. Seleccione el **sodadatastore** que se creó y
    haga clic en **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

11. Bajo Choose a storage path, seleccione **Enter storage path
    manually** e ingrese **/** para la ruta de almacenamiento. Active
    **Skip data validation.** Haga clic en **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

12. Revise los detalles y haga clic en **Create.**

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

13. De vuelta en el panel **Select or create data**, seleccione
    **sodaObjects.** Haga clic en **Next.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

14. En la página **Incremental refresh**, seleccione **Enable
    incremental refresh at regular intervals.** Haga clic en **Next.**

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

15. En la página **Label categories**, haga clic en **Add label
    category** dos veces para agregar dos marcadores de nombre de
    categoría más, además del que ya existe.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

16. Después de agregar, escriba +++**coke**+++, +++**diet_coke**+++ and
    +++**sprite**+++, uno en cada marcador de posición de categoría de
    etiqueta. Haz clic en **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

17. Deje las instrucciones de etiquetado en blanco y haga clic en
    **Next**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

18. Haga clic en **Next** en la página **Quality control(preview)**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

19. Deshabilite la opción **Enable** **ML assisted labelling** y haga
    clic en **Create** **project**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing**. El proyecto se está inicializando en la pantalla
    de etiquetado de datos (Data Labelling). Haga clic en el proyecto
    **soda**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

21. Haga clic en **Label data**.

> ![](./media/image39.png)

22. Las **Shortcut keys** en la parte superior derecha muestran los
    diferentes accesos directos disponibles.

> ![A group of soda cans on a table Description automatically generated
> with medium confidence](./media/image40.png)

23. La barra de menú superior proporciona las diferentes opciones
    disponibles.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

24. Se abre la primera imagen en la pantalla. Seleccione la etiqueta
    adecuada en el panel **Tags** de la izquierda.

> Luego, haga clic en la imagen y arrástrela un poco para ver la
> etiqueta que se adjunta. Haga clic en **submit**.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image42.png)

25. Repita el mismo proceso para las siguientes imágenes que aparezcan
    al enviar la actual.

> Etiqueta al menos 10 imágenes.
>
> ![](./media/image43.png)

26. Se carga la siguiente imagen hasta completar el proceso. Deténgase
    cuando supere las 10 imágenes o continúe y complete el etiquetado de
    todas las imágenes.

27. Haga clic en soda en la ruta de navegación superior para regresar al
    **Dashboard**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image44.png)

28. El **Dashboard** proporciona detalles sobre los **labeled assets** y
    la **label distribution**.

> ![](./media/image45.png)
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image46.png)

29. Haga clic en **Export.**

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image47.png)

30. En el panel **Export data**, seleccione la opción

    - **Asset type - Labeled**

    - **Export format -** **Azure ML dataset**

> Haga clic en **Submit**.
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image48.png)

31. **Labels successfully exported** se muestra en la página del panel
    de control una vez finalizada la exportación. Haga clic en **file
    link** del mensaje para ver los detalles del archivo exportado.

> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image49.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image50.png)

32. Haga clic en el enlace **View in** **datastores** o **View in Azure
    portal** en la sección **Datasources** -\> **Actions**.

> ![](./media/image51.png)

33. Ver en almacenes de datos.

> ![A picture containing text, number, software, font Description
> automatically generated](./media/image52.png)

**Resumen**

En este laboratorio, aprendió a crear un activo de datos desde el
almacenamiento de Azure y a etiquetar las imágenes y crear un conjunto
de datos etiquetado.

Todo este conjunto de tareas también pertenece a la etapa **Explore &
prepare** del flujo de **trabajo del proyecto de Machine Learning**.
