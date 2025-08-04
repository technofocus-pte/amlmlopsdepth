# Atelier 07 : Développer et tester un flux d'invite à partir d'Azure Machine Learning Studio

**Objectif :**

Dans cet atelier, nous allons découvrir le parcours utilisateur
principal de l'utilisation du flux d'invite dans Azure Machine Learning
Studio. Vous apprendrez à activer le flux d'invite dans votre espace de
travail Azure Machine Learning, à créer et à développer un flux
d'invite, à tester et à évaluer le flux, puis à le déployer en
production.

Durée prévue – 60 minutes

## Tâche 1 : Préparation des ressources Azure

### Tâche 1.1 : Créer un espace de travail Azure Machine Learning

Cette tâche se concentre sur la création d'un espace de travail Azure
Machine Learning. Vous découvrirez comment mettre en place un espace de
travail dédié pour organiser et gérer efficacement leurs projets de
machine learning. Cet espace de travail sert de plaque tournante
centrale pour la collaboration, l'expérimentation et le déploiement.

1.  Connectez-vous au portail Azure à l'adresse
    +++<https://portal.azure.com>+++ et connectez-vous avec vos
    informations d'identification de locataire administrateur.

2.  Dans la page d'accueil du portail Azure, sélectionnez **+ Create a
    resource**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.png)

3.  Dans la page **Create a resource**, utilisez la barre de recherche
    pour trouver +++Azure Machine Learning+++ et sélectionnez **Azure
    Machine Learning**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image2.png)

4.  Sous **Marketplace**, cliquez sur **Create dropdown** et
    sélectionnez **Azure Machine Learning**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image3.png)

5.  Fournissez les informations suivantes pour configurer votre nouvel
    espace de travail :

    - **Subscription** : sélectionnez l**'abonnement Azure qui vous a
      été attribué**

    - **Resource Group** : sélectionnez le **Resource Group qui vous est
      attribué**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image4.png)
>
> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region** : sélectionnez la région la plus proche **(North Central
  US** est sélectionné ici)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstanceId+++**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image5.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

6.  Une fois que vous avez terminé de configurer l'espace de travail,
    sélectionnez **Review + Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.png)

7.  Une fois la Validation passée, cliquez sur **Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.png)

8.  Cliquez sur **Go to resource**, pour afficher le nouvel espace de
    travail.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**,
    sélectionnez **Launch studio** sous **Work with your model in Azure
    Machine Learning studio**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

### Tâche 1.2 : Créer un Calcul (Compute)

Cette tâche illustre la création d'une ressource de calcul (compute)
dans Azure. Vous explorerez différentes options de calcul (compute),
telles que les machines virtuelles ou les clusters de calcul (compute)
gérés, et comprendrez comment configurer et provisionner des ressources
pour exécuter efficacement des charges de travail de machine learning.

1.  Une fois qu’**Azure Machine Learning Studio** s'ouvre, cliquez sur
    **Compute** sous **Manage** dans le volet gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.png)

2.  Cliquez sur **+ New** sur l'écran **Compute instances**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image12.png)

3.  Sur l'écran Créer une instance de calcul (compute), entrez les
    détails ci-dessous.

    1.  Compute name – +++**pfcompute**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Sélect **Standard_E4ds_v4**

> Cliquez sur **Review + Create**.

**Remarque :** Notez ce nom de calcul (compute) pour une utilisation
ultérieure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.png)

4.  Cliquez sur **Create** dans l'écran suivant pour créer le calcul
    (compute).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.png)

**Remarque :** Le calcul (compute) prend environ 10 minutes pour
atteindre l'état En cours d'exécution.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image15.png)

**Important :** Une fois que le calcul (compute) est opérationnel, vous
pouvez passer aux tâches suivantes. Toutefois, si vous faites une pause
dans l'exécution du laboratoire, assurez-vous **stop** l'instance de
calcul (compute) et de la redémarrer lorsque vous démarrez après
l'interruption.

### Tâche 1.3 : Créer une ressource Azure OpenAI

1.  À partir du portail Azure +++https://portal.azure.com+++, recherchez
    et sélectionnez +++**AzureOpenAI**+++.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.png)

2.  Cliquez sur **+ Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

3.  Remplissez les détails ci-dessous et cliquez sur **Next**.

- Resource Group : sélectionnez le Resource Group qui vous est attribué

- Région : sélectionnez une région (North Central US est utilisé ici)

- Nom - +++**AOAI-PF@lab.LabInstanceId**+++

