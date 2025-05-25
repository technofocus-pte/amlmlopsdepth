# Lab 06 - Trainieren des besten Regressionsmodells für das Hardware-Dataset

Objektiv

In diesem Lab erfahren Sie, wie Sie AutoML zum Trainieren eines
Regressionsmodells verwenden können. Wir verwenden das Dataset "Hardware
Performance", um das Modell für die Verwendung in Rückschlussszenarien
zu trainieren und bereitzustellen. Das Regressionsziel besteht darin,
die Leistung bestimmter Kombinationen von Hardwareteilen vorherzusagen.

Erwartete Dauer – 60 Minuten

# Übung 0: Vorbereiten der Umgebung

### **Aufgabe 1: Starten des AML-Arbeitsbereichs**

1.  Melden Sie sich beim Azure-Portal an,
    +++[**https://portal.azure.com**](https://portal.azure.com)+++,
    falls Sie noch nicht angemeldet sind.

2.  Wählen Sie im Menü des Azure-Portals die Option **All resources**
    aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image1.png)

3.  Wählen Sie den Azure Machine Learning-Arbeitsbereich
    (**Azuemlws@lab.LabInstanceId**) aus.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image2.png)

4.  Klicken Sie auf **Launch Studio**.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image3.png)

5.  Wählen Sie im linken Fensterbereich **Compute** aus, um eine
    Compute-Instanz zu erstellen. Wählen Sie **+ New** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.png)

6.  Geben Sie die folgenden Details ein und klicken Sie auf **Review +
    Create**.

- Name des Computes - +++**auto-compute**+++

- Typ der virtuellen Maschine – **CPU**

- Virtuelle Maschine – **Standard-E4ds_v4**

![](./media/image5.png)

7.  Wählen Sie **Create** aus, um die Compute-Instanz zu erstellen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

### **Aufgabe 2: Hochladen des Notebooks in den AML-Arbeitsbereich**

1.  Klicken Sie im linken Bereich auf **Notebooks**. Klicken Sie unter
    **Users** auf die drei Punkte neben dem **Username** und wählen Sie
    **Upload folder** aus.

![](./media/image7.png)

2.  Wählen Sie “Click to browse and select folder(s)” aus, und
    durchsuchen Sie **C:\Labfiles**, um den Ordner
    **automl-regression-task-hardware-performance** auszuwählen**,** und
    klicken Sie auf **Upload.**

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image8.png)

3.  Wenn Sie ein Pop-up-Fenster mit der Frage " Upload 3 files to this
    site?" erhalten, klicken Sie auf **"Upload**".

![Ein Bild, das Text, Screenshot, Anzeige, Schriftart enthält
Beschreibung wird automatisch generiert](./media/image9.png)

4.  Aktivieren Sie das Kontrollkästchen **I trust contents of these
    files**, und wählen Sie dann **Upload** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image10.png)

5.  Öffnen Sie das Notebook (die .ipynb-Datei),
    **automl-regression-task-hardware-performance**. Das Notebook wird
    automatisch mit dem Compute verbunden, das wir zuvor erstellt haben.

![](./media/image11.png)

## **Übung 1: Herstellen einer Verbindung mit Azure Machine Learning Workspace**

### **Aufgabe 1: Importieren der erforderlichen Bibliotheken**

1.  Führen Sie die erste Zelle der Zelle unter **1.1 Import the required
    libraries** aus, um die für diese Lab-Ausführung erforderlichen
    Bibliotheken zu importieren, indem Sie oben links in der Zelle auf
    die Schaltfläche “Run cell” klicken.

2.  Stellen Sie sicher, dass die Ausführung erfolgreich ist, indem Sie
    unten links in der Zelle nach einem Häkchensymbol suchen.

![Ein Screenshot eines Computerbildschirms Beschreibung wird automatisch
mit geringer Zuverlässigkeit generiert](./media/image12.png)

### **Aufgabe 2: Konfigurieren Sie die Details des Arbeitsbereichs und erhalten Sie einen Verweis auf den Arbeitsbereich**

