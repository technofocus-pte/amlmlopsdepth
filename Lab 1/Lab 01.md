# Lab 01 – Vorbereiten eines Datasets, Trainieren und Bereitstellen eines Klassifizierungsmodells mit Azure Machine Learning Studio

**Objektiv**

In diesem Lab geht es vor allem darum, Sie durch den Prozess des
Einrichtens einer Azure Machine Learning-Umgebung, des Hochladens, des
Zugriffs und Untersuchens von Daten sowie des Trainings und
Bereitstellens eines Bildklassifizierungsmodells mit Azure Machine
Learning Studio zu führen.

Erwartete Dauer: 45 Minuten

## Übung 1: Einrichten des Azure Machine Learning-Arbeitsbereichs

### Aufgabe 1: Synchronisieren der VM-Uhr

1.  Nachdem Sie sich bei der VM angemeldet haben, klicken Sie mit der
    rechten Maustaste auf die Uhr in der unteren rechten Ecke des
    Bildschirms.

2.  Wählen Sie **Adjust date and time.**

&nbsp;

3.  Klicken Sie auf dem sich öffnenden Bildschirm "Settings" unter "
    Additional Settings " auf **Sync now**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image1.png)

4.  Dies kümmert sich um die Synchronisierung der Uhrzeit nur für den
    Fall, dass die automatische Synchronisierung nicht funktioniert.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image2.png)

### Aufgabe 2: Vorbereiten der Azure-Ressourcen

Diese Aufgabe konzentriert sich auf das Erstellen eines Azure Machine
Learning-Arbeitsbereichs. Sie erfahren, wie Sie einen dedizierten
Arbeitsbereich einrichten können, um ihre Machine Learning-Projekte
effektiv zu organisieren und zu verwalten. Dieser Arbeitsbereich dient
als zentraler Hub für Zusammenarbeit, Experimente und Bereitstellung.

#### Aufgabe 2.1: Registrieren der erforderlichen Ressourcenanbieter 

1.  Navigieren Sie auf der Startseite des Azure-Portals zu Ihrem
    zugewiesenen **Subscription**.

2.  Wählen Sie im linken Bereich unter **Settings** die Option Resource
    Providers aus.

3.  Suchen Sie nach +++Microsoft.StreamAnalytics+++ und wählen Sie die
    drei Punkte neben dem Namen aus, und klicken Sie auf **Register**.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image3.png)

4.  Wiederholen Sie die Schritte zum Registrieren von
    +++Microsoft.Cdn+++ und +++Microsoft.PolicyInsights+++

#### Aufgabe 2.2: Erstellen eines Azure Machine Learning-Arbeitsbereichs

1.  Melden Sie sich unter +++https://portal.azure.com+++ mit dem
    **Username** und dem **Password** auf der Registerkarte
    **Resources** beim Azure-Portal an.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image4.png)

2.  Wählen Sie auf der Startseite des Azure-Portals die Option **+
    Create a resource** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.png)

3.  Verwenden Sie auf der Seite **Create a resource** die Suchleiste, um
    +++**Azure** **Machine Learning+++** zu suchen, und wählen Sie
    **Azure Machine Learning** aus**.**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image6.png)

4.  Klicken Sie unter **Marketplace** auf **Create dropdown**, und
    wählen Sie **Azure Machine Learning** aus**.**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image7.png)

5.  Geben Sie die folgenden Informationen ein, um Ihren neuen
    Arbeitsbereich zu konfigurieren, und klicken Sie auf **Review +
    create**.

    - **Abonnement**: Wählen Sie Ihr **zugewiesenes**
      **Azure-Abonnement** aus.

    - **Ressourcengruppe**: Wählen Sie die **Ressourcengruppe** aus, die
      Ihnen **zugewiesen ist**.

> **Details zum Arbeitsbereich:**

- **Name des Arbeitsbereichs:** +++**Azuremlws@lab.LabInstance.Id+++**

- **Region**: Wählen Sie die nächstgelegene Region aus **(**hier wird
  die Option **" North Central US** " ausgewählt)

&nbsp;

- **Container Registry Wählen Sie Create new aus. Geben Sie +++
  azuremlcr@lab.LabInstance.Id +++ ein.**

**Hinweis:** Die Nummer, die an die Namen der Ressourcen angehängt wird,
ist Ihre Labinstance-ID, um die Eindeutigkeit zu gewährleisten. Die
Screenshots haben eine andere Nummer, da sie einzigartig sind.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

