# Lab 07: Entwickeln und Testen des Prompt-Flows aus Azure Machine Learning Studio

**Objektiv:**

In diesem Lab lernen wir die wichtigste Benutzer-Reise bei der
Verwendung des Prompt-Flows in Azure Machine Learning Studio kennen. Sie
erfahren, wie Sie den Prompt-Flow in Ihrem Azure Machine
Learning-Arbeitsbereich aktivieren, einen Prompt-Flow erstellen und
entwickeln, den Flow testen und evaluieren und dann in der Produktion
bereitstellen.

Erwartete Dauer – 60 Minuten

## Aufgabe 1: Vorbereiten der Azure-Ressourcen

### Aufgabe 1.1: Erstellen eines Azure Machine Learning-Arbeitsbereichs

Diese Aufgabe konzentriert sich auf das Erstellen eines Azure Machine
Learning-Arbeitsbereichs. Sie erfahren, wie Sie einen dedizierten
Arbeitsbereich einrichten können, um ihre Machine Learning-Projekte
effektiv zu organisieren und zu verwalten. Dieser Arbeitsbereich dient
als zentraler Hub für Zusammenarbeit, Experimente und Bereitstellung.

1.  Melden Sie sich unter +++[https://portal.azure.com+++
    beim](https://portal.azure.com) Azure-Portal an , und melden Sie
    sich mit den Anmeldeinformationen Ihres Administrator-Tenants an.

2.  Wählen Sie auf der Startseite des Azure-Portals die Option **+
    Create a resource** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image1.png)

3.  Verwenden Sie auf der Seite **Create a resource** die Suchleiste, um
    +++Azure Machine Learning+++ zu suchen, und wählen Sie **Azure
    Machine Learning** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image2.png)

4.  Klicken Sie unter **Marketplace** auf **Create dropdown**, und
    wählen Sie **Azure Machine Learning** aus**.**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image3.png)

5.  Geben Sie die folgenden Informationen an, um Ihren neuen
    Arbeitsbereich zu konfigurieren:

    - **Abonnement**: Wählen Sie Ihr **zugewiesenes**
      **Azure-Abonnement** aus.

    - **Ressourcengruppe**: Wählen Sie die Ihnen **zugewiesene
      Ressourcengruppe** aus.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image4.png)
>
> **Details zum Arbeitsbereich:**

- **Name des Arbeitsbereichs:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: Wählen Sie die nächstgelegene Region aus **(**hier wird
  die Option **" North Central US "** ausgewählt)

&nbsp;

- **Container Registry: Wählen Sie “Create new” aus. Geben Sie
  +++azuremlcr@lab.LabInstanceId+++**

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image5.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image6.png)

6.  Wenn Sie mit der Konfiguration des Arbeitsbereichs fertig sind,
    wählen Sie **Review + Create** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image7.png)

7.  Sobald die Validierung bestanden ist, klicken Sie auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image8.png)

8.  Klicken Sie auf **Go to Resource**, um den neuen Arbeitsbereich
    anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image9.png)

9.  Klicken Sie auf der Seite **Microsoft.MachineLEarningServices |
    Overview**, wählen Sie unter **Work with your model in Azure Machine
    Learning studio** die Option **Launch Studio** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image10.png)

### Aufgabe 1.2: Erstellen eines Computes

Diese Aufgabe veranschaulicht die Erstellung einer Compute-Ressource in
Azure. Sie erkunden verschiedene Computeoptionen, z. B. virtuelle
Maschine oder verwaltete Computecluster, und verstehen, wie Sie
Ressourcen konfigurieren und bereitstellen, um Machine
Learning-Workloads effizient auszuführen.

1.  Nachdem **Azure** **Machine Learning Studio** geöffnet wurde,
    klicken Sie im linken Bereich unter **Manage** auf **Compute**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image11.png)

2.  Klicken Sie auf dem Bildschirm **Compute-Instances** auf **+ New**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image12.png)

