# Lab 02 - Creazione di un set di data etichettato con gli strumenti di etichettatura dei data di Azure Machine Learning

**Obiettivo**

In questo lab si apprenderà come usare Azure Machine Learning Data Tools
in Azure Machine Learning Studio per gestire le raccolte di data non
etichettati in set di data etichettati che ospitano le classi che
verrebbero rilevate dal modello di rilevamento degli oggetti sottoposto
a training.

Durata prevista - 40 min

## **Esercizio 1: Preparazione delle risorse di Azure**

### **Attività 1: Creare un account di archiviazione di Azure**

1.  Nella **home page** del **Azure portal,
    (+++https://portal.azure.com+++)** digitare +++**storage**
    **account**+++ nella barra di ricerca e selezionare **Storage
    accounts.**

![](./media/image1.png)

2.  Seleziona **+Create**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image2.png)

3.  Nella pagina Crea un account di archiviazione immettere i dettagli
    seguenti.

> **Project details**

- Abbonamento: selezionare la **subscription**.

- Gruppo di risorse: selezionare il **Resource group** assegnato.

> **Dettagli dell'istanza**

- Nome dell'account di archiviazione:
  +++**imagestoreacc@lab.LabInstance.Id** +++

- Regione: selezionare la **Region** in cui è stata creata la **AML
  Workspace**

- Prestazioni – Seleziona **Standard**

- Ridondanza: selezionare **Locally-redundant storage(LRS)**

Seleziona **Next.**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image3.png)

4.  Nella scheda Avanzate assicurarsi che l'opzione **Allow cross-tenant
    replication** nella sezione **Blob storage** sia deselezionata.
    Accettare le altre impostazioni predefinite e selezionare **Review +
    create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.png)

5.  Una volta superata la convalida, fare clic su **Create**.

> ![Uno screenshot di un errore del computer Descrizione generata
> automaticamente](./media/image5.png)

6.  Una volta completata la distribuzione, fare clic su **Go to
    resource**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

7.  Prendere nota del nome dell'account di archiviazione perché verrà
    usato nella parte successiva del lab. Rimani sulla stessa pagina e
    continua con l'attività successiva.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.png)

### **Attività 2: Creare un contenitore di archiviazione di Azure**

1.  Dal menu a sinistra della pagina dell'account di archiviazione
    scorrere fino alla sezione **Data Storage**, quindi selezionare
    **Containers**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

2.  Selezionare il **+ Container**. Nel riquadro Nuovo contenitore che
    si apre, digita il nome del contenitore come +++imagedata+++ e
    quindi fai clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

3.  Dopo aver creato il contenitore, selezionare le **Access Keys** in
    **Security + networking** nel riquadro sinistro. Nella pagina Chiavi
    di accesso fare clic su **Show** per il valore della chiave, quindi
    **copy** la chiave. Archiviare il valore copiato in un blocco note
    per riferimento futuro.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image10.png)

4.  Tornare alla pagina dei contenitori selezionando **Containers** nel
    riquadro sinistro.

![](./media/image11.png)

5.  Selezionare il contenitore appena creato, **imagedata**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image12.png)

6.  Fare clic su **Upload**. Nel riquadro **blob Upload** fare clic su
    **Browser for files** e aprire la cartella **train_img** da
    C**:\Labfiles**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image13.png)

7.  Seleziona tutti i file nella cartella train_img e fai clic su
    **Open**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

8.  Fare clic su **Upload** nella pagina Carica BLOB.

![](./media/image15.png)

9.  Dopo il caricamento, viene visualizzato il messaggio **Successfully
    uploaded blob(s)**, chiudere il riquadro **Upload blob**.

![](./media/image16.png)

10. Al termine, si noterà che tutte le 242 immagini sono state aggiunte
    al contenitore di archiviazione di Azure.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image17.png)

## **Esercizio 2: Creare un progetto di etichettatura dei data di Azure Machine Learning**

1.  Nella home page di Azure Machine Learning Studio selezionare **Data
    Labeling** in **Manage** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.png)

2.  Seleziona **+ Create.**

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image19.png)

3.  Nella sezione **Project details**, fornisci i seguenti dettagli.

    1.  **Project name** - +++**soda**+++

    2.  Media type – **Image**

    3.  **Labeling task type – Object Identification (Bounding Box)**

Seleziona **Next**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image20.png)

4.  Nella schermata **Add Workforce (optional**), lasciare l'opzione
    disabilitata e selezionare **Next** per continuare.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image21.png)

5.  Nella **Select or create data page**, fai clic su **+ Create**.

> ![](./media/image22.png)

6.  Nel riquadro **Data type** della pagina **Create data asset**
    specificare i dettagli seguenti.

    1.  **Name** – +++**SodaObjects**+++

    2.  **Description –** +++**Image Labelling**+++

    3.  **Type –** File

> Fare clic su **Next**.
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image23.png)

7.  Nel riquadro **Data source** della pagina **Create data asset**
    selezionare l' opzione **From Azure storage** e quindi fare clic su
    **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image24.png)