6.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

7.  Klicken Sie auf **Go to resource**, um den neuen Arbeitsbereich
    anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image11.png)

8.  Klicken Sie auf der Seite **Microsoft.MachineLEarningServices |
    Overview page**, wählen Sie unter **Work with your model in Azure
    Machine Learning studio** die Option **Launch studio** aus.

![Ein Screenshot eines Software-Updates Beschreibung wird automatisch
generiert](./media/image12.png)

#### Aufgabe 2.3: Erstellen eines Computes

Diese Aufgabe veranschaulicht die Erstellung einer Computeressource in
Azure. Sie erkunden verschiedene Computeoptionen, z. B. virtuelle
Maschine oder verwaltete Computecluster, und verstehen, wie Sie
Ressourcen konfigurieren und bereitstellen, um Machine
Learning-Workloads effizient auszuführen.

1.  Nachdem **Azure** **Machine Learning Studio** geöffnet wurde,
    klicken Sie im linken Bereich unter **Manage** auf **Compute**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.png)

2.  Klicken Sie auf dem Bildschirm **Compute-Instances** auf **+ New**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

3.  Geben Sie auf dem Bildschirm Create compute instance
    (Compute-Instanz erstellen) die folgenden Details ein.

    1.  Compute-Name – +++**cpu-cluster-fs@lab.labInstance.Id+++**

    2.  Typ der virtuellen Maschine – **CPU**

    3.  Größe der virtuellen Maschine – Wählen Sie **Standard_E4ds_v4**
        aus

> Klicken Sie auf **Review + Create**.

**Hinweis:** Notieren Sie sich diesen Computenamen für die spätere
Verwendung.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.png)

4.  Klicken Sie im nächsten Bildschirm auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.png)

**Hinweis:** Es dauert etwa 10 Minuten, bis die Compute den Status "Wird
ausgeführt"(Running) erreicht.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

**Wichtig:** Sobald Compute betriebsbereit ist, können Sie mit den
nächsten Aufgaben fortfahren. Wenn Sie jedoch eine Pause von der
Lab-Ausführung einlegen, stellen Sie sicher, dass Sie die
Compute-Instanz **stoppen** und erneut starten, wenn Sie nach der Pause
starten.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image18.png)

**Zusammenfassung der Übung:**

Die Übung macht die Teilnehmer mit den wesentlichen Schritten vertraut,
die beim Einrichten einer Azure Machine Learning-Umgebung erforderlich
sind. Im Rahmen der Reihe von Aufgaben haben die Teilnehmer gelernt, wie
sie ein Speicherkonto erstellen, das Machine Learning SDK installieren,
sich mit der Azure CLI anmelden, einen Azure Machine
Learning-Arbeitsbereich erstellen und eine Compute-Ressource einrichten.
Durch das Abschließen dieser Übung haben Sie die grundlegenden
Kenntnisse und praktischen Fähigkeiten erworben, die erforderlich sind,
um eine funktionsfähige Azure Machine Learning-Umgebung einzurichten,
die es Ihnen ermöglicht, Ihre Machine Learning-Projekte sicher zu
beginnen.

## Übung 2: Hochladen, Zugreifen auf und Untersuchen von Daten in Azure Machine Learning

**Objektiv**

In dieser Übung lernen Sie Folgendes:

- Laden Sie Ihre Daten in den Cloud-Speicher hoch

- Erstellen eines Azure Machine Learning-Datenassets

- Greifen Sie auf Ihre Daten in einem Notebook zu, um die interaktive
  Entwicklung zu ermöglichen

- Erstellen neuer Versionen von Datenassets

Der Beginn eines Projekts für maschinelles Lernen umfasst in der Regel
die explorative Datenanalyse (EDA), die Datenvorverarbeitung
(Bereinigung, Feature-Engineering) und die Erstellung von Prototypen für
maschinelles Lernen zur Validierung von Hypothesen. Diese Projektphase
des Prototyping ist hochgradig interaktiv. Es eignet sich für die
Entwicklung in einer IDE oder einem Jupyter-Notebook mit einer
interaktiven *Python*-Konsole. In diesem Lab werden diese Ideen
beschrieben.

Wir befinden uns in der Phase **" Data: Explore & prepare** " der
**Machine Learning Project Workflow.**

![](./media/image19.png)

