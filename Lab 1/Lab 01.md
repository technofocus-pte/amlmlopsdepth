# Atelier 01 : Préparer un jeu de données, entraîner et déployer un modèle de classification à l'aide d'Azure Machine Learning Studio

**Objectif**

Cet atelier vise à vous guider tout au long du processus de
configuration d'un environnement Azure Machine Learning, de chargement,
d'accès et d'exploration des données, de formation et de déploiement
d'un modèle de classification d'images à l'aide d'Azure Machine Learning
Studio.

Durée prévue - 45 minutes

## Exercice 1 : Configuration de l'espace de travail (workspace) Azure Machine Learning

### Tâche 1 : Synchroniser l'horloge de la machine virtuelle

1.  Après vous être connecté à la machine virtuelle, faites un clic
    droit sur l'horloge dans le coin inférieur droit de l'écran.

2.  Sélectionnez **Adjust date and time.**

&nbsp;

3.  Sur l'écran Paramètres qui s'ouvre, cliquez sur **Sync now** sous
    Paramètres supplémentaires (Additional settings).

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image1.png)

4.  Cela permet de synchroniser l'heure au cas où la synchronisation
    automatique ne fonctionnerait pas.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance moyen](./media/image2.png)

### Tâche 2 : Préparation des ressources Azure

Cette tâche se concentre sur la création d'un espace de travail
(workspace) Azure Machine Learning. Vous découvrirez comment mettre en
place un espace de travail (workspace) dédié pour organiser et gérer
efficacement leurs projets de machine learning. Cet espace de travail
(workspace) sert de plaque tournante centrale pour la collaboration,
l'expérimentation et le déploiement.

#### Tâche 2.1 : Inscrire les fournisseurs de ressources (Resource Providers) requis 

1.  Accédez à **subscription** qui vous a été attribué à partir de la
    page d'accueil du portail Azure.

2.  Sélectionnez Resource Providers sous **Settings** dans le volet
    gauche.

3.  Recherchez +++Microsoft.StreamAnalytics+++ et sélectionnez les trois
    points en regard du nom, puis cliquez sur **Register**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.png)

4.  Répétez les étapes pour inscrire +++Microsoft.Cdn+++ et
    +++Microsoft.PolicyInsights+++

#### Tâche 2.2 : Créer un espace de travail (workspace) Azure Machine Learning

1.  Connectez-vous au portail Azure à l'adresse
    +++https://portal.azure.com+++ à l'aide du **Username** et du
    **Password** de l'onglet **Resources**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image4.png)

2.  Dans la page d'accueil du portail Azure, sélectionnez **+ Create a
    resource**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image5.png)

3.  Dans la page **Create a resource**, utilisez la barre de recherche
    pour trouver +++**Azure** **Machine Learning+++** et sélectionnez
    **Azure Machine Learning**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image6.png)

4.  Sous **Marketplace**, cliquez sur **Create dropdown** et
    sélectionnez **Azure Machine Learning**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image7.png)

5.  Fournissez les informations suivantes pour configurer votre nouvel
    espace de travail (workspace) et cliquez sur **Review + create**.

    - **Subscription** : sélectionnez **abonnement Azure qui vous a été
      attribué**

    - **Resource group:** sélectionnez **Resource Group assigned**.

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region** : sélectionnez la région la plus proche **(North Central
  US** est sélectionné ici)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstance.Id+++**

**Remarque :** Le numéro qui est ajouté aux noms des ressources est
votre ID Labinstance pour garantir l'unicité. Les captures d'écran
auront un numéro différent car elles sont uniques.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

6.  Une fois la Validation effectuée, cliquez sur **Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

7.  Cliquez sur **Go to resource**, pour afficher le nouvel espace de
    travail (workspace).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.png)

8.  **Sur la page Microsoft.MachineLEarningServices | Overview**,
    sélectionnez **Launch studio** sous **Work with your model in Azure
    Machine Learning studio**.

![Une capture d'écran d'une mise à jour logicielle Description générée
automatiquement](./media/image12.png)

#### Tâche 2.3 : Créer un calcul (compute)

