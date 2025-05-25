# **Lab 10 - Utilizzo della dashboard di intelligenza artificiale responsabile per migliorare le prestazioni dei modelli di apprendimento automatico**

**Obiettivo**

Questo laboratorio ha lo scopo di ottenere un apprendimento pratico su
come utilizzare la dashboard di intelligenza artificiale responsabile
per eseguire il debug dei modelli di apprendimento automatico al fine di
migliorare le prestazioni del modello per renderlo più equo, inclusivo,
sicuro, affidabile e trasparente.

In questo lab verrà illustrato come usare la sezione **Model Overview**
del dashboard Azure Responsible AI (RAI). Utilizzeremo le coorti create
dal laboratorio di analisi degli errori per indagare sul motivo per cui
il comportamento del modello è migliore in una coorte rispetto a
un'altra.

Durata prevista – 60 minuti

## **Esercizio 1: Preparazione delle risorse**

### Attività 1: Clonare il repository per questo lab

1.  Da un browser accedere al portale di Azure all' [indirizzo
    https://portal.azure.com](https://portal.azure.com)

2.  Aprire la **cloud shell** facendo clic sull'icona della shell cloud
    nel portale di Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.png)

3.  Nel prompt dei comandi di Azure Cloud Shell clonare il repository
    GitHub del progetto di **Diabetes Hospital Readmission** eseguendo
    il comando seguente.

> **+++Clone Git
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> In questo modo il contenuto del repository verrà clonato localmente.
>
> ![](./media/image2.png)

4.  Passare alla directory del progetto eseguendo il comando seguente.

**+++cd RAI-Diabetes-Hospital-Readmission-Classification+++**

### Attività 2: Accedere con l'interfaccia della riga di comando di Azure

1.  Dalla shell cloud, eseguire il comando seguente.

**az login**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image3.png)

2.  Apri la url nella console e digita il codice nel browser.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image4.png)

3.  Selezionare le credenziali di **Azure login**.

> ![Uno screenshot di un telefono Descrizione generata automaticamente
> con confidenza media](./media/image5.png)

4.  Fare clic su **Continue**.

> ![Uno screenshot di un errore del computer Descrizione generata
> automaticamente con confidenza media](./media/image6.png)

5.  Chiudere il browser e tornare al portale di Azure.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image7.png)

6.  I dettagli di accesso vengono visualizzati nella shell cloud.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image8.png)

7.  Impostare l'impostazione predefinita dell'ambiente sul di assigned
    **Resource group**.

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **Esercizio 2: Esecuzione di processi per l'addestramento del modello e la creazione del dashboard RAI**

1.  Eseguire il comando seguente per registrare il di **training
    dataset** nell'area di lavoro di Azure Machine Learning.

> **az ml data create -f cloud/train_data.yml**

L'asset di data viene creato e i dettagli vengono visualizzati nella
shell cloud.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image10.png)

2.  Eseguire il comando seguente per registrare il **testing dataset**
    nell'area di lavoro di Azure Machine Learning.

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  Creare un di **compute instance** per l'esecuzione dei processi.
    Quindi, copiare il nome di calcolo (ad esempio,
    ***compute-xxxxxxxxxxxx***) alla fine dell'esecuzione per usarlo in
    un secondo momento.

- Eseguire il comando seguente per **create** il **compute**.

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![Schermata di un computer Descrizione generata automaticamente con
confidenza media](./media/image12.png)

4.  Nel menu Cloud Shell fare clic sul riquadro **Open editor { }** per
    modificare alcuni file.