### Aufgabe 1: Vorbereiten der Azure-Ressourcen

**Wichtig:** Stellen Sie sicher, dass der Compute, den wir in der
letzten Übung erstellt haben, betriebsbereit ist. Wenn Sie eine Pause
von der Lab-Ausführung einlegen, stellen Sie bitte sicher, dass Sie sie
**stoppen** und erneut starten, wenn Sie nach der Pause beginnen.

#### Aufgabe 1.1: Hochladen eines Notizbuchs 

1.  Wählen Sie in Azure Machine Learning Studio im linken Bereich die
    Option **Notebooks** aus, sobald das Compute eingerichtet und
    ausgeführt wird.![](./media/image20.png)

2.  Schließen Sie das Dialogfeld **What’s new in Notebooks**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image21.png)

3.  Der Bereich Notebook-Files wird mit der Struktur **Users -\> \<
    UserName \>** geöffnet. Klicken Sie auf die drei Punkte neben dem
    Benutzernamen und wählen Sie **Create new folder**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

4.  Geben Sie den Ordnernamen als +++**Azuremlnotebooks**+++ ein, und
    klicken Sie auf **Create.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image23.png)

5.  Sobald der Ordner erstellt ist, klicken Sie auf die **Menüoptionen**
    (die drei Punkte neben dem Ordnernamen) des Ordners
    **Azuremlnotebooks** und klicken Sie auf **Upload files**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image24.png)

6.  Wählen Sie **Click to browse and select file(s).** Navigieren Sie
    unter **C:\Labfiles** zur **explore-data.ipynb** und klicken Sie auf
    **Open**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image25.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image26.png)

7.  Aktivieren Sie das Kontrollkästchen **Open file after upload** und
    **I trust the contents of this file.** Klicken Sie dann auf
    **Upload.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.png)

8.  Dadurch wird das hochgeladene Notebook geöffnet.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.png)

9.  Klicken Sie auf **Authenticate** , wenn das Studio Sie zur
    Authentifizierung auffordert, da Sie sich zum ersten Mal im Studio
    anmelden.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image29.png)

### Aufgabe 2: Hochladen von Daten, Zugreifen und Erkunden von Daten 

#### Aufgabe 2.1: Herunterladen von Daten

1.  Klicken Sie unter dem Bereich **Files** von **Notebooks** auf die 3
    Punkte neben dem Ordnernamen **Azuremlnotebooks** und dann auf
    **Create new folder.**

![](./media/image30.png)

2.  Geben Sie den Namen des Ordners als +++**data**+++ ein und klicken
    Sie auf **Create**.

![](./media/image31.png)

3.  Sobald die Ordnererstellung erfolgreich ist, klicken Sie auf die
    Menüoptionen der Ordner **Data** und wählen Sie **Upload files**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image32.png)

4.  Wählen Sie **Click to browse and select file(s)**, und navigieren
    Sie zu **C:\Labfiles**, um die
    **default_of_credit_card_clients.csv-**Datei auszuwählen, und
    klicken Sie auf **Open**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image33.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image34.png)

5.  Sobald der Upload abgeschlossen ist, wird unter den
    Benachrichtigungen die Meldung "**File uploaded successfully**"
    angezeigt.

![Nahaufnahme eines Computerbildschirms Beschreibung wird automatisch
mit geringer Zuverlässigkeit generiert](./media/image35.png)

#### Aufgabe 2.2: Erstellen eines Handles für den Arbeitsbereich

1.  Kehren Sie zum Notebook (**explore-data**) zurück.

2.  Bevor wir uns mit dem Code befassen, benötigen Sie eine Möglichkeit,
    auf Ihren Arbeitsbereich zu verweisen. Sie erstellen ml_client für
    einen Ziehpunkt für den Arbeitsbereich. Anschließend verwenden Sie
    ml_client, um Ressourcen und Aufträge zu verwalten.

3.  Ersetzen Sie in der ersten Zelle unter **" Create handle to
    workspace** " die Platzhalter von **\< SUBSCRIPTION_ID \>**, **\<
    RESOURCE_GROUP \>** und dem **\< AML_WORKSPACE_NAME \>.**

4.  Ersetzen Sie \< RESOURCE_GROUP\> durch den Namen der zugewiesenen
    Ressourcengruppe.

