# Laboratorio 08 – Implementación de generación de datos de preguntas y respuestas (QA) con RAG utilizando un flujo de prompts

**Objetivo:**

La generación de datos de preguntas y respuestas (QA) es parte del
proceso de creación de RAG (Retrieval Augmented Generation), donde el
conjunto de datos QA autogenerado se utiliza para obtener el mejor
prompt para RAG y para obtener métricas de evaluación de RAG.

En este laboratorio, se explicará cómo crear un conjunto de datos QA a
partir de su información.

Tiempo estimado – 60 minutos

## Ejercicio 1: Cree implementaciones de AOAI 

En este ejercicio, se crearán las implementaciones del modelo
gpt-35-turbo utilizando el recurso de Azure OpenAI creado en el
laboratorio anterior.

1.  Desde Azure Machine Learning Studio, seleccione **Model Catalog** en
    el panel izquierdo. Busque **gpt-35-turbo** y seleccione
    **gpt-35-turbo** de la lista de modelos.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  Asegúrese de que el recurso **AOAI-PF@lab.LabInstanceId** esté
    seleccionado en el campo **Azure OpenAI resource**. Luego,
    seleccione **Deploy** para implementar el modelo.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  Acepte el **Deployment name** y seleccione **Deploy.**

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  Repita la implementación del modelo para **text-embedding-ada-002**
    utilizando el nombre de implementación
    +++**text-embedding-ada-002-2**+++

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## Ejercicio 2: Configure el entorno

1.  En el panel izquierdo de Studio, seleccione **Notebooks**. Haga clic
    en los tres puntos junto al nombre de usuario y seleccione **Upload
    files.**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

2.  Navegue hasta **C:\LabFiles** y seleccione el archivo
    **qa_data_generation.ipynb**. Marque la casilla **I trust contents
    of this file** y haga clic en **Upload.**

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  Abra el notebook y seleccione **Serverless Spark Compute** en la
    opción **Compute.**

![A screenshot of a computer program Description automatically
generated](./media/image7.png)

4.  Una vez que el cómputo esté adjunto, seleccione **Configure
    session** para cargar el archivo conda.yml y configurar el entorno
    para la ejecución utilizando dicho archivo.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  Seleccione **Python packages** -\>**Upload Conda file** -\> haga
    clic en **Browse**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  Seleccione el archivo **conda.yml** de **C:\LabFiles** y seleccione
    **Apply.**

> ![A screenshot of a computer program Description automatically
> generated](./media/image10.png)

## Ejercicio 3: Obtenga el cliente para el espacio de trabajo de AzureML

1.  Ejecute la primera celda del notebook para instalar las
    dependencias.

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

**Nota:** Esto tomará de 10 a 15 minutos para completarse.

2.  Ejecute la siguiente celda con az login para **iniciar sesión** en
    **Azure CLI.**

![A screenshot of a computer Description automatically
generated](./media/image13.png)

3.  El espacio de trabajo es el recurso de nivel superior para Azure
    Machine Learning, proporcionando un lugar centralizado para trabajar
    con todos los artefactos que cree cuando use Azure Machine Learning.
    En esta sección, nos conectaremos al espacio de trabajo en el que se
    ejecutará el trabajo. MLClient es la forma en que interactúa con
    AzureML.

4.  Reemplace los marcadores de posición para **Subscription ID** con
    +++@lab.Subscription()+++, **Resource group** con el nombre de su
    **grupo de recursos** y **Azure ML Workspace** con +++
    **Azuremlws@lab.LabInstanceId** +++ en la siguiente celda para crear
    el MClient.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer Description automatically
generated](./media/image15.png)

5.  **Ejecute** la siguiente celda que establece el **nombre de la
    conexión**. Si ha utilizado otro nombre al crear la conexión,
    proporcione ese valor en esta celda y luego ejecute.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