1.  In der Zelle unter **1.2. Configure workspace details and get a
    handle to the workspace,** Ersetzen

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id+++**

- RESOURCE_GROUP – **Der Name der zugewiesenen Ressourcengruppe**

- AML_WORKSPACE_NAME – +++ **Azuremlws@lab.LabInstanceId** +++

2.  Klicken Sie auf die Option “Run cell” oben links in der Zelle und
    stellen Sie sicher, dass Sie unten links ein Häkchensymbol erhalten,
    sobald die Ausführung erfolgreich war.

3.  Unterhalb der Zelle wird die Ausgabe **" Found the config file in :
    /config.json** **"** angezeigt.

![](./media/image13.png)

### **Aufgabe 3: Anzeigen von Informationen zum Azure ML-Arbeitsbereich**

1.  Führen Sie die nächste Zelle aus (die Zelle unter Informationen zum
    Show Azure ML-Workspace).

2.  Stellen Sie sicher, dass die Details des Arbeitsbereichs, des
    Abonnements, des Standorts und der Ressourcengruppe, die als Ausgabe
    unterhalb der Zelle aufgeführt werden, korrekt sind.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image14.png)

## **Übung 2: MLTable mit eingegebenen Trainingsdaten**

### **Aufgabe 1: Erstellen einer MLTable-Dateneingabe**

1.  Führen Sie die nächste Zelle aus (die Zelle unter **2.1 Create
    MLTable data input**).

2.  Stellen Sie sicher, dass die Ausführung erfolgreich ist.

![Ein Bild, das Text, Schriftart, Screenshot, Software enthält
Beschreibung wird automatisch generiert](./media/image15.png)

## **Übung 3: Konfigurieren und Ausführen des AutoML-Regressionstrainingsjobs**

1.  Führen Sie die Zellen unter **4.1 Configure and run the AutoML
    Regression training job** nacheinander aus, und stellen Sie sicher,
    dass jede Zelle erfolgreich ausgeführt wird.

2.  Die Zelle unter **4.2 Run the Command** übermittelt den AutoML-Job.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image16.png)

3.  Sie können den Status des Jobs überprüfen, indem Sie im linken
    Bereich auf **Jobs** klicken und das Experiment auswählen, das sich
    im Status “Running” befindet.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image18.png)

**Hinweis:** Dies dauert etwa 10 bis 15 Minuten.

4.  Die nächste Zelle im Notebook wartet, bis der AutoMLjob
    abgeschlossen ist.

5.  Führen Sie es aus und warten Sie, bis die Ausführung abgeschlossen
    ist, um zur nächsten Zelle zu gelangen.

![](./media/image19.png)

6.  Fahren Sie erst dann mit dem nächsten Schritt fort, wenn die
    Ausführung abgeschlossen ist.

![](./media/image20.png)

7.  Führen Sie die nächsten 2 Zellen nacheinander aus, wodurch die URL
    und der Jobname abgerufen werden.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

## **Übung 4: Abrufen der besten Testversion (Testphase des besten Models)**

1.  Fügen Sie in dieser Übung eine Zelle über der ersten Zelle hinzu.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image22.png)

2.  Kopieren Sie den folgenden Code. Klicken Sie auf **Run cell.**

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image23.png)

3.  Fahren Sie mit der Ausführung der nächsten 3 Zellen fort, indem Sie
    jeden Code und seine Ausgabe analysieren.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image24.png)

4.  Führen Sie die nächste Zelle aus, um **die übergeordnete
    Ausführung(Parent Run) abzurufen**.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
mit geringer Zuverlässigkeit generiert](./media/image25.png)

5.  Führen Sie die nächste Zelle aus, um **die** **übergeordneten Tags**
    **auszugeben.**

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image26.png)

6.  Führen Sie die nächste Zelle aus, um **die beste untergeordnete
    AutoML-Ausführung abzurufen**.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
mit mittlerer Zuverlässigkeit generiert](./media/image27.png)

7.  Führen Sie die nächste Zelle aus, um **die Metriken des besten
    Modelllaufs abzurufen**.

![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image28.png)