5.  Ersetzen Sie \<AML_WORKSPACE_NAME\> durch
    [+++**Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  Ersetzen Sie \< SUBSCRIPTION_ID \> durch
    +++**@lab.CloudSubscription.Id+++**.

7.  Klicken Sie auf die Schaltfläche **Run cell**, die oben links in der
    Zelle verfügbar ist. Suchen Sie nach einem Häkchen am unteren Rand
    der Zelle, sobald die Ausführung erfolgreich war.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.png)

#### Aufgabe 2.3: Hochladen von Daten in den Cloud-Speicher

1.  Ein Azure Machine Learning-Datenasset ähnelt den Lesezeichen
    (Favoriten) von Webbrowsern. Anstatt sich lange Speicherpfade (URIs)
    zu merken, die auf Ihre am häufigsten verwendeten Daten verweisen,
    können Sie ein Datenasset erstellen und dann mit einem Anzeigenamen
    auf dieses Asset zugreifen.

2.  In der nächsten Notebook-Zelle wird das Datenasset erstellt. Im
    Codebeispiel wird die Rohdatendatei in die angegebene
    Cloudspeicherressource hochgeladen.

3.  Jedes Mal, wenn Sie ein Datenasset erstellen, benötigen Sie eine
    eindeutige Version dafür. Wenn die Version bereits vorhanden ist,
    erhalten Sie eine Fehlermeldung. In diesem Code verwenden wir die
    Zeit, um jedes Mal, wenn die Zelle ausgeführt wird, eine eindeutige
    Version zu generieren.

4.  Führen Sie die nächste Zelle aus, indem Sie auf die Schaltfläche
    Execute oben links in der Zelle klicken.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image37.png)

5.  **" Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx "** ist die Ausgabe, die unterhalb der Zelle
    angezeigt wird.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image38.png)

6.  Klicken Sie im linken Bereich auf **Data** und dann auf das
    **Credit-card** Datenasset, das durch die Ausführung im obigen
    Schritt erstellt wurde. Erkunden Sie die Details, und navigieren Sie
    zurück zum Bereich **Notebooks**.

![](./media/image39.png)

#### Aufgabe 2.4: Zugreifen auf Ihre Daten in einem Notebook

1.  Zurück im Notebook führen Sie die Zelle mit dem Befehl **%pip** aus,
    um die Python-Bibliothek **azureml-fsspec** in Ihrem
    **Jupyter**-Kernel zu installieren.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
mit geringer Zuverlässigkeit generiert](./media/image40.png)

2.  Führen Sie die nächste Zelle aus, um auf die CSV-Datei in **Pandas**
    zuzugreifen.

3.  Der **Data asset-URI** wird am unteren Rand der Zelle gedruckt, und
    die Daten werden ebenfalls angezeigt.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image41.png)

#### Aufgabe 2.5: Erstellen einer neuen Version des Datenassets

1.  Möglicherweise haben Sie bemerkt, dass die Daten ein wenig bereinigt
    werden müssen, um sie für das Trainieren eines Machine
    Learning-Modells geeignet zu machen. Es hat:

    1.  zwei Kopfzeilen

    2.  eine Client-ID-Spalte; Wir würden diese Funktion beim
        maschinellen Lernen nicht verwenden

    3.  Leerzeichen im Namen der Antwortvariablen

2.  Außerdem ist das **Parquet**-Dateiformat im Vergleich zum CSV-Format
    eine bessere Möglichkeit, diese Daten zu speichern. Parquet bietet
    Komprimierung und behält das Schema bei. Um die Daten zu bereinigen
    und in Parquet zu speichern, führen Sie daher die nächste Zelle aus.

3.  Stellen Sie sicher, dass die Ausführung erfolgreich ist, indem Sie
    das Teilstrich am unteren Rand der Zelle verwenden.

![](./media/image42.png)

4.  Diese Tabelle zeigt die Struktur der Daten in der ursprünglichen
    **default_of_credit_card_clients.csv** Datei . CSV-Datei, die in
    einem früheren Schritt heruntergeladen wurde. Die hochgeladenen
    Daten enthalten 23 erklärende Variablen und 1 Antwortvariable, wie
    hier gezeigt:

[TABLE]

5.  Führen Sie die nächste Zelle aus, um eine neue *Version* des
    Datenassets zu erstellen (die Daten werden automatisch in den
    Cloud-Speicher hochgeladen).

