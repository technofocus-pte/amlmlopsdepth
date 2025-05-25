# Lab 01- Preparare un set di data, addestrare e distribuire un modello di classificazione, utilizzando Azure Machine Learning Studio

**Obiettivo**

Questo lab è incentrato sulla guida dell'utente attraverso il processo
di configurazione di un ambiente Azure Machine Learning, il caricamento,
l'accesso e l'esplorazione dei data e il training e la distribuzione di
un modello di classificazione delle immagini usando Azure Machine
Learning Studio.

Durata prevista - 45 minuti

## Esercizio 1: Configurazione dell'area di lavoro di Azure Machine Learning

### Attività 1: Sincronizzare l'orologio della VM

1.  Dopo aver effettuato l'accesso alla VM, fare clic con il pulsante
    destro del mouse sull'orologio nell'angolo in basso a destra dello
    schermo.

2.  Seleziona **Adjust date and time.**

&nbsp;

3.  Nella schermata Impostazioni che si apre, fai clic su **Sync now**
    in Impostazioni aggiuntive.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image1.png)

4.  Questo si occupa di sincronizzare l'ora nel caso in cui la
    sincronizzazione automatica non funzioni.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image2.png)

### Attività 2: Preparazione delle risorse di Azure

Questa attività è incentrata sulla creazione di un'area di lavoro di
Azure Machine Learning. Scoprirai come impostare uno spazio di lavoro
dedicato per organizzare e gestire efficacemente i propri progetti di
machine learning. Questa area di lavoro funge da hub centrale per la
collaborazione, la sperimentazione e la distribuzione.

#### Attività 2.1: Registrare i provider di risorse necessari 

1.  Passare alla **subscription** assegnata dalla home page del portale
    di Azure.

2.  Selezionare Provider di risorse in **Settings** nel riquadro
    sinistro.

3.  Cercare +++Microsoft.StreamAnalytics+++ e selezionare i tre punti
    accanto al nome e fare clic su **Register**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image3.png)

4.  Ripetere i passaggi per registrare +++Microsoft.Cdn+++ e
    +++Microsoft.PolicyInsights+++

#### Attività 2.2: Creare un'area di lavoro di Azure Machine Learning

1.  Accedere al portale di Azure al numero
    +++https://portal.azure.com+++ usando il **Username** e la
    **Password** della scheda **Resources**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image4.png)

2.  Nella home page del portale di Azure selezionare **+ Create a
    resource**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image5.png)

3.  Nella pagina **Create a resource** usare la barra di ricerca per
    trovare +++**Azure** **Machine Learning+++** e selezionare **Azure
    Machine Learning**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image6.png)

4.  In **Marketplace** fare clic sull' **Create dropdown** e selezionare
    **Azure Machine Learning**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image7.png)

5.  Fornisci le seguenti informazioni per configurare la tua nuova area
    di lavoro e fai clic su **Review+ create**.

    - **Subscription**: selezionare la **assigned Azure subscription**

    - **Gruppo di risorse**: selezionare il **Resource Group assigned**
      .

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: selezionare la regione più vicina **(North Central US** è
  selezionato qui)

&nbsp;

- **Container registry:** selezionare Crea nuovo**.** Immettere
  **+++azuremlcr@lab.LabInstance.Id+++**

**Nota:** il numero che viene aggiunto ai nomi delle risorse è l'ID
Labinstance per garantire l'univocità. Gli screenshot avranno un numero
diverso poiché sono unici.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

6.  Una volta superata la convalida, fare clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

7.  Fare clic su **Go to resource** per visualizzare la nuova area di
    lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.png)

8.  Nella casella **Microsoft.MachineLEarningServices | Pagina di
    panoramica**, selezionare **Launch studio** in **Work with your
    model in Azure Machine Learning Studio**.

![Uno screenshot di un aggiornamento software Descrizione generata
automaticamente](./media/image12.png)

#### Attività 2.3: Creare un calcolo

Questa attività illustra la creazione di una risorsa di calcolo in
Azure. Esplorerai diverse opzioni di calcolo, ad esempio macchine
virtuali o cluster di calcolo gestiti, e capirai come configurare ed
effettuare il provisioning delle risorse per eseguire carichi di lavoro
di Machine Learning in modo efficiente.

