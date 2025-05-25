# **Lab 04 – Trainieren eines Klassifizierungsmodells mit No-Code AutoML in Azure Machine Learning Studio**

**Objektiv**

In diesem Lab erfahren Sie, wie Sie ein Klassifizierungsmodell mit
No-Code-AutoML mithilfe von automatisiertem Azure Machine Learning-ML in
Azure Machine Learning Studio trainieren. Dieses Klassifizierungsmodell
sagt voraus, ob ein Kunde eine Festgeldanlage bei einem Finanzinstitut
zeichnen wird. Automatisiertes maschinelles Lernen iteriert schnell über
viele Kombinationen von Algorithmen und Hyperparametern, um Ihnen zu
helfen, das beste Modell basierend auf einer Erfolgsmetrik Ihrer Wahl zu
finden.

Erwartete Dauer – 60 Minuten

Wir befinden uns in der Phase **" Deploy Model** " von Azure Machine
Learning.

![](./media/image1.png)

## **Übung 1: Erstellen eines Azure Machine Learning-Arbeitsbereichs**

1.  Melden Sie sich beim Azure-Portal an – +++
    **https://portal.azure.com** +++ mit den Anmeldeinformationen auf
    der Registerkarte **Resources**.

2.  Wählen Sie auf der Startseite des Azure-Portals die Option **+
    Create a resource** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image2.png)

3.  Verwenden Sie unter **Create a resource** die Suchleiste, um
    +++**Azure** **Machine Learning+++** zu suchen**.** Wählen Sie unter
    **Marketplace** die Option **Azure** **Machine Learning** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image3.png)

4.  Klicken Sie unter **Marketplace** auf die Dropdownliste **Create**,
    und wählen Sie **Azure Machine Learning** aus**.**

> ![Ein Screenshot einer Software Beschreibung wird automatisch
> generiert](./media/image4.png)

5.  Geben Sie die folgenden Informationen an, um Ihren neuen
    Arbeitsbereich zu konfigurieren:

    - **Abonnement**: Wählen Sie Ihr **zugewiesenes**
      **Azure-Abonnement** aus.

    - **Ressourcengruppe**: Wählen Sie die zugewiesene Ressourcengruppe
      aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image5.png)

**Details zum Arbeitsbereich:**

- **Name des Arbeitsbereichs: +++Azuremlws@lab. LabInstanceId+++**

&nbsp;

- **Region**: Hier wird Region **" North Central US "** auswählen

- **Container Registry:** Wählen Sie **Create new** aus. Geben Sie
  **+++Azuremlcr@lab ein. LabInstanceId+++**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image7.png)

6.  Wenn Sie mit der Konfiguration des Arbeitsbereichs fertig sind,
    wählen Sie **Review + Create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

7.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

8.  Klicken Sie auf **Go to resource**, um den neuen Arbeitsbereich
    anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

9.  Klicken Sie auf der Seite **Microsoft.MachineLEarningServices |
    Overview page**, wählen Sie unter **Work with your model in Azure
    Machine Learning studio** die Option **Launch studio** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image11.png)

## **Übung 2: Erstellen eines automatisierten ML-Auftrags**

1.  Navigieren Sie zur Registerkarte Azure Machine Learning Studio.

2.  Wählen Sie im linken Bereich im Abschnitt "**Authoring**" die Option
    "**Automated ML**" aus .

3.  Klicken Sie auf **+ New Automated ML job**.

![](./media/image12.png)

### **Aufgabe 1: Erstellen eines Datenassets**

1.  Geben Sie auf der Seite **Basic settings** den Namen des neuen
    Experiments als +++MarketingExperiment+++ ein, übernehmen Sie die
    anderen Standardwerte, und klicken Sie auf **Next**.

![](./media/image13.png)

2.  Wählen Sie auf der Seite Task type & data unter **Select task type**
    die Option **Classification** aus, und wählen Sie unter **Select
    data** die Option **+ Create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

3.  Geben Sie auf der Seite “Create data asset” die folgenden Details
    an.

- **Name** – +++marketingdata+++

- **Typ** – **Tabular**

- Klicken Sie auf **Next**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image15.png)

4.  Wählen Sie im Bereich **Data source** die Option **From local
    files** aus, und klicken Sie auf **Next**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.png)

5.  Wählen Sie unter **Destination storage type** den Standard-Data
    Store aus, der während der Erstellung Ihres Arbeitsbereichs
    automatisch eingerichtet wurde: **workspaceblobstore**. Sie laden
    Ihre Datendatei an diesen Speicherort hoch, um sie für Ihren
    Arbeitsbereich verfügbar zu machen. Wählen Sie **Next** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

6.  Wählen Sie in der “**File or folder selection” Upload files or
    folder** \> **Upload files** aus. Wählen Sie die
    **bankmarketing_train.csv** Datei aus **C:/Labfiles** aus. Wählen
    Sie **Next** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.png)

7.  Wenn der Upload abgeschlossen ist, wird der Bereich **Data preview**
    basierend auf dem Dateityp ausgefüllt. Überprüfen Sie im Formular
    **Settings** die Werte für Ihre Daten. Wählen Sie dann **Next** aus.