6.  Bei erfolgreicher Ausführung wird eine Ausgabe mit dem Namen **"Data
    asset created. Name: credit_card, version:
    YYYY.MM.DD.xxxxxx_cleaned”** wird nach der Zelle angezeigt.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image43.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image44.png)

**Wichtig:**

Diese Python-Codezelle legt **Namens**- und **Versions**werte für das
erstellte Datenasset fest. Dies hat zur Folge, dass der Code in dieser
Zelle fehlschlägt, wenn er mehr als einmal ausgeführt wird, ohne dass
diese Werte geändert werden. Feste **Namens**- und **Versions**werte
bieten eine Möglichkeit, Werte zu übergeben, die für bestimmte
Situationen geeignet sind, ohne sich um automatisch generierte oder
zufällig generierte Werte kümmern zu müssen.

7.  Bei der bereinigten Parquet-Datei handelt es sich um die Datenquelle
    der neuesten Version. Der Code in der nächsten Zelle zeigt zuerst
    die Resultset der CSV-Version und dann die Parquet-Version bei der
    Ausführung an.

8.  Führen Sie die nächste Zelle aus, und prüfen Sie, ob das Ergebnis
    unten angezeigt wird.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image45.png)

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image46.png)

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
mit geringer Zuverlässigkeit generiert](./media/image47.png)

![Ein Bild, das Text, Screenshot, Zahl, Anzeige enthält Beschreibung
wird automatisch generiert](./media/image48.png)

9.  Suchen Sie unter **Data** nach den bereinigten Daten.

> ![](./media/image49.png)

**Wichtig:** Von hier aus kannst du mit der nächsten Übung weitermachen.
Wenn Sie jedoch eine Pause von der Lab-Ausführung einlegen, stellen Sie
sicher, dass Sie die Compute-Instanz **stoppen** und erneut starten,
wenn Sie nach der Pause fortfahren.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image50.png)

**Zusammenfassung der Übung**

In dieser Übung haben Sie gelernt, wie Sie Ihre Daten in den
Cloudspeicher hochladen, ein Azure Machine Learning-Datenasset
erstellen, auf Ihre Daten in einem Notebook für die interaktive
Entwicklung zugreifen und neue Versionen von Datenassets erstellen.

## Übung 3: Trainieren und Bereitstellen eines Bildklassifizierungsmodells in Azure Machine Learning Studio

**Objektiv**

In dieser Übung lernen Sie zu

1.  Herstellen einer Verbindung mit einem Arbeitsbereich und Einrichten
    einer Computing-Ressource über die Notebook-UI von Azure Machine
    Learning Studio

2.  Bringen Sie Daten ein und bereiten Sie sie für die Verwendung für
    Schulungen vor

3.  Trainieren eines Modells für die Bildklassifizierung

4.  Anzeigen und Analysieren der Metriken zur Optimierung Ihres Modells

5.  Stellen Sie das Modell online bereit und testen Sie es

Wir befinden uns in der **Train & Validate-Modellphase** der **Machine
Learning Project Workflow.**

### ![Ein Bild, das Text, Schriftart, Nummer, Screenshot enthält Beschreibung wird automatisch generiert](./media/image51.png)Aufgabe 1: Hochladen eines Notebooks

1.  Klicken Sie auf der Seite Azure Machine Learning Studio auf der
    Seite **Notebooks** auf die Menüoptionen für den Ordner
    **AzureMLnotebooks,** und klicken Sie auf **Upload files**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image52.png)

2.  Wählen Sie **" Click to browse and select file(s)"** aus, navigieren
    Sie zu **"C:\Labfiles",** und wählen Sie die Datei
    **"azureml-getting-started-studio"** (eine Jupyter-Quelldatei) aus.

> ![Ein Screenshot eines Computerbildschirms Beschreibung wird
> automatisch mit mittlerer Zuverlässigkeit
> generiert](./media/image53.png)
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image54.png)

3.  Aktivieren Sie das Kontrollkästchen **Open file after upload** und
    klicken Sie dann auf **Upload**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image55.png)

4.  Sobald der Datei-Upload erfolgreich ist, wird sie in Studio geöffnet
    und automatisch mit dem Compute (cpu-cluster-fs) verbunden, das sich
    im Status Running befindet.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image56.png)

### Aufgabe 2: Herstellen einer Verbindung mit dem Azure Machine Learning-Arbeitsbereich