1.  Dopo l' apertura di **Azure Machine Learning Studio**, fare clic su
    **Compute** in **Manage** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.png)

2.  Fare clic su **+ New** nella schermata di **Compute instances**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

3.  Nella schermata Crea istanza di calcolo immettere i dettagli
    seguenti.

    1.  Nome di calcolo: +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  Tipo di macchina virtuale - **CPU**

    3.  Dimensioni macchina virtuale: selezionare **Standard_E4ds_v4**

> Fare clic su **Review + Create**.

**Nota:** prendere nota di questo nome di calcolo per un uso successivo.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.png)

4.  Fare clic su **Create** nella schermata successiva.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.png)

**Nota:** il calcolo impiega circa 10 minuti per raggiungere lo stato In
esecuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

**Importante:** una volta che il calcolo è attivo e funzionante, puoi
continuare con le attività successive. Tuttavia, se si sta facendo una
pausa dall'esecuzione del lab, assicurarsi di arrestare l'istanza di
calcolo e riavviarla quando si avvia dopo l'interruzione.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image18.png)

**Riassunto dell'esercizio:**

L'esercizio consente ai partecipanti di familiarizzare con i passaggi
essenziali necessari per la configurazione di un ambiente Azure Machine
Learning. Attraverso la serie di attività, i partecipanti hanno appreso
come creare un account di archiviazione, installare Machine Learning
SDK, accedere usando l'interfaccia della riga di comando di Azure,
creare un'area di lavoro di Azure Machine Learning e configurare una
risorsa di calcolo. Completando questo esercizio, sono state acquisite
le conoscenze fondamentali e le competenze pratiche necessarie per
creare un ambiente di Azure Machine Learning funzionale, che consenta di
intraprendere i progetti di Machine Learning in tutta sicurezza.

## Esercizio 2 - Caricamento, accesso ed esplorazione dei data in Azure Machine Learning

**Obiettivo**

In questo esercizio si apprenderà come:

- Carica i suoii data nell'archiviazione cloud

- Creare un asset di data di Azure Machine Learning

- Accedi ai suoii data in un notebook per lo sviluppo interattivo

- Creare nuove versioni degli asset di data

L'avvio di un progetto di Machine Learning prevede in genere l'analisi
esplorativa dei data (EDA), la pre-elaborazione dei data (pulizia,
progettazione delle funzionalità) e la creazione di prototipi di modelli
di Machine Learning per convalidare le ipotesi. Questa fase del progetto
di prototipazione è altamente interattiva. Si presta allo sviluppo in un
IDE o in un notebook Jupyter, con una console interattiva Python. Questo
laboratorio descrive queste idee.

Siamo nella fase **Data: Explore & prepare** del flusso di lavoro del
**Machine Learning project workflow.**

![](./media/image19.png)

### Attività 1: Preparazione delle risorse di Azure

**Importante:** assicurarsi che il calcolo creato nell'ultimo esercizio
sia attivo e in esecuzione. Se stai facendo una pausa dall'esecuzione
del laboratorio, assicurati di fermarla e riavviarla quando inizi dopo
la pausa.

#### Attività 1.1: Caricare il blocco appunti 

1.  In Azure Machine Learning Studio, una volta che il calcolo è attivo
    e in esecuzione, selezionare l'opzione **Notebook** nel riquadro
    sinistro. ![](./media/image20.png)

2.  Chiudere la finestra di dialogo **What’s new in Notebooks**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image21.png)

3.  Viene visualizzato il riquadro File del blocco appunti con la
    struttura **Users -\> \<UserName\>**. Fai clic sui tre punti accanto
    al nome utente e seleziona **Create new folder**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

4.  Immettere il nome della cartella come +++**Azuremlnotebooks**+++ e
    fare clic su **Create.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image23.png)

5.  Una volta creata la cartella, fare clic sulle **menu options** (i
    tre punti accanto al nome della cartella) della cartella
    **Azuremlnotebooks** e fare clic su **Upload Files**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.png)

