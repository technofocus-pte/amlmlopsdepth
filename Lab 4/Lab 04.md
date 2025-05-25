# **Lab 04 – Training di un modello di classificazione con AutoML no-code in Azure Machine Learning Studio**

**Obiettivo**

In questo lab si apprenderà come eseguire il training di un modello di
classificazione con AutoML senza codice usando Azure Machine Learning
Machine Learning automatizzato in Azure Machine Learning Studio. Questo
modello di classificazione prevede se un cliente sottoscriverà un
deposito a termine fisso presso un istituto finanziario. L'apprendimento
automatico automatizzato esegue rapidamente l'iterazione di molte
combinazioni di algoritmi e iperparametri per aiutarti a trovare il
modello migliore in base a una metrica di successo di tua scelta.

Durata prevista – 60 minuti

Siamo nella fase di **Deploy Model** di Azure Machine Learning.

![](./media/image1.png)

## **Esercizio 1: Creare un'area di lavoro di Azure Machine Learning**

1.  Accedere al portale di Azure - +++**https://portal.azure.com**+++
    usando le credenziali della scheda **Resources**.

2.  Nella home page del portale di Azure selezionare **+Create a
    resource**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image2.png)

3.  In **Create a resource** usare la barra di ricerca per trovare
    +++**Azure** **Machine Learning**+++. Selezionare **Azure Machine
    Learning** in **Marketplace**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image3.png)

4.  In **Marketplace** fare clic sull' elenco a discesa **Create** e
    selezionare **Azure Machine Learning**.

> ![Uno screenshot di un software Descrizione generata
> automaticamente](./media/image4.png)

5.  Fornisci le seguenti informazioni per configurare la tua nuova area
    di lavoro:

    - **Subscription**: selezionare la **assigned** **Azure
      subscription.**

    - **Resource group**: selezionare il **assigned Resource Group.**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image5.png)

**Workspace Details:**

- **Workspace name: +++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **Region**: seleziona Regione **North Central US** viene usato qui

- **Container registry:** selezionare **Create new.** Immettere
  **+++Azuremlcr@lab.LabInstanceId**+++

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image7.png)

6.  Al termine della configurazione dell'area di lavoro, selezionare
    **Review+ Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

7.  Una volta superata la convalida, fare clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

8.  Fare clic su **Go to resource,** per visualizzare la nuova area di
    lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**,
    selezionare **Launch studio** in **Work with your model in Azure
    Machine Learning Studio**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.png)

## **Esercizio 2: Creare un processo di Machine Learning automatizzato**

1.  Passare alla scheda Azure Machine Learning Studio.

2.  Nel riquadro sinistro selezionare **Automated ML** nella sezione
    **Authoring**.

3.  Fare clic su **+ New Automated ML job**.

![](./media/image12.png)

### **Attività 1: Creare un asset di data**

1.  Nella pagina **Basic settings**, assegna il nome del nuovo
    esperimento come +++**MarketingExperiment**+++, accetta le altre
    impostazioni predefinite e fai clic su **Next**.

![](./media/image13.png)

2.  Nella pagina Tipo di attività e data selezionare **Classification**
    in **Select task type** e selezionare **+ Create** in **Select
    data.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

3.  Nella pagina Crea asset di data specificare i dettagli seguenti.

- **Name** – +++marketingdata+++

- **Type** – **Tabular**

- Fare clic su **Next**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image15.png)

4.  Nel riquadro **Data source**, selezionare **From local files** e
    fare clic su **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.png)

5.  In **Destination storage type** selezionare l'archivio data
    predefinito configurato automaticamente durante la creazione
    dell'area di lavoro: **workspaceblobstore**. È possibile caricare il
    file di data in questa posizione per renderlo disponibile nell'area
    di lavoro. Seleziona **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

6.  In **File or folder selection** selezionare **Upload files or
    folder** \> **Upload files**. Scegli il file
    **bankmarketing_train.csv** da **C:/Labfiles**. Seleziona **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.png)

7.  Al termine del caricamento, l' area di **Data preview** viene
    popolata in base al tipo di file. Nel modulo **Settings** esaminare
    i valori dei data. Quindi seleziona **Next**.

