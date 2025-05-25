# Lab 07: Sviluppare e testare il flusso di prompt da Azure Machine Learning Studio

**Obiettivo:**

In questo lab verranno illustrati i principali percorsi utente relativi
all'uso del flusso di prompt in Azure Machine Learning Studio. Si
apprenderà come abilitare il flusso di prompt nell'area di lavoro di
Azure Machine Learning, creare e sviluppare un flusso di prompt, testare
e valutare il flusso e quindi distribuirlo nell'ambiente di produzione.

Durata prevista – 60 minuti

## Attività 1: Preparazione delle risorse di Azure

### Attività 1.1: Creare un'area di lavoro di Azure Machine Learning

Questa attività è incentrata sulla creazione di un'area di lavoro di
Azure Machine Learning. Scoprirai come impostare uno spazio di lavoro
dedicato per organizzare e gestire efficacemente i propri progetti di
machine learning. Questa area di lavoro funge da hub centrale per la
collaborazione, la sperimentazione e la distribuzione.

1.  Accedere al portale di Azure al numero
    +++<https://portal.azure.com>+++ e accedere con le credenziali del
    tenant di amministratore.

2.  Nella home page del portale di Azure selezionare **+ Create a
    resource**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image1.png)

3.  Nella pagina **Create a resource** usare la barra di ricerca per
    trovare +++Azure Machine Learning+++ e selezionare **Azure Machine
    Learning**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image2.png)

4.  In **Marketplace** fare clic sull' **Create dropdown** e selezionare
    **Azure Machine Learning**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image3.png)

5.  Fornisci le seguenti informazioni per configurare la tua nuova area
    di lavoro:

    - **Subscription**: selezionare la **assigned Azure subscription**

    - **Resource group**: selezionare il **assigned Resource Group**.

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image4.png)
>
> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: selezionare la regione più vicina **(North Central US** è
  selezionato qui)

&nbsp;

- **Container registry: Select Create new. Enter +++azuremlcr@lab.
  LabInstanceId+++**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image5.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image6.png)

6.  Al termine della configurazione dell'area di lavoro, selezionare
    **Review + Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image7.png)

7.  Una volta superata la convalida, fare clic su **Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image8.png)

8.  Fare clic su **Go to resource,** per visualizzare la nuova area di
    lavoro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image9.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**,
    selezionare **Launch studio** in **Work with your model in Azure
    Machine Learning Studio**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image10.png)

### Attività 1.2: Creare un calcolo

Questa attività illustra la creazione di una risorsa di calcolo in
Azure. Esplorerai diverse opzioni di calcolo, ad esempio macchine
virtuali o cluster di calcolo gestiti, e capirai come configurare ed
effettuare il provisioning delle risorse per eseguire carichi di lavoro
di Machine Learning in modo efficiente.

1.  Dopo l' apertura di **Azure Machine Learning Studio**, fare clic su
    **Compute** in **Manage** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image11.png)

2.  Fare clic su **+ New** nella schermata **Compute instances**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image12.png)

3.  Nella schermata Crea istanza di calcolo immettere i dettagli
    seguenti.

    1.  Compute name: +++**pfcompute**+++

    2.  Virtual machine type - **CPU**

    3.  Virtual machine size: selezionare **Standard_E4ds_v4**

> Fare clic su **Review + Create**.

**Nota:** prendere nota di questo nome di calcolo per un uso successivo.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image13.png)

4.  Fare clic su **Create** nella schermata successiva per creare il
    calcolo.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image14.png)

**Nota:** il calcolo impiega circa 10 minuti per raggiungere lo stato In
esecuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image15.png)

**Importante:** una volta che il calcolo è attivo e funzionante, puoi
continuare con le attività successive. Tuttavia, se si sta facendo una
pausa dall'esecuzione del lab, assicurarsi di arrestare l'istanza di
calcolo e riavviarla quando si avvia dopo l'interruzione.

### Attività 1.3: Creare una risorsa Azure OpenAI

1.  Dal portale di Azure +++https://portal.azure.com+++ cercare e
    selezionare +++**AzureOpenAI**+++.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image16.png)

2.  Fare clic su **+ Create**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image17.png)

3.  Compila i dettagli sottostanti e fai clic su **Next**.

- Resource group: selezionare your assigned **Resource group**

- Region: selezionare un'area (**North Central US** viene utilizzato
  qui)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image18.png)

