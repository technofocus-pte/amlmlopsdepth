# Lab 06 - Addestramento del miglior modello di Regressione per il dataset Hardware

Obiettivo

In questo lab viene illustrato come usare AutoML per il training di un
modello di regressione. Useremo il set di dataPrestazioni hardware per
addestrare e distribuire il modello da usare in scenari di inferenza.
L'obiettivo della regressione consiste nel prevedere le prestazioni di
determinate combinazioni di parti hardware.

Durata prevista – 60 minuti

# Esercizio 0: Preparare l'ambiente

### **Attività 1: Avviare l'area di lavoro AML**

1.  Accedere al portale di Azure,
    +++[**https://portal.azure.com**](https://portal.azure.com)+++ se
    non è già stato effettuato l'accesso.

2.  Dal menu portale di Azure selezionare **All resources.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.png)

3.  Selezionare l'area di lavoro di Azure Machine Learning
    (**Azuemlws@lab.LabInstanceId**).

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image2.png)

4.  Fare clic su **Launch studio**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image3.png)

5.  Selezionare **Compute** nel riquadro sinistro per creare un'istanza
    di calcolo. Seleziona **+ New**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.png)

6.  Fornisci i dettagli di seguito e fai clic su **Review + Create**.

- Compute name - +++**auto-compute**+++

- Virtual machine type - **CPU**

- Virtual Machine - **Standard E4ds_v4**

![](./media/image5.png)

7.  Selezionare **Create** per creare l'istanza di calcolo.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

### 

### **Attività 2: Caricare il blocco appunti nell'area di lavoro AML**

1.  Fare clic su **Notebooks** nel riquadro di sinistra. Fai clic sui
    tre punti accanto al **username** in **Users** e seleziona **Upload
    folder**.

![](./media/image7.png)

2.  Seleziona Fai clic per sfogliare e selezionare le cartelle e sfoglia
    **C:\Labfiles** per selezionare la cartella
    **automl-regression-task-hardware-performance** e fare clic su
    **Upload.**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image8.png)

3.  Se viene visualizzato un pop-up che chiede **Upload 3 files to this
    site?** fai clic su **Upload**.

![Un'immagine contenente testo, screenshot, visualizzazione, carattere
Descrizione generata automaticamente](./media/image9.png)

4.  Seleziona la casella di controllo, **I trust contents of these
    files**, quindi seleziona **Upload**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image10.png)

5.  Aprire il notebook (il file con estensione ipynb),
    **automl-regression-task-hardware-performance**. Il notebook viene
    connesso automaticamente al calcolo creato in precedenza.

![](./media/image11.png)

## **Esercizio 1: Connettersi all'area di lavoro di Azure Machine Learning**

### **Attività 1: Importare le librerie necessarie**

1.  Esegui la prima cella in **1.1 Import the required libraries** per
    importare le librerie necessarie per l'esecuzione di questo lab
    facendo clic sul pulsante Esegui cella in alto a sinistra della
    cella.

2.  Assicurati che l'esecuzione abbia esito positivo cercando un simbolo
    di spunta in basso a sinistra della cella.

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente con bassa confidenza](./media/image12.png)

### **Attività 2: Configurare i dettagli dell'area di lavoro e ottenere un handle per l'area di lavoro**

1.  Nella cella inferiore a **1,2. Configure workspace details and get
    handle to the workspace,** sostituire

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP : **Your assigned Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  Fai clic sull'opzione Esegui cella in alto a sinistra della cella e
    assicurati di ottenere un simbolo di spunta in basso a sinistra una
    volta che l'esecuzione è andata a buon fine.

3.  Sotto la cella viene visualizzato un output che indica **Found the
    config file in:** **/config.json.**

![](./media/image13.png)

### **Attività 3: Visualizzare le informazioni sull'area di lavoro di Azure ML**

1.  Eseguire la cella successiva (la cella sotto Mostra informazioni
    sull'area di lavoro di Azure ML).

2.  Assicurarsi che i dettagli dell'area di lavoro, della
    sottoscrizione, della posizione e del gruppo di risorse elencati
    come output sotto la cella siano tutti corretti.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image14.png)

## 

## **Esercizio 2: MLTable con datadi training di input**

### **Attività 1: Creare input di dataMLTable**

1.  Eseguire la cella successiva (quella in **2.1 Create MLTable data
    input**).

2.  Assicurarsi che l'esecuzione sia riuscita.

![Un'immagine contenente testo, carattere, screenshot, software
Descrizione generata automaticamente](./media/image15.png)

## **Esercizio 3: Configurare ed eseguire il processo di training di regressione AutoML**

1.  Eseguire le celle in **4.1 Configure and run the AutoML Regression
    training job** uno per uno e assicurarsi che ogni cella venga
    eseguita correttamente.

2.  La cella sotto **4.2 Run the Command**, invia il processo AutoML.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image16.png)

3.  È possibile controllare lo stato del processo facendo clic su
    **Jobs** nel riquadro sinistro e selezionando l'esperimento che si
    trova nello stato In esecuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image18.png)