> ![Apri l'editor](./media/image13.png)

5.  Fare clic sulla cartella
    **RAI-Diabetes-Hospital-Readmission-classification** per espandere
    la directory.

![Espandi directory](./media/image14.png)

6.  Passare al file **cloud/training_job.yml.** Sostituire quindi il
    segnaposto per il nome di calcolo con il nome dell **compute
    instance name** copiato in precedenza.

![Aggiornamento del processo di formazione](./media/image15.png)

7.  Fare clic con il pulsante destro del mouse in un punto qualsiasi del
    file, quindi selezionare l'opzione **Save** per salvare il file.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con confidenza media](./media/image16.png)

8.  Quindi, vai al file **cloud/rai_dashboard_pipeline.yml**. Aggiorna
    quindi il segnaposto per il nome di calcolo con il nome dell
    **compute instance name** copiato in precedenza.

![Aggiornamento gasdotto Rai](./media/image17.png)

9.  Fare clic con il pulsante destro del mouse in un punto qualsiasi del
    file, quindi selezionare l'opzione **Save** per salvare il file.

10. Fare clic con il pulsante destro del mouse in un punto qualsiasi del
    file, quindi selezionare l'opzione **Quit** per chiudere la finestra
    dell'editor.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente con confidenza media](./media/image18.png)

11. Tornare al prompt dei comandi di Cloud Shell e inviare il processo
    per eseguire il training del modello. Attendere che il processo
    aggiorni lo stato di esecuzione su **Completed** durante il
    training. Copia il blocco di codice seguente per farlo.

> **run_id=$(az ml job create --name my_training_job -f
> cloud/training_job.yml --query name -o tsv)**
>
> **\# wait for job to finish while checking for status**
>
> **if \[\[ -z "$run_id" \]\]**
>
> **then**
>
> **echo "Job creation failed"**
>
> **exit 3**
>
> **Fi**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **if \[\[ -z "$status" \]\]**
>
> **then**
>
> **echo "Status query failed"**
>
> **exit 4**
>
> **Fi**
>
> **running=("Queued" "Starting" "Preparing" "Running" "Finalizing")**
>
> **while \[\[ ${running\[\*\]} =~ $status \]\]**
>
> **do**
>
> **sleep 8**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **eco $status**
>
> **done**
>
> **Nota:** se questo script non viene incollato correttamente, copiarlo
> e incollarlo manualmente
>
> **Nota:** l'esecuzione di questo script dovrebbe richiedere dai 3 ai 5
> minuti.
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image19.png)
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image20.png)

12. Facoltativamente, è possibile controllare lo stato del processo in
    esecuzione dai processi di [**Azure Machine Learning Studio
    (**](https://ml.azure.com/)https://ml.azure.com/) **-\> Jobs**

> ![Uno screenshot di un computer I contenuti generati dall'intelligenza
> artificiale potrebbero non essere corretti.](./media/image21.png)

13. Al termine del processo di training, registrare il modello nell'area
    di lavoro di Azure Machine Learning. Esegui il comando seguente per
    farlo.

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> Questo comando registra il modello nell'area di lavoro AML e fornisce
> i dettagli nella shell cloud, come negli screenshot seguenti.
>
> ![Un'immagine contenente testo, screenshot, software, software
> multimediale Descrizione generata
> automaticamente](./media/image22.png)
>
> ![Un'immagine contenente testo, carattere, screenshot Descrizione
> generata automaticamente](./media/image23.png)

14. Inviare la pipeline di lavoro per creare il **RAI dashboard**.
    Esegui il comando seguente per farlo.

az ml job create --file cloud/rai_dashboard_pipeline.yml

Questo comando invia il processo e la shell cloud viene popolata con la
fase iniziale della pipeline, che è il **Preparing** stato.

![Un'immagine contenente testo, screenshot, software Descrizione
generata automaticamente](./media/image24.png)

![Un'immagine contenente testo, screenshot, software, font Descrizione
generata automaticamente](./media/image25.png)

15. Accedere ad **Azure Machine Learning Studio** all
    <https://ml.azure.com/> per monitorare il processo della pipeline
    per la creazione del dashboard RAI.

16. Selezionare **Pipelines.** Per visualizzare l'avanzamento del
    processo di creazione del progetto RAI dashboard, fare clic sul
    **Display name** del processo.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image26.png)

17. L'esperimento sarà nello stato **Running**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

18. Al termine, lo stato cambia in **Completed** e viene creata la
    dashboard RAI.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image28.png)

19. Fare clic sulla scheda **Models** nella barra di navigazione a
    sinistra. Quindi fare clic sul nome del modello per aprire la pagina
    dei dettagli.

> ![](./media/image29.png)

20. Seleziona l' opzione **Responsible AI** nel menu in alto.

> ![Uno screenshot di un computer I contenuti generati dall'intelligenza
> artificiale potrebbero non essere corretti.](./media/image30.png)

21. Ora sei pronto per iniziare a utilizzare la **RAI dashboard**.

## **Esercizio 3: Analisi degli errori:**