Cette tâche illustre la création d'une ressource de calcul (compute)
dans Azure. Vous explorerez différentes options de calcul (compute),
telles que les machines virtuelles ou les clusters de calcul (compute)
gérés, et comprendrez comment configurer et provisionner des ressources
pour exécuter efficacement des charges de travail de machine learning.

1.  Une fois qu’**Azure Machine Learning Studio** s'ouvre, cliquez sur
    **Compute** sous **Manage** dans le volet gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.png)

2.  Cliquez sur **+ New** sur l'écran **Compute instances**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.png)

3.  Sur l'écran Créer une instance de calcul (compute), entrez les
    détails ci-dessous.

    1.  Compute name – +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size : sélectionnez **Standard_E4ds_v4**

> Cliquez sur **Review + Create**.

**Remarque :** Notez ce nom de calcul (compute) pour une utilisation
ultérieure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image15.png)

4.  Cliquez sur **Create** dans l'écran suivant.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.png)

**Remarque :** Le calcul (compute) prend environ 10 minutes pour
atteindre l'état En cours d'exécution.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

**Important :** Une fois que le calcul (compute) est opérationnel, vous
pouvez passer aux tâches suivantes. Toutefois, si vous faites une pause
dans l'exécution du laboratoire, assurez-vous de **Stop** l'instance de
calcul (compute) et de la redémarrer lorsque vous démarrez après
l'interruption.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image18.png)

**Résumé de l'exercice :**

L'exercice familiarise les participants avec les étapes essentielles de
la configuration d'un environnement Azure Machine Learning. Grâce à
cette série de tâches, les participants ont appris à créer un compte de
stockage, à installer le SDK Machine Learning, à se connecter à l'aide
d'Azure CLI, à créer un espace de travail (workspace) Azure Machine
Learning et à configurer une ressource de calcul (compute). En réalisant
cet exercice, vous avez acquis les connaissances fondamentales et les
compétences pratiques nécessaires pour établir un environnement Azure
Machine Learning fonctionnel, ce qui vous permet d'entreprendre vos
projets de Machine Learning en toute confiance.

## Exercice 2 – Charger, accéder et explorer vos données dans Azure Machine Learning

**Objectif**

Dans cet exercice, vous allez apprendre à :

- Téléchargez vos données sur un stockage cloud

- Créer une ressource de données (dataset) Azure Machine Learning

- Accédez à vos données dans un Notebook pour un développement
  interactif

- Créer de nouvelles versions de ressources de données

Le démarrage d'un projet de machine learning implique généralement
l'analyse exploratoire des données (exploratory data analysis (EDA)), le
prétraitement des données (nettoyage, ingénierie des fonctionnalités) et
la création de prototypes de modèles de Machine Learning pour valider
des hypothèses. Cette phase de projet de prototypage est très
interactive. Il se prête au développement dans un IDE ou un notebook
Jupyter, avec une console interactive *Python*. Cet atelier décrit ces
idées.

Nous sommes à l’étape **Data : Explore & prepare** du flux de travail du
**Machine Learning project workflow.**

![](./media/image19.png)

### Tâche 1 : Préparation des ressources Azure

**Important :** Assurez-vous que le calcul (compute) que nous avons créé
lors du dernier exercice est opérationnel. Si vous faites une pause dans
l'exécution du Lab, assurez-vous de l'arrêter (**stop**) et de la
recommencer lorsque vous commencez après la pause.

#### Tâche 1.1 : Télécharger le Notebook 

1.  À partir d'Azure Machine Learning Studio, une fois le calcul
    (compute) opérationnel, sélectionnez l'option **Notebooks** dans le
    volet gauche. ![](./media/image20.png)

2.  Fermez la boîte de dialogue **What’s new in Notebooks**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image21.png)

3.  Le volet Fichiers de Notebook s'ouvre avec la structure **Users -\>
    \< UserName \>**. Cliquez sur les trois points à côté du nom
    d'utilisateur, puis sélectionnez **Create new folder**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

4.  Entrez le nom du dossier sous la forme +++**Azuremlnotebooks**+++ et
    cliquez sur **Create.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image23.png)

5.  Une fois le dossier créé, cliquez sur le **menu options** (les trois
    points à côté du nom du dossier) du dossier **Azuremlnotebooks** et
    cliquez sur **Upload files**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.png)

