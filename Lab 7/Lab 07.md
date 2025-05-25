# Laboratorio 07: Desarrolle y pruebe de un flujo de prompts desde Azure Machine Learning Studio

**Objetivo:**

En este laboratorio se explorará el recorrido principal del usuario al
utilizar *prompt flow* en Azure Machine Learning Studio. Se aprenderá a
habilitar *prompt flow* en el espacio de trabajo de Azure Machine
Learning, crear y desarrollar un *prompt flow*, probar y evaluar el
flujo, y luego implementarlo en un entorno de producción.

Tiempo estimado: 60 minutos

## Tarea 1: Preparación de los recursos de Azure

### Tarea 1.1: Crear un espacio de trabajo de Azure Machine Learning

Esta tarea se centra en la creación de un espacio de trabajo de Azure
Machine Learning. Se mostrará cómo configurar un espacio dedicado para
organizar y gestionar proyectos de *machine learning* de manera eficaz.
Este espacio de trabajo actúa como un centro centralizado para la
colaboración, experimentación e implementación.

1.  Inicie sesión en el portal de Azure en +++
    <https://portal.azure.com> +++ e inicie sesión con sus credenciales
    de tenant administrador.

2.  Desde la página de inicio del portal de Azure, seleccione **+ Create
    a resource**.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  En la página **Create a resource**, use la barra de búsqueda para
    encontrar +++Azure Machine Learning **+++** y seleccione **Azure**
    **Machine Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  En **Marketplace**, haga clic en **Create dropdown** y seleccione
    **Azure Machine Learning.**

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  Proporcione la siguiente información para configurar su nuevo
    espacio de trabajo:

    - **Subscription**: Seleccione su **assigned Azure subscription.**

    - **Resource group**: Seleccione su **assigned Resource Group**.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **Detalles del espacio de trabajo:**

- **Workspace name:** +++ **Azuremlws@lab.LabInstanceId** +++

- **Region**: Seleccione su región más cercana (Aquí se selecciona
  **North Central US)**

&nbsp;

- **Container registry:** Seleccione **Create new. Ingrese +++ azuremlcr
  @lab.LabInstanceId +++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  Una vez que haya terminado de configurar el espacio de trabajo,
    seleccione **Review + Create.**

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  Una vez pasada la Validación, haga clic en **Create**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  Haga clic en **Go to resource** para ver el nuevo espacio de
    trabajo.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  **En la página Microsoft.MachineLEarningServices | Overview**,
    seleccione **Launch studio** en **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### Tarea 1.2: Cree un cómputo

Esta tarea demuestra la creación de un recurso de cómputo en Azure. Se
explorarán diferentes opciones de cómputo, como máquinas virtuales o
clústeres administrados, y se explicará cómo configurar y aprovisionar
recursos para ejecutar cargas de trabajo de *machine learning* de manera
eficiente.

1.  Una vez que se abra **Azure Machine Learning Studio**, haga clic en
    **Compute** en **Manage** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  Haga clic en **+ New** en la pantalla **Compute instances**.

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  En la pantalla Create compute instance, ingrese los siguientes
    detalles.

    1.  Compute name – +++**pfcompute**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Seleccione **Standard_E4ds_v4**

> Haga clic en **Review + Create**.

**Nota: T**ome nota del nombre de este recurso de cómputo para su uso
posterior.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  Haga clic en **Create** en la siguiente pantalla para crear el
    cálculo.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**Nota:** El recurso de cómputo tarda aproximadamente 10 minutos en
alcanzar el estado "En ejecución".

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**Importante:** Una vez que el cómputo esté en funcionamiento, puede
continuar con las siguientes tareas. Sin embargo, si se toma un descanso
del laboratorio, asegúrese de **detener** la instancia de cómputo y
reiniciarla al comenzar después del descanso.

### Tarea 1.3: Cree un recurso de Azure OpenAI