Bevor wir uns mit dem Code befassen, müssen Sie eine Verbindung mit
Ihrem Arbeitsbereich herstellen. Der Arbeitsbereich ist die Ressource
auf oberster Ebene für Azure Machine Learning und bietet einen zentralen
Ort für die Arbeit mit allen Artefakten, die Sie bei der Verwendung von
Azure Machine Learning erstellen.

Wir verwenden **DefaultAzureCredential,** um Zugriff auf den
Arbeitsbereich zu erhalten. **DefaultAzureCredential** sollte in der
Lage sein, die meisten Szenarien zu verarbeiten.

> *\# Handle to the workspace*
>
> **from** azure.ai.ml **import** MLClient
>
> *\# Authentication package*
>
> **from** azure.identity **import** DefaultAzureCredential
>
> credential **=** DefaultAzureCredential()
>
> *\# Get a handle to the workspace. You can find the info on the
> workspace tab on ml.azure.com*
>
> ml_client **=** MLClient(
>
> credential**=**credential,
>
> subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
> xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*
>
> resource_group_name**=**"\<RESOURCE_GROUP\>",
>
> workspace_name**=**"\<AML_WORKSPACE_NAME\>",
>
> )

1.  Ersetzen Sie im obigen Code (Erste Zelle des Notebooks)
    **SUBSCRIPTION_ID, RESOURCE_GROUP Namen** und die Platzhalter
    **AML_WORKSPACE_NAME** durch die Werte, die wir in der vorherigen
    Übung gespeichert haben.

2.  Ihre erste Zelle im Notebook sollte nun wie folgt aussehen. Klicken
    Sie auf die Schaltfläche **Run** oben links in der ersten Zelle.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image57.png)

3.  Stellen Sie sicher, dass die Zelle erfolgreich ausgeführt wurde,
    indem Sie ihren Status am unteren Rand der Zelle anzeigen.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
mit mittlerer Zuverlässigkeit generiert](./media/image58.png)

In \[ \]:

### Aufgabe 3: Hochladen von Daten

Zum Ausführen eines Azure Machine Learning-Trainingsauftrags benötigen
Sie eine Umgebung.

In diesem Lab verwenden Sie eine vorgefertigte Umgebung mit dem Namen
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest die alle erforderlichen
Bibliotheken (python, MLflow, numpy, pip usw.) enthält.

1.  Führen Sie den Code in der nächsten Zelle aus, um die Daten
    hochzuladen.

2.  Stellen Sie sicher, dass eine Meldung mit dem Hinweis " **Data asset
    created "** als Ausgabe der Zelle angezeigt wird.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
mit mittlerer Zuverlässigkeit generiert](./media/image59.png)

###  Aufgabe 4: Erstellen des zu trainierenden Befehlsauftrags

Nachdem Sie nun über alle Ressourcen verfügen, die zum Ausführen Ihres
Auftrags erforderlich sind, ist es an der Zeit, den Auftrag selbst mit
dem Azure ML Python SDK v2 zu erstellen. Wir werden einen
Befehlsauftrag(command job) erstellen.

Ein AzureML-Befehlsauftrag ist eine Ressource, die alle Details angibt,
die zum Ausführen des Trainingscodes in der Cloud erforderlich sind:
Eingaben und Ausgaben, die Art der zu verwendenden Hardware, die zu
installierende Software und die Ausführung des Codes. Der Befehlsauftrag
enthält Informationen zum Ausführen eines einzelnen Befehls.

#### Aufgabe 4.1: Erstellen eines Trainingsskripts

1.  Beginnen wir mit der Erstellung des Trainingsskripts - der
    **main.py** Python-Datei.

2.  Führen Sie die nächste Zelle aus, und stellen Sie sicher, dass sie
    erfolgreich ausgeführt wird.

![Ein Bild, das Text, Schriftart, Linie, Screenshot enthält Beschreibung
wird automatisch generiert](./media/image60.png)

