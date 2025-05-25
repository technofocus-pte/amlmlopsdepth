# **Lab 05 - Previsione della domanda con l'apprendimento automatico senza codice nello studio Azure Machine Learning**

**Obiettivo**

In questo lab si apprenderà come creare un modello di previsione delle
serie temporali senza scrivere una singola riga di codice usando Machine
Learning automatizzato in Azure Machine Learning Studio. Questo modello
prevede la domanda di noleggio di un servizio di bike sharing.

In questo lab non si scriverà alcun codice. Utilizzerai l'interfaccia di
Studio per eseguire la formazione.

Durata prevista – 60 minuti

## **Esercizio 1: Preparare l'ambiente**

### **Attività 1: Avviare l'area di lavoro AML**

1.  Accedere al portale di Azure, +++**https://portal.azure.com**+++ se
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

## **Esercizio 2: Creare un processo di Machine Learning automatizzato**

1.  In Azure Machine Learning Studio fare clic su **Automated ML** nella
    sezione **Author** nel riquadro sinistro.

2.  Selezionare **+ New Automated ML job.**

![](./media/image4.png)

### **Attività 1: Creare un asset di data**

1.  Assegna il nome all'esperimento come +++**experiment_forecast**+++,
    accetta le altre impostazioni predefinite e seleziona **Next**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image5.png)

2.  Seleziona **Select task type** come **Time series forecasting** e
    quindi fai clic su **+ Create.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

3.  Nella pagina **Create data asset** specificare i dettagli seguenti.

    1.  Name – +++**bikedata**+++

    2.  Type – Tabular

> Fare clic su **Next**.
>
> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image7.png)

4.  Nel Origine **Data source**, selezionare **From local files** e fare
    clic su **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

5.  Nel **Destination storage type** selezionare workspaceblob e
    selezionare **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

6.  Nella selezione File o cartella, seleziona **Upload files** e
    seleziona **bike-no.csv** dalla cartella **C:\Labfiles** e fai clic
    su **Next**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

7.  Verificare che il modulo **Settings and preview** sia popolato come
    indicato di seguito e selezionare **Next**.

[TABLE]

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.png)

8.  Il modulo **Schema** consente un'ulteriore configurazione dei data
    per questo esperimento. Per questo esempio, selezionare la **toggle
    switch** in modo che sia disattivato per il

    1.  **casual** e

    2.  colonne **registered**.

> Fare clic su **Next**.

Queste colonne sono una suddivisione della colonna **cnt**, quindi, non
le includiamo.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image12.png)

9.  Nel modulo **Review** verificare le informazioni e fare clic su
    **Create** per completare la creazione dell'asset di data.

> ![](./media/image13.png)

10. Nella pagina **Create a new Automated ML job page,** viene
    visualizzato un messaggio di **success** per la creazione dell'asset
    di data.

11. Seleziona i **bikedata** appena creati e fai clic su **Next.**

> **Nota: Refresh** il riquadro degli asset di data se bikedata non
> viene visualizzato.
>
> ![](./media/image14.png)

### **Attività 2: Configurare il processo**

1.  Nella pagina **Task settings**, fornisci i dettagli seguenti e
    seleziona **View additional configuration settings**.

> Target column– **cnt(Integer)**
>
> Time column **– data (Date)**
>
> **Deseleziona Rilevamento automatico orizzonte di previsione** e
> fornisci il valore come +++**14**+++.
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image15.png)

2.  Nel riquadro Configurazione aggiuntiva, fornisci i dettagli seguenti
    e fai clic su **Save**.

- Primary metric – **Normalized root mean squared error**

- Explain best model – **Enable**

- Blocked algorithms- **Extreme Random Trees**

> Espandi le impostazioni di previsione aggiuntive

- Autodetect Forecast target lags - **Unselected**

- Autodetect Target rolling window size- **Unselected**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image16.png)

3.  Seleziona **Limits** e inserisci +++**60**+++ per il campo
    **Experiment timeout(minutes**).

![Uno screenshot di un test I contenuti generati dall'intelligenza
artificiale potrebbero non essere corretti.](./media/image17.png)

4.  Selezionare i valori seguenti in **Validate and test** e quindi
    selezionare **Next**.

> Validataon type – **k-fold cross-validataon**
>
> Number of cross validataons – **5**
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image18.png)

5.  Selezionare **automl-compute** (quello creato nel lab precedente).
    Fare clic su **Next.**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image19.png)

6.  Esaminare i dettagli e selezionare **Submit training job**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image20.png)

7.  La pagina di stato mostra lo stato iniziale come **Running.**
    Continua ad aggiornare la pagina per conoscere lo stato.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image21.png)

8.  Una volta completato il training, lo stato cambia in **Completed**.

**Nota:** la formazione dura dai 30 ai 45 minuti per essere completata.

## **Esercizio 3: Esplorare i modelli**

1.  Passare alla scheda **Models** per visualizzare gli algoritmi
    (modelli) testati. Per impostazione predefinita, i modelli vengono
    ordinati in base al punteggio della metrica man mano che vengono
    completati.

2.  Per questa esercitazione, il modello che ottiene il punteggio più
    alto in base alla metrica **di Normalized root mean squared error**
    scelta si trova all'inizio dell'elenco.

3.  Mentre attendi il completamento di tutti i modelli dell'esperimento,
    seleziona il **Algorithm name** di un modello completato per
    esaminarne i dettagli sulle prestazioni.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image22.png)

4.  Fare clic sulla **Overview** e visualizzarne i dettagli.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image23.png)

5.  Fai clic sulla scheda **Metrics** ed esplora i dettagli.

> ![Uno screenshot di un computer Descrizione generata automaticamente
> con confidenza media](./media/image24.png)
>
> **Importante:** continua a eseguire il laboratorio successivo mentre
> questo corso di formazione viene completato. Riprendi questo
> laboratorio da qui, una volta completata la formazione.
>
> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image25.png)

## **Esercizio 4: Identificare il modello migliore**

Machine Learning automatizzato in Azure Machine Learning Studio consente
di distribuire il modello migliore come servizio Web in pochi passaggi.
L'implementazione è l'integrazione del modello in modo che possa
prevedere nuovi data e identificare potenziali aree di opportunità.

1.  Una volta completato il processo, torna alla pagina del lavoro
    principale selezionando **the job name** nella parte superiore dello
    schermo.

![](./media/image26.png)

2.  Nella sezione Best model summary, viene selezionato il modello
    migliore nel contesto di questo esperimento in base alla metrica di
    **Normalized root mean squared error metric.**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

3.  Fai clic sul nome dell'algoritmo per aprirlo ed esplorare i
    dettagli.

4.  Il modello può essere distribuito anche come servizio Web**.**

**Sommario**

In questo lab è stato usato Machine Learning automatizzato nello studio
di Azure Machine Learning per creare un modello di previsione delle
serie temporali che stima la domanda di noleggio di bike sharing.