8.  Nel riquadro **Storage type** della pagina **Create data asset**
    selezionare **Create new datastore**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image25.png)

9.  Nel riquadro **New datastore**, fornire i dettagli seguenti.

    1.  **Datastore name** – +++**sodadatastore**+++

    2.  **Datastore type**: selezionare **Azure Blob Storage**

    3.  **Account selection method:** seleziona **From Azure
        subscription**

    4.  **Subscription ID:** selezionare la **Azuremlsubscription**

    5.  **Storage account –** Select **imagestoreacc**

    6.  **Blob container:** selezionare **imagedata**

    7.  **Authentication type:** selezionare **Account Key**

    8.  **Account Key:** immettere la chiave account salvata in
        precedenza nell Exercise 1

> Fare clic su **Create**.
>
> ![](./media/image26.png)
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image27.png)

10. Il messaggio di **Create success** viene visualizzato nella pagina.
    **Select a datastore.** Selezionare il **sodadatastore** che è stato
    creato. Fare clic su **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image28.png)

11. In Scegli un percorso di archiviazione, seleziona **Enter storage
    path manually** e digita **/** per il percorso di archiviazione,
    abilita **Skip data validataon**. Fare clic su **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image29.png)

12. Rivedi i dettagli e fai clic su **Create**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image30.png)

13. Tornare al riquadro **Select or create data** e selezionare
    **sodaObjects.** Fare clic su **Next**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image31.png)

14. Nella pagina **Incremental refresh** selezionare **Enable
    incremental refresh at regular intervals.** Fare clic su **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image32.png)

15. Nella pagina **Label categories**, fare clic due volte su **Add
    label category** etichetta per aggiungere altri due segnaposto per
    il nome della categoria oltre a quello già esistente.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image33.png)

16. Dopo l'aggiunta, digita +++**coke**+++, +++**diet_coke**+++ e
    +++**sprite**+++, uno in ogni segnaposto della categoria di
    etichette. Fare clic su **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image34.png)

17. Lasciare vuote le Istruzioni per l'etichettatura e fare clic su
    **Next**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image35.png)

18. Fare clic su **Next** nella pagina **Quality control (preview**).

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image36.png)

19. Disabilita l'opzione **Enable ML assisted labelling** e fai clic su
    **Create project**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is intializing** Il messaggio di inizializzazione del progetto viene
    visualizzato nella schermata Etichettatura data. Clicca sul progetto
    **soda.**

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image38.png)

21. Fare clic su **Label data**.

> ![](./media/image39.png)

22. I **Shortcut keys** in alto a destra mostrano le diverse scorciatoie
    disponibili.

> ![Un gruppo di lattine di soda su un tavolo Descrizione generata
> automaticamente con confidenza media](./media/image40.png)

23. La barra dei menu in alto fornisce le diverse opzioni disponibili.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image41.png)

24. La prima immagine si apre sullo schermo. Seleziona il tag
    appropriato dal riquadro **Tags** a sinistra.

> Quindi, fai clic sull'immagine e trascina leggermente per vedere
> l'etichetta attaccata all'immagine. Fare clic su **submit**.
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image42.png)

25. Ripeti la stessa procedura per le immagini successive che vengono
    visualizzate all'invio di quella corrente.

> Etichetta almeno 10 immagini.
>
> ![](./media/image43.png)

26. L'immagine successiva viene caricata fino a quando non viene
    raggiunta la fine delle immagini. Si prega di fermarsi in qualsiasi
    punto oltre le 10 immagini o procedere e completare l'etichettatura
    per tutte le immagini.

27. Fare clic su soda nel percorso di navigazione in alto per tornare
    alla **Dashboard**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image44.png)

28. La **Dashboard** fornisce i dettagli sulle **labeled assets** e
    **label distribution**.

> ![](./media/image45.png)
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con bassa confidenza](./media/image46.png)

29. Fare clic su **Export.**

> ![Uno screenshot di un grafico Descrizione generata automaticamente
> con bassa confidenza](./media/image47.png)

30. Nel riquadro **Export data**, selezionare l'icona

    - **Asset type - Labeled**

    - **Export format- Azure ML dataset**

> Fare clic su **Submit**.
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con bassa confidenza](./media/image48.png)

31. Il messaggio **Labels successfully exported** viene visualizzato
    nella pagina Dashboard una volta completata l'esportazione. Fare
    clic sul **file link** nel messaggio di successo per aprire i
    dettagli del file esportato.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con bassa confidenza](./media/image49.png)
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image50.png)

32. Fare clic sul collegamento **View in datastores** o **View in Azure
    portal** nella sezione **Datasources** - \>**Actions**.

> ![](./media/image51.png)

33. Visualizza nei datastore.

> ![Un'immagine contenente testo, numero, software, carattere
> Descrizione generata automaticamente](./media/image52.png)

**Sommario**

In questo lab si è appreso come creare un asset di data da Archiviazione
di Azure e come etichettare le immagini e creare un set di data
etichettato.

L'intero set di attività appartiene anche alla fase **Data: Explore &
prepare** del flusso di lavoro del **Machine Learning project
workflow.**