3.  Geben Sie auf dem Bildschirm Create compute instance
    (Compute-Instanz erstellen) die folgenden Details ein.

    1.  Compute-Name – +++**pfcompute**+++

    2.  Typ der virtuellen Maschine – **CPU**

    3.  Größe der virtuellen Maschine – Wählen Sie **Standard_E4ds_v4**
        aus

> Klicken Sie auf **Review + Create**.

**Hinweis:** Notieren Sie sich diesen Computenamen für die spätere
Verwendung.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image13.png)

4.  Klicken Sie im nächsten Bildschirm auf **Create**, um die Compute zu
    erstellen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image14.png)

**Hinweis:** Es dauert etwa 10 Minuten, bis die Compute den Status
"Running" erreicht.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image15.png)

**Wichtig:** Sobald Compute betriebsbereit ist, können Sie mit den
nächsten Aufgaben fortfahren. Wenn Sie jedoch eine Pause von der
Lab-Ausführung einlegen, stellen Sie sicher, dass Sie die
Compute-Instanz **stoppen** und erneut starten, wenn Sie nach der Pause
starten.

### Aufgabe 1.3: Erstellen einer Azure OpenAI-Ressource

1.  Suchen Sie im Azure-Portal +++https://portal.azure.com+++ nach
    +++**AzureOpenAI**+++, und wählen Sie es aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image16.png)

2.  Klicken Sie auf **+ Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image17.png)

3.  Geben Sie die folgenden Details ein und klicken Sie auf **Next**.

- Ressourcengruppe: Wählen Sie die zugewiesene Ressourcengruppe aus.

- Region – Wählen Sie eine Region aus (North Central US wird hier
  verwendet)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Tarif – **Standard**

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image18.png)

4.  Übernehmen Sie die Standardeinstellungen auf den nächsten Seiten und
    klicken Sie auf der Seite **Review + submit** auf **Create**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image19.png)

5.  Klicken Sie auf **Go to resource,** sobald die Bereitstellung
    abgeschlossen ist.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image20.png)

6.  Wählen Sie im linken Bereich **Keys and Endpoint** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image21.png)

7.  Kopieren Sie den **Key** und den **Endpoint,** und speichern Sie sie
    in einem Editor, um sie in einem späteren Teil des Labs zu
    verwenden.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image22.png)

8.  Wählen Sie in **Azure Machine Learning Studio** im linken Bereich
    **Model catalog** und dann **gpt-4o** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image23.png)

9.  Klicken Sie auf **Deploy** , um das Modell bereitzustellen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image24.png)

10. Übernehmen Sie den Bereitstellungsnamen, und wählen Sie **Deploy**
    aus. Notieren Sie sich diesen Namen für die zukünftige Verwendung.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image25.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image26.png)

## Aufgabe 2: Einrichten einer Prompt-Flow-Verbindung

1.  Wählen Sie im linken Navigationsbereich von Azure Machine Learning
    Studio die Option **Prompt flow** aus. Wählen Sie in der Menüleiste
    **Connections** aus. Wählen Sie die Dropdownliste neben **Create**
    aus, und wählen Sie **Azure OpenAI** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image27.png)

2.  Geben Sie im Assistenten zum Add Azure OpenAI connection die
    folgenden Details an, und wählen Sie **Save** aus.

- Name – +++**AoaiML_pf**+++

- Anbieter – Wählen Sie **Azure OpenAI** aus

- Abonnement-ID – Wählen Sie Ihr **zugewiesenes** **Abonnement** aus

- Azure OpenAI-Kontoname: Wählen Sie **AOAI-PF@lab.LabInstanceId** aus.

- Authentifizierungsmodus – **API-Key** auswählen

- API-Key: Geben Sie den **Key** an, den wir in der **Azure
  OpenAI-Ressource** gespeichert haben.

- API-Base: Geben Sie den **Endpoint** an, den wir aus der **Azure
  OpenAI-Ressource** gespeichert haben.

> ![Ein Screenshot eines Computers Beschreibung wird automatisch
> generiert](./media/image28.png)

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image29.png)

3.  Überprüfen Sie, ob die Verbindungserstellung erfolgreich war.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image30.png)

## Aufgabe 3: Erstellen und Entwickeln des Prompt-Flows

