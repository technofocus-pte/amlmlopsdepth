# **Atelier 05 - Prévision de la demande avec l'apprentissage automatique sans code dans le studio Azure Machine Learning**

**Objectif**

Dans cet atelier, vous allez apprendre à créer un modèle de prévision de
séries chronologiques sans écrire une seule ligne de code à l'aide du
machine learning automatisé dans le studio Azure Machine Learning. Ce
modèle permettra de prédire la demande de location pour un service de
partage de vélos.

Vous n'allez pas écrire de code dans cet atelier ; Vous utiliserez
l'interface du studio pour effectuer des formations.

Durée prévue – 60 minutes

## **Exercice 1 : Préparer l'environnement**

### **Tâche 1 : Lancer l'espace de travail AML**

1.  Connectez-vous au portail Azure, +++**https://portal.azure.com**+++
    si vous n'êtes pas déjà connecté.

2.  Dans le menu du portail Azure, sélectionnez **All resources.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.png)

3.  Sélectionnez l'espace de travail Azure Machine Learning
    (**Azuemlws@lab.LabInstanceId**).

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image2.png)

4.  Cliquez sur **Launch studio**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.png)

## **Exercice 2 : Création d'une tâche de ML automatisé**

1.  Dans Azure Machine Learning Studio, cliquez sur **Automated ML**
    sous la section Auteur dans le volet gauche.

2.  Sélectionnez **+ New Automated ML job.**

![](./media/image4.png)

### **Tâche 1 : Créer une ressource de données**

1.  Donnez au test le nom +++ **experiment_forecast** +++, acceptez les
    autres valeurs par défaut et sélectionnez **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image5.png)

2.  Sélectionnez **Select task type** comme **Time series forecasting**,
    puis cliquez sur **+ Create.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

3.  Sur la page Créer une ressource de données, fournissez les détails
    suivants.

    1.  Name – +++**bikedata**+++

    2.  Type – Tabular

> Cliquez sur **Next**.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance moyen](./media/image7.png)

4.  Dans le volet **Data Source**, sélectionnez **From local files** et
    cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.png)

5.  Dans **Destination storage type**, sélectionnez workspaceblob et
    sélectionnez **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

6.  Dans la sélection Fichier ou dossier, sélectionnez **Upload files**
    et sélectionnez **bike-no.csv** dans le dossier **C :\Labfiles** et
    cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

7.  Vérifiez que le formulaire **Settings and preview** est renseigné
    comme suit et sélectionnez **Next**.

[TABLE]

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.png)

8.  Le formulaire **Schema** permet de configurer davantage vos données
    pour cette expérience. Pour cet exemple, sélectionnez **toggle
    switch** pour qu'il soit à l'état d'arrêt pour le

    1.  **Casual** and

    2.  **registered** columns.

> Cliquez sur **Next**.

Ces colonnes sont une répartition de la colonne **cnt**, donc nous ne
les incluons pas.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image12.png)

9.  Dans le formulaire **Review**, vérifiez les informations et cliquez
    sur **Create** pour terminer la création de votre ressource de
    données.

> ![](./media/image13.png)

10. De retour sur la page **Create a new Automated ML job**, un message
    de **success** pour la création de la ressource de données
    s'affiche.

11. Sélectionnez les **bikedata** nouvellement créées et cliquez sur
    **Next**.

> **Remarque : Refresh** le volet des ressources de données si bikedata
> ne s'affiche pas.
>
> ![](./media/image14.png)

### **Tâche 2 : Configurer le Job**

1.  Sur la page **Task settings**, fournissez les détails ci-dessous et
    sélectionnez **View additional configuration settings**.

> Target column – **cnt(Integer)**
>
> Time column **– date (Date)**
>
> Désélectionnez **Deselect** **Autodetect forecast horizon** et
> indiquez la valeur +++**14**+++.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image15.png)

2.  Dans le volet de configuration supplémentaire, fournissez les
    détails ci-dessous et cliquez sur **Save**.

- Primary metric - **Normalized root mean squared error**

- Explain best model – **Enable**

- Blocked algorithms - **Extreme Random Trees**

> Développez les paramètres de prévision supplémentaires

- Autodetect Forecast target lags – **Non sélectionné**

- Autodetect Target rolling window size – **Non sélectionnée**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image16.png)

3.  Sélectionnez **Limits** et entrez +++**60**+++ pour le champ
    **Experiment timeout(minutes).**

![Une capture d'écran d'un test Le contenu généré par l'IA peut être
incorrect.](./media/image17.png)

4.  Sélectionnez les valeurs ci-dessous sous **Validate and test** ,
    puis sélectionnez **Next**.

> Validation type – **k-fold cross-validation**
>
> Number of cross validations – **5**
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image18.png)

5.  Sélectionnez **automl-compute** (celui que nous avons créé dans le
    labo précédent). Cliquez sur **Next.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image19.png)

6.  Vérifiez les détails et sélectionnez **Submit training job**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image20.png)

7.  La page d'état indique l'état initial **Running.** Continuez à
    actualiser la page pour connaître l'état.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image21.png)

8.  Une fois la formation terminée, le statut passe à **Completed**.

**Remarque :** La formation dure environ 30 à 45 minutes.

## **Exercice 3 : Explorer les modèles**

1.  Accédez à l'onglet **Models** pour voir les algorithmes (modèles)
    testés. Par défaut, les modèles sont classés par score de métrique
    au fur et à mesure qu'ils se terminent.

2.  Pour ce didacticiel, le modèle qui obtient le score le plus élevé en
    fonction de la **métrique Normalized root mean squared error**
    choisie figure en haut de la liste.

3.  Pendant que vous attendez la fin de tous les modèles d'expérience,
    sélectionnez le **Algorithm name** d'un modèle terminé pour explorer
    les détails de ses performances.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image22.png)

4.  Cliquez sur **Overview** et consultez ses détails.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image23.png)

5.  Cliquez sur l'onglet **Metrics** et explorez les détails.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image24.png)
>
> **Important :** Poursuivez l'exécution de l'atelier suivant pendant la
> fin de cette formation. Reprenez votre retour à ce laboratoire à
> partir d'ici, une fois la formation terminée.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image25.png)

## **Exercice 4 : Identifier le meilleur modèle**

Le Machine Learning automatisé dans Azure Machine Learning Studio vous
permet de déployer le meilleur modèle en tant que service web en
quelques étapes. Le déploiement est l'intégration du modèle afin qu'il
puisse prédire sur de nouvelles données et identifier les zones
d'opportunités potentielles.

1.  Une fois le travail terminé, revenez à la page du travail parent en
    sélectionnant **the job name** en haut de votre écran.

![](./media/image26.png)

2.  Dans la section **Best model summary**, le meilleur modèle dans le
    contexte de cette expérience est sélectionné en fonction de
    **Normalized root mean squared error metric.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

3.  Cliquez sur le nom de l'algorithme pour l'ouvrir et explorer les
    détails.

4.  Le modèle peut également être déployé en tant que service Web**.**

**Résumé**

Dans cet atelier, vous avez utilisé le ML automatisé dans le studio
Azure Machine Learning pour créer un modèle de prévision de séries
chronologiques qui prédit la demande de location de vélos en
libre-service.
