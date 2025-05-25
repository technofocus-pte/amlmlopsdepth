# Lab 08 – Implementierung der QA-Datengenerierung mit RAG über einen Prompt-Flow

**Objektiv:**

Die QA-Datengenerierung ist ein Teil des RAG-Erstellungsprozesses
(Retrieval Augmented Generation), bei dem der automatisch generierte
QA-Dataset verwendet wird, um die beste Prompt für RAG und
Bewertungsmetriken für RAG zu erhalten

In diesem Lab erfahren Sie, wie Sie aus Ihren Daten ein QA-Dataset
erstellen.

Erwartete Dauer – 60 Minuten

## Übung 1: Erstellen von AOAI-Bereitstellungen 

In dieser Übung erstellen wir die Bereitstellungen der
gpt-35-turbo-Modelle mithilfe der Azure OpenAI-Ressource, die wir im
vorherigen Lab erstellt haben.

1.  Wählen Sie in Azure Machine Learning Studio im linken Bereich
    **Model Catalog** aus . Suchen Sie nach +++**gpt-35-turbo+++** und
    wählen Sie **gpt-35-turbo** aus der Modellliste aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image1.png)

2.  Stellen Sie sicher, dass die AOAI-Ressource
    **AOAI-PF@lab.LabInstanceId** im **Azure OpenAI-Resource**-Feld
    ausgewählt ist. Wählen Sie **Deploy** aus, um das Modell
    bereitzustellen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image2.png)

3.  Übernehmen Sie den **Deployment name**, und wählen Sie **Deploy**
    aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image3.png)

4.  Wiederholen Sie die Modellbereitstellung für
    **text-embedding-ada-002** mit dem Bereitstellungsnamen als +++
    **text-embedding-ada-002-2**+++

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.png)

## Übung 2: Einrichten der Umgebung

1.  Wählen Sie im linken Bereich von Studio die Option **Notebooks**
    aus. Klicken Sie auf die drei Punkte neben dem Benutzernamen und
    wählen Sie **Upload files**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.png)

2.  Navigieren Sie zu **C:\LabFiles**, und wählen Sie die Datei
    **qa_data_generation.ipynb** aus. Aktivieren Sie das
    Kontrollkästchen **"** **I trust contents of this file** " und
    klicken Sie auf **Upload**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

3.  Öffnen Sie das Notebook, und wählen Sie in der Option **Compute**
    die Option **Serverless Spark Compute** aus.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image7.png)

4.  Nachdem die Compute angefügt wurde, wählen Sie **Configure session**
    aus, um die conda.yml-Datei hochzuladen und die Umgebung für die
    Ausführung mit dieser Datei einzurichten.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

5.  Wählen Sie **Python packages** -\>**Upload Conda file** -\> klicken
    Sie auf **Browse**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

6.  Wählen Sie die **conda.yml** aus **C:\LabFiles** aus, und wählen Sie
    **Apply** aus.

> ![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
> generiert](./media/image10.png)

## Übung 3: Abrufen des Clients für den AzureML-Arbeitsbereich

1.  Führen Sie die erste Zelle des Notebooks aus, um die Abhängigkeiten
    zu installieren

![](./media/image11.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image12.png)

**Hinweis:** Dies dauert 10 bis 15 Minuten

2.  Führen Sie die nächste Zelle mit az login aus, um **sich** bei der
    **Azure CLI anzumelden**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.png)

3.  Der Arbeitsbereich ist die Ressource auf oberster Ebene für Azure
    Machine Learning und bietet einen zentralen Ort für die Arbeit mit
    allen Artefakten, die Sie bei der Verwendung von Azure Machine
    Learning erstellen. In diesem Abschnitt stellen wir eine Verbindung
    zu dem Arbeitsbereich her, in dem der Job ausgeführt wird. MLClient
    ist die Art und Weise, wie Sie mit AzureML interagieren

4.  Ersetzen Sie die Platzhalter für die **Subscription ID** durch
    +++@lab.Subscription()+++, **Ressourcengruppe** mit dem **Namen
    Ihrer** **Ressourcengruppe** und **Azure ML-Workspace** mit
    +++**Azuremlws@lab.LabInstanceId+++** in der nächsten Zelle, um den
    MClient zu erstellen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.png)

5.  **Führen Sie** die nächste Zelle **aus**, die den **Connection
    name** festlegt. Wenn Sie beim Erstellen der Verbindung einen
    anderen Namen verwendet haben, geben Sie diesen Wert in diese Zelle
    ein, und führen Sie ihn dann aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.png)

6.  Ersetzen Sie den **Key**-Wert durch den **Azure openAI-Key** und den
    **Target**-Wert durch den **Endpoint**-Wert der Azure
    OpenAI-Ressource, den wir zuvor gespeichert haben.

**Führen Sie** die Zelle **aus**, nachdem Sie die Werte ersetzt haben.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