1.  Desde el portal de Azure +++https://portal.azure.com+++, busque y
    seleccione +++ **AzureOpenAI** +++.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  Haga clic en **+ Create**.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  Complete los detalles a continuación y haga clic en **Next**.

- Resource group – Seleccione su grupo de recurso asignado.

- Region – Seleccione una región (En este caso usaremos **North Central
  US**)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  Acepte los valores predeterminados en las siguientes páginas y haga
    clic en **Create** en la página **Review + submit**.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  Haga clic en **Go to resource** una vez que se complete la
    implementación.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  Seleccione **Keys and Endpoint** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  Copie la **Key** y el **Endpoint** y guárdelos en un bloc de notas
    para usarlos en una parte posterior del laboratorio.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  Desde **Azure Machine Learning Studio**, seleccione **Model
    catalog** en el panel izquierdo y seleccione **gpt-4o.**

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  Haga clic en **Deploy** para implementar el modelo.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. Acepte el nombre de la implementación y seleccione **Deploy**.
    Guarde este nombre para futuras consultas.

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## Tarea 2: Configure una conexión de Prompt flow

1.  En el panel de navegación izquierdo de Azure Machine Learning
    Studio, seleccione **Prompt flow**. Seleccione **Connections** en la
    barra de menú. Seleccione el menú desplegable junto a **Create** y
    seleccione **Azure OpenAI** .

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  En el asistente para agregar la conexión de Azure OpenAI,
    proporcione los siguientes detalles y seleccione **Save**.

- Name – +++**AoaiML_pf**+++

- Provider –Seleccione **Azure OpenAI**

- Subscription ID- Seleccione su **suscripción asignada.**

- Azure OpenAI Account Name – seleccione **AOAI-PF@lab.LabInstanceId**

- Auth Mode – Seleccione **API Key**

- API Key - Proporcione la **key** con la que guardamos el **recurso de
  Azure OpenAI**

- API base – Proporcione el **endpoint** que guardamos del **recurso de
  Azure OpenAI.**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  Verifique que la creación de la conexión sea exitosa.

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## Tarea 3: Cree y desarrolle su flujo de indicaciones

1.  En la pestaña **Flows** de la página principal del **Prompt flow**,
    seleccione **Create** para crear el prompt flow. La página **Create
    a new flow**  muestra los tipos de flujo que puede crear, ejemplos
    integrados que puede clonar para crear un flujo y formas de
    importarlo.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  Seleccione **Clone** en la categoría **WebClassification**.

En la **Explore gallery**, puede explorar los ejemplos integrados y
seleccionar **View detail** en cualquier mosaico para obtener una vista
previa si es adecuado para su escenario.

En este laboratorio se utiliza el ejemplo de **Web Classification** para
recorrer el recorrido principal del usuario.

La clasificación web es un flujo que demuestra la clasificación
multiclase con un LLM. Dada una URL, el flujo la clasifica en una
categoría web con solo unas pocas capturas, un resumen simple y
sugerencias de clasificación. Por ejemplo, dada la URL
https://www.imdb.com, la clasifica como Película.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  Acepte el nombre predefinido para el **Folder name** y luego
    seleccione **Clone**.

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  Se requiere una sesión de cómputo para la ejecución del flujo. Esta
    sesión administra los recursos informáticos necesarios para la
    ejecución de la aplicación, incluyendo una imagen de Docker que
    contiene todos los paquetes de dependencia necesarios.

5.  En la página de creación de flujo, inicie una sesión de cálculo
    seleccionando **Start compute session**.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**Nota:** Tomará alrededor de **10 minutos** lograr que la sesión de
cómputo pase al estado en ejecución.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## Tarea 4: Inspeccione la página de creación de flujo

La sesión de cómputo puede tardar algunos minutos en iniciarse. Mientras
tanto, se puede explorar la interfaz de creación de flujo:

- **Vista Flow o Flatten (lado izquierdo)**: área principal de trabajo
  para crear el flujo agregando o eliminando nodos, editándolos y
  ejecutándolos en línea, así como modificando los *prompts*. Las
  secciones ***Inputs*** y ***Outputs*** permiten ver, agregar, eliminar
  o editar entradas y salidas.  
  Al clonar el flujo de ejemplo *Web Classification*, ya se encuentran
  definidos los *inputs* y *outputs*. El esquema de entrada es:  
  **name**: url; **type**: string (una URL de tipo cadena). Este valor
  predeterminado puede cambiarse manualmente, por ejemplo, a
  https://www.imdb.com.

- **Archivos (parte superior derecha)**: muestra la estructura de
  carpetas y archivos del flujo. Cada carpeta contiene el archivo
  flow.dag.yaml, archivos de código fuente y carpetas del sistema. Es
  posible crear, subir o descargar archivos para pruebas, implementación
  o colaboración.

- **Vista Graph (parte inferior derecha)**: permite visualizar el
  **flujo** de manera gráfica. Se puede hacer *zoom* o aplicar diseño
  automático (*auto layout*).

Los archivos pueden editarse directamente desde la vista ***Flow*** o
*Flatten*, o se puede activar el modo ***Raw file*** y seleccionar un
archivo desde la sección ***Files*** para abrirlo en una pestaña y
editarlo.

.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## Tarea 5: Configure los nodos LLM

Para cada nodo LLM, debe seleccionar una **conexión** para configurar
las claves API de LLM. Seleccione su conexión de Azure OpenAI.

Según el tipo de conexión, debe seleccionar un **nombre de
implementación** o un modelo en la lista desplegable. Para una conexión
de Azure OpenAI, seleccione una implementación.

1.  Para summarize_text_content, complete los detalles a continuación.

Conexión – Seleccione **AoaiML_pf**

API – Seleccione **chat**

deployment name- Seleccione **gpt-4o-2024-11-20**

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  Configure la conexión de manera similar para los nodos LLM
    **classify_with_llm**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

3.  Para probar y depurar un solo nodo, seleccione el icono **Run** en
    la parte superior del nodo en la vista **Flow**. Puede expandir
    **Inputs** y cambiar la URL de entrada del flujo para probar el
    comportamiento del nodo con diferentes URL.

4.  El estado de la ejecución aparece en la parte superior del nodo. Una
    vez finalizada, su resultado aparece en la sección **node Output**.

5.  Vaya al inicio del flujo y ejecute la **URL
    fetch_text_content_from** y ejecute el bloque.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

La vista **de gráfico** también muestra el estado de un solo nodo de
ejecución.

6.  En la sección **Inputs**, proporcione el valor para el campo
    **Value** como
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++

Seleccione **Run** en la parte superior derecha para probar y depurar
todo el flujo.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## Tarea 5: Ver resultados de flujo

También puedes configurar salidas de flujo para consultar las salidas de
varios nodos en un solo lugar. Las salidas de flujo le ayudan a:

- Consultar los resultados de pruebas masivas en una sola tabla.

- Definir el mapeo de la interfaz de evaluación.

- Establecer el esquema de respuesta de implementación.

1.  Seleccione **View outputs**  en el banner superior o en la barra de
    menú superior para ver información detallada de entrada, salida,
    ejecución de flujo y orquestación.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  En la pestaña Outputs de la pantalla Outputs, tenga en cuenta que el
    flujo predice la URL de entrada con una **category** y
    **evidence**.![A screenshot of a computer Description automatically
    generated](./media/image42.png)

3.  Seleccione la pestaña **Trace** en la pantalla **Outputs** y, a
    continuación, seleccione **flow** en el **node name** para ver
    información detallada del flujo en el panel derecho. Expanda
    **flow** y seleccione cualquier paso para ver información detallada
    de ese paso.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**Resumen:**

En este laboratorio, se clasificó una URL en una categoría web
utilizando un resumen simple y prompts de clasificación mediante el
flujo de prompts en Azure Machine Learning Studio.