4.  Accetta le impostazioni predefinite nelle pagine successive e fai
    clic su **Create** nella pagina **Review+Submit**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image19.png)

5.  Fare clic su **Go to resource** una volta completata la
    distribuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image20.png)

6.  Selezionare **Keys and Endpoint** nel riquadro sinistro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image21.png)

7.  Copiare la **Key** e la **Endpoint** e salvarli in un blocco note
    per utilizzarli in una parte successiva del lab.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image22.png)

8.  In **Azure Machine Learning Studio** selezionare **Model catalog**
    nel riquadro sinistro e selezionare **gpt-4o**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image23.png)

9.  Fare clic su **Deploy** per distribuire il modello.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image24.png)

10. Accettare il nome della distribuzione e selezionare **Deploy**.
    Prendi nota di questo nome per un utilizzo futuro.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image25.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image26.png)

## Attività 2: Configurare una connessione al flusso di richiesta

1.  Nel riquadro di spostamento sinistro di Azure Machine Learning
    Studio selezionare **Prompt flow**. Seleziona **Connections** dalla
    barra dei menu. Selezionare l'elenco a discesa accanto a **Create**
    e selezionare **Azure OpenAI**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image27.png)

2.  Nella procedura guidata Aggiungi connessione Azure OpenAI
    specificare i dettagli seguenti e selezionare **Salva**.

- Name – +++**AoaiML_pf**+++

- Provider: selezionare **Azure OpenAI**

- Subscription ID: selezionare la **sottoscrizione assegnata**

- Azure OpenAI Account Name: selezionare **AOAI-PF@lab.LabInstanceId**

- Auth Mode: seleziona la **API Key**

- API Key: fornire la **Key** in cui è stata salvata la **Azure OpenAI
  resource**

- API base: specificare l' **endpoint** salvato dalla **Azure OpenAI
  resource**

> ![Uno screenshot di un computer Descrizione generata
> automaticamente](./media/image28.png)

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image29.png)

3.  Verificare che la creazione della connessione sia riuscita.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image30.png)

## 

## Attività 3: Creare e sviluppare il flusso di prompt

1.  Nella scheda **Flows** della home page del di **Prompt flow**,
    seleziona **Create** per creare il flusso di prompt. La pagina
    **Create a new flow** mostra i tipi di flusso che è possibile
    creare, gli esempi predefiniti che è possibile clonare per creare un
    flusso e i modi per importare un flusso.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image31.png)

2.  Selezionare **Clone** nella categoria **WebClassification**.

Nella **Explore gallery** è possibile esplorare gli esempi predefiniti e
selezionare **View datail** in qualsiasi riquadro per visualizzare in
anteprima se è adatto allo scenario.

Questo lab usa l' esempio di **Web Classification** per illustrare il
percorso utente principale.

La classificazione Web è un flusso che dimostra la classificazione
multiclasse con un LLM. Dato un URL, il flusso classifica l'URL in una
categoria Web con pochi scatti, un semplice riepilogo e suggerimenti di
classificazione. Ad esempio, data una https://www.imdb.com URL,
classifica l'URL in Film.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image32.png)

3.  Accettare il nome compilato per **Folder name,** quindi selezionare
    **Clone**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image33.png)

4.  Per l'esecuzione del flusso è necessaria una sessione di calcolo. La
    sessione di calcolo gestisce le risorse di calcolo necessarie per
    l'esecuzione dell'applicazione, inclusa un'immagine Docker che
    contiene tutti i pacchetti di dipendenza necessari.

5.  Nella pagina di creazione del flusso avviare una sessione di calcolo
    selezionando **Start compute session**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image34.png)

**Nota:** ci vorranno circa **10 minutes** per portare la sessione di
calcolo nello stato In esecuzione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image35.png)

## Attività 4: Esaminare la pagina di creazione del flusso

L'avvio della sessione di calcolo può richiedere alcuni minuti. Durante
l'avvio della sessione di calcolo, visualizzare le parti della pagina di
creazione del flusso.

- La visualizzazione **Flow** o *appiattisci* sul lato sinistro della
  pagina è l'area di lavoro principale, in cui è possibile creare il
  flusso aggiungendo o rimuovendo nodi, modificando ed eseguendo nodi in
  linea o modificando i prompt. Nelle sezioni **Inputs** e **Outputs** è
  possibile visualizzare, aggiungere o rimuovere e modificare input e
  output.

