# Lab 03 - Sviluppare e registrare un set di funzionalità con l'archivio di funzionalità gestite ed eseguire il training di modelli utilizzando le funzionalità

In questa esercitazione viene descritto come creare una specifica del
set di funzionalità con trasformazioni personalizzate. Utilizza quindi
tale set di funzionalità per generare data di addestramento, abilitare
la materializzazione ed eseguire un backfill. La materializzazione
calcola i valori delle funzionalità per una finestra delle funzionalità
e quindi archivia tali valori in un archivio di materializzazione. Tutte
le query di funzionalità possono quindi utilizzare tali valori
dall'archivio materializzazione.

Senza materializzazione, una query del set di funzionalità applica le
trasformazioni all'origine in tempo reale, per calcolare le funzionalità
prima di restituire i valori. Questo processo funziona bene per la fase
di prototipazione. Tuttavia, per le operazioni di training e inferenza
in un ambiente di produzione, è consigliabile materializzare le
funzionalità, per una maggiore affidabilità e disponibilità.

Durata prevista – 50 minuti

## Esercizio 1: Assegnare i ruoli richiesti:

1.  Nella home page del portale di Azure selezionare il **Resource
    group** assegnato nella scheda **Resources**. Nel riquadro sinistro
    selezionare **Access control(IAM**). Fare clic sull'elenco a discesa
    accanto ad **Add** e selezionare **Add rol**e **assignment**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.png)

2.  Cercare +++**AzureML Data Scientist**+++ e selezionarlo. Fare clic
    su **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image2.png)

3.  Nella scheda Membri, fai clic su **+ Select members**, cerca il suoi
    **User name,** +++@lab.CloudPortalCredential(User1).Username+++.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image3.png)

4.  Seleziona il suoi **Username** e quindi fai clic sul pulsante
    **Select**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image4.png)

5.  Fai clic su **Review + assign** nelle prossime 2 schermate.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image5.png)

6.  Il messaggio di assegnazione di ruolo aggiunto viene ottenuto al
    termine dell'assegnazione.

7.  Ripetere lo stesso set di passaggi per aggiungere i ruoli
    +++**Storage Blob Data Reader**+++ e +++**Storage Blob Data
    Contributor +++**.

## Esercizio 2: Sviluppare un set di funzionalità ed eseguire la registrazione con l'archivio funzionalità gestite

Questa esercitazione è la prima parte della serie di esercitazioni
sull'archivio funzionalità gestite. Qui imparerai come:

- Creare una nuova risorsa minima dell'archivio funzionalità.

- Sviluppare e testare localmente un set di funzionalità con
  funzionalità di trasformazione delle funzionalità.

- Registrare un'entità dell'archivio funzionalità con l'archivio
  funzionalità.

- Registrare il set di funzionalità sviluppato con l'archivio
  funzionalità.

- Generare un DataFrame di training di esempio utilizzando le
  funzionalità create.

- Abilita la materializzazione offline sui set di funzionalità e riempi
  i data delle funzionalità.

### Attività 1: Preparare l'ambiente

1.  Nel riquadro sinistro di Azure Machine Learning Studio selezionare
    **Notebooks** in **Authoring**. Fare clic sui tre punti accanto al
    nome utente e selezionare **Upload folder**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

2.  Sfoglia e seleziona la cartella del **featurestore** da
    **C:\Labfiles** e fai clic su **Upload**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.png)

3.  Passare a **featurestore-\> notebooks-\>sdk_and_cli** e aprire il
    notebook 1.Develop-feature-set-and-register.ipynb

![](./media/image8.png)

4.  Selezionare **Serverless Spark Compute** in **Compute**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

5.  Selezionare **Configure session** per configurare la sessione con i
    prerequisiti.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

6.  Seleziona **Python packages-\> Upload Conda file**. Fare clic su
    **Browse** e selezionare **conda.yml** da **C:\Labfiles,** quindi
    selezionare **Apply**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.png)

