# **Lab 09 - Configurazione di MLOps con GitHub**

**Obiettivo:**

Azure Machine Learning consente l'integrazione con **GitHub Actions**
per automatizzare il ciclo di vita di Machine Learning.

In questo lab si apprenderà come usare Azure Machine Learning per
configurare una pipeline MLOps end-to-end che esegue una regressione
lineare per stimare le tariffe dei taxi a New York. La pipeline è
costituita da componenti, ognuno dei quali svolge funzioni diverse, che
possono essere registrati nell'area di lavoro, sottoposti a controllo
delle versioni e riutilizzati con vari input e output.

Durata prevista: 60 minuti

Siamo nella fase MLOps di Azure Machine Learning

![](./media/image1.png)

## **Esercizio 1: Preparazione delle risorse di Azure**

### **Attività 1: Creare un'area di lavoro di Azure Machine Learning**

1.  Accedere al portale di Azure al numero
    +++<https://portal.azure.com>+++ se non è già stato effettuato
    l'accesso.

2.  Nella home page del portale di Azure selezionare **+ Create a
    resource**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image2.png)

3.  Nella pagina **Create a resource** usare la barra di ricerca per
    trovare +++Azure Machine Learning+++

4.  Seleziona **Machine Learning**.

> ![](./media/image3.png)

5.  In **Marketplace** fare clic sull **Create dropdown** e selezionare
    **Azure Machine Learning**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image4.png)

6.  Fornisci le seguenti informazioni per configurare la tua nuova area
    di lavoro:

    - **Subscription**: selezionare la **assigned Azure subscription**

    - **Resource group**: selezionare il **Resource Group assigned** .

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: selezionare la regione più vicina **(North Central US** è
  selezionato qui)

&nbsp;

- **Container registry:** selezionare **Create new. Enter
  +++azuremlcr@lab.LabInstance.Id+++**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image5.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

7.  Una volta superata la convalida, fare clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.png)

8.  Fare clic su **Go to resource,** per visualizzare la nuova area di
    lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

9.  Nella casella **Microsoft.MachineLEarningServices | Overview page**,
    selezionare **Launch studio** in **Work with your model in Azure
    Machine Learning Studio**.

![Uno screenshot di un aggiornamento software Descrizione generata
automaticamente](./media/image9.png)

### **Attività 2: Creare un calcolo**

1.  Dopo l'apertura di Azure Machine Learning Studio, fare clic su
    **Compute** in **Manage** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

2.  Seleziona la scheda **Compute clusters** e fai clic su **+ New**.

![](./media/image11.png)

3.  Nella schermata di **Create compute cluster**, inserisci i dettagli
    seguenti.

    1.  Location: selezionare la **Region** in cui è stata creata l'area
        di lavoro di Azure Machine Learning

    2.  Virtual machine tier - **Dedicated**

    3.  Virtual machine type - **CPU**

    4.  Virtual machine size: seleziona **Standard_E4s_v3 (** seleziona
        Seleziona da tutte le opzioni per trovare la dimensione della
        macchina virtuale**)**

> Fare clic su **Next**.
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image12.png)

4.  Nella pagina **Advanced Settings**, inserisci i dettagli seguenti.

&nbsp;

1.  Compute name: +++**cpu-cluster@lab. LabInstanceId**+++

2.  Minimum number of nodes – 0

3.  Maximum number of nodes – 1

> Fare clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.png)

**Nota:** il calcolo impiega circa 10 minuti per raggiungere lo stato In
esecuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

## **Esercizio 2: Recuperare le risorse di Azure**

1.  Dal portale di Azure (<https://portal.azure.com>) aprire il gruppo
    di risorse e prendere nota dei nomi delle risorse seguenti,

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage account**

> E salvali localmente in un blocco note per essere aggiornati nel file
> di configurazione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.png)

## **Esercizio 3: Preparazione dell'account GitHub e delle risorse**

**Nota:** Se non hai già un account con GitHub, creane uno da qui
+++**https://github.com/**+++ -\> **Signup**.

### 

### 

### **Attività 2: Eseguire il fork della demo mlops del repository nell'account GitHub**

1.  Apri un browser e inserisci questo link -
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  Clicca su **Fork** in alto a destra.

![Uno screenshot di una chat Descrizione generata automaticamente con
confidenza media](./media/image16.png)

3.  Si apre una pagina **Create a new fork**. Fare clic su **Create
    fork.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

4.  Nel progetto GitHub selezionare **Settings**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image18.png)

5.  Selezionare **Actions** in **Secrets and variables.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.png)

6.  Selezionare **New repository secret**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image20.png)

7.  Assegnare a questo segreto il nome **+++AZURE_CREDENTIALS+++** e
    incollare l'output dell **Service Principal** come contenuto del
    segreto. Questa entità servizio è stata creata automaticamente.
    Seleziona **Add secret**.

> {
>
> "clientId": "+++@lab . Variabile(spAppId)+++",
>
> "clientSecret": "+++@lab . Variabile(spClientSecret)+++",
>
> "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
> "tenantId": "+++@lab. CloudSubscription.TenantId+++",
>
> "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
> "resourceManagerEndpointUrl": "https://management.azure.com/",
>
> "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
> "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
> "galleryEndpointUrl": "https://gallery.azure.com/",
>
> "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![Schermata di un computer Descrizione generata automaticamente con
> bassa confidenza](./media/image21.png)

8.  Il **AZURE_CREDENTIALS** segreto aggiunto viene visualizzato in
    **Repository secrets**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image22.png)

9.  Fare clic su **New repository secret**.

![](./media/image23.png)

10. Fornisci i dettagli seguenti.

    1.  Name – +++ARM_CLIENT_ID+++

    2.  Secret – +++@lab.Variable(spAppId)+++

