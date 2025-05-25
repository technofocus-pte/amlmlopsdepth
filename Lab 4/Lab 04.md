# **Atelier 04 – Entraînement d'un modèle de classification avec AutoML no-code dans Azure Machine Learning Studio**

**Objectif**

Dans cet atelier, nous allons apprendre à entraîner un modèle de
classification avec AutoML no-code à l'aide du ML automatisé Azure
Machine Learning dans le studio Azure Machine Learning. Ce modèle de
classification prédit si un client souscrira à un dépôt à terme fixe
auprès d'une institution financière. L'apprentissage automatique
effectue rapidement une itération sur de nombreuses combinaisons
d'algorithmes et d'hyperparamètres pour vous aider à trouver le meilleur
modèle en fonction d'une mesure de réussite de votre choix.

Durée prévue – 60 minutes

Nous sommes à la phase de **déploiement du modèle (Deploy Model)**
d'Azure Machine Learning.

![](./media/image1.png)

## **Exercice 1 : Créer un espace de travail (workspace) Azure Machine Learning**

1.  Connectez-vous au portail Azure – +++**https://portal.azure.com**+++
    à l'aide des informations d'identification de l’onglet **+ Create a
    resource**.

2.  Dans la page d'accueil du portail Azure, sélectionnez **+ Create a
    resource**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image2.png)

3.  Dans **Create a resource**, utilisez la barre de recherche pour
    trouver +++**Azure** **Machine Learning+++.** Sélectionnez **Azure
    Machine Learning** sous **Marketplace**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image3.png)

4.  Sous **Marketplace**, cliquez sur le menu déroulant Create et
    sélectionnez **Azure Machine Learning.**

> ![Une capture d'écran d'un logiciel Description générée
> automatiquement](./media/image4.png)

5.  Fournissez les informations suivantes pour configurer votre nouvel
    espace de travail (workspace) :

    - **Subscription**: sélectionnez l**'abonnement Azure qui vous a été
      attribué**

    - **Resource group**: sélectionnez le Resource Group attribué

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image5.png)

**Workspace Details:**

- **Workspace name: +++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **Region** : Sélectionnez la région **North Central US** est utilisé
  ici

- **Container registry:** sélectionnez **Create new.** Entrez
  **+++Azuremlcr@lab.LabInstanceId**+++

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image7.png)

6.  Une fois que vous avez terminé de configurer l'espace de travail
    (workspace), sélectionnez **Review + Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.png)

7.  Une fois la Validation effectuée, cliquez sur **Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

8.  Cliquez sur **Go to resource**, pour afficher le nouvel espace de
    travail (workspace).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

9.  **Sur la page Microsoft.MachineLEarningServices | Overview**,
    sélectionnez **Launch studio** sous **Work with your model in Azure
    Machine Learning studio**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.png)

## **Exercice 2 : Création d'une tâche de ML automatisé**

1.  Accédez à l'onglet Azure Machine Learning Studio.

2.  Dans le volet gauche, sélectionnez **Automated ML job** dans la
    section **Authoring**.

3.  Cliquez sur **+ New Automated ML job**.

![](./media/image12.png)

### **Tâche 1 : Créer une ressource de données**

1.  Sur la page **Basic settings**, indiquez le nom du nouveau test
    +++MarketingExperiment+++, acceptez les autres valeurs par défaut et
    cliquez sur **Next**.

![](./media/image13.png)

2.  Dans la page Type de tâche et données, sélectionnez
    **Classification** sous **Select task type** et sélectionnez **+
    Create** sous **Select data.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.png)

3.  Sur la page Create a resource de données, fournissez les détails
    ci-dessous.

- **Name** – +++marketingdata+++

- **Type** – **Tabular**

- Cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image15.png)

4.  Dans le volet **Data source**, sélectionnez **From local files** et
    cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.png)

5.  Dans **Destination storage type,** sélectionnez la banque de données
    par défaut qui a été configurée automatiquement lors de la création
    de votre espace de travail (workspace) : **workspaceblobstore**.
    Vous téléchargez votre fichier de données à cet emplacement pour le
    mettre à la disposition de votre espace de travail (workspace).
    Sélectionnez **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

6.  Dans **File or folder selection**, sélectionnez **Upload files or
    folder** \> **Upload files**. Choisissez le
    **bankmarketing_train.csv** dans **C :/Labfiles**. Sélectionnez
    **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.png)

