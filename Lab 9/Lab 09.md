# **Lab 09 - Einrichten von MLOps mit GitHub**

**Objektiv:**

Azure Machine Learning ermöglicht Ihnen die Integration in **GitHub
Actions**, um den Machine Learning-Lebenszyklus zu automatisieren.

In diesem Lab erfahren Sie, wie Sie mithilfe von Azure Machine Learning
eine End-to-End-MLOps-Pipeline einrichten, die eine lineare Regression
ausführt, um Taxi-Tarife in NYC vorherzusagen. Die Pipeline besteht aus
Komponenten, die jeweils unterschiedliche Funktionen erfüllen, die beim
Arbeitsbereich registriert, versioniert und mit verschiedenen Ein- und
Ausgaben wiederverwendet werden können.

Erwartete Dauer: 60 Minuten

Wir befinden uns in der MLOps-Phase von Azure Machine Learning

![](./media/image1.png)

## **Übung 1: Vorbereiten der Azure-Ressourcen**

### **Aufgabe 1: Erstellen eines Azure Machine Learning-Arbeitsbereichs**

1.  Melden Sie sich unter +++[https://portal.azure.com+++
    beim](https://portal.azure.com) Azure-Portal an , wenn Sie noch
    nicht angemeldet sind.

2.  Wählen Sie auf der Startseite des Azure-Portals die Option **+
    Create a resource** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image2.png)

3.  Verwenden Sie auf der Seite **Create a resource** die Suchleiste, um
    +++Azure Machine Learning+++ zu suchen.

4.  Wählen Sie **Machine Learning** aus**.**

> ![](./media/image3.png)

5.  Klicken Sie unter **Marketplace** auf **Create dropdown**, und
    wählen Sie **Azure Machine Learning** aus**.**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image4.png)

6.  Geben Sie die folgenden Informationen an, um Ihren neuen
    Arbeitsbereich zu konfigurieren:

    - **Abonnement**: Wählen Sie Ihr **zugewiesenes**
      **Azure-Abonnement** aus.

    - **Ressourcengruppe**: Wählen Sie die **Ressourcengruppe** aus, die
      Ihnen **zugewiesen ist**.

> **Details zum Arbeitsbereich:**

- **Name des Arbeitsbereichs:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: Wählen Sie die nächstgelegene Region aus **(**hier wird
  die Option **" North Central US** " ausgewählt)

&nbsp;

- **Container Registry: Wählen Sie “Create new” aus. Geben Sie
  +++azuremlcr@lab.LabInstance.Id+++ ein**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image5.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

7.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.png)

8.  Klicken Sie auf **Go to Resource**, um den neuen Arbeitsbereich
    anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

9.  Klicken Sie auf der Seite **Microsoft.MachineLEarningServices |
    Overview**, wählen Sie unter **Work with your model in Azure Machine
    Learning studio** die Option **Launch studio** aus**.**

![Ein Screenshot eines Software-Updates Beschreibung wird automatisch
generiert](./media/image9.png)

### **Aufgabe 2: Erstellen eines Computes**

1.  Nachdem Azure Machine Learning Studio geöffnet wurde, klicken Sie im
    linken Bereich unter **Manage** auf **Compute**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

2.  Wählen Sie die Registerkarte **Compute-Clusters** aus und klicken
    Sie auf **+ New**.

![](./media/image11.png)

3.  Geben Sie auf dem Bildschirm **Create compute cluster
    (Computecluster erstellen**) die folgenden Details ein.

    1.  Standort: Wählen Sie die **Region** aus , in der Sie Ihren Azure
        Machine Learning-Arbeitsbereich erstellt haben.

    2.  Ebene der virtuellen Maschine – **Dedicated**

    3.  Typ der virtuellen Maschine – **CPU**

    4.  Größe der virtuellen Maschine – Wählen Sie **Standard_E4s_v3**
        aus (Aktivieren Sie “Select from all options” um die VM-Größe zu
        finden**)**

> Klicken Sie auf **Next**.
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image12.png)

4.  Geben Sie auf der Seite **Advanced Settings** die folgenden Details
    ein.

&nbsp;

1.  Name des Computes: +++ **cpu-cluster@lab.LabInstanceId +++**

2.  Mindestanzahl von Knoten: 0

3.  Maximale Anzahl von Knoten: 1

> Klicken Sie auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.png)

**Hinweis:** Es dauert etwa 10 Minuten, bis die Compute den Status
"Running" erreicht.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

## **Übung 2: Abrufen der Azure-Ressourcen**