[TABLE]

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image19.png)

8.  Das **Schema**-Formular ermöglicht die weitere Konfiguration Ihrer
    Daten für dieses Experiment. Wählen Sie in diesem Beispiel den
    Kippschalter für die **day_of_week** aus**,** um sie nicht
    einzuschließen. Wählen Sie **Next** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image20.png)

9.  Überprüfen Sie im Formular **Review**, ob die Informationen
    vorhanden sind, und wählen Sie **Create** aus , um die Erstellung
    Ihres **Datenassets** abzuschließen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

10. Auf der Seite **" Create a new Automated ML job** " wird eine
    **Erfolgsmeldung** für die Erstellung des Datenassets angezeigt.
    Wählen Sie das erstellte **Marketingdata**-Datenasset aus und
    klicken Sie auf **Next**.

> **Hinweis:** Wenn die **Marketingdaten** nicht angezeigt werden,
> klicken Sie auf Refresh, um sie aufgelistet zu bekommen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

### **Aufgabe 2: Auftrag konfigurieren**

1.  Wählen Sie auf der Seite **Task Settings y (String)** als **Target
    column** aus, die Sie vorhersagen möchten. Diese Spalte gibt an, ob
    der Kunde eine Termineinlage gezeichnet hat oder nicht.

2.  Wählen Sie **View additional configuration settings** aus, und
    füllen Sie die Felder wie folgt aus. Diese Einstellungen dienen
    dazu, den Trainingsjob besser zu steuern. Andernfalls werden
    Standardwerte basierend auf der Auswahl des Experiments und den
    Daten angewendet.

- Primäre Metrik – AUCWeighted

- Erklären Sie das beste Modell – Aktivieren

- Alle unterstützten Modelle verwenden – Aktivieren

- Blockierte Modelle – Keine

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image23.png)

3.  Wählen Sie **Limits** aus, und geben Sie +++**60**+++ in das Feld
    **Experiment-Timeout (Minutes)** ein.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image24.png)

![Ein Screenshot eines Tests KI-generierte Inhalte können falsch
sein.](./media/image25.png)

4.  Geben Sie unter **Validate and test** die folgenden Werte ein, und
    klicken Sie auf **Next**.

- Validierungstyp - Wählen Sie **k-fold cross-validation** aus

- Anzahl der Kreuzvalidierungen – Wählen Sie **2** aus

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.png)

5.  Wählen Sie auf der Seite “Compute” den Berechnungs-Typ als **Compute
    cluster** aus und klicken Sie auf **+ New**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.png)

6.  Wählen Sie im Bereich **Create compute cluster** die folgenden
    Details aus und klicken Sie auf **Next**.

- Standort – **North Central US** (identisch mit dem Standort Ihres
  Azure Machine Learning-Arbeitsbereichs)

- Ebene der virtuellen Maschine – **Dedicated**

- Typ der virtuellen Maschine – **CPU**

- Größe der virtuellen Maschine – Wählen Sie **Standard_DS12_v2**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.png)

7.  Geben Sie in den erweiterten Einstellungen die folgenden Details
    ein, und wählen Sie **Create** aus.

- Name des Computes - +++automl-compute+++

- Mindestanzahl von Knoten: 0

- Maximale Anzahl von Knoten: 1

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image29.png)

8.  Wählen Sie **Next** aus, sobald die Compute-Bereitstellung
    erfolgreich abgeschlossen ist.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image30.png)

9.  Wählen Sie auf der Seite **"Review**" die Option **" Submit the
    training job "** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image31.png)

10. Der Bildschirm **Overview** wird mit dem **Status** oben geöffnet,
    während die Vorbereitung des Experiments beginnt. Dieser Status wird
    im Verlauf des Experiments aktualisiert. Im Studio werden auch
    Benachrichtigungen angezeigt, die Sie über den Status Ihres
    Experiments informieren.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image32.png)

> **Hinweis:** Die Schulung dauert ca. 40 Minuten.

## **Übung 3: Untersuchen von Modellen**

Während des Trainings können Sie die Modelle erkunden, die zugeordnet
sind.

1.  Navigieren Sie zur Registerkarte **Models + Child-Jobs**, um die
    getesteten Algorithmen (Modelle) anzuzeigen.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image33.png)

2.  Wählen Sie das Modell **StandardScalerWrapper, XGBoostClassifier**
    aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image34.png)

3.  Klicken Sie auf **Metrics** und erkunden Sie die Details auf der
    Registerkarte Metrics.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image35.png)

4.  Während Sie darauf warten, dass alle Experimentmodelle abgeschlossen
    sind, wählen Sie den **Algorithm name** eines abgeschlossenen
    Modells aus, um dessen Leistungsdetails zu untersuchen. Wählen Sie
    die Registerkarten **Overview** und **Metrics** aus, um
    Informationen zum Auftrag zu erhalten.

> **Wichtig:** Das Modelltraining dauert ca. 40 Minuten. Bitte fahren
> Sie mit dem nächsten Lab fort, während dieses ausgeführt wird. Setzen
> Sie dieses Lab fort, sobald sich der Status in **Completed** ändert.