7.  Une fois le téléchargement terminé, la zone **Data preview** est
    renseignée en fonction du type de fichier. Sous **Settings**,
    vérifiez les valeurs de vos données. Sélectionnez ensuite **Next**.

[TABLE]

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image19.png)

8.  Le formulaire **Schema** permet de configurer davantage vos données
    pour cette expérience. Pour cet exemple, sélectionnez la bascule de
    **day_of_week**, afin de ne pas l'inclure. Sélectionnez **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.png)

9.  Dans l'écran **Review**, vérifiez que les informations et
    sélectionnez **Create** pour terminer la création de votre data
    asset.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

10. De retour sur la page **Create a new Automated ML job**, un message
    de **success** pour la création de la ressource de données
    s'affiche. Sélectionnez l'actif de données **marketingdata** créé et
    cliquez sur **Next**.

> **Remarque :** Si **marketingdata** ne s'affichent pas, cliquez sur
> Actualiser (Refresh) pour les répertorier.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

### **Tâche 2 : Configurer la tâche**

1.  Dans la page **Task Settings**, sélectionnez **Y (String)** comme
    **Target column**, c'est-à-dire ce que vous souhaitez prédire. Cette
    colonne indique si le client a souscrit à un dépôt à terme ou non.

2.  Sélectionnez **View additional configuration settings** et
    renseignez les champs comme suit. Ces paramètres permettent de mieux
    contrôler le travail d'entraînement. Sinon, les valeurs par défaut
    sont appliquées en fonction de la sélection et des données du test.

- Primary metric – AUCWeighted

- Explain best model – Activer

- Use all supported models - Activer

- Blocked models – Aucun

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image23.png)

3.  Sélectionnez **Limits** et entrez +++**60**+++ pour le champ
    **Experiment timeout(minutes).**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image24.png)

![Une capture d'écran d'un test Le contenu généré par l'IA peut être
incorrect.](./media/image25.png)

4.  Sous **Validate and test**, indiquez les valeurs ci-dessous et
    cliquez sur **Next**.

- Type de validation - Sélectionner **k-fold cross-validation**

- Nombre de validations croisées – Sélectionnez **2**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image26.png)

5.  Sur la page Calcul, sélectionnez le type de calcul Sélectionner
    **Compute cluster** et cliquez sur **+ New**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

6.  Dans le volet **Create compute cluster**, sélectionnez les détails
    ci-dessous et cliquez sur **Next**.

- Location – **North Central US** (identique à l'emplacement de votre
  espace de travail (workspace) Azure Machine Learning)

- Virtual machine tier – **Dedicated**

- Virtual machine type - **CPU**

- Virtual machine size -Select **Standard_DS12_v2**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image28.png)

7.  Dans les paramètres avancés, fournissez les détails ci-dessous et
    sélectionnez **Creater**.

- Compute name - +++automl-compute+++

- Minimum number of nodes - 0

- Maximum number of nodes – 1

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image29.png)

8.  Sélectionnez **Next** une fois que le provisionnement du calcul
    (compute provisioning) a réussi.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image30.png)

9.  Dans la page **Review**, sélectionnez **Submit the training job**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image31.png)

10. L'écran **Overview** s'ouvre et le **Status** s’affiche en haut au
    début de la préparation de l'expérience. Ce statut est mis à jour au
    fur et à mesure de la progression de l'expérience. Des notifications
    apparaissent également dans le studio pour vous informer de l'état
    de votre expérience.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image32.png)

> **Remarque :** La formation dure environ 40 minutes.

## **Exercice 3 : Explorer les modèles**

Pendant que la formation est en cours, vous pouvez explorer les modèles
associés.

1.  Accédez à l'onglet **Models + child** pour voir les algorithmes
    (modèles) testés.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image33.png)

2.  Sélectionnez le modèlél **StandardScalerWrapper,
    XGBoostClassifier**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image34.png)

3.  Cliquez sur **Metrics** et explorez les détails sous l'onglet
    Métriques.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image35.png)

4.  Pendant que vous attendez la fin de tous les modèles d'expérience,
    sélectionnez le **name** de l'algorithme d'un modèle terminé pour
    explorer les détails de ses performances. Sélectionnez les onglets
    **Overview** et Metrics pour plus d'informations sur la tâche.

> **Important :** La formation du modèle dure environ 40 minutes.
> Veuillez passer au prochain Lab pendant qu'il est en cours. Revenez à
> cet atelier une fois que l'état passe à **Completed**.