6.  Reemplace el **key** **value** con la clave de **Azure OpenAI** y el
    **target** value con el valor del **Endpoint** del recurso de Azure
    OpenAI que guardó anteriormente. **Ejecute** la celda después de
    reemplazar los valores.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

7.  Ahora que su espacio de trabajo tiene una conexión con Azure OpenAI,
    nos aseguraremos de que el modelo gpt-35-turbo haya sido
    implementado y esté listo para la inferencia.

8.  **Ejecute** la siguiente celda para establecer los nombres del
    modelo y de la **implementación**. Reemplace los valores del nombre
    del modelo y el nombre de la implementación si ha utilizado nombres
    diferentes al crear el modelo y la implementación.

![A screenshot of a computer code Description automatically
generated](./media/image18.png)

9.  Finalmente, combinaremos la información de la implementación y el
    modelo en un formato URI que los componentes de embeddings de
    AzureML esperan como entrada. **Ejecute** la siguiente celda para
    realizar esto.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

Ejercicio 4: Configuración del Pipeline

Los pipelines de AzureML conectan múltiples componentes. Cada componente
define entradas, el código que consume esas entradas y los resultados
producidos a partir de dicho código. Los pipelines en sí mismos pueden
tener entradas y salidas producidas al conectar componentes
individuales. Para procesar sus datos para el embedding e indexado,
encadenaremos varios componentes, cada uno realizando su propio paso en
el flujo de trabajo.

Los componentes están publicados en un registro, azureml, al que debería
tener acceso de forma predeterminada. Este registro puede ser accedido
desde cualquier espacio de trabajo. En la siguiente celda, obtenemos las
definiciones de los componentes desde el registro de azureml.

1.  Ejecute la siguiente celda y asegúrese de que se ejecute
    correctamente sin ningún problema.

![A screenshot of a computer code Description automatically
generated](./media/image20.png)

2.  Cada componente tiene documentación que proporciona una descripción
    general de su propósito y de cada una de las entradas y salidas. Por
    ejemplo, podemos entender qué hace el componente
    **data_generation_component** inspeccionando la definición del
    componente. **Ejecute** la siguiente celda para esto y observe la
    salida.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

3.  A continuación, se construye un Pipeline mediante la definición de
    una función en Python que enlaza los componentes anteriores, sus
    entradas y salidas. Los argumentos de la función son las entradas
    del Pipeline en sí, y el valor de retorno es un diccionario que
    define las salidas del Pipeline. Asegúrese de que la **siguiente
    celda** se **ejecute correctamente**.

![A screenshot of a computer code Description automatically
generated](./media/image22.png)

![A screenshot of a computer program Description automatically
generated](./media/image23.png)

4.  Los ajustes a continuación muestran cómo configurar los diferentes
    parámetros de git' y 'data_source para procesar únicamente la
    documentación de AzureML del repositorio más grande de AzureDocs,
    asegurando que la URL de origen de cada documento se procese para
    vincularla a la URL pública alojada, en lugar de la URL de Git.

5.  Ejecute las siguientes dos celdas y asegúrese de que se ejecuten
    correctamente.

![](./media/image24.png)

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

Ejercicio 5: Envíe el pipeline

El resultado de cada paso en el pipeline puede ser inspeccionado a
través de la interfaz de usuario de Workspace. Haga clic en el enlace
bajo **Details Page** después de ejecutar la celda a continuación.

1.  Ejecute la siguiente celda y haga clic en el enlace en la salida
    para ver el estado del flujo.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

2.  La ejecución se abrirá en el flujo de comandos. Explore cada etapa
    del flujo.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

6.  Una vez que el flujo haya sido exitoso, pase al siguiente paso.

## Ejercicio 6: Revise los datos QA generados

1.  Ejecute las siguientes 2 celdas y revise la salida de los datos QA.

![A screenshot of a computer code Description automatically
generated](./media/image29.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image30.png)

Resumen:

En este laboratorio, hemos aprendido a crear un conjunto de datos de QA
a partir de sus datos.