## **Übung 4: Modellerklärungen**

Die Modellerklärungen können bei Bedarf generiert werden. Das Dashboard
für Modellerklärungen, das Teil der Registerkarte **Explanations
(preview)** ist, fasst diese Erklärungen zusammen.

1.  Wählen Sie auf der Registerkarte Models + Child jobs (aus dem
    übergeordneten Auftrag) die Option **MaxAbsScaler, LightGBM** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.png)

2.  Wählen Sie die Registerkarte **Explain model** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image37.png)

3.  Wählen Sie im sich öffnenden Fensterausschnitt Explain Model die
    Option

    1.  Auswählen des Computetyps – **Compute cluster**

    2.  AzureML-Compute-Instanz auswählen: Wählen Sie **automl-compute**
        aus.

Wählen Sie **Create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image38.png)

4.  Die Erfolgsmeldung wird angezeigt. Wählen Sie die Registerkarte
    **Explanations(preview).** Diese Registerkarte wird nach Abschluss
    des Erklärbarkeitslaufs aufgefüllt.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image39.png)

5.  Erweitern Sie den linken Bereich. Wählen Sie unter **Features** die
    Zeile mit der Aufschrift **raw** aus**.** Wählen Sie die
    Registerkarte **Aggregate-Feature-Importance** aus . Dieses Diagramm
    zeigt, welche Daten-Features die Vorhersagen des ausgewählten
    Modells beeinflusst haben.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

In diesem Beispiel scheint die **Dauer** den größten Einfluss auf die
Vorhersagen dieses Modells zu haben.

## **Übung 5: Bereitstellen des besten Modells**

Die automatisierte Machine Learning-Schnittstelle ermöglicht es Ihnen,
das beste Modell als Webdienst bereitzustellen. *Deployment* ist die
Integration des Modells, damit es auf der Grundlage neuer Daten
Vorhersagen treffen und potenzielle Chancenbereiche identifizieren kann.
Für dieses Experiment bedeutet die Bereitstellung in einem Webdienst,
dass das Finanzinstitut nun über eine iterative und skalierbare
Weblösung zur Identifizierung potenzieller Festgeldkunden verfügt.

Nachdem der Experimentlauf abgeschlossen ist, wird die Seite **Details**
mit dem Abschnitt **Best model summary** aufgefüllt. In diesem
Experimentkontext wird **VotingEnsemble** als das beste Modell
angesehen, basierend auf der **AUCWeighted**-Metrik.

1.  Wählen Sie im linken Fensterbereich **Jobs** aus, und wählen Sie das
    Experiment aus, das Sie erstellt haben.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image41.png)

2.  Klicken Sie auf den Anzeigenamen des Experiments.

![](./media/image42.png)

3.  Überprüfen Sie, ob der Status **Completed** lautet.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image43.png)

4.  Sobald die Experiment-Durchlauf abgeschlossen ist, wird die
    **Details**-Seite mit dem Abschnitt **Best model summary**
    aufgefüllt . In diesem Experimentkontext wird **VotingEnsemble** als
    das beste Modell angesehen, basierend auf der Metrik
    **AUC_weighted**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image44.png)

Wir stellen dieses Modell bereit, aber beachten Sie, dass die
Bereitstellung etwa 20 Minuten dauert. Der Bereitstellungsprozess
umfasst mehrere Schritte, einschließlich der Registrierung des Modells,
des Generierens von Ressourcen und deren Konfiguration für den
Webdienst.

5.  Wählen Sie **VotingEnsemble** aus, um die modellspezifische Seite zu
    öffnen.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image45.png)

6.  Wählen Sie oben links das Menü **Deploy** aus, und wählen Sie
    **Deploy to web service** aus**.**

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image46.png)

7.  Füllen Sie den Bereich **Deploy a model** wie folgt auf:

[TABLE]

> Klicken Sie auf **Deploy**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image47.png)

8.  Auf dem Modellbildschirm wird eine Erfolgsmeldung angezeigt, die
    besagt, dass **Model deployment is successfully triggered,** und der
    Status lautet “**Running”**.

![](./media/image48.png)

9.  Sobald die Bereitstellung abgeschlossen ist, ändert sich der Status
    in **Completed**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image49.png)

> Jetzt verfügen Sie über einen betriebsbereiten Webdienst zum
> Generieren von Vorhersagen.

## **Übung 6: Löschen der Ressourcen**

### **Aufgabe 1: Löschen des Endpunkts**

1.  Klicken Sie im linken Bereich von AML Studio auf **Endpoints**.

2.  Wählen Sie den Endpunkt **my-automl-deploy** aus und klicken Sie auf
    **Delete**.

![](./media/image50.png)

3.  Wählen Sie im Dialogfeld “Delete real-time endpoint” die Option
    **Delete** aus.

4.  Sie sollten eine Erfolgsmeldung erhalten, sobald der Endpunkt
    gelöscht wurde.

**Zusammenfassung**

In diesem Lab haben wir gelernt, wie man ein Klassifizierungsmodell mit
No-Code AutoML im Azure Machine Learning Studio trainiert und das beste
Modell als Webdienst bereitstellt.
