# **Laboratorio 09: Configuración de MLOps con GitHub**

**Objetivo:**

Azure Machine Learning le permite integrarse con **GitHub Actions** para
automatizar el ciclo de vida del aprendizaje automático.

En este laboratorio, se aprenderá a utilizar Azure Machine Learning para
configurar un pipeline de MLOps de extremo a extremo que ejecuta una
regresión lineal para predecir tarifas de taxi en Nueva York. El
pipeline está compuesto por componentes, cada uno con funciones
específicas, que pueden registrarse en el workspace, versionarse y
reutilizarse con diferentes entradas y salidas.

Tiempo estimado: 60 minutos

Estamos en la fase MLOps de Azure Machine Learning

![](./media/image1.png)

## **Ejercicio 1: Preparación de los recursos de Azure**

### **Tarea 1: Cree un espacio de trabajo de Azure Machine Learning**

1.  Inicie sesión en el portal de Azure en +++
    <https://portal.azure.com> +++ si aún no ha iniciado sesión.

2.  Desde la página de inicio del portal de Azure, seleccione **+ Create
    a resource**.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  En la página **Create a resource**, use la barra de búsqueda para
    encontrar +++Azure Machine Learning+++

4.  Seleccione **Machine Learning**.

> ![](./media/image3.png)

5.  En **Marketplace**, haga clic en **el menú desplegable Create** y
    seleccione **Azure Machine Learning.**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

6.  Proporcione la siguiente información para configurar su nuevo
    espacio de trabajo:

    - **Subscription**: Seleccione su **suscripción de Azure asignada**

    - **Resource group**: Seleccione el **grupo de recursos que** le fue
      asignado.

> **Workspace Details:**

- **Workspace name:** +++ **Azuremlws@lab.LabInstance.Id** +++

- **Region**: Seleccione su región más cercana (Aquí se selecciona
  **North Central US).**

&nbsp;

- **Container registry:** Seleccione **Create new. Ingrese +++
  azuremlcr@lab.LabInstance.Id +++**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  Una vez pasada la Validación, haga clic en **Create**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

8.  Haga clic en **Go to resource** para ver el nuevo espacio de
    trabajo.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9.  **En la página Microsoft.MachineLEarningServices | Overview**,
    seleccione **Launch studio** en **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a software update Description automatically
generated](./media/image9.png)

### **Tarea 2: Cree un recurso de cómputo**

1.  Una vez que se abra Azure Machine Learning Studio, haga clic en
    **Compute** dentro de **Manage** en el panel izquierdo.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

2.  Seleccione la pestaña **Compute clusters** y haga clic en **+ New**.

![](./media/image11.png)

3.  En la pantalla **Create compute cluster**, ingrese los siguientes
    detalles.

    1.  Location: Seleccione la **Region** en la que creó su espacio de
        trabajo de Azure Machine Learning

    2.  Virtual machine tier – **Dedicated**

    3.  Virtual machine type – **CPU**

    4.  Virtual machine size: Seleccione **Standard_E4s_v3 (**marque
        **Select from all options** para encontrar el tamaño de la
        máquina virtual**)**

> Haga clic en **Next**.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

4.  En la página **Advanced Settings**, ingrese los siguientes detalles:

&nbsp;

1.  Compute name – +++**cpu-cluster@lab.LabInstanceId**+++

2.  Minimum number of nodes – 0

3.  Maximum number of nodes – 1

> Haga clic en **Create.**

![A screenshot of a computer Description automatically
generated](./media/image13.png)

**Nota:** El proceso tarda aproximadamente 10 minutos en llegar al
estado de ejecución.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

## **Ejercicio 2: Recuperar los recursos de Azure**