6.  Sélectionnez **Click to browse and select file(s).** Accédez à
    **explore-data.ipynb** sous **C :\Labfiles** et cliquez sur
    **Open**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image25.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image26.png)

7.  Cochez la case Ouvrir **Open file after upload** et **I trust the
    contents of this file.** Cliquez ensuite sur **Upload.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

8.  Cela ouvre le Notebook téléchargé.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image28.png)

9.  Cliquez sur **Authenticate** si le studio vous demande de vous
    authentifier, car c'est la première fois que vous vous connectez au
    studio.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image29.png)

### Tâche 2 : Télécharger, accéder et explorer vos données 

#### Tâche 2.1 : Télécharger les données

1.  Dans le volet **Files** de **Notebooks**, cliquez sur les 3 points à
    côté du nom du dossier **Azuremlnotebooks** et cliquez sur **Create
    new folder.**

![](./media/image30.png)

2.  Tapez le nom du dossier sous la forme +++**data**+++ et cliquez sur
    **Create**.

![](./media/image31.png)

3.  Une fois la création du dossier réussie, cliquez sur les options de
    menu **data** du dossier et sélectionnez **Upload files**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image32.png)

4.  Sélectionnez **Click to browse and select file(s),** puis accédez à
    **C :\Labfiles** pour sélectionner le fichier
    **default_of_credit_card_clients.csv** et cliquez sur **Open**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image33.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image34.png)

5.  Un message indiquant **File uploaded successfully** s'affiche sous
    les notifications une fois le téléchargement terminé.

![Gros plan d'un écran d'ordinateur Description générée automatiquement
avec un niveau de confiance faible](./media/image35.png)

#### Tâche 2.2 : Création d'un handle dans l'espace de travail (workspace)

1.  Revenez au Notebook (**explore-data**).

2.  Avant de plonger dans le code, vous avez besoin d'un moyen de
    référencer votre espace de travail (workspace). Vous allez créer
    ml_client pour un handle de l'espace de travail (workspace). Vous
    utiliserez ensuite ml_client pour gérer les ressources et les
    tâches.

3.  Dans la première cellule sous **Create handle to workspace**,
    remplacez les espaces réservés \< SUBSCRIPTION_ID \>, **\<
    RESOURCE_GROUP \>** et le **\< AML_WORKSPACE_NAME \>.**

4.  Remplacez \< RESOURCE_GROUP\> par le nom du groupe de ressources qui
    vous a été attribué.

5.  Remplacez \<AML_WORKSPACE_NAME\> par
    **<+++Azuremlws@lab.LabInstance.Id>+++**

6.  Remplacez \< SUBSCRIPTION_ID \> par le
    +++**@lab.CloudSubscription.Id+++**.

7.  Cliquez sur le bouton Exécuter la **Run cell** disponible en haut à
    gauche de la cellule. Une fois l'exécution réussie, recherchez une
    coche au bas de la cellule.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image36.png)

#### Tâche 2.3 : Télécharger des données sur le stockage cloud

1.  Une ressource de données Azure Machine Learning est similaire aux
    signets de navigateur web (favoris). Au lieu de mémoriser les
    chemins de stockage longs (URI) qui pointent vers vos données les
    plus fréquemment utilisées, vous pouvez créer une ressource de
    données, puis accéder à cette ressource avec un nom convivial.

2.  La cellule de Notebook suivante crée la ressource de données.
    L'exemple de code télécharge le fichier de données brutes dans la
    ressource de stockage cloud désignée.

3.  Chaque fois que vous créez une ressource de données, vous avez
    besoin d'une version unique de celle-ci. Si la version existe déjà,
    vous obtiendrez une erreur. Dans ce code, nous utilisons time pour
    générer une version unique à chaque exécution de la cellule.

4.  Exécutez la cellule suivante en cliquant sur le bouton Exécuter en
    haut à gauche de la cellule.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image37.png)

5.  **“Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx”** est la sortie qui s'affiche sous la cellule.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image38.png)

6.  Cliquez sur **Data** dans le volet de gauche et cliquez sur l'actif
    de données **credit-card** qui a été créé par l'exécution que nous
    avons effectuée à l'étape ci-dessus. Explorez les détails et revenez
    au volet **Notebooks**.