7.  **Execute** la prima cella del notebook. Questo installerà tutte le
    **dependencies** e completerà la sua esecuzione. Il completamento
    dell'operazione richiederà circa **10 minuti** .

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image12.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.png)

8.  Una volta avviata la sessione Spark, sostituisci il **User name**
    con il suoi nome utente ed esegui la cella successiva

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

![Uno screenshot di un errore del computer Descrizione generata
automaticamente](./media/image15.png)

9.  Eseguire le 3 celle successive per configurare l'interfaccia della
    riga di comando di Azure.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.png)

10. Nella cella successiva e seguire i passaggi nell' **output** per
    accedere ad **Azure**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image18.png)

### Attività 2: Creare un archivio funzionalità minimo

1.  **Execute** la **first** cella per impostare il nome, la posizione e
    altri valori per l'archivio funzionalità.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image19.png)

2.  **Execute** la cella successiva che **creates the feature store**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image20.png)

3.  La cella successiva **initializes AzureML feature store core SDK
    client. Execute it.**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image21.png)

### Attività 3: Prototipare e sviluppare un set di funzionalità di aggregazione in sequenza delle transazioni in questo notebook

1.  **Execute** la prima cella in questa sezione per esplorare i data di
    origine delle **transactions**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

2.  Eseguire la seconda cella per **Develop a transactions feature set**
    in locale.

![Uno screenshot di un codice informatico Descrizione generata
automaticamente](./media/image23.png)

3.  Eseguire la cella successiva per **generate a spark dataframe**
    dalla specifica del set di funzionalità.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.png)

4.  Per registrare la specifica del set di funzionalità con l'archivio
    funzionalità, è necessario salvarla in un formato specifico. Si
    prega di ispezionare le transazioni generate FeaturesetSpec: Apri
    questo file dall'albero dei file per vedere la specifica:
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

Eseguire la cella successiva da esportare come specifica del set di
funzionalità.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image25.png)

### Attività 4: Registrare un'entità dell'archivio funzionalità

1.  L'entità consente di applicare le procedure consigliate in base alle
    quali le stesse definizioni di chiave di join vengono utilizzate tra
    i set di funzionalità che utilizzano le stesse entità logiche.
    Eseguire la cella per registrare un'entità dell'archivio
    funzionalità.

> ![Schermata di un computer Descrizione generata
> automaticamente](./media/image26.png)

### Attività 5: Registrare il set di funzionalità della transazione con l'archivio funzionalità

1.  Dal portale di Azure (+++https://portal.azure.com+++) passare all'
    **Storage account** che inizia con il di **featureset** nel gruppo
    di risorse assegnato**.**

> ![Uno screenshot di un computer I contenuti generati dall'intelligenza
> artificiale potrebbero non essere corretti.](./media/image27.png)

2.  Nel riquadro sinistro selezionare Controllo di accesso (IAM).
    Selezionare **Add** -\> **Add role assignment**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image28.png)

3.  Cercare e selezionare +++**Storage Blob Data Reader**+++.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image29.png)

4.  Completare l'assegnazione del ruolo in modo simile a quelle eseguite
    nell'Esercizio 1.

5.  Analogamente, aggiungere il ruolo +++**Storage Blob Data
    Contributor**+++.

6.  Tornare ad Azure Machine Learning Studio.

7.  È possibile registrare una risorsa del set di funzionalità con
    l'archivio funzionalità in modo da poterla condividere e
    riutilizzare con altri utenti. Sono inoltre disponibili funzionalità
    gestite come il controllo delle versioni e la materializzazione.
    L'asset del set di funzionalità fa riferimento alla specifica del
    set di funzionalità creata in precedenza e a proprietà aggiuntive
    come le impostazioni di versione e materializzazione.

8.  **Execute** la cella successiva per **register the transaction
    feature set** con l'archivio funzionalità.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image30.png)

### Attività 6: Esplorare l'interfaccia utente dell'archivio funzionalità

