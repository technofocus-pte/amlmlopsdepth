# Lab 02: Erstellen eines bezeichneten Datasets mit Azure Machine Learning-Datenbeschriftungstools

**Objektiv**

In diesem Lab erfahren Sie, wie Sie die Azure Machine
Learning-Datentools in Azure Machine Learning Studio verwenden, um ihre
Sammlungen von nicht bezeichneten Daten in bezeichneten Datasets zu
verwalten, die die Klassen aufnehmen, die vom trainierten
Objekterkennungsmodell erkannt werden.

Voraussichtliche Dauer: 40 Minuten

## **Übung 1: Vorbereiten der Azure-Ressourcen**

### **Aufgabe 1: Erstellen eines Azure Storage-Kontos**

1.  Geben Sie auf der **Home**-Seite des **Azure-Portals**
    (+++**https://portal.azure.com**+++) **+++storageaccount+++** in die
    Suchleiste ein, und wählen Sie die **Storage accounts** aus**.**

![](./media/image1.png)

2.  Wählen Sie **+Create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image2.png)

3.  Geben Sie auf der Seite “Create a storage account” die folgenden
    Details ein.

> **Details zum Projekt**

- Abonnement – Wählen Sie Ihr **Abonnement** aus.

- Ressourcengruppe – Wählen Sie die Ihnen zugewiesene
  **Ressourcengruppe** aus.

> **Details zur Instanz**

- Name des Speicherkontos – +++**imagestoreacc@lab.LabInstance.Id** +++

- Region – Wählen Sie die **Region** aus, in der Sie Ihren
  **AML-Workspace** erstellt haben.

- Leistung – **Standard** auswählen

- Redundanz – Wählen Sie **Locally-redundant storage(LRS)** aus.

Wählen Sie **Next** aus**.**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image3.png)

4.  Stellen Sie auf der Registerkarte “Advanced” sicher, dass die Option
    **Allow cross-tenant replication** im Abschnitt **Blob Storage**
    deaktiviert ist. Übernehmen Sie die anderen Standardwerte, und
    wählen Sie **Review + create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image4.png)

5.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

> ![Ein Screenshot eines Computerfehlers Beschreibung wird automatisch
> generiert](./media/image5.png)

6.  Sobald die Bereitstellung abgeschlossen ist, klicken Sie auf **Go to
    resource**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

7.  Notieren Sie sich den Namen des Speicherkontos, da dieser im
    späteren Teil des Labs verwendet wird. Bleiben Sie auf der gleichen
    Seite und fahren Sie mit der nächsten Aufgabe fort.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.png)

### **Aufgabe 2: Erstellen eines Azure Storage-Containers**

1.  Scrollen Sie im linken Menü der Speicherkontoseite zum Abschnitt
    **Data Storage**, und wählen Sie dann **Containers** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

2.  Wählen Sie den **Container +** aus. Geben Sie im sich öffnenden
    Bereich New Container den Namen des Containers als +++imagedata+++
    ein und klicken Sie dann auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

3.  Nachdem der Container erstellt wurde, wählen Sie im linken Bereich
    unter **Security + networking** die **Access keys** aus. Klicken Sie
    auf der Seite Access keys auf **Show** für den Schlüsselwert
    anzeigen, und **kopieren Sie** dann den Schlüssel. Speichern Sie den
    kopierten Wert in einem Editor, damit Sie später darauf
    zurückgreifen können.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image10.png)

4.  Navigieren Sie zurück zur Containerseite, indem Sie im linken
    Bereich **Containers** auswählen.

![](./media/image11.png)

5.  Wählen Sie den neu erstellten Container **imagedata** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image12.png)

6.  Klicken Sie auf **Upload**. Klicken Sie im Bereich **Upload blob**
    auf **Browse for files,** und öffnen Sie den Ordner **train_img**
    unter **C:\Labfiles**

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image13.png)

7.  Wählen Sie alle Dateien im Ordner train_img aus und klicken Sie auf
    **Open**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

8.  Klicken Sie auf der Seite **“**Upload blob**”** auf **Upload**.