7.  Nachdem Ihr Arbeitsbereich nun über eine Verbindung mit Azure OpenAI
    verfügt, stellen wir sicher, dass das gpt-35-turbo-Modell
    bereitgestellt wurde und für Inferenz bereit ist.

8.  **Führen Sie** die nächste Zelle **aus**, um die Modell- und
    **Deployment-**Namen festzulegen. Ersetzen Sie die Werte des
    Modellnamens und des Bereitstellungsnamens, wenn Sie beim Erstellen
    des Modells und der Bereitstellung unterschiedliche Namen angegeben
    haben.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch
generiert](./media/image18.png)

9.  Schließlich kombinieren wir die Bereitstellungs- und
    Modellinformationen in einer uri-Form, die von den
    AzureML-Einbettungskomponenten als Eingabe erwartet wird. **Führen
    Sie** dazu die nächste Zelle **aus**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.png)

## Übung 4: Einrichten der Pipeline

AzureML-Pipelines verbinden mehrere Komponenten miteinander. Jede
Komponente definiert Eingaben, d. h. Code, der die aus dem Code
erzeugten Ein- und Ausgaben verarbeitet. Pipelines selbst können
Eingaben und Ausgänge haben, die durch die Verbindung einzelner
Unterkomponenten erzeugt werden. Um Ihre Daten für die Einbettung und
Indizierung zu verarbeiten, verketten wir mehrere Komponenten, die
jeweils einen eigenen Schritt des Workflows ausführen.

Die Komponenten werden in einer Registry, azureml, veröffentlicht, auf
die standardmäßig zugegriffen werden sollte und auf die von jedem
Arbeitsbereich aus zugegriffen werden kann. In der folgenden Zelle
erhalten wir die Komponentendefinitionen aus der azureml-Registry.

1.  Führen Sie die nächste Zelle aus und stellen Sie sicher, dass sie
    ohne Probleme ausgeführt wird.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch
generiert](./media/image20.png)

2.  Jede Komponente verfügt über eine Dokumentation, die eine allgemeine
    Beschreibung des Zwecks der Komponente und jeder der Ein-/Ausgabe
    enthält. Zum Beispiel können wir verstehen, was
    **data_generation_component** tut, indem wir die
    Komponentendefinition überprüfen. **Führen Sie** dazu die nächste
    Zelle **aus** und beobachten Sie die Ausgabe.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

3.  Unten wird eine Pipeline erstellt, indem eine Python-Funktion
    definiert wird, die die oben genannten Komponenten, Ein- und Ausgabe
    miteinander verkettet. Argumente für die Funktion sind Eingaben für
    die Pipeline selbst, und der Rückgabewert ist ein Wörterbuch, das
    die Ausgaben der Pipeline definiert. Stellen Sie sicher, dass die
    **nächste Zelle** erfolgreich **ausgeführt wird**.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch
generiert](./media/image22.png)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image23.png)

4.  Die folgenden Einstellungen zeigen, wie die verschiedenen git- und
    data_source-Parameter so festgelegt werden können, dass nur die
    AzureML-Dokumentation aus dem größeren AzureDocs-Git-Repository
    verarbeitet wird, und wie sichergestellt wird, dass die Quell-URL
    für jedes Dokument so verarbeitet wird, dass sie mit der öffentlich
    gehosteten URL anstelle der Git-URL verknüpft ist.

5.  Führen Sie die nächsten beiden Zellen aus, und stellen Sie sicher,
    dass sie erfolgreich ausgeführt werden.

![](./media/image24.png)

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image25.png)

## Übung 5: Übermitteln einer Pipeline

1.  Die Ausgabe jedes Schritts in der Pipeline kann über die
    Workspace-UI überprüft werden, klicken Sie auf den Link unter
    "DetailS-Seite", nachdem Sie die folgende Zelle ausgeführt haben.

2.  Führen Sie die nächste Zelle aus und klicken Sie auf den Link in der
    Ausgabe, um den Flow-Status anzuzeigen

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.png)

3.  Die Ausführung wird im Prompt-Flow geöffnet. Erkunden Sie jede Phase
    des Flows.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image28.png)

4.  Sobald der Flow erfolgreich ist, fahren Sie mit dem nächsten Schritt
    fort.

## Übung 6: Überprüfen der generierten QA-Daten

1.  Führen Sie die nächsten 2 Zellen aus und überprüfen Sie die Ausgabe
    für die QA-Daten.

![Ein Screenshot eines Computercodes Beschreibung wird automatisch
generiert](./media/image29.png)

> ![Ein Screenshot eines Computercodes Beschreibung wird automatisch
> generiert](./media/image30.png)

Zusammenfassung:

In diesem Lab haben wir gelernt, aus Ihren Daten einen QA-Dataset zu
erstellen.