3.  Das Skript in der nächsten Zelle übernimmt die Vorverarbeitung der
    Daten und teilt sie in Test- und Trainingsdaten auf. Anschließend
    werden diese Daten verwendet, um ein baumbasiertes Modell zu
    trainieren und das Ausgabemodell
    zurückzugeben. [MLFlow](https://mlflow.org/docs/latest/tracking.html)
    wird verwendet, um die Parameter und Metriken während des Laufs
    unserer Pipeline zu protokollieren.

4.  Führen Sie die Zelle aus und stellen Sie sicher, dass sie mit der
    Ausgabe erfolgreich ausgeführt wird.

**Writing ./src/main.py**

> ![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
> mit geringer Zuverlässigkeit generiert](./media/image61.png)
>
> ![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
> mit mittlerer Zuverlässigkeit generiert](./media/image62.png)

5.  Wie Sie in diesem Skript sehen können, wird die Modelldatei nach dem
    Trainieren des Modells gespeichert und im Arbeitsbereich
    registriert. Jetzt können Sie das registrierte Modell in
    Rückschlussendpunkten verwenden.

#### Aufgabe 4.2: Konfigurieren des Befehls

Da Sie nun über ein Skript verfügen, das die gewünschten Aufgaben
ausführen kann, verwenden Sie den Befehl "Universell", mit dem
Befehlszeilenaktionen ausgeführt werden können. Diese
Befehlszeilenaktion kann der direkte Aufruf von Systembefehlen oder das
Ausführen eines Skripts sein.

1.  Hier verwenden Sie Eingabedaten, das Split-Verhältnis, die Lernrate
    und den Namen des registrierten Modells als Eingabevariablen.

2.  Wählen Sie im linken Fensterbereich **Data** und dann
    **credit-card-data** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image63.png)

3.  Suchen Sie im Abschnitt **Data sources** nach dem Wert des
    **Datastore-URI,** und kopieren Sie ihn. Speichern Sie es für die
    Verwendung im nächsten Schritt.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image64.png)

4.  Ersetzen Sie in der nächsten Zelle das Symbol

    1.  Wert des **Path** mit dem **Datastore-URI,** der im vorherigen
        Schritt gespeichert wurde.

    2.  Wert der **Compute** mit
        +++**cpu-cluster-fs@lab.LabInstance.Id+++** (der Name des
        Clusters, den wir in Lab 1 gespeichert haben)

5.  Klicken Sie auf **Run**. Stellen Sie sicher, dass die Zelle
    erfolgreich ausgeführt wird.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image65.png)

### Aufgabe 6: Übermitteln des Auftrags

Jetzt ist es an der Zeit, den Auftrag für die Ausführung in AzureML zu
übermitteln. **Die Ausführung des Auftrags dauert 2 bis 3 Minuten**. Es
kann länger dauern (bis zu 10 Minuten), wenn die Compute-Instanz auf
null Knoten herunterskaliert wurde und die benutzerdefinierte Umgebung
noch erstellt wird.

1.  Führen Sie die Zelle mit dem folgenden Befehl aus, um den Auftrag zu
    übergeben.

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  Klicken Sie auf **Run**. Stellen Sie sicher, dass die Ausführung
    erfolgreich war und in der Spalte **" Details Page "** ein Link zum
    Ergebnis vorhanden ist.

**Hinweis**: Dies dauert ca. 2 Minuten.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image66.png)

3.  Öffnen Sie den Link, der in der Spalte **" Details Page "** des
    Ergebnisses verfügbar ist, in einer neuen Registerkarte.

### Aufgabe 7: Anzeigen des Ergebnisses eines Trainingsauftrags

1.  Sie können das Ergebnis eines Trainingsauftrags anzeigen, indem Sie
    **auf die URL klicken, die nach dem Übermitteln eines Auftrags
    generiert wurde**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image67.png)

2.  Alternativ können Sie auch im linken Navigationsmenü auf **Jobs**
    klicken. Ein Auftrag ist eine Gruppierung vieler Ausführungen aus
    einem angegebenen Skript oder Codeabschnitt. Informationen für die
    Run werden unter diesem Auftrag gespeichert.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image68.png)

3.  Auf der **Overview**-Seite wird zunächst der **Status** im Bereich
    **Properties** als **Running** angezeigt.

4.  Der Status ändert sich in **Completed**, sobald er bereit ist.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image69.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image70.png)

5.  Wählen Sie den Bereich **Metrics** aus, um die Metriken anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image71.png)

6.  Wählen Sie die Registerkarte **Images** aus , um die
    training_confusion Matrix, die Präzisionsabrufkurve und die
    roc-Kurve anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image72.png)

1)  In der **Overview** können Sie den Status des Auftrags sehen.

2)  **Metrics** zeigen unterschiedliche Visualisierungen der Metriken
    an, die Sie im Skript angegeben haben.