[TABLE]

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image19.png)

8.  Il modulo **Schema** consente un'ulteriore configurazione dei data
    per questo esperimento. Per questo esempio, selezionare
    l'interruttore a levetta per il **day_of_week**, in modo da non
    includerlo. Seleziona **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image20.png)

9.  Nel modulo **Review** verificare le informazioni e selezionare
    **Create** per completare la creazione dell **data asset.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

10. Nella pagina **Create a new Automated ML job**, viene visualizzato
    un messaggio di **success** per la creazione dell'asset di data.
    Seleziona l' asset di data **marketingdata** creato e fai clic su
    **Next**.

> **Nota:** se i **marketingdata** non vengono visualizzati, fai clic su
> Aggiorna per visualizzarli.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

### **Attività 2: Configurare il processo**

1.  Nella pagina **Task Settings** selezionare **y (String)** come
    **Target column**, ovvero ciò che si desidera prevedere. Questa
    colonna indica se il cliente ha sottoscritto o meno un deposito a
    termine.

2.  Selezionare **View additional configuration settings** e compilare i
    campi come indicato di seguito. Queste impostazioni servono a
    controllare meglio il processo di formazione. In caso contrario, le
    impostazioni predefinite vengono applicate in base alla selezione e
    ai data dell'esperimento.

- Primary metric - AUCWeighted

- Explain best model – Enable

- Use all supported models - Enable

- Blocked models – None

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image23.png)

3.  Seleziona **Limits** e inserisci +++**60**+++ per il campo
    **Experiment timeout(minutes**).

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image24.png)

![Uno screenshot di un test I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image25.png)

4.  In **Validate and test** specificare i valori seguenti e fare clic
    su **Next**.

- Validataon type – Seleziona **k-fold cross-validataon**

- Number of cross validataons – Seleziona **2**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image26.png)

5.  Nella pagina Calcolo selezionare Seleziona tipo di calcolo come
    **Compute cluster** e fare clic su **+ New**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

6.  Nel riquadro di **Create compute cluster**, selezionare i dettagli
    seguenti e fare clic su **Next**.

- Location: **North Central US** (uguale alla posizione dell'area di
  lavoro di Azure Machine Learning)

- Virtual machine tier - **Dedicated**

- Virtual machine type - **CPU**

- Virtual machine size - Seleziona **Standard_DS12_v2**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image28.png)

7.  Nelle impostazioni avanzate, fornisci i dettagli seguenti e
    seleziona **Create**.

- Compute name- +++automl-compute+++

- Numero minimo di nodi - 0

- Numero massimo di nodi – 1

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image29.png)

8.  Selezionare **Next** una volta completato il provisioning di
    calcolo.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image30.png)

9.  Nella pagina **Review,** selezionare **Submit the training job**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image31.png)

10. Si apre la schermata **Overview** con lo **Status** in alto
    all'inizio della preparazione dell'esperimento. Questo stato viene
    aggiornato man mano che l'esperimento procede. Nello studio vengono
    visualizzate anche notifiche per informarti sullo stato
    dell'esperimento.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image32.png)

> **Nota:** il corso di formazione dura circa 40 minuti.

## **Esercizio 3: Esplorare i modelli**

Durante il training, è possibile esplorare i modelli associati.

1.  Passare alla scheda **Models + child jobs** per visualizzare gli
    algoritmi (modelli) testati.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image33.png)

2.  Selezionare il modello **StandardScalerWrapper, XGBoostClassifier**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image34.png)

3.  Fai clic su **Metrics** ed esplora i dettagli nella scheda Metriche.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image35.png)

4.  Mentre attendi il completamento di tutti i modelli dell'esperimento,
    seleziona il **Algorithm name** di un modello completato per
    esaminarne i dettagli sulle prestazioni. Selezionare le schede
    **Overview** e **Metrics** per informazioni sul processo.

> **Importante:** il completamento dell'addestramento del modello
> richiede circa 40 minuti. Si prega di procedere con il laboratorio
> successivo mentre questo è in corso. Riprendi questo lab quando lo
> stato cambia in **Completed**.

## **Esercizio 4: Spiegazioni del modello**