La sezione Analisi degli errori della dashboard RAI aiuta a fornire una
distribuzione degli errori dei gruppi di funzionalità che contribuiscono
al tasso di errore del modello. Gli errori spesso non sono distribuiti
uniformemente tra i diversi sottogruppi di data e l'analisi degli errori
consente di identificare le funzionalità con i tassi di errore più
elevati.

### Attività 1: Trovare gli errori del modello:

In questa attività, esploreremo come utilizzare l'analisi degli errori
per trovare gli errori nel modello sottoposto a training per
identificare dove si trovano gli errori. Inoltre, impareremo come creare
coorti di data per indagare sul motivo per cui un modello ha prestazioni
scarse in alcune coorti e non in altre.

1.  Fare clic sul nome **Diabetes Hospital Readadmission.**

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image31.png)

2.  Selezionare l'opzione **Compute**.

![](./media/image32.png)

#### **Compito 1.1: Identificare e creare una coorte per il percorso ad albero con gli errori più elevati**

Per iniziare l'analisi, è possibile osservare che il nodo radice mostra
che su 994 data di test totali, sono state trovate 168 previsioni errate
durante la valutazione del modello.

1.  Trova il percorso dell'albero con il maggior numero di errori. Più
    scura è la tonalità rossa nel nodo, maggiore è il tasso di errore.

2.  Nel nostro caso il sentiero ad albero con il colore rosso più scuro
    è il nodo fogliare che si trova per secondo in basso a destra.

![](./media/image33.png)

3.  **Double click** su questo **node** per selezionare la **entire
    path** che conduce al nodo. In questo modo viene evidenziato il
    percorso e viene visualizzata la condizione della funzionalità per
    ogni nodo del percorso.

4.  Crea una coorte dal percorso selezionato facendo clic sul pulsante
    **Save as a new cohort** nella parte in alto a destra della sezione
    Analisi degli errori.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image34.png)

5.  Inserisci il **Cohort name** come **+++Err: Prior_Inpatient \>0;
    Num_meds \>11.50 & \<= 21.50+++**

Fare clic su **Save.**

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image35.png)

#### **Compito 1.2: Identificare e creare una coorte per il percorso ad albero con il minor numero di errori**

A scopo di contrasto, creare un'altra coorte con il percorso ad albero
con il minor numero di errori per vedere se possiamo ottenere
informazioni dettagliate sul motivo per cui il modello funziona bene in
una coorte rispetto a un'altra. Il **leaf node** con la condizione di
funzionalità **num_lab_procedures ≤ 56.50*,*** all'estrema sinistra
dell'albero, è il percorso dell'albero con il minor numero di errori.

1.  **Double-click** sul nodo.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image36.png)

2.  Fai clic su **Save as a new cohort**. Il **Filter** in questo set di
    data è: num_lab_procedures \<= 56,50, number_diagnoses \<= 6,50,
    prior_inpatient \<= 0,00.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image37.png)

3.  **Name** alla coorte: **+++Prior_Inpatient = 0; num_diagnoses \<=
    6,50; lab_procedures \<= 56,50+++** e fai clic su **Save**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image38.png)

#### **Attività 1.3: Utilizzare l'elenco delle funzioni per identificare la funzione principale che contribuisce agli errori del modello**

1.  Fare clic su **Feature list**.

![](./media/image39.png)

2.  L'elenco viene ordinato in base al contributo delle funzionalità
    agli errori. Più una funzionalità è alta in questo elenco, maggiore
    è l'importanza del suo contributo agli errori del modello.

3.  Nel nostro modello di riammissione ospedaliera per diabetologi, la
    **delle Feature list** indica che le seguenti caratteristiche sono
    tra le principali responsabili degli errori del modello.

    - Age

    - num_medications

    - Medicare

    - time_in_hospital

    - num_procedures

    - insulin

    - discharge_destination

### Attività 2: Trovare gli errori utilizzando la mappa termica

Dall'elenco delle funzionalità, **Age** è stato uno dei principali
contributori all'errore. Quindi, useremo la scheda Mappa di calore per
esplorare quale fascia di età dei pazienti sta guidando il modello a
prestazioni scarse.

1.  Selezionare **Heat map** in **Error Analysis**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image40.png)

2.  Nella scheda Mappa termica, selezionare **Age** nel menu a discesa
    **Rows: Feature 1** per vedere quale fattore gioca negli errori del
    modello.