6.  Selezionare **Click to browse and select file(s).** Passare a
    **explore-data.ipynb** in **C:\Labfiles** e fare clic su **Open**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image25.png)

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image26.png)

7.  Seleziona la casella di controllo, **Open file after upload** e **I
    trust the contents of this file.** Quindi fare clic su **Upload.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

8.  Verrà aperto il **Notebook** caricato.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image28.png)

9.  Fai clic su **Authenticate** se lo studio ti chiede di autenticarti,
    poiché questa è la prima volta che accedi allo studio.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image29.png)

### Attività 2: Caricare, accedere ed esplorare i data 

#### Attività 2.1: Scaricare i data

1.  Dal riquadro **Files** di **Notebooks**, fare clic sui 3 punti
    accanto al nome della cartella **Azuremlnotebooks** e fare clic su
    **Create new folder.**

![](./media/image30.png)

2.  Digita il nome della cartella come +++**data**+++ e fai clic su
    **Create**.

![](./media/image31.png)

3.  Una volta che la creazione della cartella è andata a buon fine, fare
    clic sulle opzioni di menu dei **data** della cartella e selezionare
    **Upload files**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image32.png)

4.  Selezionare **Click to browser and select file(s),** quindi passare
    a **C:\Labfiles** per selezionare il file
    **default_of_credit_card_clients.csv** e fare clic su **Open**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image33.png)

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image34.png)

5.  Al termine del caricamento, viene visualizzato un messaggio che
    indica che il file è stato caricato correttamente, sotto le
    notifiche.

![Un primo piano dello schermo di un computer Descrizione generata
automaticamente con bassa confidenza](./media/image35.png)

#### Attività 2.2: Creare un handle per l'area di lavoro

1.  Torna al taccuino (**explore-data**).

2.  Prima di immergerci nel codice, è necessario un modo per fare
    riferimento all'area di lavoro. Si creerà ml_client per un handle
    per l'area di lavoro. Utilizzerai quindi ml_client per gestire
    risorse e processi.

3.  Nella prima cella sotto, **Create handle to workspace**, sostituire
    i segnaposto di **\< SUBSCRIPTION_ID \>**, **\< RESOURCE_GROUP \>**
    e il **\< AML_WORKSPACE_NAME \>.**

4.  Sostituire \< RESOURCE_GROUP\> con il nome del gruppo di risorse
    assegnato.

5.  Sostituire \<AML_WORKSPACE_NAME\> con
    [+++**Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  Sostituire \< SUBSCRIPTION_ID \> con
    +++**@lab.CloudSubscription.Id**+++.

7.  Fare clic sul pulsante **Run cell** disponibile in alto a sinistra
    della cella. Cerca un segno di spunta nella parte inferiore della
    cella una volta che l'esecuzione è andata a buon fine.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image36.png)

#### Attività 2.3: Caricare i data nell'archiviazione cloud

1.  Un asset di data di Azure Machine Learning è simile ai segnalibri
    del Web browser (preferiti). Invece di ricordare i percorsi di
    archiviazione lunghi (URI) che puntano ai data usati più di
    frequente, è possibile creare un asset di data e quindi accedere a
    tale asset con un nome descrittivo.

2.  La cella del notebook successiva crea l'asset di data. L'esempio di
    codice carica il file di data non elaborati nella risorsa di
    archiviazione cloud designata.

3.  Ogni volta che si crea un asset di data, è necessaria una versione
    univoca per esso. Se la versione esiste già, riceverai un errore. In
    questo codice, stiamo usando il tempo per generare una versione
    univoca ogni volta che la cella viene eseguita.

4.  Esegui la cella successiva facendo clic sul pulsante Esegui in alto
    a sinistra della cella.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image37.png)

5.  **"Data asset created. Name: credit-card,  version:
    YYYY:MM:DD.xxxxxx"** è l'output che viene visualizzato sotto la
    cella.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image38.png)

6.  Fare clic su **Data** nel riquadro di sinistra e fare clic
    sull'asset di data della **credit-card** creato dall'esecuzione
    eseguita nel passaggio precedente. Esplorare i dettagli e tornare al
    riquadro **Notebooks**.