![](./media/image39.png)

#### Tâche 2.4 : Accéder à vos données dans un notebook

1.  De retour dans le notebook, exécutez la cellule avec la commande
    **%pip** pour installer la bibliothèque Python **azureml-fsspec**
    dans votre **Jupyter** kernel.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image40.png)

2.  Exécutez la cellule suivante pour accéder au fichier CSV dans
    **Pandas**.

3.  **Data asset URI** est imprimé au bas de la cellule et les données
    sont également affichées.

![Une capture d'écran d'un code informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image41.png)

#### Tâche 2.5 : Créer une nouvelle version de l'actif de données

1.  Vous avez peut-être remarqué que les données ont besoin d'un léger
    nettoyage, pour les adapter à l'entraînement d'un modèle
    d'apprentissage automatique. Il dispose de :

    1.  deux en-têtes

    2.  une colonne d'ID client ; nous n'utiliserions pas cette
        fonctionnalité dans l'apprentissage automatique

    3.  espaces dans le nom de la variable de réponse

2.  De plus, par rapport au format CSV, le format de fichier **Parquet**
    devient un meilleur moyen de stocker ces données. Parquet offre une
    compression et maintient le schéma. Par conséquent, pour nettoyer
    les données et les stocker dans Parquet, exécutez la cellule
    suivante.

3.  Assurez-vous que l'exécution est réussie par la coche au bas de la
    cellule.

![](./media/image42.png)

4.  Ce tableau présente la structure des données dans le fichier
    **default_of_credit_card_clients.csv** d'origine . CSV téléchargé
    lors d'une étape précédente. Les données téléchargées contiennent 23
    variables explicatives et 1 variable de réponse, comme indiqué ici :

[TABLE]

5.  Exécutez la cellule suivante pour créer une nouvelle *version* de la
    ressource de données (les données sont automatiquement téléchargées
    sur le stockage cloud).

6.  En cas d'exécution réussie, une sortie indiquant Resource **Data
    asset created. Name: credit_card, version:
    YYYY.MM.DD.xxxxxx_cleaned** apparaît après la cellule.

![Une capture d'écran d'un code informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image43.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image44.png)

**Important:**

Cette cellule de code Python définit les valeurs **name** et **version**
de la ressource de données qu'elle crée. Par conséquent, le code de
cette cellule échouera s'il est exécuté plusieurs fois, sans
modification de ces valeurs. Les valeurs de **name** et de **version**
fixes permettent de transmettre des valeurs qui fonctionnent pour des
situations spécifiques, sans se soucier des valeurs générées
automatiquement ou de manière aléatoire.

7.  Le fichier parquet nettoyé est la source de données de la dernière
    version. Le code de la cellule suivante affiche d'abord le jeu de
    résultats de la version CSV, puis la version Parquet lors de
    l'exécution.

8.  Exécutez la cellule suivante et vérifiez le résultat ci-dessous.

![Une capture d'écran d'un code informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image45.png)

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image46.png)

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image47.png)

![Une image contenant du texte, une capture d'écran, un numéro, un
affichage Description générée automatiquement](./media/image48.png)

9.  Recherchez les données nettoyées sous les **Data**.

> ![](./media/image49.png)

**Important :** Vous pouvez passer à l'exercice suivant à partir d'ici.
Toutefois, si vous faites une pause dans l'exécution du laboratoire,
assurez-vous d’arrêter **(stop)** l'instance de calcul (compute) et de
la redémarrer lorsque vous reprenez l'interruption.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image50.png)

**Résumé de l'exercice**

Dans cet exercice, vous allez apprendre à charger vos données sur le
stockage cloud, à créer une ressource de données Azure Machine Learning,
à accéder à vos données dans un notebook pour le développement
interactif et à créer de nouvelles versions de ressources de données.

## Exercice 3 – Entraîner et déployer un modèle de classification d'images sur Azure Machine Learning Studio

**Objectif**

Dans cet exercice, vous allez apprendre à

1.  Connectez-vous à l'espace de travail (workspace) et configurez une
    ressource de calcul (compute) à l'aide de l'interface utilisateur du
    Notebook Azure Machine Learning Studio

