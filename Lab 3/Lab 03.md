# Atelier 03 - Développer et inscrire un ensemble de fonctionnalités avec le Feature Store géré et entraîner des modèles à l'aide de fonctionnalités

Cet atelier explique comment créer une spécification d'ensemble de
fonctionnalités avec des transformations personnalisées. Il utilise
ensuite cet ensemble de fonctionnalités pour générer des données
d'entraînement, activer la matérialisation et effectuer un renvoi. La
matérialisation calcule les valeurs de fonction d'une fenêtre de
fonction, puis stocke ces valeurs dans un store de matérialisation.
Toutes les requêtes de fonctionnalités peuvent ensuite utiliser ces
valeurs du store de matérialisation.

Sans matérialisation, une requête d'ensemble de fonctionnalités applique
les transformations à la source à la volée, afin de calculer les
caractéristiques avant de renvoyer les valeurs. Ce processus fonctionne
bien pour la phase de prototypage. Toutefois, pour les opérations
d'entraînement et d'inférence dans un environnement de production, nous
vous recommandons de matérialiser les fonctionnalités, pour plus de
fiabilité et de disponibilité.

Durée prévue – 50 minutes

## Exercice 1 : Attribuer les rôles requis :

1.  Dans la page d'accueil du portail Azure, sélectionnez **Resource
    group** attribué dans l'onglet **Resources**. Dans le volet gauche,
    sélectionnez **Access control(IAM).** Cliquez sur le menu déroulant
    en regard de **Add** et sélectionnez **Add rôle assignment**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.png)

2.  Recherchez +++**AzureML Data Scientist**+++ et sélectionnez-le.
    Cliquez sur **Next**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image2.png)

3.  Dans l'onglet Membres, cliquez sur **+ Select members**, recherchez
    votre **User name,**
    +++@lab.CloudPortalCredential(User1).Username+++.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image3.png)

4.  Sélectionnez votre **Username**, puis cliquez sur le bouton
    **Select.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.png)

5.  Cliquez sur **Review + assign** dans les 2 écrans suivants.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image5.png)

6.  Le message d'attribution de rôle ajouté est obtenu une fois
    l'attribution terminée.

7.  Répétez le même ensemble d'étapes pour ajouter les rôles
    +++**Storage Blob Data Reader**+++ et +++**Storage Blob Data
    Contributor+++.**

## Exercice 2 : Développer un ensemble de fonctionnalités et s'inscrire auprès du Feature Store géré

Ce tutoriel est la première partie de la série de tutoriels du feature
store gérées. Ici, vous allez apprendre à :

- Créez une nouvelle ressource de Feature Store minimale.

- Développez et testez localement un ensemble de fonctionnalités avec
  une capacité de transformation des caractéristiques.

- Enregistrez une entité de Feature Store dans le Feature Store.

- Enregistrez l’ensemble de fonctionnalités que vous avez développé dans
  le Feature Store.

- Générez un exemple de DataFrame d'entraînement à l'aide des
  fonctionnalités que vous avez créées.

- Activez la matérialisation hors ligne des ensembles de fonctionnalités
  et effectuez un remplissage rétroactif des données de
  caractéristiques.

### Tâche 1 : Préparer l'environnement

1.  Dans le volet gauche d'Azure Machine Learning Studio, sélectionnez
    **Notebooks** sous **Authoring**. Cliquez sur les trois points à
    côté du nom d'utilisateur et sélectionnez **Upload folder**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

2.  Parcourez et sélectionnez le dossier **featurestore** dans
    **C :\Labfiles** et cliquez sur **Upload**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image7.png)

3.  Accédez à **featurestore-\> notebooks-\>sdk_and_cli** et ouvrez le
    notebook 1.Develop-feature-set-and-register.ipynb

![](./media/image8.png)

4.  Sélectionnez **Serverless Spark Compute** sous **Compute.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image9.png)

5.  Sélectionnez **Configurer la session** pour configurer la session
    avec les conditions préalables.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image10.png)

6.  Sélectionnez **Python packages -\> Upload Conda file**. Cliquez sur
    **Browse** et sélectionnez **conda.yml** dans **C :\Labfiles**, puis
    sélectionnez **Apply**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image11.png)