1.  Desde el portal de Azure ( <https://portal.azure.com> ), abra su
    grupo de recursos y tome nota de los nombres de los siguientes
    recursos,

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage account**

> Y guárdelos localmente en un bloc de notas para actualizarlos en el
> archivo de configuración.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

## **Ejercicio 3: Preparación de la cuenta y los recursos de GitHub**

**Nota:** Si aún no tiene una cuenta en GitHub, cree una desde aquí
+++**https://github.com/**+++ -\> **Signup**.

### **Tarea 2: Bifurque el repositorio mlops demo en su cuenta de GitHub**

1.  Abra un navegador e ingrese a este enlace: +++ [https://github.com/
    getazureready
    /mlops-v2-gha-demo](https://github.com/getazureready/mlops-v2-gha-demo)
    +++

2.  Haga clic en **Fork** en la parte superior derecha.

![A screenshot of a chat Description automatically generated with medium
confidence](./media/image16.png)

3.  Se abrirá la página **Create a new fork**. Haga clic en **Create
    fork.**

![A screenshot of a computer Description automatically
generated](./media/image17.png)

4.  Desde su proyecto de GitHub, seleccione **Settings**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.png)

5.  Seleccione **Actions** en **Secrets and variables.**

![A screenshot of a computer Description automatically
generated](./media/image19.png)

6.  Seleccione **New repository secret**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

7.  Asigne a este secreto el nombre **+++ AZURE_CREDENTIALS+++** y pegue
    el resultado **de la entidad de servicio** como contenido. Esta
    entidad de servicio ya está creada. Seleccione **Add secret**.

> {
>
> "clientId": "+++@lab .Variable(spAppId)+++",
>
>   "clientSecret": "+++@lab .Variable(spClientSecret)+++",
>
>   "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
>   "tenantId": "+++@lab.CloudSubscription.TenantId+++",
>
>   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
>   "resourceManagerEndpointUrl": "https://management.azure.com/",
>
>   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
>   "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
>   "galleryEndpointUrl": "https://gallery.azure.com/",
>
>   "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![A screen shot of a computer Description automatically generated with
> low confidence](./media/image21.png)

8.  El secreto **AZURE_CREDENTIALS** que se agrega se muestra en
    **Repository secrets.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

9.  Haga clic en **New repository secret**.

![](./media/image23.png)

10. Proporcione los siguientes detalles.

    1.  Name – +++ARM_CLIENT_ID+++

    2.  Secret – +++@lab .Variable(spAppId)+++

> ![A screenshot of a computer secret Description automatically
> generated with low confidence](./media/image24.png)

11. Repita los pasos 9 y 10 para los siguientes valores y cree secretos
    de GitHub adicionales.

    - +++ARM_CLIENT_SECRET+++ - +++@lab .Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab.CloudSubscription.TenantId+++

## **Ejercicio 4: Configurar los parámetros del entorno de Machine Learning**

1.  Desde la página de secretos, navegue a la página del repositorio
    haciendo clic en **mlops-v2-gha-demo** junto a su ID de GitHub en la
    parte superior izquierda.

![](./media/image25.png)

2.  Seleccione el archivo **config-infra- prod.yml** en la raíz. Haga
    clic en **Edit** (el icono del lápiz).

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  Cambie los valores,

    1.  **Namespace** – **mlopsliteXX** (Reemplace XX con un número
        aleatorio)

    2.  **Postfix** – **c**

    3.  **Location:** **La misma que la región de su espacio de
        trabajo**

> Haga clic en **Commit changes**.
>
> En la sección **For pipeline reference**, reemplace los valores de los
> recursos de Azure con los valores obtenidos y guardados en el
> **Ejercicio 2**.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

4.  Haga clic en **Commit changes** en el panel de confirmación de
    cambios.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

5.  Abra **el archivo deploy-model-training-pipeline-classical.yml**
    desde**. github /workflows**. Haga clic en **Edit** (el ícono del
    lápiz).

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

6.  En el contenido del archivo, reemplace el valor de **Size** con
    **+++Standard_E4s_v3+++**

Seleccione **Commit changes**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image30.png)