![](./media/image39.png)

#### Attività 2.4: Accedere ai data in un blocco appunti

1.  Tornare al notebook ed eseguire la cella con il comando **%pip** per
    installare la libreria Python **azureml-fsspec** nel kernel
    **Jupyter**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con bassa confidenza](./media/image40.png)

2.  Esegui la cella successiva per accedere al file CSV in **Pandas**.

3.  Verrà stampato **Data asset URI** nella parte inferiore della cella
    e verranno visualizzati anche i data.

![Uno screenshot di un codice informatico Descrizione generata
automaticamente con bassa confidenza](./media/image41.png)

#### 

#### Attività 2.5: Creare una nuova versione dell'asset di data

1.  Potresti aver notato che i data hanno bisogno di un po' di pulizia
    leggera, per renderli adatti al training di un modello di
    apprendimento automatico. Dispone di:

    1.  due intestazioni

    2.  una colonna ID client; non useremmo questa funzione in Machine
        Learning

    3.  spazi nel nome della variabile di risposta

2.  Inoltre, rispetto al formato CSV, il formato di file **Parquet**
    diventa un modo migliore per archiviare questi data. Parquet offre
    la compressione e mantiene lo schema. Pertanto, per pulire i data e
    archiviarli in Parquet, eseguire la cella successiva.

3.  Assicurarsi che l'esecuzione abbia esito positivo in base al segno
    di graduazione nella parte inferiore della cella.

![](./media/image42.png)

4.  Questa tabella mostra la struttura dei data nel file
    **default_of_credit_card_clients.csv** originale . CSV scaricato in
    un passaggio precedente. I data caricati contengono 23 variabili
    esplicative e 1 variabile di risposta, come mostrato di seguito:

[TABLE]

5.  Eseguire la cella successiva per creare una nuova *versione*
    dell'asset di data (i data vengono caricati automaticamente
    nell'archiviazione cloud).

6.  Al termine dell'esecuzione, viene visualizzato un output che indica,
    **Data asset created. Name: credit_card, version: AAAA.
    MM.DD.xxxxxx_cleaned** appare dopo la cella.

![Uno screenshot di un codice informatico Descrizione generata
automaticamente con bassa confidenza](./media/image43.png)

![Uno screenshot di un computer Descrizione generata automaticamente con
bassa confidenza](./media/image44.png)

**Importante:**

Questa cella di codice Python imposta i valori di **name** e **version**
per l'asset di data creato. Di conseguenza, il codice in questa cella
avrà esito negativo se eseguito più di una volta, senza una modifica a
questi valori. I valori di **name** e **version** fissi offrono un modo
per passare valori che funzionano per situazioni specifiche, senza
preoccuparsi dei valori generati automaticamente o in modo casuale.

7.  Il file di parquet pulito è l'origine data della versione più
    recente. Il codice nella cella successiva mostra prima il set di
    risultati della versione CSV, quindi la versione di Parquet
    all'esecuzione.

8.  Esegui la cella successiva e controlla il risultato di seguito.

![Uno screenshot di un codice informatico Descrizione generata
automaticamente con bassa confidenza](./media/image45.png)

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image46.png)

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente con bassa confidenza](./media/image47.png)

![Un'immagine contenente testo, screenshot, numero, display Descrizione
generata automaticamente](./media/image48.png)

9.  Cerca i data puliti sotto **Data**.

> ![](./media/image49.png)

**Importante:** da qui puoi continuare con l'esercizio successivo.
Tuttavia, se si sta facendo una pausa dall'esecuzione del lab,
assicurarsi di arrestare l'istanza di calcolo e riavviarla quando si
riprende dalla pausa.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image50.png)

**Riassunto dell'esercizio**

In questo esercizio si è appreso come caricare i data nell'archiviazione
cloud, creare un asset di data di Azure Machine Learning, accedere ai
data in un notebook per lo sviluppo interattivo e creare nuove versioni
di asset di data.

## Esercizio 3 - Eseguire il training e distribuire un modello di classificazione delle immagini in Azure Machine Learning Studio