**Nota:** il completamento dell'operazione richiede dai 10 ai 15 minuti.

4.  La cella successiva nel notebook attende il completamento del
    processo AutoML.

5.  Eseguilo e attendi fino al completamento dell'esecuzione per passare
    alla cella successiva.

![](./media/image19.png)

6.  Procedere al passaggio successivo solo una volta completata
    l'esecuzione.

![](./media/image20.png)

7.  Esegui le 2 celle successive una per una che recupera l'URL e il
    nome del lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

## **Esercizio 4: Recuperare la versione di prova migliore (prova/esecuzione del modello migliore)**

1.  Aggiungi una cella sopra la prima cella di questo esercizio.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image22.png)

2.  Copia il codice qui sotto. Fare clic su **Run cell.**

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image23.png)

3.  Continua a eseguire le 3 celle successive una per una analizzando
    ogni codice e il suo output.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image24.png)

4.  Eseguire la cella successiva per **Get the parent run**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con bassa confidenza](./media/image25.png)

5.  Esegui la cella successiva per **print the parent tags**.

![Uno screenshot di un computer Descrizione generata automaticamente con
bassa confidenza](./media/image26.png)

6.  Eseguire la cella successiva per **Get the AutoML best child run**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con confidenza media](./media/image27.png)

7.  Esegui la cella successiva per **Get the best model run’s metrics**.

![Uno screenshot di un errore del computer Descrizione generata
automaticamente con bassa confidenza](./media/image28.png)

8.  Esegui le prossime 3 celle per **Download the best model locally**.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image29.png)

## **Esercizio 5: Registrare il modello migliore e distribuirlo**

### **Attività 1: Creare un endpoint online gestito**

1.  Esegui le prime 2 celle in questa attività.

![](./media/image30.png)

2.  Esegui la cella successiva con il codice,

**ml_client.begin_create_or_update(endpoint).result()**

In questo modo viene creato un endpoint online denominato
**regression-\<Currentdate&time\>.**

![Uno screenshot di un computer Descrizione generata automaticamente con
bassa confidenza](./media/image31.png)

3.  Verificare la presenza della notifica che indica che la **Endpoint
    "regression-\<Currentdate&time\>” update completed.**

> ![Uno screenshot di un computer I contenuti generati dall'intelligenza
> artificiale potrebbero non essere corretti.](./media/image32.png)

### **Attività 2: Registrare il modello migliore e distribuirlo**

1.  Eseguire la prima cella in Registra il modello migliore e
    distribuire -\> **Register model**, per registrare il modello
    denominato **hardware-performance-model**.

2.  Una volta che l'esecuzione è andata a buon fine, eseguire la cella
    successiva per recuperare l'ID del modello registrato.

> ![](./media/image33.png)

### **Attività 3: Distribuzione**

1.  Nella prima cella in Distribuisci, sostituire il valore
    **instance_type** con **Standard_E4s_v3.**

2.  Quindi, eseguire la cella per distribuire il modello migliore.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image34.png)

3.  Eseguire la cella successiva per creare la distribuzione.

![Un'immagine contenente testo, screenshot, linea, carattere Descrizione
generata automaticamente](./media/image35.png)

4.  **This will take aroung 40 minutes to complete**. È anche possibile
    controllare lo stato da **Endpoint** (selezionare **Endpoint** dal
    riquadro sinistro e quindi fare clic sull' endpoint
    **regression-XXXXXXX** distribuito in precedenza).

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image36.png)

5.  Una volta completata l'esecuzione e completata la distribuzione, la
    cella restituisce i dettagli della distribuzione.

![](./media/image37.png)

6.  Inoltre, nella pagina dei dettagli Endpoint, lo stato della
    distribuzione diventa **Succeeded**.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image38.png)

7.  Eseguire la cella successiva nel notebook per la distribuzione per
    acquisire il 100% del traffico.

![Uno screenshot di un computer Descrizione generata automaticamente con
bassa confidenza](./media/image39.png)

8.  Controllare che l'allocazione del traffico in tempo reale sia 100%
    nella pagina dei dettagli degli endpoint.

![Uno screenshot di un computer I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image40.png)

## **Esercizio 6: Testare la distribuzione**

1.  Eseguire la cella in Testare la distribuzione.

2.  Verificare l'output.

![](./media/image41.png)

3.  Segui ed esegui le celle rimanenti per eliminare il punto finale.

![](./media/image42.png)

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image43.png)

4.  Verificare lo stato dell'endpoint nella scheda Endpoint.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image44.png)

**Sommario**

In questo laboratorio, abbiamo imparato come

- Connettersi all'area di lavoro AML da Python SDK

- Creare un processo di regressione AutoML con la funzione factory
  'regression()'.

- Eseguire il training del modello usando AmlCompute inviando/eseguendo
  il processo di training di regressione AutoML

- Ottenere il modello e le previsioni dei punteggi con esso