3.  Dopo aver selezionato la **Age**, possiamo vedere come la dashboard
    abbia un'intelligenza integrata per dividere la funzione in diverse
    celle con le possibili condizioni.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image41.png)

2.  **However** il mouse su ogni cella, puoi vedere il numero di
    previsioni corrette e errate, la copertura degli errori e il tasso
    di errore per il gruppo di data rappresentato nella cella.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image42.png)

3.  La cella con **Over 60 years** ha **536** previsioni corrette e
    **126** errate. La copertura degli errori è del **73,81%** e il
    tasso di errore del **18,79%**

4.  La cella con **30-60 years** ha **273** previsioni corrette e **25**
    errate. La copertura degli errori è del **25,60%** e il tasso di
    errore del **13,61%.**

5.  La cella con **30 years or younger** ha **17** previsioni del
    modello corrette e **1** errata.

> Poiché la nostra osservazione mostra che la **Age** gioca un ruolo
> significativo nelle previsioni errate del modello, creeremo coorti per
> ogni gruppo di età per ulteriori analisi nel prossimo laboratorio.

#### ***Compito 2.1: Creare coorti in base alle fasce d'età***

1.  Fare clic sulla casella della percentuale della cella **Over 60
    years**. Vedrai un bordo blu attorno alla cella quadrata.

2.  Fai clic su **Save as a new cohort**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image43.png)

3.  Nella finestra di dialogo Salva come nuova coorte, immettere

    - Cohort name - **+++Age==Over 60 year+++**

Fare clic su **Save**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image44.png)

4.  Ripetere i passaggi 2 e 3 per creare una coorte per ciascuna delle
    altre due cellule Age.

- **Cohort \#4:** Name - **+++Age == 30-60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### Attività 3: Visualizzare gli elenchi delle coorti

1.  Fare clic sull' icona a forma di ingranaggio **Settings**
    nell'angolo in alto a destra della sezione Analisi errori.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image45.png)

2.  Si aprirà un **Cohort Settings window pane** con l'elenco di tutte
    le coorti create.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image46.png)

## Esercizio 4: Utilizzo di RAI per eseguire l'analisi del modello

In questo lab verrà illustrato come usare la sezione del **Model
Overview** del dashboard Azure Responsible AI (RAI). Utilizzeremo le
coorti create dal laboratorio di analisi degli errori per indagare sul
motivo per cui il comportamento del modello è migliore in una coorte
rispetto a un'altra.

## **Esercizio 4.1: Panoramica del modello**

### Attività 1: Esaminare e confrontare la tabella delle metriche delle prestazioni del modello

1.  Scorri verso il basso sotto l'Analisi degli errori per trovare la
    sezione Panoramica del modello.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image47.png)

2.  In Panoramica del modello selezionare il riquadro **Dataset
    Cohorts**. In questo modo vengono visualizzate le diverse coorti
    create in una tabella con le metriche del modello.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image48.png)

3.  Confronta la coorte con il maggior numero di errori **Err:
    Prior_Inpatient \> 0; Num_Meds \> 11 and ≤ 21,50** versetto il
    minimo errore **Prior_inpatient = 0; num_diagnose ≤ 6,50;
    lab_procedures \< 56,50.**

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image49.png)

4.  Passa il mouse sopra la linea del grafico per visualizzare i
    dettagli della misurazione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image50.png)

5.  Si noti che il punteggio di accuratezza per la **erroneous cohort**
    è 0,806, che è negativo. Il tasso di **False Positive** è **very
    low** e il valore di **False Negative** è **high**. Ciò significa
    che la maggior parte dei pazienti previsti dal modello ha un alto
    tasso di previsione dei pazienti che non saranno riammessi come
    riammessi entro 30 giorni di ritorno in ospedale.

> ![Una linea rossa in un foglio bianco Descrizione generata
> automaticamente](./media/image51.png)

6.  Successivamente, esamina le metriche per la **cohort** con il di
    **least errors** con un punteggio di accuratezza di 0,94, che è di
    gran lunga migliore del punteggio di accuratezza complessivo del
    modello con tutti i data. Tuttavia, questa coorte ha anche un basso
    di **False positive** a **0**.

![Un'immagine contenente testo, screenshot, riga, numero Descrizione
generata automaticamente](./media/image52.png)

### Attività 2: Esaminare il grafico della distribuzione di probabilità