1.  Wählen Sie auf der Startseite des **Prompt-Flows** auf der
    Registerkarte **Flows** die Option **Create** aus, um den Promptflow
    zu erstellen. Auf der Seite **"** **Create a new flow** " werden
    Flow-Typen angezeigt, die Sie erstellen können, integrierte
    Beispiele, die Sie klonen können, um einen Flow zu erstellen, und
    Möglichkeiten zum Importieren eines Flows.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image31.png)

2.  Wählen Sie **Clone** unter der Kategorie **WebClassification** aus.

In der **Galerie "Explore**" können Sie die integrierten Beispiele
durchsuchen und auf einer beliebigen Kachel die Option **" View detail**
" auswählen, um eine Vorschau anzuzeigen, ob sie für Ihr Szenario
geeignet ist.

In diesem Lab wird das Beispiel für die **Web Classification** verwendet
, um die Hauptuser Journey zu durchlaufen.

Die Webklassifizierung ist ein Flow, der die Klassifizierung mehrerer
Klassen mit einem LLM demonstriert. Bei einer gegebenen URL
klassifiziert der Flow die URL mit nur wenigen Aufnahmen, einer
einfachen Zusammenfassung und Klassifizierung-Prompts in eine
Webkategorie. Wenn z. B. eine URL https://www.imdb.com, wird die URL in
Film klassifiziert.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image32.png)

3.  Übernehmen Sie den Namen, der für **Folder Name** ausgefüllt ist,
    und wählen Sie dann **Clone** aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image33.png)

4.  Für die Ausführung des Flows ist eine Compute-Sitzung erforderlich.
    Die Compute-Sitzung verwaltet die Computing-Ressourcen, die für die
    Ausführung der Anwendung erforderlich sind, einschließlich eines
    Docker-Images, das alle erforderlichen Abhängigkeitspakete enthält.

5.  Starten Sie auf der Flow-Erstellungsseite eine Computesitzung, indem
    Sie **Start compute session** auswählen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image34.png)

**Hinweis:** Es dauert etwa **10 Minuten**, bis die Compute-Sitzung in
den Status “Running” versetzt wird.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image35.png)

## Aufgabe 4: Überprüfen der Flow-Authoring-Seite

Der Start der Computesitzung kann einige Minuten dauern. Während die
Compute-Sitzung gestartet wird, zeigen Sie die Teile der
Flow-Authoring-Seite an.

- Die **Flow-** oder *Flatten*-Ansicht auf der linken Seite der Seite
  ist der Hauptarbeitsbereich, in dem Sie den Flow erstellen können,
  indem Sie Knoten hinzufügen oder entfernen, Knoten inline bearbeiten
  und ausführen oder Prompts bearbeiten. In den Abschnitten
  **Inputs** und **Outputs** können Sie Ein- und Ausgänge anzeigen,
  hinzufügen oder entfernen und bearbeiten.

Beim Klonen des aktuellen Webklassifizierungsbeispiels waren die Ein-
und Ausgaben bereits festgelegt. Das Eingabeschema für den Flow lautet
name: url; type: string, eine URL vom Typ string. Sie können den
voreingestellten Eingabewert manuell in einen anderen Wert ändern, z. B.
https://www.imdb.com.

- **Dateien** oben rechts zeigt den Ordner und die Dateistruktur des
  Flows an. Jeder Flow-Ordner enthält eine Datei *flow.dag.yaml*,
  Quellcodedateien und Systemordner. Sie können Dateien für Tests,
  Bereitstellungen oder die Zusammenarbeit erstellen, hochladen oder
  herunterladen.

- In der **Graph**-Ansicht unten rechts können Sie visualisieren, wie
  der Flow aussieht. Sie können die Ansicht vergrößern oder verkleinern
  oder das automatische Layout verwenden.

Sie können Dateien inline in der **Flow**- oder Flatten-Ansicht
bearbeiten, oder Sie können den Schalter für den **Raw file-Modus**
aktivieren und eine Datei aus **Files** auswählen, um die Datei in einer
Registerkarte zur Bearbeitung zu öffnen.