2.  Apportez des données et préparez-les à être utilisées pour la
    formation

3.  Entraîner un modèle pour la classification d'images

4.  Afficher et analyser les métriques pour optimiser votre modèle

5.  Déployez le modèle en ligne et testez-le

Nous sommes à l'étape **Train & validate model** de **Machine Learning
project workflow.**

### ![Une image contenant du texte, une police, un numéro, une capture d'écran Description générée automatiquement](./media/image51.png)Tâche 1: Télécharger le Notebook

1.  Dans la page **Notebooks** Azure Machine Learning Studio, cliquez
    sur les options de menu du dossier **AzureMLnotebooks,** puis sur
    **Upload files**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image52.png)

2.  Sélectionnez, **Click to browse and select file(s),** accédez à
    **C :\Labfiles** et sélectionnez le fichier
    **azureml-getting-started-studio** (un fichier source Jupyter).

> ![Une capture d'écran d'un écran d'ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image53.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image54.png)

3.  Cochez la case Ouvrir **Open file after upload**, puis cliquez sur
    **Upload**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image55.png)

4.  Une fois le téléchargement du fichier réussi, il s'ouvre dans le
    studio, se connecte automatiquement au Compute(cpu-cluster-fs) qui
    est à l'état En cours d'exécution.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image56.png)

### Tâche 2 : Se connecter à l'espace de travail (workspace) Azure Machine Learning

Avant de plonger dans le code, vous devez vous connecter à votre espace
de travail (workspace). L'espace de travail (workspace) est la ressource
de niveau supérieur pour Azure Machine Learning, fournissant un
emplacement centralisé pour travailler avec tous les artefacts que vous
créez lorsque vous utilisez Azure Machine Learning.

Nous utilisons **DefaultAzureCredential** pour accéder à l'espace de
travail (workspace). **DefaultAzureCredential** doit être capable de
gérer la plupart des scénarios.

*\# Handle to the workspace*

**from** azure.ai.ml **import** MLClient

*\# Authentication package*

**à partir d'** azure.identity **import** DefaultAzureCredential

credential **=** DefaultAzureCredential()

*\# Get a handle to the workspace. You can find the info on the
workspace tab on ml.azure.com*

ml_client **=** MLClient(

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  Dans le code ci-dessus (Première cellule du carnet), remplacez
    **SUBSCRIPTION_ID, RESOURCE_GROUP name** et les espaces réservés
    **AML_WORKSPACE_NAME** par les valeurs que nous avons enregistrées
    dans l'exercice précédent.

2.  Votre première cellule dans le carnet de notes devrait maintenant
    ressembler à ceci. Cliquez sur le bouton **Run** en haut à gauche de
    la première cellule.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image57.png)

3.  Assurez-vous que la cellule s'est exécutée correctement en voyant
    son état au bas de la cellule.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance moyen](./media/image58.png)

En \[ \] :

### Tâche 3 : Télécharger les données

Pour exécuter un travail d'entraînement Azure Machine Learning, vous
avez besoin d'un environnement.

Dans cet atelier, vous allez utiliser un environnement prêt à l'emploi
appelé AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest qui contient
toutes les bibliothèques requises (python, MLflow, numpy, pip, etc.).

1.  Exécutez le code dans la cellule suivante pour télécharger les
    données.

2.  Assurez-vous qu'un message indiquant **Data asset created**
    s'affiche en tant que sortie de la cellule.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance moyen](./media/image59.png)

###  Tâche 4 : Générer la tâche de commande à entraîner

Maintenant que vous disposez de toutes les ressources requises pour
exécuter votre travail, il est temps de créer la tâche (job) lui-même, à
l'aide du Kit de développement logiciel (SDK) Python Azure ML v2. Nous
allons créer une tâche de commande.

Un travail de commande AzureML est une ressource qui spécifie tous les
détails nécessaires à l'exécution de votre code d'entraînement dans le
cloud : entrées et sorties, type de matériel à utiliser, logiciel à
installer et comment exécuter votre code. Le Job de commande contient
des informations permettant d'exécuter une seule commande.

#### Tâche 4.1 : Créer un script de formation

1.  Commençons par créer le script d'entraînement -
    the **main.py** python file.