7.  **Exécute** la première cellule du bloc-notes. Cela installera
    toutes les **dependancies** et terminera son exécution. Cela prendra
    environ **10 minutes** .

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image12.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image13.png)

8.  Une fois la session Spark démarrée, remplacez **User name** par
    votre nom d'utilisateur et exécutez la cellule suivante

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image14.png)

![Une capture d'écran d'une erreur informatique Description générée
automatiquement](./media/image15.png)

9.  Exécutez les 3 cellules suivantes pour configurer Azure CLI.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image16.png)

10. Dans la cellule suivante, suivez les étapes **output** pour vous
    connecter à **Azure**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image18.png)

### Tâche 2 : Créer un Feature Store minimal

1.  **Exécute** la **première** cellule afin de définir le nom,
    l'emplacement et d'autres valeurs du Feature Store.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image19.png)

2.  **Exécutez** la cellule suivante **creates the feature store**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image20.png)

3.  La cellule suivante **AzureML feature store core SDK client**.
    **Exécutez**-la.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image21.png)

### Tâche 3 : Prototyper et développer un ensemble de fonctionnalités d'agrégation de transactions propagées dans ce notebook

1.  **Exécutez** la première cellule de cette section pour explorer les
    données sources des **transactions**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image22.png)

2.  Exécutez la deuxième cellule pour **Develop a transactions feature
    set** localement.

![Une capture d'écran d'un code informatique Description générée
automatiquement](./media/image23.png)

3.  Exécutez la cellule suivante pour **generate a spark dataframe** à
    partir de la spécification de l'ensemble de fonctionnalités.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image24.png)

4.  Pour enregistrer la spécification de l'ensemble de fonctionnalités
    auprès du feature store, elle doit être enregistrée dans un format
    spécifique. Veuillez inspecter les transactions générées
    FeaturesetSpec : Ouvrez ce fichier à partir de l'arborescence des
    fichiers pour voir la spécification :
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

Exécutez la cellule suivante à exporter en tant que spécification
d'ensemble de fonctionnalités.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image25.png)

### Tâche 4 : Inscrire une entité de feature store

1.  Entity permet d'appliquer la meilleure pratique selon laquelle les
    mêmes définitions de clé de jointure sont utilisées dans tous les
    ensembles de fonctionnalités qui utilisent les mêmes entités
    logiques. Exécutez la cellule pour enregistrer une entité de feature
    store.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image26.png)

### Tâche 5 : Enregistrer l'ensemble de fonctionnalités de transaction auprès du Feature Store

1.  À partir du portail Azure(+++https ://portal.azure.com+++), accédez
    au **Storage account** qui commence par **featureset** sous le
    Resource group qui vous est attribué**.**

> ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
> être incorrect.](./media/image27.png)

2.  Dans le volet gauche, sélectionnez Contrôle d'accès (IAM).
    Sélectionnez **Add** -\> **Add role assignment**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image28.png)

3.  Recherchez et sélectionnez +++**Storage Blob Data Reader**+++.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image29.png)

4.  Terminez l'attribution de rôle de la même manière que celles que
    nous avons faites dans l'exercice 1.

5.  De même, ajoutez le rôle +++**Storage Blob Data Contributor**+++.

6.  Revenez à Azure Machine Learning Studio.

7.  Vous inscrivez un actif d'ensemble de fonctionnalités auprès du
    feature store afin de pouvoir le partager et le réutiliser avec
    d'autres utilisateurs. Vous bénéficiez également de fonctionnalités
    gérées telles que la gestion des versions et la matérialisation.
    L'actif de l'ensemble de fonctionnalités fait référence à la
    spécification de l'ensemble de fonctionnalités que vous avez créée
    précédemment et à des propriétés supplémentaires telles que les
    paramètres de version et de matérialisation.

8.  **Exécutez** la cellule suivante pour **register the transaction
    feature set** auprès du Feature Store.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image30.png)

### Tâche 6 : Explorer l'interface utilisateur du Feature Store

1.  Ouvrez un nouvel onglet dans le navigateur et accédez à la page
    d'accueil mondiale d'Azure ML à l'adresse
    +++https://ml.azure.com/home+++.

2.  Cliquez sur **Feature Stores** dans le volet de navigation de
    gauche.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image31.png)

3.  Cliquez sur le **featurestore**.