Sie können Dateien inline in der **Flow**- oder Flatten-Ansicht
bearbeiten, oder Sie können den Schalter für den Rohdateimodus
aktivieren und eine Datei aus **Files** auswählen, um die Datei in einer
Registerkarte zur Bearbeitung zu öffnen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image36.png)

## Aufgabe 5: Einrichten von LLM-Knoten

Für jeden LLM-Knoten müssen Sie eine **Connection** auswählen, um die
LLM-API-Schlüssel festzulegen. Wählen Sie Ihre Azure OpenAI-Verbindung
aus.

Je nach Verbindungstyp müssen Sie eine **deployment_name** oder ein
Modell aus der Dropdown-Liste auswählen. Wählen Sie für eine Azure
OpenAI-Verbindung eine Bereitstellung aus. 

1.  Geben Sie für die summarize_text_content die folgenden Details ein.

Verbindung – Wählen Sie **AoaiML_pf**

Api – **Chat** auswählen

Bereitstellungsname – Wählen Sie **gpt-4o-2024-11-20** aus

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image37.png)

2.  Richten Sie die Verbindung auf ähnliche Weise für die LLM-Knoten
    **classify_with_llm ein**.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image38.png)

3.  Um einen einzelnen Knoten zu testen und zu debuggen, wählen Sie das
    Symbol **Run** oben auf einem Knoten in der **Flow**-Ansicht aus.
    Sie können **Inputs** erweitern und die Flow-Eingabe-URL ändern, um
    das Knotenverhalten für verschiedene URLs zu testen.

4.  Der Ausführungsstatus wird oben auf dem Knoten angezeigt. Nach
    Abschluss der Ausführung wird die Ausführungsausgabe im Abschnitt
    **Output** des Knotens angezeigt .

5.  Wechseln Sie zum Anfang des Flows, führen Sie die
    **fetch_text_content_from url** aus und führen Sie den Block aus.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image39.png)

In der **Graph**-Ansicht wird auch der Status des einzelnen Laufknotens
angezeigt.

6.  Geben Sie im Abschnitt **Inputs** den Wert für das Feld **Value**
    als
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++
    an.

Wählen Sie oben rechts **Run** aus, um den gesamten Flow zu testen und
zu debuggen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image40.png)

## Aufgabe 5: Anzeigen von Flow-Ausgaben

Sie können auch Flow-Ausgaben festlegen, um die Ausgaben mehrerer Knoten
an einem Ort zu überprüfen. Flow-Ausgaben unterstützen Sie bei
Folgendem:

- Überprüfen Sie die Ergebnisse von Massentests in einer einzigen
  Tabelle.

- Definieren Sie das Mapping der Auswertungsschnittstelle.

- Legen Sie das Antwortschema für die Bereitstellung fest.

1.  Wählen Sie im oberen Banner oder in der oberen Menüleiste die Option
    **View outputs** aus, um detaillierte Informationen zu Eingaben,
    Ausgaben, Flow-Ausführung und Orchestrierung anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image41.png)

2.  Beachten Sie, dass der Flow auf der Registerkarte "Outputs" des
    Bildschirms "Outputs" die Eingabe-URL mit einer **Category** und
    einem **Evidence** vorhersagt. ![Ein Screenshot eines Computers
    Beschreibung wird automatisch generiert](./media/image42.png)

3.  Wählen Sie auf dem Bildschirm **Outputs** die Registerkarte
    **Trace** aus, und wählen Sie dann unter **Node Name** die Option
    **Flow** aus, um detaillierte Informationen zur Flow-Übersicht im
    rechten Bereich anzuzeigen. Erweitern Sie **Flow**, und wählen Sie
    einen beliebigen Schritt aus, um detaillierte Informationen für
    diesen Schritt anzuzeigen.

![Ein Screenshot eines Computers Beschreibung wird automatisch
generiert](./media/image43.png)

**Zusammenfassung:**

In diesem Lab haben wir gelernt, die URL mit einfacher Zusammenfassung
und Klassifizierung-Prompts mithilfe des Prompt-Flows in Azure Machine
Learning Studio in eine Webkategorie zu klassifizieren.