## **Exercice 4 : Explications du modèle**

Les explications du modèle peuvent être générées à la demande. Le
tableau de bord des explications du modèle, qui fait partie de l’onglet
**Explications (préversion),** récapitule ces explications.

1.  Sous l'onglet Modèles + tâches enfants (à partir de la tâche
    parent), sélectionnez **MaxAbsScaler, LightGBM.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image36.png)

2.  Sélectionnez l'onglet **Explication model**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image37.png)

3.  Dans le volet Expliquer le modèle qui s'ouvre, sélectionnez

    1.  Sélectionner le type de calcul - **Compute cluster**

    2.  Sélectionner une instance de calcul AzureML - Sélectionner
        **automl-compute**

Sélectionnez **Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image38.png)

4.  Le message de réussite s'affiche. Sélectionnez l'onglet
    **Explications (preview).** Cet onglet se renseigne une fois
    l'exécution de l'explicabilité terminée.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image39.png)

5.  Développez le volet gauche. Sous **Features**, sélectionnez la ligne
    qui indique **brut**. Sélectionnez l'onglet **Aggregate feature
    importance**. Ce graphique montre quelles caractéristiques de
    données ont influencé les prédictions du modèle sélectionné.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

Dans cet exemple, la **duratrion** semble avoir le plus d'influence sur
les prédictions de ce modèle.

## **Exercice 5 : Déployer le meilleur modèle**

L'interface d'apprentissage automatique vous permet de déployer le
meilleur modèle en tant que service Web. *Le déploiement* est
l'intégration du modèle afin qu'il puisse prédire sur de nouvelles
données et identifier les zones d'opportunités potentielles. Pour cette
expérience, le déploiement sur un service web signifie que l'institution
financière dispose désormais d'une solution web itérative et évolutive
pour identifier les clients potentiels des dépôts à terme.

Une fois l'exécution de l'expérience terminée, la page **Details** est
renseignée avec une section **Résumé du meilleur modèle**. Dans ce
contexte d'expérimentation, **VotingEnsemble** est considéré comme le
meilleur modèle, basé sur la **metric AUCWeighted**.

1.  Sélectionnez **Jobs** dans le volet gauche et sélectionnez
    l'expérience que vous avez créée.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image41.png)

2.  Cliquez sur le nom d'affichage de l'expérience.

![](./media/image42.png)

3.  Vérifiez si le statut est **Completed**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image43.png)

4.  Une fois l'exécution de l'expérience terminée, la page **Details**
    est renseignée avec une section **Best model summary**. Dans ce
    contexte d'expérimentation, **VotingEnsemble** est considéré comme
    le meilleur modèle, sur la base de la **métric AUC_weighted**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image44.png)

Nous déployons ce modèle, mais attention, le déploiement prend environ
20 minutes. Le processus de déploiement comprend plusieurs étapes,
notamment l'enregistrement du modèle, la génération de ressources et
leur configuration pour le service Web.

5.  Sélectionnez **VotingEnsemble** pour ouvrir la page spécifique au
    modèle.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image45.png)

6.  Sélectionnez le menu **Deployer** en haut à gauche, puis
    sélectionnez **Deploy to web service**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image46.png)

7.  Remplissez le volet **Deploy un model** comme suit :

[TABLE]

> Cliquez sur **Deploy**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image47.png)

8.  Un message de réussite indiquant **Model deployment is successfully
    triggered** s'affiche sur l'écran Modèle et que l'état est
    **Running** d'exécution.

![](./media/image48.png)

9.  Une fois le déploiement terminé, l'état passe à **Completed**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image49.png)

> Vous disposez maintenant d'un service web opérationnel pour générer
> des prédictions.

## **Exercice 6 : Supprimer les ressources**

### **Tâche 1 : Supprimer le point de terminaison (Endpoint)**

1.  Dans le volet gauche d'AML Studio, cliquez sur **Endpoints**.

2.  Sélectionnez le point de terminaison, **my-automl-deploy** et
    cliquez sur **Delete**.

![](./media/image50.png)

3.  Sélectionnez **Delete** dans la boîte de dialogue Supprimer le point
    de terminaison en temps réel.

4.  Vous devriez recevoir un message de réussite une fois le point de
    terminaison supprimé.

**Résumé**

Dans cet atelier, nous avons appris à entraîner un modèle de
classification no-code AutoML dans le studio Azure Machine Learning et à
déployer le meilleur modèle en tant que service web.