![](./media/image15.png)

9.  Nach dem Hochladen wird die Meldung **Successfully uploaded
    blob(s)** angezeigt, schließen Sie den Bereich **Upload blob**.

![](./media/image16.png)

10. Nach Abschluss des Vorgangs sollten Sie sehen, dass alle 242-Bilder
    dem Azure Storage-Container hinzugefügt wurden.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image17.png)

## **Übung 2: Erstellen eines Azure Machine Learning-Datenbezeichnungsprojekts**

1.  Wählen Sie auf der Startseite von Azure Machine Learning Studio im
    linken Bereich unter **Manage** die Option **Data Labeling** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.png)

2.  Wählen Sie **+ Create** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image19.png)

3.  Geben Sie im Abschnitt **Project details** die folgenden Details an.

    1.  **Projektname** - +++**soda**+++

    2.  Medientyp – **Image**

    3.  **Beschriftungsaufgabe-Typ - Object Identification (Bounding
        Box)**

Wählen Sie **Next** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch mit
mittlerer Zuverlässigkeit generiert](./media/image20.png)

4.  Lassen Sie im Bildschirm **Add workforce (optional)** die Option
    deaktiviert und wählen Sie **Next** aus, um fortzufahren.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image21.png)

5.  Klicken Sie auf der Seite **Select or create data page** auf **+
    Create**.

> ![](./media/image22.png)

6.  Geben Sie auf der Seite **Create data asset** im Bereich **Data
    type** die folgenden Details an.

    1.  **Name** – +++**sodaObjects**+++

    2.  **Beschreibung –** +++ **Image labelling** +++

    3.  **Typ –** File

> Klicken Sie auf **Next**.
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image23.png)

7.  Wählen Sie auf der Seite **Create data asset** im Bereich **Data
    source** die Option **From Azure storage** aus**,** und klicken Sie
    dann auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image24.png)

8.  Wählen Sie auf der Seite **Create data asset** im Bereich **Storage
    type** die Option **Create new datastore** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image25.png)

9.  Geben Sie im Bereich **New** **datastore** die folgenden Details an.

    1.  **Name des Datenspeichers** – +++**sodadatastore**+++

    2.  **Datenspeichertyp**: Wählen Sie **Azure Blob Storage** aus.

    3.  **Kontoauswahlmethode: From Azure-Subscription** auswählen

    4.  **Abonnement-ID –** Wählen Sie Ihr Abonnement aus

    5.  **Speicherkonto –** Wählen Sie **imagestoreacc** aus.

    6.  **Blob Container –** Wählen Sie **imagedata** aus

    7.  **Authentifizierungstyp – Account Key** auswählen

    8.  **Account Key –** Geben Sie den Kontoschlüssel ein, der zuvor in
        Übung 1 gespeichert wurde

> Klicken Sie auf **Create**.
>
> ![](./media/image26.png)
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image27.png)

10. Die Meldung **Create success** wird auf der Seite **Select a
    datastore** angezeigt. Wählen Sie den **sodadatastore** aus, der
    erstellt wurde. Klicken Sie auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image28.png)