**Obiettivo**

In questo esercizio, imparerai a:

1.  Connettersi all'area di lavoro e configurare una risorsa di calcolo
    usando l'interfaccia utente del notebook di Azure Machine Learning
    Studio

2.  Inserire i data e prepararli per essere utilizzati per il training

3.  Eseguire il training di un modello per la classificazione delle
    immagini

4.  Visualizzare e analizzare le metriche per ottimizzare il modello

5.  Distribuisci il modello online e testalo

Siamo nella fase di **Train & validate model** del flusso di **Machine
Learning project workflow.**

### ![Un'immagine contenente testo, carattere, numero, screenshot Descrizione generata automaticamente](./media/image51.png)

### Attività 1: Caricare il blocco appunti

1.  Dalla pagina Azure Machine Learning Studio, **Notebooks**, fare clic
    sulle opzioni di menu per la cartella **AzureMLnotebooks** e fare
    clic su **Upload files**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image52.png)

2.  Selezionare, **Click to browser and select file(s)**, passare a
    **C:\Labfiles** e selezionare il file,
    **azureml-getting-started-studio** (un file di origine Jupyter).

> ![Uno screenshot dello schermo di un computer Descrizione generata
> automaticamente con confidenza media](./media/image53.png)
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image54.png)

3.  Seleziona la casella di controllo **Open file after upload** e
    quindi fai clic su **Upload**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image55.png)

4.  Una volta che il caricamento del file è andato a buon fine, viene
    aperto in studio, connesso automaticamente al
    Compute(cpu-cluster-fs) che si trova nello stato In esecuzione.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image56.png)

### Attività 2: Connettersi all'area di lavoro di Azure Machine Learning

Prima di immergerci nel codice, dovrai connetterti alla tua area di
lavoro. L'area di lavoro è la risorsa di primo livello per Azure Machine
Learning e offre una posizione centralizzata in cui lavorare con tutti
gli artefatti creati quando si usa Azure Machine Learning.

Stiamo usando **DefaultAzureCredential** per ottenere l'accesso all'area
di lavoro. **DefaultAzureCredential** deve essere in grado di gestire la
maggior parte degli scenari.

*\# Maniglia per l'area di lavoro*

**from** azure.ai.ml **import** MLClient

*\# Pacchetto di autenticazione*

**from** azure.identity **import** DefaultAzureCredential

credential **=** DefaultAzureCredential()

*\# Get a handle to the workspace. You can find the info on the
workspace tab on ml.azure.com*

ml_client **=** MLClient(

credential=credential,

subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  Nel codice precedente (prima cella del notebook), sostituisci
    **SUBSCRIPTION_ID, RESOURCE_GROUP name** e i segnaposto
    AML_WORKSPACE_NAME con i valori salvati nell'esercizio precedente.

2.  La prima cella del taccuino dovrebbe ora essere simile a questa.
    Fare clic sul pulsante **Run** in alto a sinistra della prima cella.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image57.png)

3.  Assicurarsi che la cella sia stata eseguita correttamente
    visualizzando il suo stato nella parte inferiore della cella.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con confidenza media](./media/image58.png)

In \[ \]:

### Attività 3: Caricare i data

Per eseguire un processo di training di Azure Machine Learning, è
necessario un ambiente.

In questo lab si userà un ambiente pronto all'uso denominato
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest che contiene tutte le
librerie necessarie (python, MLflow, numpy, pip e così via).

1.  Esegui il codice nella cella successiva per caricare i data.

2.  Assicurarsi che un messaggio che indica **Data asset created** venga
    visualizzato come output della cella.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con confidenza media](./media/image59.png)

###  Attività 4: Compilare il processo di comando per il training

Ora che si dispone di tutti gli asset necessari per eseguire il
processo, è il momento di creare il processo stesso, usando Azure ML
Python SDK v2. Creeremo un lavoro di comando.

Un processo di comando AzureML è una risorsa che specifica tutti i
dettagli necessari per eseguire il codice di training nel cloud: input e
output, tipo di hardware da usare, software da installare e modalità di
esecuzione del codice. Il processo di comando contiene informazioni per
eseguire un singolo comando.