1.  Aprire una nuova scheda nel browser e passare alla pagina di
    destinazione globale di Azure ML all'indirizzo
    +++https://ml.azure.com/home+++.

2.  Fai clic su **Feature stores** nel menu di navigazione a sinistra.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image31.png)

3.  Fare clic sul **featurestore**.

**Nota:** la creazione e l'aggiornamento delle risorse dell'archivio
funzionalità (set di funzionalità ed entità) è possibile solo tramite
SDK e CLI. È possibile utilizzare l'interfaccia utente per
cercare/sfogliare l'archivio funzionalità.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image32.png)

### Attività 7: Generare un frame di data di training utilizzando le funzionalità registrate

1.  Iniziamo esplorando i data di osservazione. I data di osservazione
    sono in genere i data principali utilizzati nei data di
    addestramento e inferenza. Questo viene quindi unito ai data delle
    funzionalità per creare i data di addestramento completi. I data di
    osservazione sono i data acquisiti durante il periodo dell'evento:
    in questo caso contengono i data principali della transazione, tra
    cui l'ID della transazione, l'ID del conto, l'importo della
    transazione. In questo caso, poiché è per l'addestramento, ha anche
    la variabile target aggiunta (is_fraud).

2.  **Execute** il cel land osservare i data di uscita.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image33.png)

3.  **Execute** la cella successiva per ottenere il **registered feature
    set** ed **list its features**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image34.png)

4.  **Execute** la cella successiva per **print** i **sample values.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image35.png)

5.  **Execute** la cella successiva. In questo passaggio verranno
    **select features** che si desidera includere nei **training data**
    e verrà usato l'SDK dell'archivio funzionalità per generare i data
    di training.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image36.png)

6.  Eseguire la cella successiva per generare il frame di data di
    training utilizzando i data delle funzionalità e i data di
    osservazione.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image37.png)

### Attività 8: Abilitare la materializzazione offline nel set di funzionalità delle transazioni

Una volta abilitata la materializzazione in un set di funzionalità, è
possibile eseguire il recupero informazioni o programmare processi di
materializzazione ricorrenti.

1.  Esegui la cella successiva per impostare
    spark.sql.shuffle.partitions nel file yaml in base alla dimensione
    dei data della funzione

2.  La configurazione spark spark.sql.shuffle.partitions è un parametro
    OPTIONAL che può influire sul numero di file parquet generati (al
    giorno) quando il set di funzionalità viene materializzato
    nell'archivio offline. Il valore predefinito di questo parametro
    è 200. La procedura consigliata consiste nell'evitare di generare
    molti file di parquet di piccole dimensioni. Se il recupero delle
    funzionalità offline risulta lento dopo che il set di funzionalità
    si è materializzato, vai alla cartella corrispondente nell'archivio
    offline per verificare se il problema è di avere troppi file parquet
    piccoli (al giorno) e regola il valore di questo parametro di
    conseguenza.

**Nota:** i data di esempio utilizzati in questo notebook sono di
piccole dimensioni. Quindi questo parametro è impostato su 1 nel file
featureset_asset_offline_enabled.yaml.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image38.png)

3.  La materializzazione è il processo di calcolo dei valori delle
    funzionalità per una data finestra delle funzionalità e la loro
    memorizzazione in un archivio di materializzazione. La
    materializzazione delle funzionalità ne aumenterà l'affidabilità e
    la disponibilità. Tutte le query di funzionalità utilizzeranno i
    valori materializzati dall'archivio di materializzazione. In questo
    passaggio viene eseguito un backfill una tantum per una finestra di
    funzionalità di 18 mesi.

4.  La seguente cella di codice **materialize data** in base allo stato
    corrente Nessuno o Incompleto per la finestra delle funzionalità
    definita. **Execute** it.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image39.png)