Quando è stato clonato l'esempio di classificazione Web corrente, gli
input e gli output erano già impostati. Lo schema di input per il flusso
è name: url; type: string, un URL di tipo stringa. È possibile
modificare manualmente il valore di input preimpostato in un altro
valore, ad esempio https://www.imdb.com.

- **Files** in alto a destra mostra la cartella e la struttura dei file
  del flusso. Ogni cartella di flusso contiene un *file flow.dag.yaml*,
  file di codice sorgente e cartelle di sistema. È possibile creare,
  caricare o scaricare file per il test, la distribuzione o la
  collaborazione.

- La visualizzazione **Graph** in basso a destra consente di
  visualizzare l'aspetto del flusso. È possibile ingrandire o ridurre
  oppure utilizzare il layout automatico.

È possibile modificare i file in linea nella visualizzazione **Flow** o
Appiattisci oppure attivare l'interruttore **Raw file mode** e
selezionare un file da **Files** per aprire il file in una scheda per la
modifica.

È possibile modificare i file in linea nella visualizzazione **Flow** o
Appiattisci oppure attivare l'interruttore **raw file mode** e
selezionare un file da **File** per aprire il file in una scheda per la
modifica.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image36.png)

## Attività 5: Configurare i nodi LLM

Per ogni nodo LLM, è necessario selezionare una **Connection** per
impostare le chiavi API LLM. Selezionare la connessione Azure OpenAI.

A seconda del tipo di connessione, è necessario selezionare un
**deployment_name** o un modello dall'elenco a discesa. Per una
connessione Azure OpenAI, selezionare una distribuzione. 

1.  Per il summarize_text_content, compila i dettagli sottostanti.

Connection – Seleziona **AoaiML_pf**

Api – Seleziona la **chat**

Deployment name: selezionare **gpt-4o-2024-11-20**

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image37.png)

2.  Impostare la connessione in modo simile per i nodi LLM
    **classify_with_llm**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image38.png)

3.  Per testare ed eseguire il debug di un singolo nodo, selezionare l'
    icona **Run** nella parte superiore di un nodo nella visualizzazione
    **Flow**. È possibile espandere **Inputs** e modificare l'URL di
    input del flusso per testare il comportamento del nodo per URL
    diversi.

4.  Lo stato di esecuzione viene visualizzato nella parte superiore del
    nodo. Al termine dell'esecuzione, l'output dell'esecuzione viene
    visualizzato nella sezione **Output** del nodo .

5.  Passa all'inizio del flusso ed esegui la **fetch_text_content_from
    url** ed esegui il blocco.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image39.png)

La visualizzazione **Graph** mostra anche lo stato del singolo nodo di
esecuzione.

6.  Nella sezione **Input**, fornisci il valore per il campo **Value**
    come
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++

Seleziona **Run** in alto a destra per testare ed eseguire il debug
dell'intero flusso.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image40.png)

## Attività 5: Visualizzare gli output del flusso

È inoltre possibile impostare gli output di flusso per controllare gli
output di più nodi in un'unica posizione. Le uscite di flusso ti aiutano
a:

- Controlla i risultati dei test di massa in un'unica tabella.

- Definire la mappatura dell'interfaccia di valutazione.

- Impostare lo schema di risposta della distribuzione.

1.  Selezionare Visualizza **outputs** nel banner superiore o nella
    barra dei menu superiore per visualizzare informazioni dettagliate
    su input, output, esecuzione del flusso e orchestrazione.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image41.png)

2.  Nella scheda Output della schermata Output, si noti che il flusso
    prevede l'URL di input con una **category** e un'**evidence**.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image42.png)

3.  Selezionare la scheda **Trace** nella schermata **Output** e quindi
    selezionare il **flow** sotto il **node name** per visualizzare
    informazioni dettagliate sulla panoramica del flusso nel riquadro
    destro. Espandi **flow** e seleziona un passaggio per visualizzare
    informazioni dettagliate per quel passaggio.

![Uno screenshot di un computer Descrizione generata
automaticamente](./media/image43.png)

**Sommario:**

In questo lab si è appreso come classificare l'URL in una categoria Web
con un semplice riepilogo e prompt di classificazione usando il flusso
di prompt in Azure Machine Learning Studio.