**Remarque : La** création et la mise à jour des ressources du Feature
Store (ensembles de fonctionnalités et entités) ne sont possibles que
via le SDK et l'interface de ligne de commande. Vous pouvez utiliser
l'interface utilisateur pour effectuer des recherches/parcourir le
Feature Store.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image32.png)

### Tâche 7 : Générer une trame de données d'entraînement à l'aide des fonctionnalités enregistrées

1.  Nous commençons par explorer les données d'observation. Les données
    d'observation sont généralement les données de base utilisées dans
    les données d'entraînement et d'inférence. Celles-ci sont ensuite
    associées aux données de caractéristique pour créer les données
    d'entraînement complètes. Les données d'observation sont les données
    capturées au moment de l'événement : dans ce cas, il s'agit des
    données de transaction de base, notamment l'ID de transaction, l'ID
    de compte, le montant de la transaction. Dans ce cas, puisqu'il
    s'agit d'un entraînement, la variable cible (is_fraud) lui est
    également ajoutée.

2.  **Exécutez-la** cel land observez les données de sortie.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image33.png)

3.  **Exécutez** la cellule suivante pour obtenir **registered feature
    set** and **list its features**.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image34.png)

4.  **Exécutez** la cellule suivante **print** **sample values.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image35.png)

5.  **Exécute** la cellule suivante. Dans cette étape, nous allons
    **select features** que nous souhaitons intégrer au **training
    data** et utiliser le SDK du Feature Store pour générer les données
    d'entraînement.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image36.png)

6.  Exécutez la cellule suivante pour générer une trame de données
    d'entraînement à l'aide de données de caractéristiques et de données
    d'observation.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image37.png)

### Tâche 8 : Activer la matérialisation hors ligne sur les transactions

Une fois la matérialisation activée sur un ensemble de fonctionnalités,
vous pouvez effectuer des tâches de remplissage ou de matérialisation
récurrentes.

1.  Exécutez la cellule suivante pour définir
    spark.sql.shuffle.partitions dans le fichier yaml en fonction de la
    taille des données de la fonctionnalité

2.  La configuration Spark spark.sql.shuffle.partitions est un paramètre
    FACULTATIF qui peut affecter le nombre de fichiers Parquet générés
    (par jour) lorsque l'ensemble de fonctionnalités est matérialisé
    dans le store hors ligne. La valeur par défaut de ce paramètre
    est 200. La meilleure pratique est d'éviter de générer de nombreux
    petits dossiers parquet. Si la récupération des fonctionnalités hors
    ligne s'avère lente après la matérialisation de l'ensemble des
    fonctionnalités, veuillez vous rendre dans le dossier correspondant
    dans le store hors ligne pour vérifier s'il s'agit d'un problème lié
    à un trop grand nombre de petits fichiers parquet (par jour), et
    ajustez la valeur de ce paramètre en conséquence.

**Remarque :** Les exemples de données utilisés dans ce notebook sont
petits. Ce paramètre est donc défini sur 1 dans le fichier
featureset_asset_offline_enabled.yaml.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image38.png)

3.  La matérialisation est le processus qui consiste à calculer les
    valeurs de fonction pour une fenêtre de fonctionnalité donnée et à
    les stocker dans un store de matérialisation. La matérialisation des
    fonctionnalités augmentera sa fiabilité et sa disponibilité. Toutes
    les requêtes de fonctionnalités utiliseront les valeurs
    matérialisées du store de matérialisation. Dans cette étape, vous
    allez effectuer un renvoi unique pour une fenêtre de fonctionnalité
    de 18 mois.

4.  La cellule de code suivante **materialize data** selon l'état actuel
    Aucun ou Incomplet pour la fenêtre de fonction définie.
    **Exécutez-la**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image39.png)

5.  **Imprimons des exemples de données** (**print sample data**) à
    partir de l'ensemble de fonctionnalités dans la cellule suivante.
    **Exécutez-la**. Vous pouvez remarquer à partir des informations de
    sortie que les données ont été récupérées à partir du store de
    matérialisation. get_offline_features() utilisée pour récupérer les
    données d'entraînement/d'inférence utilisera également le store de
    matérialisation par défaut.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image40.png)

## Exercice 3 : Expérimenter et entraîner des modèles à l'aide de fonctionnalités

Dans ce carnet, vous allez apprendre à :