1.  Scorri verso il basso per vedere la di **Probability distribution**.

2.  Il grafico della distribuzione della probabilità mostra la
    probabilità del modello che prevede se i pazienti nelle coorti
    saranno riammessi o non riammessi in ospedale entro 30 giorni.

3.  Confrontare la probabilità che i pazienti non vengano riammessi per
    tutte e 3 le coorti.

4.  Vedrai che la coorte **All data** con il set di data del test Tutti
    i pazienti mostra che la maggior parte dei pazienti non verrà
    riammessa in ospedale entro 30 giorni, con una probabilità mediana
    di pazienti non riammessi a 0,854 e un quartile superiore a 0,986,
    il che è buono.

5.  Successivamente, la coorte con il tasso di errore più alto: ***Err:
    Prior_Inpatient \>0; Num_meds \>11,50 & \<= 21,50***, mostra una
    probabilità leggermente inferiore a 0,89 e una mediana di 0,719.

6.  Infine, la coorte con il minor tasso di errore: ***Prior_Inpatient =
    0*; *num_diagnoses \<= 6,50*; *lab_procedures \<= 56,50***, mostrano
    che una probabilità di pazienti non riammessi ha una mediana di 0,90
    e un quartile superiore di 0,986.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image53.png)

7.  Per modificare il grafico in modo da mostrare la probabilità che i
    pazienti vengano riammessi per le 3 coorti, fare clic sul pulsante
    **Choose Label** sull'asse x.

8.  Selezionare il pulsante di opzione **Probability: Readmitted**. Nel
    riquadro della finestra popup.

9.  Quindi fare clic sul pulsante **Apply**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image54.png)

10. Confrontare la probabilità che i pazienti vengano riammessi per le 3
    coorti

> ![Uno screenshot di un grafico Descrizione generata automaticamente
> con bassa confidenza](./media/image55.png)

9.  Si vede che le 3 coorti hanno una probabilità di essere riammesse
    inferiore a 0,55. La coorte con il minor numero di errori del
    modello ha la probabilità più bassa di 0,179. La coorte con il
    maggior numero di errori ha la probabilità più alta a 0,543.

### Attività 3: Esaminare il grafico di visualizzazione delle metriche

A questo punto, è possibile ottenere una comprensione più approfondita
delle prestazioni del modello passando al riquadro Visualizzazioni
metriche.

1.  Fare clic sulla scheda **Metric Visualizations**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image56.png)

2.  Per scegliere un'altra metrica, fai clic su **Choose metric**
    sull'asse x per scegliere **di Precision score** dall'elenco delle
    altre metriche disponibili. Quindi fare clic sul pulsante **Apply**.

> **Nota**: Poiché il modello addestrato è un problema di
> classificazione, il dashboard RAI visualizzerà solo le metriche di
> classificazione.
>
> ![](./media/image57.png)

3.  Esaminando il grafico, si noterà che le prestazioni del modello per
    tutte le coorti di data di test e le coorti errate sono corrette a
    ~70% delle volte.

4.  Il tasso di **Precision score** per la **least erroneous cohort** è
    **0,94** per i pazienti senza precedente ricovero in ospedale e il
    numero di diagnosi è inferiore a 7. Ciò è coerente con il punteggio
    di precisione.

![Uno screenshot di un computer Descrizione generata automaticamente con
confidenza media](./media/image58.png)

5.  Infine, modificare la metrica in **Recall** per verificare in che
    misura il modello è stato in grado di prevedere correttamente che i
    pazienti nelle coorti verranno riammessi in ospedale entro 30
    giorni.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image59.png)

6.  Il richiamo mostra che la del **model’s prediction** era del
    **correct less than 25%** delle volte per tutte le coorti di
    pazienti riammessi. Ciò rivela che le previsioni del modello non
    sono corrette la maggior parte delle volte quando si cerca di
    prevedere i pazienti che verranno riammessi entro 30 giorni.

![Uno screenshot di un grafico Descrizione generata automaticamente con
bassa confidenza](./media/image60.png)

### Compito 4: Osservare la matrice della confusione

La matrice di confusione è utile per verificare la velocità del modello
facendo correttamente la previsione corretta. Ciò rivelerà quanto bene
il modello sta imparando per i casi in cui il paziente viene riammesso
in ospedale entro 30 giorni rispetto a Non riammesso .