2.  Exécutez la cellule suivante et assurez-vous qu'elle est exécutée
    correctement.

![Une image contenant du texte, une police, une ligne, une capture
d'écran Description générée automatiquement](./media/image60.png)

3.  Le script de la cellule suivante gère le prétraitement des données,
    en les divisant en données de test et d'entraînement. Il consomme
    ensuite ces données pour entraîner un modèle basé sur une
    arborescence et retourner le modèle de
    sortie. [MLFlow](https://mlflow.org/docs/latest/tracking.html) sera
    utilisé pour enregistrer les paramètres et les métriques pendant
    l'exécution de notre pipeline.

4.  Exécuter la cellule et s'assurer qu'elle est exécutée avec succès
    avec la sortie,

**Writing ./src/main.py**

> ![Une capture d'écran d'un programme informatique Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image61.png)
>
> ![Une capture d'écran d'un programme informatique Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image62.png)

5.  Comme vous pouvez le voir dans ce script, une fois le modèle formé,
    le fichier de modèle est enregistré et enregistré dans l'espace de
    travail (workspace). Vous pouvez maintenant utiliser le modèle
    inscrit dans l'inférence de points de terminaison.

#### Tâche 4.2 : Configurer la commande

Maintenant que vous disposez d'un script capable d'effectuer les tâches
souhaitées, vous allez utiliser la commande à usage général qui peut
exécuter des actions de ligne de commande. Cette action de ligne de
commande peut consister à appeler directement des commandes système ou à
exécuter un script.

1.  Ici, vous allez utiliser les données d'entrée, le taux de
    fractionnement, le taux d'apprentissage et le nom du modèle
    enregistré comme variables d'entrée.

2.  Dans le volet gauche, sélectionnez **Data,** puis
    **credit-card-data** .

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image63.png)

3.  Dans la section **Data sources**, recherchez la valeur **Datastore
    URI** et copiez-la. Enregistrez-le pour l'utiliser à l'étape
    suivante.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image64.png)

4.  Dans la cellule suivante, remplacez l'icône

    1.  Valeur de **path** avec **Datastore URI** enregistrée à l'étape
        précédente.

    2.  Valeur de **compute** avec
        +++**cpu-cluster-fs@lab.LabInstance.Id**+++ (le nom du cluster
        que nous avons enregistré dans l'atelier 1)

5.  Cliquez sur **Run**. Assurez-vous que la cellule est exécutée
    correctement.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image65.png)

### Tâche 6 : Soumettre la tâche (job)

Il est maintenant temps de soumettre la tâche (job) pour qu'il s'exécute
dans AzureML. **L'exécution du travail prendra 2 à 3 minutes**. Cela
peut prendre plus de temps (jusqu'à 10 minutes) si l'instance de calcul
(compute) a été réduite à zéro nœud et que l'environnement personnalisé
est toujours en cours de création.

1.  Exécutez la cellule avec la commande ci-dessous pour soumettre la
    tâche.

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  Cliquez sur **Run**. Assurez-vous que l'exécution a réussi et qu'il
    existe un lien vers le résultat dans la colonne **Details Page**.

**Remarque** : Cela prendra environ 2 minutes à compléter.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image66.png)

3.  Ouvrez le lien disponible sous la colonne **Details Page** du
    résultat, dans un nouvel onglet.

### Tâche 7 : Afficher le résultat d'une tâche d'entraînement

1.  Vous pouvez afficher le résultat d'une tâche d'entraînement en
    **cliquant sur l'URL générée après la soumission de la tâche
    (Job)**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image67.png)

2.  Vous pouvez également cliquer sur **Jobs** dans le menu de
    navigation de gauche. Une tâche est un regroupement de plusieurs
    exécutions à partir d'un script ou d'un morceau de code spécifié.
    Les informations relatives à l'exécution sont stockées sous cette
    tâche (job).

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image68.png)

3.  La page **Overview** indique d'abord que **Status** sous le volet
    **Properties** est en cours d'exécution **(Running)**.

4.  Le statut passe à **Completed** une fois qu'il est prêt.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image69.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image70.png)

5.  Sélectionnez le volet **Metrics** pour afficher les mesures.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image71.png)