- Prototypez une spécification de nouvel ensemble de fonctionnalités de
  comptes, en utilisant des valeurs précalculées existantes comme
  fonctionnalités. Ensuite, enregistrez la spécification de l'ensemble
  de fonctionnalités local en tant qu'ensemble de fonctionnalités dans
  le Feature Store. Ce processus diffère du premier didacticiel, où vous
  avez créé un ensemble de fonctionnalités comportant des
  transformations personnalisées.

- Sélectionnez des fonctions pour le modèle dans les ensembles de
  fonctionnalités Transactions et comptes, puis enregistrez-les en tant
  que spécification d'extraction de fonctionnalités.

- Exécutez un pipeline d'entraînement qui utilise la spécification de
  récupération de fonctionnalités pour entraîner un nouveau modèle. Ce
  pipeline utilise le composant de récupération de fonctionnalités
  intégré pour générer les données d'entraînement.

### Tâche 1 : Configurer l'environnement

1.  Dans le volet Notebooks, ouvrez le bloc-notes **Experiment and train
    models using features**.

2.  Cliquez sur **Configure session** et chargez le fichier
    **conda.yaml** de la même manière que nous l'avons fait pour le
    bloc-notes précédent.

3.  **Exécutez** la **première cellule** pour démarrer la session. Cela
    prendra environ 10 minutes.

![Un objet rectangulaire blanc avec du texte vert Description générée
automatiquement](./media/image41.png)

4.  Dans la cellule suivante, remplacez l'espace réservé pour **\<
    your_user_alias \>** par votre **user name** dans la structure des
    dossiers et **Exécutez** la cellule.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image42.png)

5.  **Exécutez** les 3 **cellules** suivantes pour **setup CLI**.

6.  La cellule suivante initialise les variables de l'espace de travail
    du projet. **Exécutez-la** pour **initialiser les variables
    (initialize the variables)**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image43.png)

7.  La cellule suivante initialise les variables du Feature Store.
    Exécutez-la.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image44.png)

8.  Exécutez la cellule suivante pour **Initialize the Feature Store
    consumption client.**

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement](./media/image45.png)

### Tâche 2 : Créer un ensemble de fonctionnalités de comptes localement à partir de données précalculées

Pour l'intégration de fonctionnalités précalculées, vous pouvez créer
une spécification de jeu de fonctionnalités sans écrire de code de
transformation. Featureset spec est une spécification permettant de
développer et de tester un ensemble de fonctionnalités dans un
environnement entièrement local/de développement sans se connecter à un
feature store. Dans cette étape, vous allez créer la spécification de
l'ensemble de fonctionnalités localement et échantillonner les valeurs à
partir de celle-ci.

1.  Exécutez la cellule ci-dessous pour **explore the source data for
    accounts.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image46.png)

2.  Exécutez la cellule suivante pour **créer des spécifications de jeu
    de fonctionnalités de comptes** (**create accounts feature set
    spec)** en local à partir de ces fonctionnalités précalculées.

![Capture d'écran d'un code informatique Description générée
automatiquement](./media/image47.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image48.png)

3.  **Exécutez** la cellule suivante pour **générer une trame de données
    Spark (generate a spark dataframe)** à partir de la spécification de
    l'ensemble de fonctionnalités.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image49.png)

4.  Pour enregistrer la spécification de l'ensemble de fonctionnalités
    dans le feature store, elle doit être enregistrée dans un format
    spécifique. Action : Après avoir exécuté la cellule ci-dessous,
    veuillez inspecter les comptes générés FeatureSetSpec : Ouvrez ce
    fichier à partir de l'arborescence des fichiers pour voir les
    spécifications :
    featurestore/featuresets/accounts/spec/FeatureSetSpec. **Exécutez**
    la cellule suivante.![Une capture d'écran d'un programme
    informatique Description générée
    automatiquement](./media/image50.png)

### Tâche 3 : Tester localement les fonctionnalités non enregistrées et s'inscrire auprès du Feature Store lorsque vous êtes prêt

Lorsque vous développez des fonctionnalités, vous pouvez tester/valider
localement avant de vous inscrire au Feature Store ou d'exécuter des
pipelines d'entraînement dans le cloud. Dans cette étape, vous allez
générer des données d'entraînement pour le modèle ML à partir de la
combinaison de fonctionnalités d'un ensemble de fonctionnalités local
non enregistré (comptes) et d'un ensemble de fonctionnalités enregistré
dans le Feature Store (transactions).