1.  Öffnen Sie im Azure-Portal (<https://portal.azure.com>) Ihre
    Ressourcengruppe, und notieren Sie sich die Namen der folgenden
    Ressourcen:

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage Account**

> Speichern Sie sie lokal in einem Notepad, um sie in der
> Konfigurationsdatei zu aktualisieren.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.png)

## **Übung 3: Vorbereiten des GitHub-Kontos und der Ressourcen**

**Hinweis:** Wenn Sie noch kein Konto bei GitHub haben, erstellen Sie
hier +++ **https://github.com/**+++ -\> **Signup**.

### **Aufgabe 2: Forken Sie die Repo mlops-Demo in Ihr GitHub-Konto**

1.  Öffnen Sie einen Browser und geben Sie diesen Link ein -
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  Klicken Sie oben rechts auf **Fork**.

![Ein Screenshot eines Chats Beschreibung wird automatisch mit mittlerer
Zuverlässigkeit generiert](./media/image16.png)

3.  Daraufhin wird die Seite **Create a new fork** geöffnet. Klicken Sie
    auf **Create fork.**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

4.  Wählen Sie in Ihrem GitHub-Projekt **Settings** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image18.png)

5.  Wählen Sie unter **Secrets and variables** die Option **Actions**
    aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.png)

6.  Wählen Sie **New repository secret** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image20.png)

7.  Benennen Sie diesen geheimen Schlüssel als
    **+++AZURE_CREDENTIALS+++**, und fügen Sie die folgende **Service
    Principal**-Ausgabe als Inhalt des geheimen Schlüssels ein. Dieser
    Dienstprinzipal wurde vorab für Sie erstellt. Wählen Sie **Add
    secret** aus.

> {
>
> "clientId": "+++@lab .Variable(spAppId)+++",
>
>   "clientSecret": "+++@lab .Variable(spClientSecret)+++",
>
>   "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
>   "tenantId": "+++@lab.CloudSubscription.TenantId+++",
>
>   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
>   "resourceManagerEndpointUrl": "https://management.azure.com/",
>
>   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
>   "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
>   "galleryEndpointUrl": "https://gallery.azure.com/",
>
>   "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> geringer Zuverlässigkeit generiert](./media/image21.png)

8.  Der hinzugefügte Secret **AZURE_CREDENTIALS** wird unter
    **Repository-Secrets** angezeigt.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image22.png)

9.  Klicken Sie auf **New Repository-secret.**

![](./media/image23.png)

10. Geben Sie die folgenden Details an.

    1.  Name – +++ARM_CLIENT_ID+++

    2.  Secret – +++@lab . Variable(spAppId)+++

> ![Ein Screenshot eines geheimen Computergeheimnisses Beschreibung wird
> automatisch mit geringer Zuverlässigkeit
> generiert](./media/image24.png)

11. Wiederholen Sie die Schritte 9 und 10 für die folgenden Werte, und
    erstellen Sie zusätzliche GitHub-Secrets.

    - +++ARM_CLIENT_SECRET+++ - +++@lab . Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab. CloudSubscription.TenantId+++

## **Übung 4: Konfigurieren von Machine Learning-Umgebungsparametern**

1.  Navigieren Sie auf der Secrets-Seite zur Repository-Seite, indem Sie
    oben links neben Ihrer GitHub-ID auf **mlops-v2-gha-demo** klicken.

![](./media/image25.png)

2.  Wählen Sie die **config-infra-prod.yml**-Datei im Stammverzeichnis
    aus. Klicken Sie auf **Edit** (Das Bleistiftsymbol).

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.png)

3.  Ändern Sie die Werte,

    1.  **Namespace**– **mlopsliteXX** (XX durch eine Zufallszahl
        ersetzen)

    2.  **Postfix** – **c**

    3.  **Standort** – **Identisch mit der Region Ihres
        Arbeitsbereichs**

> Klicken Sie auf **Commit changes**.
>
> Ersetzen Sie im **Abschnitt** **For pipeline reference die Werte** der
> Azure-Ressourcen durch die Werte, die wir in Übung 2 abgerufen und
> gespeichert haben.
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image27.png)

4.  Klicken Sie im Bereich " Commit Changes " auf " **Commit changes**
    ".

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image28.png)

5.  Öffnen Sie **deploy-model-training-pipeline-classical.yml** von
    **.github/workflows**. Klicken Sie auf **Edit** (Das
    Bleistiftsymbol).

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image29.png)

6.  Ersetzen Sie im Inhalt der Datei den Wert von **Size** durch
    **+++Standard_E4s_v3+++**

Wählen Sie **Commit changes** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image30.png)