#### Task 4.1 : Creare uno script di training

1.  Iniziamo creando lo script di addestramento, il file python
    **main.py**.

2.  Esegui la cella successiva e assicurati che venga eseguita
    correttamente.

![Un'immagine contenente testo, carattere, linea, screenshot Descrizione
generata automaticamente](./media/image60.png)

- Lo script nella cella successiva gestisce la pre-elaborazione dei
  data, suddividendoli in data di test e data di training. Utilizza
  quindi questi data per eseguire il training di un modello basato su
  albero e restituire il modello di output. MLFlow utilizzato per
  registrare i parametri e le metriche durante l'esecuzione della
  pipeline.

3.  Esegui la cella e assicurati che venga eseguita correttamente con
    l'output,

**Writing./src/main.py**

> ![Uno screenshot di un programma per computer Descrizione generata
> automaticamente con bassa confidenza](./media/image61.png)
>
> ![Uno screenshot di un programma per computer Descrizione generata
> automaticamente con confidenza media](./media/image62.png)

4.  Come si può vedere in questo script, una volta che il modello è
    stato addestrato, il file del modello viene salvato e registrato
    nell'area di lavoro. A questo punto è possibile utilizzare il
    modello registrato per l'inferenza degli endpoint.

#### Attività 4.2: Configurare il comando

Ora che si dispone di uno script in grado di eseguire le attività
desiderate, si utilizzerà il comando generico in grado di eseguire
azioni della riga di comando. Questa azione della riga di comando può
essere la chiamata diretta dei comandi di sistema o l'esecuzione di uno
script.

1.  In questo caso, utilizzerai i data di input, il rapporto di
    divisione, la velocità di apprendimento e il nome del modello
    registrato come variabili di input.

2.  Nel riquadro di sinistra, seleziona **Data** e seleziona i data
    della **credit-card-data.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image63.png)

3.  Nella sezione **Data sources**, cerca il valore del **Datastore
    URI** e copialo. Salvalo per utilizzarlo nel passaggio successivo.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image64.png)

4.  Nella cella successiva, sostituire il

    1.  Valore del **path** con la **Datastore** **URI** salvato nel
        passaggio precedente.

    2.  Valore del **compute** con
        +++**cpu-cluster-fs@lab.LabInstance.Id**+++ (il nome del cluster
        salvato nel Lab 1)

5.  Fare clic su **Run**. Assicurarsi che la cella venga eseguita
    correttamente.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image65.png)

### Attività 6: Inviare il processo

È ora possibile inviare il processo per l'esecuzione in AzureML. **The
job will take 2 to 3 minutes to run**. Potrebbe essere necessario più
tempo (fino a 10 minuti) se l'istanza di calcolo è stata ridotta a zero
nodi e l'ambiente personalizzato è ancora in fase di creazione.

1.  Eseguire la cella con il comando seguente per inviare il lavoro.

> ***\# submit the command job ***
>
> ***ml_client.create_or_update(job) ***

2.  Fare clic su **Run**. Assicurarsi che l'esecuzione sia riuscita e
    che sia presente un collegamento al risultato nella colonna
    **Details Page**.

**Nota**: il completamento dell'operazione richiederà circa 2 minuti.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image66.png)

3.  Apri il link disponibile nella colonna **Details Page** del
    risultato, in una nuova scheda.

### Attività 7: Visualizzare il risultato di un processo di formazione

1.  È possibile visualizzare il risultato di un processo di formazione
    **clicking the URL generated after submitting a job**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image67.png)

2.  In alternativa, puoi anche fare clic su **Jobs** lavoro nel menu di
    navigazione a sinistra. Un processo è un raggruppamento di molte
    esecuzioni da uno script o da una parte di codice specificata. Le
    informazioni per l'esecuzione vengono archiviate in tale processo.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image68.png)

3.  Nella pagina **Overview** viene innanzitutto visualizzato lo
    **status** nel riquadro **Properties** in **Running**.

4.  Lo stato cambia in **Completed** una volta che è pronto.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image69.png)

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image70.png)