1.  Fare clic sulla scheda di **Confusion matrix** .

&nbsp;

2.  Noterai che il **model** sta funzionando **better** con i pazienti
    che non sono **Not Readmitted** rispetto ai **Readdmitted**.

3.  Il numero di falsi negativi deve essere inferiore a quello di vero
    negativo. Ciò significa che tra tutti i data dei pazienti, il
    modello è stato in grado di prevedere correttamente solo 24 pazienti
    che sarebbero stati riammessi in ospedale in \< 30 giorni.

- Il numero di Veri Positivi (TP) è: **802**

- Il numero di falsi negativi (FN) è: **159**

- Il numero di falsi positivi (FP) è: **9**

- Il numero di Veri Negativi (TN) è: **24**

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image61.png)

## **Esercizio 2: Coorte di funzionalità**

Poiché la coorte con l'errore più alto ha pazienti con il numero di
*Prior_Inpatient \> 0* giorni e il numero di farmaci compreso tra 11 e
22 era dove il modello aveva un tasso di errore più elevato, dare
un'occhiata più da vicino al *Prior_Inpatient* e *Num_medications*
aiuterà a isolare dove ci sono problemi. Per questo laboratorio,
analizzeremo solo *Prior_Inpatient*.

1.  Fai clic sulla scheda di **Feature Cohorts**.

2.  Nel menu a discesa **Feature(s),** scorrere l'elenco verso il basso
    e selezionare la casella di controllo **prior_inpatient**. Verranno
    visualizzate 3 diverse coorti di funzionalità e le metriche delle
    prestazioni del modello.

&nbsp;

3.  La coorte **prior_inpatient *\< 3*** ha una dimensione del campione
    di **943**. Ciò significa che la maggior parte dei pazienti nei data
    del test è stata ricoverata in ospedale meno di 3 volte in passato.
    Il di **model’s accuracy rate** per questa coorte è **0,838**, il
    che è buono.

4.  Solo 39 pazienti dai data del test rientrano nella coorte
    **prior_inpatient *≥ 3 and \< 6***. Il tasso di precisione del
    modello è **0,692**, il che non è buono.

5.  Infine, solo 12 pazienti dai data del test hanno un precedente
    ricovero maggiore o uguale a 6 giorni. La **model accuracy** di
    **0,75** per questa coorte è ok.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image62.png)

### 

### Attività 1: Distribuzione di probabilità delle funzionalità

Analogamente alla coorte del set di data, è possibile visualizzare la
"distribuzione di probabilità".

1.  Si può vedere che minore è il numero di ricoveri prior_inpatient del
    paziente diabetico, più è probabile che il paziente non venga
    riammesso entro 30 giorni.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image63.png)

### Attività 2: Visualizzazioni delle metriche delle funzionalità

1.  Selezionare **Metrics visualization**. Sull'asse x, fare clic sul
    pulsante **Choose metric**. Selezionare quindi la metrica di
    **Precision score**.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image64.png)

2.  Si vede che il punteggio di precisione per i pazienti con
    **prior_inpatient \< 3** è 0,40, il che è molto negativo. Ciò
    significa che di tutte le previsioni fatte dal modello, solo il 40%
    era corretto per questa coorte.

> ![Un grafico a barre in bianco e blu Descrizione generata
> automaticamente](./media/image65.png)

3.  Il punteggio di precisione per le altre 2 coorti è buono.

4.  Selezionare quindi **Recall score** per l'asse x.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image66.png)

5.  Al contrario, vedrai che il punteggio di richiamo per i pazienti con
    **prior_inpatient \< 3** è 0,013. Ciò significa che, per la maggior
    parte dei pazienti nei data del test, il modello ha difficoltà a
    prevedere correttamente se il paziente sarà riammesso entro 30
    giorni o meno.

> ![Un'immagine contenente screenshot, software, riga, testo Descrizione
> generata automaticamente](./media/image67.png)
>
> **Sommario**
>
> Questo laboratorio mostra come le metriche tradizionali delle
> prestazioni del modello (ad esempio, accuratezza, richiamo, matrice di
> confusione, ecc.) siano ancora molto importanti. Combinando gli
> approfondimenti RAI e la metrica di performance tradizionale, la
> dashboard ci offre uno strumento olistico per analizzare ed eseguire
> il debug del modello a un livello più granulare.