- Niveau tarifaire - **Standard**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.png)

4.  Acceptez les valeurs par défaut dans les pages suivantes et cliquez
    sur **Create** dans la page **Review + submit**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image19.png)

5.  Cliquez sur **Go to resource** une fois le déploiement terminé.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.png)

6.  Sélectionnez **Keys and Endpoint** dans le volet gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

7.  Copiez la **Key** et **Endpoint**, puis enregistrez-les dans un
    bloc-notes pour les utiliser ultérieurement dans le Lab.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

8.  Dans Azure **Machine Learning Studio**, sélectionnez **Model
    catalog** dans le volet gauche, puis **gpt-4o**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image23.png)

9.  Cliquez sur **Deploy** pour déployer le modèle.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.png)

10. Acceptez le nom du déploiement et sélectionnez **Deploy**. Gardez
    une note de ce nom pour une utilisation future.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image25.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image26.png)

## Tâche 2 : Configurer une connexion de flux d'invite

1.  Dans le volet de navigation de gauche d'Azure Machine Learning
    Studio, sélectionnez **Prompt flow**. Sélectionnez **Connexions**
    dans la barre de menus. Sélectionnez la liste déroulante en regard
    de **Create**, puis sélectionnez **Azure OpenAI.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

2.  Dans l'Assistant Ajouter une connexion Azure OpenAI, fournissez les
    détails ci-dessous et sélectionnez **Save**.

- Name – +++**AoaiML_pf**+++

- Provider – Sélectionnez **Azure OpenAI**

- Subscription ID – Sélectionnez l**'abonnement (subscription) qui vous
  a été attribué**

- Azure OpenAI Account Name – Sélectionnez **AOAI-PF@lab.LabInstanceId**

- Auth Mode - Sélectionnez **API Key**

- API Key : indiquez la **key** que nous avons enregistrée dans **Azure
  OpenAI resource**

- API base – Fournir le **endpoint** que nous avons enregistré à partir
  de la **ressource Azure OpenAI**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image28.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image29.png)

3.  Vérifiez que la création de la connexion a réussi.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image30.png)

## Tâche 3 : Créer et développer votre flux d'invites

1.  Dans l'onglet **Flow** de la page d'accueil de **Prompt flow**,
    sélectionnez **Create** pour créer le flux d'invite. La page
    **Create a new flow** affiche les types de flux que vous pouvez
    créer, les exemples intégrés que vous pouvez cloner pour créer un
    flux et les méthodes d'importation d'un flux.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image31.png)

2.  Sélectionnez **Clone** dans la catégorie **WebClassification**.

Dans **Explore gallery**, vous pouvez parcourir les exemples intégrés et
sélectionner **View détail** sur n'importe quelle vignette pour savoir
si elle convient à votre scénario.

Cet atelier utilise **Web classification** pour parcourir le parcours
principal de l'utilisateur.

La classification Web est un flux illustrant la classification
multiclasse avec un LLM. À partir d'une URL, le flux classe l'URL dans
une catégorie Web en quelques clichés, un résumé simple et des invites
de classification. Par exemple, étant donné une URL
https://www.imdb.com, il classe l'URL dans Film.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image32.png)

3.  Acceptez le nom renseigné pour **Folder name**, puis sélectionnez
    **Clone**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image33.png)

4.  Une session de calcul (compute) est nécessaire pour l'exécution du
    flux. La session de calcul (compute) gère les ressources de calcul
    (compute) nécessaires à l'exécution de l'application, y compris une
    image Docker qui contient tous les packages de dépendances
    nécessaires.

5.  Sur la page de création de flux, démarrez une session de calcul
    (compute) en sélectionnant Démarrer **Start compute session**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image34.png)

**Remarque :** Il faudra environ **10 minutes** pour que la session de
calcul (compute) passe à l'état En cours d'exécution.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image35.png)

## Tâche 4 : Inspecter la page de création de flux

Le démarrage de la session de calcul (compute) peut prendre quelques
minutes. Pendant le démarrage de la session de Calcul (Compute),
affichez les parties de la page de création de flux.

- La vue **Flow** ou *Aplatir* sur le côté gauche de la page est la zone
  de travail principale, où vous pouvez créer le flux en ajoutant ou en
  supprimant des nœuds, en modifiant et en exécutant des nœuds en ligne
  ou en modifiant des invites. Dans les **sections Inputs** et
  **Outputs**, vous pouvez afficher, ajouter ou supprimer et modifier
  les entrées et les sorties.