11. Wählen Sie unter Choose a storage path die Option **Enter storage
    path manually** aus, und geben Sie **/** für Speicherpfad ein,
    aktivieren Sie **Skip data validation**. Klicken Sie auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image29.png)

12. Überprüfen Sie die Details und klicken Sie auf **Create**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image30.png)

13. Wählen Sie im Bereich **Select or create data** die Option
    **sodaObjects** aus. Klicken Sie auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image31.png)

14. Wählen Sie auf der Seite **Incremental refresh** die Option **Enable
    incremental refresh at regular intervals** aus. Klicken Sie auf
    **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image32.png)

15. Klicken Sie auf der Seite **Label categories** zweimal auf **Add
    label category**, um zusätzlich zu dem bereits vorhandenen
    Platzhalter für Kategorienamen zwei weitere Platzhalter
    hinzuzufügen.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image33.png)

16. Geben Sie nach dem Hinzufügen +++**coke**+++, +++**diet_coke**+++
    und +++**sprite**+++ ein, einen Platzhalter in jeder
    Beschriftungskategorie. Klicken Sie auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image34.png)

17. Lassen Sie die Beschriftungsanleitung leer und klicken Sie auf
    **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image35.png)

18. Klicken Sie auf der Seite **Quality control(preview)** auf **Next**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image36.png)

19. Deaktivieren Sie die Option **Enable** **ML assisted labelling** und
    klicken Sie auf **Create** **project**.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image37.png)

20. Die Meldung **Success: soda data labelling project created
    successfully. Project is initializing** wird auf dem Bildschirm Data
    Labelling angezeigt. Klicken Sie auf das **soda**-Projekt.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image38.png)

21. Klicken Sie auf **Label data**.

> ![](./media/image39.png)

22. Die **Shortcut Keys** oben rechts zeigt die verschiedenen
    verfügbaren Tastenkombinationen an.

> ![Eine Gruppe von Getränkedosen auf einem Tisch Beschreibung wird
> automatisch mit mittlerer Zuverlässigkeit
> generiert](./media/image40.png)

23. In der oberen Menüleiste finden Sie die verschiedenen verfügbaren
    Optionen.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image41.png)

24. Das erste Bild öffnet sich auf dem Bildschirm. Wählen Sie das
    entsprechende Tag aus dem Bereich **Tags** auf der linken Seite aus.

> Klicken Sie dann auf das Bild und ziehen Sie ein wenig, um zu sehen,
> wie das Label an das Bild angehängt wird. Klicken Sie auf **Submit**.
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image42.png)

25. Wiederholen Sie den gleichen Vorgang für die nächsten Bilder, die
    beim Einreichen des aktuellen Bildes angezeigt werden.

> Beschriften Sie mindestens 10 Bilder.
>
> ![](./media/image43.png)

26. Das nächste Bild wird hochgeladen, bis das Ende der Bilder erreicht
    ist. Bitte stoppen Sie an jeder Stelle nach 10 Bildern oder fahren
    Sie fort und schließen Sie die Beschriftung für alle Bilder ab.

27. Klicken Sie auf Soda im oberen Navigationspfad, um zum **Dashboard**
    zurückzukehren.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> mittlerer Zuverlässigkeit generiert](./media/image44.png)

28. Das **Dashboard** enthält Details zu den **beschrifteten Assets**
    und der **Label-Verteilung**.

> ![](./media/image45.png)
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> geringer Zuverlässigkeit generiert](./media/image46.png)

29. Klicken Sie auf **Export.**

> ![Ein Screenshot eines Diagramms Beschreibung wird automatisch mit
> geringer Zuverlässigkeit generiert](./media/image47.png)

30. Wählen Sie im Bereich **Export data** die Option

    - **Asset-Typ - Labeled**

    - **Export Format – Azure ML-Dataset**

> Klicken Sie auf **Submit**.
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> geringer Zuverlässigkeit generiert](./media/image48.png)

31. Die Meldung **" Labels successfully exported "** wird auf der
    Dashboard-Seite angezeigt, sobald der Export abgeschlossen ist.
    Klicken Sie auf den **File link** in der Erfolgsmeldung, um die
    Details der exportierten Datei zu öffnen.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch mit
> geringer Zuverlässigkeit generiert](./media/image49.png)
>
> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image50.png)

32. Klicken Sie auf den Link **View in** **datastores** oder **View**
    **in Azure-Portal** im Abschnitt **Datasources** -\> **Actions**.

> ![](./media/image51.png)

33. In Datastores anzeigen.

> ![Ein Bild, das Text, Zahl, Software, Schriftart enthält Beschreibung
> wird automatisch generiert](./media/image52.png)

**Zusammenfassung**

In diesem Lab haben Sie gelernt, wie Sie ein Datenasset aus Azure
Storage erstellen und wie Sie die Bilder bezeichnen und ein
beschriftetes Dataset erstellen.

Dieser gesamte Aufgabensatz gehört auch zur Phase **" Data: Explore &
prepare** " der **Machine Learning Project Workflow.**