> ![Uno screenshot di un segreto informatico Descrizione generata
> automaticamente con bassa confidenza](./media/image24.png)

11. Ripetere i passaggi 9 e 10 per i valori seguenti, creando segreti
    GitHub aggiuntivi.

    - +++ARM_CLIENT_SECRET+++ - +++@lab.Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab. CloudSubscription.TenantId+++

## **Esercizio 4: Configurare i parametri dell'ambiente di Machine Learning**

1.  Dalla pagina dei segreti, vai alla pagina del repository facendo
    clic su **mlops-v2-gha-demo** accanto al Suoi ID GitHub in alto a
    sinistra.

![](./media/image25.png)

2.  Seleziona il file **config-infra-prod.yml** nella radice. Fare clic
    su **Edit** (l'icona a forma di matita).

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image26.png)

3.  Modificare i valori,

    1.  **Namespace** – **mlopsliteXX**(Sostituisci XX con un numero
        casuale)

    2.  **Postfix** – **c**

    3.  **location** – **Same as your workspace region**

> Fare clic su **Commit changes**.
>
> Nella **For pipeline reference section** sostituire i **values** delle
> risorse di Azure con i valori recuperati e salvati nell'esercizio 2.
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image27.png)

4.  Fare clic su **Commit changes** nel riquadro Conferma modifiche.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image28.png)

5.  Apri **deploy-model-training-pipeline-classical.yml** da
    **.github/workflows**. Fare clic su **Edit**(l'icona a forma di
    matita).

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image29.png)

6.  Nel contenuto del file, sostituisci il valore di **Size** con
    **+++Standard_E4s_v3+++**

Seleziona **Commit changes**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image30.png)

7.  Aprire il file **online-deployment.yml** da
    **mlops/azureml/deploy/online.** Fare clic su **Edit** (l'icona a
    forma di matita).

![](./media/image31.png)

8.  Sostituisci il valore di **instance_type** come
    **+++Standard_E4s_v3+++**. Fare clic su **Commit changes**.

![Uno screenshot di un computer Descrizione generata automaticamente con
bassa confidenza](./media/image32.png)

9.  Apri **tf-gha-deploy-infra.yml** file in **.github/workflows**. Fare
    clic su **Edit** e sostituisci Azure con +++CoursesTF+++ nelle righe
    9 e 14.

Seleziona **Commit changes**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image33.png)

10. Dalla barra dei menu in alto, seleziona **Actions**. Clicca su **I
    understand my workflows, go ahead and enable them**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image34.png)

11. Vengono visualizzati i flussi di lavoro GitHub predefiniti associati
    al progetto.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image35.png)

## **Esercizio 5: Distribuire l'infrastruttura di Machine Learning**

1.  Seleziona **tf-gha-deploy-infra.yml**. Fare clic su **Runworkflow**.

Selezionare

- Branch – **main**

Seleziona **Run workflow.**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image36.png)

2.  In questo modo l'infrastruttura di Machine Learning verrà
    distribuita usando GitHub Actions e Terraform.

3.  Tenere traccia dello stato del processo e verificare che
    l'esecuzione sia riuscita.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image37.png)

**Nota:** il completamento di questo flusso di lavoro richiede circa 5
minuti.

## **Esercizio 6: Distribuzione della pipeline di addestramento del modello**

Successivamente, si distribuirà la pipeline di training del modello
nella nuova area di lavoro di Machine Learning.

Questa pipeline creerà un'istanza del cluster di calcolo, registrerà un
ambiente di addestramento definendo l'immagine Docker necessaria e i
pacchetti python, registrerà un set di data di addestramento, quindi
avvierà la pipeline di addestramento descritta nell'ultima sezione.

1.  Dalla pagina del flusso di lavoro **tf-gha-deploy-infra.yml**, fare
    clic su **Actions**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image38.png)

2.  Vengono visualizzati i flussi di lavoro GitHub predefiniti associati
    al progetto. Selezionare **deploy-model-training-pipeline**
    dall'elenco.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image39.png)

3.  Fare clic su **Run workflow**-\> **Run workflow**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image40.png)

4.  Fare clic sulla pipeline appena avviata per monitorare
    l'avanzamento.

![Un'immagine contenente testo, software, pagina web, font Descrizione
generata automaticamente](./media/image41.png)

5.  Il completamento di questa pipeline richiede dai 15 ai 45 minuti.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image42.png)

6.  Di seguito è riportata una schermata dell'esecuzione della pipeline
    riuscita.

![](./media/image43.png)

7.  Questa esecuzione registrerà il modello nell'area di lavoro di
    Machine Learning.

8.  Accedere allo studio AzureMachineLearning
    all'<https://ml.azure.com/> e fare clic su **Data** nel riquadro
    sinistro per verificare che i del **taxi-data** siano stati aggiunti
    lì. Questa operazione viene eseguita come parte del processo di
    **register-dataset** del flusso di lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image44.png)

9.  Fai clic su **Jobs** nel riquadro di sinistra e seleziona
    **taxi-fare-training**. Questa operazione viene eseguita nel
    processo di **run-pipeline** del flusso di lavoro.

![](./media/image45.png)

10. Selezionare il nome visualizzato dell'ultima esecuzione.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image46.png)

11. Esplora le fasi e i dettagli coinvolti nella formazione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image47.png)

Con il modello sottoposto a training registrato nell'area di lavoro di
Machine Learning, è possibile distribuire il modello per l'assegnazione
dei punteggi.

**Sommario**

In questo lab si è appreso come usare Azure Machine Learning per
configurare una pipeline MLOps end-to-end, che ha preparato i data e
distribuito la pipeline di training del modello nella nuova area di
lavoro di Machine Learning.