3)  **Images** ist der Ort, an dem Sie alle Bildartefakte anzeigen
    können, die Sie mit MLflow protokolliert haben.

4)  **Child jobs** enthalten untergeordnete Aufträge(child jobs), wenn
    Sie diese hinzugefügt haben.

5)  **Outputs + Logs** enthält Protokolldateien, die Sie für die
    Problembehandlung oder andere Überwachungszwecke benötigen.

6)  **Der Code** enthält das Skript/den Code, der im Auftrag verwendet
    wird.

7)  **Explanations** und **Fairness** werden verwendet, um zu sehen, wie
    Ihr Modell im Vergleich zu Responsible AI-Standards abschneidet. Sie
    sind derzeit Vorschaufunktionen und erfordern zusätzliche
    Paketinstallationen.

8)  In der **Monitoring,** können Sie Metriken für die Leistung von
    Compute-Ressourcen anzeigen.

### Aufgabe 8: Bereitstellen des Modells als Onlineendpunkt

Nachdem Sie ein Machine Learning-Modell trainiert haben, müssen Sie es
bereitstellen, damit andere Benutzer es für Rückschlüsse verwenden
können. Zu diesem Zweck können Sie mit Azure Machine Learning
**Endpoints** erstellen und **Deployments** hinzufügen.

Ein **Endpoint** ist in diesem Zusammenhang ein HTTPS-Pfad, der eine
Schnittstelle für Clients bereitstellt, um Anforderungen (Eingabedaten)
an ein trainiertes Modell zu senden und die Rückschlussergebnisse
(Bewertung) des Modells zu empfangen. Ein Endpunkt bietet Folgendes:

- Authentifizierung mit "Key oder Token"-basierter Authentifizierung

- TLS(SSL)-Beendigung

- Ein stabiler Scoring-URI (endpoint-name.region.inference.ml.azure.com)

Eine **Deployment** ist eine Gruppe von Ressourcen, die zum Hosten des
Modells erforderlich sind, das den eigentlichen Rückschluss ausführt.

#### Aufgabe 8.1: Erstellen eines Onlineendpunkts

1.  Stellen Sie nun Ihr Machine Learning-Modell als Webdienst in der
    Azure-Cloud, einem Onlineendpunkt, bereit.

2.  Wählen Sie im linken Bereich **Endpoints** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image73.png)

3.  Wählen Sie **Create** für Echtzeitendpunkte aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image74.png)

4.  Wählen Sie **credit_defaults_model** aus und klicken Sie dann auf
    **Select.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image75.png)

5.  Wählen Sie unter der virtuellen Maschine **Standard_E4s_v3** aus.
    Geben Sie die Instanzanzahl als **1** an.

> Übernehmen Sie die anderen Standardwerte eines eindeutigen **Endpoint
> name** und des **Deployment name**, und wählen Sie dann **Deploy**
> aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image76.png)

**Hinweis:** Die Erstellung des Endpunkts dauert etwa 20 Minuten.

6.  Nach Abschluss des Vorgangs ändert sich der Bereitstellungsstatus in
    **Succeeded**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image77.png)

#### Aufgabe 8.2: Testen mit einer Beispielabfrage

1.  Wählen Sie auf der Endpunktseite die Registerkarte **Test** aus.

2.  Kopieren Sie die folgende Beispielanforderungsdatei, fügen Sie sie
    in das Feld **Input data to test real-time endpoint** ein, und
    ersetzen Sie den dort bereits vorhandenen Code.

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

3.  Wählen Sie **Test** aus, und zeigen Sie das Ergebnis unter **Test
    result** an.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image78.png)

### Aufgabe 9: Löschen des Endpunkts

1.  Wählen Sie im linken Fensterbereich die **Endpoints** aus. Wählen
    Sie den Endpunkt aus, den wir erstellt haben, und klicken Sie auf
    **Delete**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image79.png)

2.  Klicken Sie im Bestätigungsdialogfeld auf **Delete**.

![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image80.png)

3.  Suchen Sie nach einer Benachrichtigung über den erfolgreichen
    Löschvorgang.

![Ein Bild, das Text, Screenshot, Schriftart, Linie enthält Beschreibung
wird automatisch generiert](./media/image81.png)

**Zusammenfassung**

In diesem Lab haben Sie gelernt, ein Bildklassifizierungsmodell in Azure
Machine Learning Studio zu trainieren und als Webdienst bereitzustellen.
