# Atelier 02 - Création d'un jeu de données étiqueté à l'aide des outils d'étiquetage de données Azure Machine Learning

**Objectif**

Dans cet atelier, vous allez apprendre à utiliser les outils de données
Azure Machine Learning dans Azure Machine Learning Studio pour gérer
leurs collections de données non étiquetées dans des jeux de données
étiquetés qui prennent en charge les classes qui seraient détectées par
le modèle de détection d'objets entraînés.

Durée prévue - 40 min

## **Exercice 1 : Préparation des ressources Azure**

### **Tâche 1 : Créer un compte de stockage Azure**

1.  À partir de la page **d'accueil** du **portail Azure**,
    (+++**https://portal.azure.com**+++), saisissez +++ **storage
    account** +++ dans la barre de recherche et sélectionnez **Storage
    accounts**.

![](./media/image1.png)

2.  Sélectionnez **+Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image2.png)

3.  Sur la page Créer un compte de stockage, entrez les détails
    ci-dessous.

> **Détails du projet**

- Subscription – Sélectionnez votre **subscription**.

- Resource group : sélectionnez **Resource group** qui vous est
  attribué.

> **Détails de l'instance**

- Storage account name – +++**imagestoreacc@lab.LabInstance.Id** +++

- Region : sélectionnez la **Region** dans laquelle vous avez créé votre
  **AML Workspace**

- Performance – Sélectionner **Standard**

- Redondancy – Sélectionnez **Locally-redundant storage(LRS)**

Sélectionnez **Next.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image3.png)

4.  Sous l'onglet Avancé, assurez-vous que l'option **Allow cross-tenant
    replication** dans la section **Blob storage** n'est pas cochée.
    Acceptez les autres valeurs par défaut et sélectionnez **Review +
    create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.png)

5.  Une fois la validation réussie, cliquez sur **Create**.

> ![Une capture d'écran d'une erreur informatique Description générée
> automatiquement](./media/image5.png)

6.  Une fois le déploiement terminé, cliquez sur **Go to resource**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

7.  Notez le nom du compte de stockage, car il sera utilisé dans la
    dernière partie du laboratoire. Restez sur la même longueur d'onde
    et passez à la tâche suivante.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.png)

### **Tâche 2 : Créer un conteneur de stockage Azure (Azure Storage Container)**

1.  Dans le menu de gauche de la page du compte de stockage, faites
    défiler jusqu'à la section **Data Storage**, puis sélectionnez
    **Containers**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image8.png)

2.  Sélectionnez le **Container +**. Dans le volet Nouveau conteneur qui
    s'ouvre, tapez le nom du conteneur sous la forme +++imagedata+++,
    puis cliquez sur **Create**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

3.  Une fois le conteneur créé, sélectionnez **Access keys** sous
    **Security + networking** dans le volet gauche. Sur la page Clés
    d'accès, cliquez sur **Show** en regard de la valeur de la clé, puis
    **copy** la clé. Stockez la valeur copiée dans un bloc-notes pour
    référence ultérieure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image10.png)

4.  Revenez à la page Conteneurs en sélectionnant **Containers** dans le
    volet gauche.

![](./media/image11.png)

5.  Sélectionnez le conteneur nouvellement créé, **imagedata**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image12.png)

6.  Cliquez sur **Upload**. Dans le volet **Upload blob**, cliquez sur
    **Browse for files** et ouvrez le dossier **train_img** sous
    **C :\Labfiles**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image13.png)

7.  Sélectionnez tous les fichiers du dossier train_img et cliquez sur
    **Open**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.png)

8.  Cliquez sur **Upload** sur la page Upload blob.

![](./media/image15.png)

9.  Une fois téléchargé le message **Successfully uploaded blob(s)**
    s'affiche, puis fermez le volet **Upload blob**.

![](./media/image16.png)

10. Une fois l'opération terminée, vous devez voir que les 242 images
    ont été ajoutées au conteneur de stockage Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image17.png)

## **Exercice 2 : Créer un projet d'étiquetage de données (data labeling Azure Machine Learning**

1.  Dans la page d'accueil d'Azure Machine Learning Studio, sélectionnez
    **Data Labeling** sous **Manage** dans le volet gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image18.png)

2.  Sélectionnez **+ Create.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image19.png)

3.  Dans la section **Project details**, donnez les détails suivants.

    1.  **Project name** - +++**soda**+++

    2.  Media type – **Image**

    3.  **Labeling task type - Object Identification (Bounding Box)**

Sélectionnez **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image20.png)

4.  Dans l’écran **Add workforce (optional),** laissez l'option
    désactivée et sélectionnez **Next** pour continuer.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image21.png)

5.  Sur la page **Select or create data**, cliquez sur **+ Create**.

> ![](./media/image22.png)

6.  Dans le volet **Data type** de données de la page **Create data
    asset**, fournissez les détails ci-dessous.

    1.  **Name** – +++**sodaObjects**+++

    2.  **Description –** +++**Image labelling**+++

    3.  **Type –** File