1.  **Exécutez** la cellule suivante pour **sélectionner les fonctions**
    du **modèle (select features** for **model).**

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image51.png)

2.  **Exécutez** les 2 cellules suivantes pour **générer des données
    d'entraînement (generate training data)** localement.

![Gros plan d'un code informatique Description générée
automatiquement](./media/image52.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image53.png)

3.  **Exécutez** la cellule suivante **register
    the accounts featureset** auprès du feature store. Une fois que vous
    avez expérimenté différentes définitions de fonctionnalités
    localement et que vous les avez testées, vous pouvez les enregistrer
    auprès du Feature Store. Pour cela, vous allez enregistrer une
    définition d'actif de jeu de fonctionnalités auprès du feature
    store.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image54.png)

4.  **Exécutez** les 2 cellules suivantes pour obtenir l'ensemble de
    fonctionnalités enregistré et le test de santé mentale.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image55.png)

### Tâche 4 : Exécuter l'expérience d'entraînement

1.  Exécutez la cellule suivante pour découvrir les fonctionnalités du
    SDK.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image56.png)

2.  Dans les étapes précédentes, vous avez sélectionné des
    fonctionnalités à partir d'une combinaison d'ensembles de
    fonctionnalités non enregistrés et enregistrés pour
    l'expérimentation et le test locaux. Vous êtes maintenant prêt à
    expérimenter dans le cloud. L'enregistrement des fonctionnalités
    sélectionnées en tant que spécification de récupération de
    fonctionnalités et leur utilisation dans le flux mlops/cicd à des
    fins d'entraînement/d'inférence augmentent votre agilité dans la
    livraison des modèles.

3.  **Exécutez** la cellule suivante pour **sélectionner les fonctions
    du modèle (select features for the model)**.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image57.png)

4.  **Exécutez** la cellule suivante et exportez les fonctions
    sélectionnées en tant que **feature-retrieval spec**.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image58.png)

### Tâche 5 : S'entraîner dans le cloud à l'aide de pipelines et enregistrer le modèle si satisfaisant

Dans cette étape, vous allez déclencher manuellement le pipeline
d'entraînement. Dans un scénario de production, cela peut être déclenché
par un pipeline ci/cd basé sur les modifications apportées à la
spécification de récupération de fonctionnalités dans le référentiel
source.

1.  **Exécutez** la cellule suivante pour **exécuter le pipeline
    d'entraînement (run the training pipeline).**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image59.png)

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image60.png)

2.  Dans le volet gauche du studio, faites un clic droit sur **Jobs** et
    ouvrez-le dans un nouvel onglet. Sélectionnez l'expérience,
    **training_on_fraud_model**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image61.png)

3.  Cliquez sur le **training job** et explorez les détails.
    L'expérience devrait prendre environ 5 à 15 minutes pour être
    terminée.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image62.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image63.png)

4.  Attendez qu'il soit terminé. Une fois terminé, sélectionnez
    **Models** dans le volet gauche. Sélectionnez **fraud_model** dans
    la liste. C'est le modèle qui a été créé maintenant.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image64.png)

5.  Sélectionnez l’onglet **Feature sets**. Ici, vous pouvez voir à la
    fois les **transactions** et les comptes (**accounts**) dont dépend
    ce modèle.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image65.png)

6.  Ouvrez **Feature Store UI** à l'adresse
    +++https://ml.azure.com/home+++. Sélectionnez **Feature Stores** -\>
    **featurestore**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image66.png)

7.  Sélectionnez **Feature sets** dans le volet gauche, puis
    sélectionnez l'un des **feature sets**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image67.png)

8.  Cliquez sur l'onglet **Models**. Vous pouvez voir la liste des
    modèles qui utilisent les ensembles de fonctionnalités (déterminée à
    partir de la spécification de récupération des fonctionnalités lors
    de l'enregistrement du modèle).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image68.png)

Résumé :

Dans cet atelier, nous avons appris à développer et à inscrire un
ensemble de fonctionnalités avec le Feature Store géré et à entraîner
des modèles à l'aide de fonctionnalités.