6.  Sélectionnez l'onglet **Images** pour afficher la matrice
    training_confusion, la courbe de rappel de précision et la courbe
    roc.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image72.png)

1)  **Overview** vous permet de voir l'état de la tâche.

2)  **Metrics** afficheraient différentes visualisations des métriques
    que vous avez spécifiées dans le script.

3)  **Images** est l'endroit où vous pouvez afficher tous les artefacts
    d'image que vous avez enregistrés avec MLflow.

4)  **Child jobs** contiennent les travaux enfants si vous les avez
    ajoutés.

5)  **Outputs + logs** contient les fichiers journaux dont vous avez
    besoin pour le dépannage ou à d'autres fins de surveillance.

6)  **Le code** contient le script/le code utilisé dans la tâche (job).

7)  **Explanations** et **Fairness** sont utilisées pour voir comment
    votre modèle se comporte par rapport aux normes d'IA responsables.
    Il s'agit actuellement de fonctionnalités en préversion qui
    nécessitent l'installation de packages supplémentaires.

8)  **Monitoring** vous permet d'afficher les métriques de performances
    des ressources de calcul (compute).

### Tâche 8 : Déployer le modèle en tant que point de terminaison en ligne

Une fois que vous avez formé un modèle de machine learning, vous devez
le déployer afin que d'autres personnes puissent l'utiliser pour
l'inférence. À cette fin, Azure Machine Learning vous permet de créer
des **Endpoints** et d'y ajouter **deployements**.

Dans ce contexte, un **endpoints** est un chemin d'accès HTTPS qui
fournit une interface permettant aux clients d'envoyer des requêtes
(données d'entrée) à un modèle formé et de recevoir les résultats
d'inférence (notation) du modèle. Un point de terminaison fournit :

- Authentification à l'aide d'une authentification basée sur une « clé
  ou un jeton »

- Terminaison TLS(SSL)

- Un URI de scoring stable (endpoint-name.region.inference.ml.azure.com)

Un **deployment** est un ensemble de ressources requises pour héberger
le modèle qui effectue l'inférence réelle.

#### Tâche 8.1 : Créer un point de terminaison en ligne

1.  Déployez maintenant votre modèle de machine learning en tant que
    service web dans le cloud Azure, un point de terminaison en ligne.

2.  Sélectionnez **Endpoints** dans le volet gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image73.png)

3.  Sélectionnez **Create** pour les points de terminaison (Endpoints)
    en temps réel

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image74.png)

4.  Sélectionnez **credit_defaults_model** puis cliquez sur **Select.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image75.png)

5.  Sélectionnez **Standard_E4s_v3** sous la machine virtuelle. Indiquez
    que le nombre d'instances est de **1**

> Acceptez les autres valeurs par défaut d'un **Endpoint name** unique
> et du **Deployment name**, puis sélectionnez **Deploy**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image76.png)

**Remarque :** La création du point de terminaison (endpoint) prend
environ 20 minutes.

6.  Une fois l'opération terminée, l'état Provisionnement passe à
    **Succeeded.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image77.png)

#### Tâche 8.2 : Tester avec un exemple de requête

1.  Sur la page du point de terminaison, sélectionnez l'onglet **Test**.

2.  Copiez et collez l'exemple de fichier de demande suivant dans le
    champ **Input data to test real-time endpoint**, en remplaçant le
    code déjà présent là-bas.

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

3.  Sélectionnez **Test** et affichez le résultat sous **Test result**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image78.png)

### Tâche 9 : Supprimer le point de terminaison

1.  Dans le volet gauche, sélectionnez **Endpoints**. Sélectionnez le
    point de terminaison que nous avons créé, puis cliquez sur
    **Delete**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image79.png)

2.  Cliquez sur **Delete** dans la boîte de dialogue de confirmation.

![Une capture d'écran d'une erreur informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image80.png)

3.  Recherchez une notification sur la suppression réussie.

![Une image contenant du texte, une capture d'écran, une police, une
ligne Description générée automatiquement](./media/image81.png)

**Résumé**

Dans cet atelier, vous avez appris à entraîner un modèle de
classification d'images sur Azure Machine Learning Studio et à le
déployer en tant que service web.