> Cliquez sur **Next**.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image23.png)

7.  Dans le volet **Data source** de la page **Create data asset**,
    sélectionnez l’option **From Azure storage**, puis cliquez sur
    **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image24.png)

8.  Dans le volet **Storage type** de la page **Create data asset**,
    sélectionnez **Create new datastore**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image25.png)

9.  Dans le volet **New** **datastore**, fournissez les détails
    ci-dessous.

    1.  **Datastore name** – +++**SodataLevel**+++

    2.  **Datastore type** : sélectionnez **Azure Blob Storage**

    3.  **Account selection method –** Sélectionner **From Azure
        subscription**

    4.  **Subscription ID –** Sélectionnez votre abonnement

    5.  **Storage account –** Sélectionnez **imagestoreacc**

    6.  **Blob container –** Sélectionner **imagedata**

    7.  **Authentication type –** Sélectionnez **Account Key**

    8.  **Account key –** Entrez la clé de compte enregistrée
        précédemment dans l'exercice 1

> Cliquez sur **Create**.
>
> ![](./media/image26.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image27.png)

10. Le message de réussite **Create success** s'affiche sur la page
    **Select a datastore.** Sélectionnez le **sodadatastore** qui a été
    créé. Cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image28.png)

11. Sous Choisir un chemin de stockage, sélectionnez **Enter storage
    path manually** et tapez **/** pour le chemin de stockage, activez
    Ignorer la **Skip data validation**. Cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image29.png)

12. Vérifiez les détails et cliquez sur **Create**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image30.png)

13. De retour dans le volet **Select or create data**, sélectionnez
    **sodaObjects.** Cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image31.png)

14. Sur la page **Incremental refresh**, sélectionnez **Enable
    incremental refresh at regular intervals.** Cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image32.png)

15. Sur la page **Label categories**, cliquez deux fois sur **Add label
    category** d'étiquette pour ajouter deux autres espaces réservés au
    nom de la catégorie en plus de celui qui existe déjà.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image33.png)

16. Après l'ajout, tapez +++**coke**+++, +++**diet_coke**+++ et
    +++**sprite**+++, un dans chaque espace réservé de catégorie
    d'étiquette. Cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image34.png)

17. Laissez les instructions d'étiquetage vides et cliquez sur **Next**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image35.png)

18. Cliquez sur **Next** dans la page **Quality control(preview)**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image36.png)

19. Désactivez l’option **Enable** **ML assisted labelling** et cliquez
    sur **Create** **project**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing** s'affiche sur l'écran d'étiquetage des données.
    Cliquez sur le projet de **soda.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image38.png)

21. Cliquez sur **Label data**.

> ![](./media/image39.png)

22. Les **Shortcut keys** en haut à droite affichent les différents
    raccourcis disponibles.

> ![Un groupe de canettes de soda sur une table Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image40.png)

23. La barre de menu supérieure fournit les différentes options
    disponibles.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image41.png)

24. La première image s'ouvre à l'écran. Sélectionnez la balise
    appropriée dans le volet **Tags** à gauche.

> Ensuite, cliquez sur l'image et faites-la glisser un peu pour voir
> l'étiquette attachée à l'image. Cliquez sur **submit**.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image42.png)

25. Répétez le même processus pour les images suivantes qui apparaissent
    lors de l'envoi de l'image actuelle.

> Étiquetez au moins 10 images.
>
> ![](./media/image43.png)

26. L'image suivante est téléchargée jusqu'à ce que la fin des images
    soit atteinte. Veuillez-vous arrêter à tout moment au-delà de 10
    images ou continuer et compléter l'étiquetage pour toutes les
    images.

27. Cliquez sur soda dans le chemin de navigation supérieur pour revenir
    au **Dashboard**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image44.png)

28. Le **Dashboard** fournit des détails sur les **labeled assets** et
    la **label distribution**.

> ![](./media/image45.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image46.png)

29. Cliquez sur **Export.**

> ![Une capture d'écran d'un graphique Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image47.png)

30. Dans le volet **Export data**, sélectionnez l'icône

    - **Asset type - Labeled**

    - **Export format - Azure ML dataset**

> Cliquez sur **Submit**.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image48.png)

31. Le message **Labels successfully exported** s'affiche sur la page
    Dashboard une fois l'exportation terminée. Cliquez sur le **file
    link** dans le message de réussite pour ouvrir les détails du
    fichier exporté.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image49.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image50.png)

32. Cliquez sur le lien **View in** **datastores** ou **View in Azure
    portal** sous la section **Datasources** -\> **Actions**

> ![](./media/image51.png)

33. Vue dans les banques de données.

> ![Une image contenant du texte, un numéro, un logiciel, une police
> Description générée automatiquement](./media/image52.png)

**Résumé**

Dans cet atelier, vous avez appris à créer une ressource de données à
partir du stockage Azure, à étiqueter les images et à créer un jeu de
données étiqueté.

L'ensemble de ces tâches appartient également à l'étape **Data: Explore
& prepare** du **Machine Learning project workflow.**