Lorsque vous avez cloné l'exemple de classification Web actuel, les
entrées et les sorties étaient déjà définies. Le schéma d'entrée du flux
est name : url ; type : string, une URL de type chaîne. Vous pouvez
remplacer la valeur d'entrée prédéfinie par une autre valeur comme
https://www.imdb.com manuellement.

- La section **Files** en haut à droite affiche la structure des
  dossiers et des fichiers du flux. Chaque dossier de flux contient un
  fichier *flow.dag.yaml, des fichiers de* code source et des dossiers
  système. Vous pouvez créer, charger ou télécharger des fichiers à des
  fins de test, de déploiement ou de collaboration.

- La vue **Graph** en bas à droite permet de visualiser à quoi ressemble
  le flux. Vous pouvez effectuer un zoom avant ou arrière, ou utiliser
  la mise en page automatique.

Vous pouvez modifier des fichiers en ligne dans la vue **Flow** ou
Aplatir, ou vous pouvez activer **Raw file mode** et sélectionner un
fichier dans **Files** pour l'ouvrir dans un onglet à des fins de
modification.

Vous pouvez modifier des fichiers en ligne dans la vue **Flow** ou
Aplatir, ou vous pouvez activer **Raw file mode** et sélectionner un
fichier dans **Files** pour l'ouvrir dans un onglet à des fins de
modification.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image36.png)

## Tâche 5 : Configurer les nœuds LLM

Pour chaque nœud LLM, vous devez sélectionner une **connexion** pour
définir les clés API LLM. Sélectionnez votre connexion Azure OpenAI.

Selon le type de connexion, vous devez sélectionner un
**deployment_name** ou un modèle dans la liste déroulante. Pour une
connexion Azure OpenAI, sélectionnez un déploiement. 

1.  Pour la summarize_text_content, remplissez les détails ci-dessous.

Connection – Sélectionnez **AoaiML_pf**

Api – Sélectionnez **chat**

deployment name – Sélectionnez **gpt-4o-2024-11-20**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image37.png)

2.  Établissez une connexion similaire pour les nœuds LLM
    **classify_with_llm**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image38.png)

3.  Pour tester et déboguer un seul nœud, sélectionnez l’icône **Run**
    en haut d'un nœud dans la vue **Flow**. Vous pouvez développer
    **Inputs** et modifier l'URL d'entrée de flux pour tester le
    comportement du nœud pour différentes URL.

4.  L'état de l'exécution s'affiche en haut du nœud. Une fois
    l'exécution terminée, la sortie de l'exécution apparaît dans la
    section **Output** du nœud .

5.  Passez au début du flux et exécutez l’URL
    **fetch_text_content_from** et exécutez le bloc.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

La vue **Graph** indique également l'état du nœud d'exécution unique.

6.  Dans la section **Inputs,** indiquez la valeur du champ **Valeur**
    sous la forme
    **+++https://play.google.com/store/apps/details?id=com.spotify.music+++**

Sélectionnez **Run** en haut à droite pour tester et déboguer l'ensemble
du flux.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

## Tâche 5 : Afficher les sorties de flux

Vous pouvez également définir des sorties de flux pour vérifier les
sorties de plusieurs nœuds au même endroit. Les sorties de flux vous
aident à :

- Vérifiez les résultats des tests en bloc dans un seul tableau.

- Définissez le mappage de l'interface d'évaluation.

- Définissez le schéma de réponse de déploiement.

1.  Sélectionnez **View outputs** dans le bandeau supérieur ou la barre
    de menus supérieure pour afficher des informations détaillées sur
    l'entrée, la sortie, l'exécution du flux et l'orchestration.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image41.png)

2.  Dans l'onglet Sorties de l'écran Sorties, notez que le flux prédit
    l'URL d'entrée avec une **category** et **une évidence**. ![Une
    capture d'écran d'un ordinateur Description générée
    automatiquement](./media/image42.png)

3.  Sélectionnez l'onglet **Trace** dans l'écran **Outputs**, puis
    sélectionnez **flow** sous **node name** pour afficher des
    informations détaillées sur la vue d'ensemble du flux dans le volet
    droit. Développez **flow** et sélectionnez n'importe quelle étape
    pour afficher des informations détaillées sur cette étape.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image43.png)

**Résumé :**

Dans cet atelier, nous avons appris à classer l'URL dans une catégorie
web à l'aide d'un résumé simple et à utiliser des invites de
classification à l'aide d'un flux d'invites dans Azure Machine Learning
Studio.