5.  Stampiamo i **print sample data** dal set di funzionalità nella
    cella successiva. **Execute** it. È possibile notare dalle
    informazioni di output che i data sono stati recuperati
    dall'archivio di materializzazione. get_offline_features()
    utilizzato per recuperare i data di addestramento/inferenza
    utilizzerà anche l'archivio di materializzazione per impostazione
    predefinita.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

## Esercizio 3: Sperimentare ed eseguire il training dei modelli utilizzando le funzionalità

In questo taccuino si apprenderà come:

- Prototipare una nuova specifica del set di funzionalità dei conti,
  utilizzando i valori precalcolati esistenti come funzionalità. Quindi,
  registrare la specifica del set di funzionalità locale come set di
  funzionalità nell'archivio funzionalità. Questo processo è diverso
  dalla prima esercitazione, in cui è stato creato un set di
  funzionalità con trasformazioni personalizzate.

- Selezionare le feature per il modello dai set di feature delle
  transazioni e dei conti e salvarle come specifica di recupero delle
  feature.

- Eseguire una pipeline di training che usa la specifica di recupero
  delle funzionalità per eseguire il training di un nuovo modello.
  Questa pipeline usa il componente di recupero delle funzionalità
  integrato per generare i data di training.

### Attività 1: Configurare l'ambiente

1.  Nel riquadro Notebook aprire il notebook **Experiment and train
    models using features**. 

2.  Fare clic su **Configure session** e caricare il file **conda.yaml**
    in modo simile a come è stato fatto per il notebook precedente.

3.  **Execute** la **first cell** per avviare la sessione. Ci vorranno
    circa 10 minuti.

![Un oggetto rettangolare bianco con testo verde Descrizione generata
automaticamente](./media/image41.png)

4.  Nella cella successiva, sostituire il segnaposto per **\<
    your_user_alias \>** con il proprio **user name** nella struttura
    delle cartelle ed **execute** la cella.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image42.png)

5.  **Execute** le **3** celle successive per **setup CLI.**

6.  La cella successiva inizializza le variabili dell'area di lavoro del
    progetto. **Execute** per **initialize the variables**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image43.png)

7.  La cella successiva inizializza le variabili dell'archivio
    funzionalità. **Execute it.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image44.png)

8.  Eseguire la cella successiva per **Initialize the feature store
    consumption client.**

![Uno screenshot dello schermo di un computer Descrizione generata
automaticamente](./media/image45.png)

### Attività 2: Creare un set di funzionalità degli account in locale da data precalcolati

Per l'onboarding delle funzionalità precalcolate, è possibile creare una
specifica del set di funzionalità senza scrivere alcun codice di
trasformazione. La specifica del set di funzionalità è una specifica per
sviluppare e testare un set di funzionalità in un ambiente completamente
locale/di sviluppo senza connettersi ad alcun featuretore. In questo
passaggio verrà creata la specifica del set di funzionalità in locale e
verranno analizzati i valori da essa.

1.  Esegui la cella seguente per **explore the source data fo
    accounts.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image46.png)

2.  Eseguire la cella successiva per **create accounts feature set
    spec** in locale da queste funzionalità precalcolate.

![Schermata di un codice informatico Descrizione generata
automaticamente](./media/image47.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image48.png)

3.  **Execute** la cella successiva per **generate a spark dataframe**
    dalla specifica del set di funzionalità.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image49.png)

4.  Per registrare la specifica del set di funzionalità con l'archivio
    funzionalità, è necessario salvarla in un formato specifico. Azione:
    Dopo aver eseguito la cella sottostante, ispezionare gli account
    generati FeatureSetSpec: Aprire questo file dall'albero dei file per
    visualizzare le specifiche:
    featurestore/featuresets/accounts/spec/FeatureSetSpec. **Execute**
    la cella successiva.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image50.png)

### Attività 3: Sperimentare le funzionalità non registrate in locale e registrarsi con l'archivio funzionalità quando si è pronti