8.  Führen Sie die nächsten 3 Zellen aus, um **das beste Modell lokal
    herunterzuladen**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image29.png)

## **Übung 5: Registrieren des besten Modells und Bereitstellen**

### **Aufgabe 1: Erstellen eines verwalteten Onlineendpunkts**

1.  Führen Sie die ersten 2 Zellen unter dieser Aufgabe aus.

![](./media/image30.png)

2.  Führen Sie die nächste Zelle mit dem Code aus,

**ml_client.begin_create_or_update(endpoint).result()**

Dadurch wird ein Onlineendpunkt mit dem Namen
**regression-\<Currentdate&time\>** erstellt.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image31.png)

3.  Überprüfen Sie, ob die Benachrichtigung über das **Update des
    Endpunkts "regression-\<Currentdate&time\>" abgeschlossen ist.**

> ![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
> sein.](./media/image32.png)

### **Aufgabe 2: Registrieren des besten Modells und Bereitstellen**

1.  Führen Sie die erste Zelle unter Register best model und deploy -\>
    **Register model** aus, um das Modell mit dem Namen
    **hardware-performance-model** zu registrieren.

2.  Sobald die Ausführung erfolgreich ist, führen Sie die nächste Zelle
    aus, um die registrierte Modell-ID abzurufen.

> ![](./media/image33.png)

### **Aufgabe 3: Bereitstellen**

1.  Ersetzen Sie in der ersten Zelle unter Deploy den Wert
    **instance_type** durch **Standard_E4s_v3.**

2.  Führen Sie dann die Zelle aus, um das beste Modell bereitzustellen.

![Ein Screenshot eines Computerprogramms Beschreibung wird automatisch
generiert](./media/image34.png)

3.  Führen Sie die nächste Zelle aus, um die Bereitstellung zu
    erstellen.

![Ein Bild, das Text, Screenshot, Linie, Schriftart enthält Beschreibung
wird automatisch generiert](./media/image35.png)

4.  **Dies dauert etwa 40 Minuten**. Sie können auch unter den
    **Endpoints** nach dem Status suchen (wählen Sie im linken Bereich
    **Endpoints** aus, und klicken Sie dann auf den Endpunkt
    **regression-XXXXXXX,** den Sie zuvor bereitgestellt haben).

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image36.png)

5.  Sobald die Ausführung abgeschlossen und die Bereitstellung
    erfolgreich war, werden in der Zelle die Bereitstellungsdetails
    ausgegeben.

![](./media/image37.png)

6.  Außerdem wird auf der Detailseite der Endpunkte der
    Bereitstellungsstatus auf **Succeeded** gesetzt.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image38.png)

7.  Führen Sie die nächste Zelle im Notebook aus, damit die
    Bereitstellung 100 % des Datenverkehrs verarbeitet.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image39.png)

8.  Überprüfen Sie, ob die Live-Traffic-Zuweisung auf der Detailseite
    der Endpunkte 100 % beträgt.

![Ein Screenshot eines Computers KI-generierte Inhalte können falsch
sein.](./media/image40.png)

## **Übung 6: Testen der Bereitstellung**

1.  Führen Sie die Zelle unter dem Feld Testen der Bereitstellung aus.

2.  Überprüfen Sie die Ausgabe.

![](./media/image41.png)

3.  Folgen Sie den verbleibenden Zellen, und führen Sie sie aus, um den
    Endpunkt zu löschen.

![](./media/image42.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image43.png)

4.  Überprüfen Sie den Status des Endpunkts auf der Registerkarte
    Endpoints.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image44.png)

**Zusammenfassung**

In diesem Lab haben wir gelernt, wie man

- Herstellen einer Verbindung mit Ihrem AML-Arbeitsbereich über das
  Python SDK

- Erstellen Sie einen AutoML-Regressionsjob mit der Factory-Funktion
  'regression()'.

- Trainieren Sie das Modell mit AmlCompute, indem Sie den
  AutoML-Regressionstrainingsjob übermitteln/ausführen

- Abrufen des Modells und Bewerten von Vorhersagen damit