7.  Abra el archivo **online-deployment.yml** desde
    **mlops/azureml/deploy/online.** Haga clic en **Edit** (el ícono del
    lápiz).

![](./media/image31.png)

8.  Reemplace el valor de **instance_type** por
    **+++Standard_E4s_v3+++.** Haga clic en **Commit changes**.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image32.png)

9.  Abra el archivo **tf-gha-deploy-infra.yml** en **.github
    /workflows**. Haga clic en **Edit** y reemplace Azure por +++
    CoursesTF +++ en las líneas 9 y 14.

Seleccione **Commit changes**.

![A screenshot of a computer Description automatically
generated](./media/image33.png)

10. En la barra de menú superior, selecciona **Actions**. Haga clic en
    **I understand my workflows, go ahead and enable them**.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

11. Aquí se muestran los flujos de trabajo de GitHub predefinidos
    asociados con su proyecto.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

## **Ejercicio 5: Implementar la infraestructura de Machine Learning**

1.  Seleccione **tf - gha -deploy- infra.yml**. Haga clic en
    **Runworkflow**.

Seleccione:

- Branch – **main**

Seleccione **Run workflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image36.png)

2.  Esto implementará la infraestructura de Machine Learning utilizando
    GitHub Actions y Terraform.

3.  Realice el seguimiento del estado del trabajo y confirme que la
    ejecución se haya completado correctamente.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

**Nota:** Este flujo de trabajo tarda aproximadamente 5 minutos en
completarse.

## **Ejercicio 6: Implementar el pipeline de entrenamiento del modelo**

A continuación, se implementará el pipeline de entrenamiento del modelo
en el nuevo espacio de trabajo de Machine Learning.

Este pipeline creará una instancia de clúster de cómputo, registrará un
entorno de entrenamiento que define la imagen de Docker y los paquetes
de Python necesarios, registrará un dataset de entrenamiento y luego
iniciará el pipeline de entrenamiento descrito en la sección anterior.

1.  Desde la página del flujo de trabajo **tf-gha-deploy-infra.yml**,
    haga clic en **Actions**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

2.  Aquí se muestran los flujos de trabajo predefinidos de GitHub
    asociados a su proyecto. Seleccione
    **deploy-model-training-pipeline** en la lista.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

3.  Haga clic en **Run workflow** -\> **Run workflow**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image40.png)

4.  Haga clic en el pipeline que acaba de iniciarse para seguir el
    progreso.

![A picture containing text, software, web page, font Description
automatically generated](./media/image41.png)

5.  Este pipeline tarda entre 15 y 45 minutos en completarse.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image42.png)

6.  A continuación, se muestra una captura de pantalla de la ejecución
    exitosa del pipeline.

![](./media/image43.png)

7.  Esta ejecución registrará el modelo en el espacio de trabajo de
    Machine Learning.

8.  Inicie sesión en el Azure Machine Learning Studio en
    <https://ml.azure.com/> y haga clic en **Data** en el panel
    izquierdo para verificar que los **datos de taxi** han sido añadidos
    allí. Esto se realiza como parte del trabajo **register-dataset**
    del flujo de trabajo.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

9.  Haga clic en **Jobs** en el panel izquierdo y seleccione
    **taxi-fare-training.** Esto se ejecuta en el trabajo
    **run-pipeline** del flujo de trabajo.

![](./media/image45.png)

10. Seleccione el nombre para mostrar de la última ejecución.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

11. Explore las etapas y los detalles involucrados en la capacitación.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

Con el modelo entrenado registrado en el espacio de trabajo de Machine
Learning, está listo para implementar el modelo para la puntuación.

**Resumen**

En este laboratorio, hemos aprendido a utilizar Azure Machine Learning
para configurar un pipeline MLOps de extremo a extremo, que preparó los
datos y desplegó el pipeline de entrenamiento del modelo en su nuevo
espacio de trabajo de Machine Learning.