Quando si sviluppano funzionalità, è possibile eseguire il
test/convalida in locale prima di registrarsi nell'archivio funzionalità
o eseguire pipeline di training nel cloud. In questo passaggio verranno
generati i data di training per il modello ML dalla combinazione di
funzionalità di un set di funzionalità locale non registrato (account) e
di un set di funzionalità registrato nell'archivio funzionalità
(transazioni).

1.  **Execute** la cella successiva per **select features** per il
    **model.**

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image51.png)

2.  **Execute** le 2 celle successive per **generate training data** in
    locale.

![Un primo piano di un codice informatico Descrizione generata
automaticamente](./media/image52.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image53.png)

3.  **Execute** la cella successiva per **register the accounts
    featureset** con il featurestore. Dopo aver sperimentato diverse
    definizioni di funzionalità in locale e averle testate di integrità,
    è possibile registrarle nell'archivio funzionalità. A tale scopo,
    verrà registrata una definizione di asset del set di funzionalità
    con l'archivio funzionalità.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image54.png)

4.  **Execute** le 2 celle successive per ottenere il set di
    funzionalità registrato e il test di sanità mentale.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image55.png)

### Attività 4: Eseguire l'esperimento di training

1.  Eseguire la cella successiva per individuare le funzionalità
    dell'SDK.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image56.png)

2.  Nei passaggi precedenti sono state selezionate le funzionalità da
    una combinazione di set di funzionalità non registrate e registrate
    per la sperimentazione e il test locali. Ora sei pronto per
    sperimentare nel cloud. Il salvataggio delle funzionalità
    selezionate come specifica di recupero delle funzionalità e
    l'utilizzo nel flusso mlops/cicd per l'addestramento/inferenza
    aumenta l'agilità nella distribuzione dei modelli.

3.  **Execute** la cella successiva per **select features for the
    model**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image57.png)

4.  **Execute** la cella successiva ed esportate le feature selezionate
    come **feature-retrieval spec**.

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image58.png)

### Attività 5: Eseguire il training nel cloud usando le pipeline e registrare il modello se soddisfacente

In questo passaggio si attiverà manualmente la pipeline di training. In
uno scenario di produzione, questa operazione potrebbe essere attivata
da una pipeline ci/cd in base alle modifiche apportate alla specifica di
recupero delle funzionalità nel repository di origine.

1.  **Execute** la cella successiva per **run the training pipeline.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image59.png)

![Uno screenshot di un programma per computer Descrizione generata
automaticamente](./media/image60.png)

2.  Dal riquadro sinistro dello studio, fai clic con il pulsante destro
    del mouse su **Jobs** e apri in una nuova scheda. Seleziona
    l'esperimento, **training_on_fraud_model**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image61.png)

3.  Clicca sul **training job** ed esplora i dettagli. L'esperimento
    dovrebbe richiedere dai 5 ai 15 minuti per essere completato.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image62.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image63.png)

4.  Attendi il completamento. Una volta completato, seleziona **Models**
    dal riquadro di sinistra. Seleziona **fraud_model** dall'elenco.
    Questo è il modello che è stato creato ora.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image64.png)

5.  Selezionare la scheda **Feature sets**. Qui puoi vedere sia
    **transactions** che i **accounts featuresets** da cui dipende
    questo modello.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image65.png)

6.  Apri la **feature store UI** in +++https://ml.azure.com/home+++.
    Selezionare **Feature stores**-\> **featurestore**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image66.png)

7.  Selezionare **Feature sets** nel riquadro a sinistra, quindi
    selezionare uno dei **feature sets**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image67.png)

8.  Fare clic sulla scheda **Models**. È possibile visualizzare l'elenco
    dei modelli che utilizzano i set di funzioni (determinati dalla
    specifica di recupero delle funzioni al momento della registrazione
    del modello).

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image68.png)

Sommario:

In questo lab si è appreso come sviluppare e registrare un set di
funzionalità con l'archivio di funzionalità gestite e il training di
modelli utilizzando le funzionalità.