7.  Öffnen Sie die Datei **online-deployment.yml** aus
    **mlops/azureml/deploy/online.** Klicken Sie auf **Edit** (das
    Bleistiftsymbol).

![](./media/image31.png)

8.  Ersetzen Sie den Wert von **instance_type** durch
    **+++Standard_E4s_v3+++**. Klicken Sie auf **Commit changes**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
geringer Zuverlässigkeit generiert](./media/image32.png)

9.  Öffnen Sie **tf-gha-deploy-infra.yml**-Datei unter
    **.github/workflows**. Klicken Sie auf **Edit** und ersetzen Sie
    Azure durch +++CoursesTF+++ in den Zeilen 9 und 14.

Wählen Sie **Commit changes** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image33.png)

10. Wählen Sie in der oberen Menüleiste **Actions** aus. Klicken Sie auf
    **I understand my workflows, go ahead and enable them**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image34.png)

11. Dadurch werden die vordefinierten GitHub-Workflows angezeigt, die
    Ihrem Projekt zugeordnet sind.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image35.png)

## **Übung 5: Bereitstellen einer Machine Learning-Infrastruktur**

1.  Wählen Sie **tf-gha-deploy-infra.yml** aus. Klicken Sie auf
    **Runworkflow**.

Auswählen

- Filiale – **main**

Wählen Sie **Run workflow** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image36.png)

2.  Dadurch würde die Machine Learning-Infrastruktur mithilfe von GitHub
    Actions und Terraform bereitgestellt.

3.  Verfolgen Sie den Status des Jobs, und bestätigen Sie, dass die
    Ausführung erfolgreich war.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image37.png)

**Hinweis:** Dieser Workflow dauert ca. 5 Minuten.

## **Übung 6: Bereitstellen der Modelltrainingspipeline**

Als Nächstes stellen Sie die Modelltrainingspipeline in Ihrem neuen
Machine Learning-Arbeitsbereich bereit.

Diese Pipeline erstellt eine Computecluster-Instanz, registriert eine
Trainingsumgebung, in der das erforderliche Docker-Image und die
Python-Pakete definiert sind, registriert ein Trainingsdataset und
startet dann die im letzten Abschnitt beschriebene Trainingspipeline.

1.  Klicken Sie auf der Workflow-Seite **tf-gha-deploy-infra.yml** auf
    **Actions**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image38.png)

2.  Dadurch werden die vordefinierten GitHub-Workflows angezeigt, die
    Ihrem Projekt zugeordnet sind. Wählen Sie
    **deploy-model-training-pipeline** aus der Liste aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image39.png)

3.  Klicken Sie auf **Run workflow** -\> **Run workflow**.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image40.png)

4.  Klicken Sie auf die gerade gestartete Pipeline, um den Fortschritt
    zu verfolgen.

![Ein Bild, das Text, Software, Webseite, Schriftart enthält
Beschreibung wird automatisch generiert](./media/image41.png)

5.  Diese Pipeline dauert etwa 15 bis 45 Minuten.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image42.png)

6.  Nachfolgend finden Sie einen Screenshot der erfolgreichen
    Pipelineausführung.

![](./media/image43.png)

7.  Durch diese Ausführung wird das Modell im Machine
    Learning-Arbeitsbereich registriert.

8.  Melden Sie sich bei <https://ml.azure.com/> beim
    AzureMachineLearning Studio [an,](https://ml.azure.com/) und klicken
    Sie im linken Bereich auf **Data**, um zu überprüfen, ob die
    **taxi-data** dort hinzugefügt wurden. Dies erfolgt im Rahmen des
    **register-dataset**-Jobs des Workflows.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image44.png)

9.  Klicken Sie im linken Fensterbereich auf **Jobs** und wählen Sie
    **taxi-fare-training**. Dies wird im **Run-Pipeline**-Job des
    Workflows ausgeführt.

![](./media/image45.png)

10. Wählen Sie den Anzeigenamen der letzten Ausführung aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image46.png)

11. Erkunden Sie die Phasen und die Details, die mit dem Training
    verbunden sind.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image47.png)

Nachdem das trainierte Modell im Machine Learning-Arbeitsbereich
registriert ist, können Sie das Modell für die Bewertung bereitstellen.

**Zusammenfassung**

In diesem Lab haben wir gelernt, wie Sie mithilfe von Azure Machine
Learning eine End-to-End-MLOps-Pipeline einrichten, die die Daten
vorbereitet und die Modelltrainingspipeline in Ihrem neuen Machine
Learning-Arbeitsbereich bereitgestellt hat.