Le spiegazioni del modello possono essere generate su richiesta. Il
dashboard delle spiegazioni del modello che fa parte della scheda
**Explanations (preview)** riepiloga queste spiegazioni.

1.  Nella scheda Modelli + lavori secondari (dal lavoro padre),
    selezionare **MaxAbsScaler, LightGBM.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image36.png)

2.  Seleziona la scheda **Explain model**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image37.png)

3.  Nel riquadro Spiega modello che si apre, selezionare

    1.  Select compute type – **Compute cluster**

    2.  Select AzureML compute instance - Selezionare **automl-compute**

Seleziona **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image38.png)

4.  Viene visualizzato il messaggio di operazione riuscita. Seleziona la
    scheda **Explanations (preview**). Questa scheda viene compilata al
    termine dell'esecuzione della spiegabilità.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image39.png)

5.  Espandere il riquadro sinistro. In **Features**, seleziona la riga
    che dice **raw**. Selezionare la scheda **Aggregate feature
    importance.** Questo grafico mostra quali caratteristiche dei data
    hanno influenzato le previsioni del modello selezionato.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

In questo esempio, la **duration** sembra avere la maggiore influenza
sulle previsioni di questo modello.

## **Esercizio 5: Distribuire il modello migliore**

L'interfaccia automatizzata di Machine Learning consente di distribuire
il modello migliore come servizio Web. *L'implementazione* è
l'integrazione del modello in modo che possa prevedere nuovi data e
identificare potenziali aree di opportunità. Per questo esperimento,
l'implementazione in un servizio Web significa che l'istituto
finanziario dispone ora di una soluzione Web iterativa e scalabile per
identificare potenziali clienti di depositi a tempo determinato.

Al termine dell'esecuzione dell'esperimento, la pagina **Details** viene
popolata con una sezione **Best model summary**. In questo contesto
dell'esperimento, **VotingEnsemble** è considerato il modello migliore,
in base alla metrica **AUCWeighted**.

1.  Selezionare **Jobs** nel riquadro sinistro e selezionare
    l'esperimento creato.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image41.png)

2.  Fai clic sul **display name** dell'esperimento.

![](./media/image42.png)

3.  Controlla se lo stato è **Completed**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image43.png)

4.  Al termine dell'esecuzione dell'esperimento, la pagina **Details**
    viene popolata con una sezione di **Best model summary**. In questo
    contesto dell'esperimento, **VotingEnsemble** è considerato il
    modello migliore, in base alla metrica **AUC_weighted**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image44.png)

Implementiamo questo modello, ma tieni presente che la distribuzione
richiede circa 20 minuti per essere completata. Il processo di
distribuzione prevede diversi passaggi, tra cui la registrazione del
modello, la generazione di risorse e la configurazione per il servizio
Web.

5.  Selezionare **VotingEnsemble** per aprire la pagina specifica del
    modello.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image45.png)

6.  Selezionare il menu **Deploy** in alto a sinistra e selezionare
    **Deploy to web service**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image46.png)

7.  Popolare il riquadro **Deploy a model** come indicato di seguito:

[TABLE]

> Fare clic su **Deploy**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image47.png)

8.  Nella schermata Modello viene visualizzato un messaggio di esito
    positivo che indica che la **Model deployment is succesfully
    triggered** e che lo stato è In **Running**.

![](./media/image48.png)

9.  Al termine della distribuzione, lo stato cambia in **Completed**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image49.png)

> A questo punto si dispone di un servizio Web operativo per generare
> previsioni.

## **Esercizio 6: Eliminare le risorse**

### **Attività 1: Eliminazione dell'endpoint**

1.  Nel riquadro sinistro di AML Studio, fare clic su **Endpoints**.

2.  Selezionare l'endpoint, **my-automl-deploy** e fare clic su
    **Delete**.

![](./media/image50.png)

3.  Selezionare **Delete** nella finestra di dialogo Elimina endpoint in
    tempo reale.

4.  Dovresti ricevere un messaggio di successo dopo l'eliminazione
    dell'endpoint.

**Sommario**

In questo lab abbiamo appreso come eseguire il training di un modello di
classificazione senza codice AutoML in Azure Machine Learning Studio e
distribuire il modello migliore come servizio Web.