5.  Selezionare il riquadro **Metrics** per visualizzare le metriche.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image71.png)

6.  Selezionare la scheda **Images** per visualizzare la matrice
    training_confusion, la curva di richiamo di precisione e la curva
    roc.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image72.png)

1)  **Overview** è dove puoi vedere lo stato del lavoro.

2)  **Metrics** visualizzeranno visualizzazioni diverse delle metriche
    specificate nello script.

3)  **Images** consente di visualizzare gli artefatti dell'immagine
    registrati con MLflow.

4)  **Child jobs** contiene i lavori figlio, se sono stati aggiunti.

5)  **Outputs + logs** contiene i file di log necessari per la
    risoluzione dei problemi o per altri scopi di monitoraggio.

6)  **Code** contiene lo script/codice utilizzato nel processo.

7)  **Explanations** e **Fairness** vengono utilizzate per vedere come
    si comporta il modello rispetto agli standard di intelligenza
    artificiale responsabili. Attualmente sono funzionalità di anteprima
    e richiedono installazioni di pacchetti aggiuntive.

8)  **Monitoring** consente di visualizzare le metriche per le
    prestazioni delle risorse di calcolo.

### Attività 8: Distribuire il modello come endpoint online

Dopo aver eseguito il training di un modello di Machine Learning, è
necessario distribuirlo in modo che altri utenti possano usarlo per
l'inferenza. A questo scopo, Azure Machine Learning consente di creare
**endpoints** e aggiungervi **deployments**.

Un **endpoint**, in questo contesto, è un percorso HTTPS che fornisce
un'interfaccia per i client per inviare richieste (data di input) a un
modello sottoposto a training e ricevere i risultati dell'inferenza
(punteggio) dal modello. Un endpoint fornisce:

- Autenticazione tramite autenticazione basata su "chiave o token"

- Terminazione TLS(SSL)

- Un URI di punteggio stabile
  (endpoint-name.region.inference.ml.azure.com)

Una **deployment** è un insieme di risorse necessarie per l'hosting del
modello che esegue l'inferenza effettiva.

#### Attività 8.1: Creare un endpoint online

1.  Distribuire ora il modello di Machine Learning come servizio Web nel
    cloud di Azure, un endpoint online.

2.  Selezionare **Endpoints** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image73.png)

3.  Selezionare **Create** per Endpoint in tempo reale

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image74.png)

4.  Seleziona **credit_defaults_model** e quindi fai clic su **Select.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image75.png)

5.  Selezionare **Standard_E4s_v3** nella cartella Macchina virtuale.
    Specificare il numero di istanze come **1**

> Accettare le altre impostazioni predefinite di un **Endpoint name**
> univoco e del **Deployment name**, quindi selezionare **Deploy**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image76.png)

**Nota:** il completamento della creazione dell'endpoint richiede circa
20 minuti.

6.  Al termine, lo stato del provisioning cambia in **Succeeded**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image77.png)

#### Attività 8.2: Test con una query di esempio

1.  Dalla pagina dell'endpoint selezionare la scheda **Test**.

2.  Copia e incolla il seguente file di richiesta di esempio nel campo
    **Input data to test real-time endpoint**, sostituendo il codice già
    presente.

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

3.  Selezionare **Test** e visualizzare il risultato in **Test result**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image78.png)

### Attività 9: Eliminare l'endpoint

1.  Nel riquadro sinistro selezionare **Endpoints**. Seleziona
    l'endpoint che abbiamo creato e fai clic su **Delete**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image79.png)

2.  Fare clic su **Delete** nella finestra di dialogo di conferma.

![Uno screenshot di un errore del computer Descrizione generata
automaticamente con bassa confidenza](./media/image80.png)

3.  Cerca una notifica sull'avvenuta eliminazione.

![Un'immagine contenente testo, screenshot, carattere, riga Descrizione
generata automaticamente](./media/image81.png)

**Sommario**

In questo lab si è appreso come eseguire il training di un modello di
classificazione delle immagini in Azure Machine Learning Studio e
distribuirlo come servizio Web.
