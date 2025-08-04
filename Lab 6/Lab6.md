# Atelier 06 - Entraînement du meilleur modèle de régression pour l'ensemble de données matérielles

Objectif

Dans cet atelier, nous allons voir comment utiliser AutoML pour
entraîner un modèle de régression. Nous utiliserons le jeu de données
Performances matérielles pour entraîner et déployer le modèle à utiliser
dans des scénarios d'inférence. L'objectif de la régression est de
prédire les performances de certaines combinaisons de pièces
matérielles.

Durée prévue – 60 minutes

# Exercice 0 : Préparez l'environnement

### **Tâche 1 : Lancer l'espace de travail AML**

1.  Connectez-vous au portail Azure,
    +++[**https://portal.azure.com**](https://portal.azure.com)+++ si
    vous n'êtes pas déjà connecté.

2.  Dans le menu du portail Azure, sélectionnez **All resources.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.png)

3.  Sélectionnez l'espace de travail (workspace) Azure Machine Learning
    (**Azuemlws@lab.LabInstanceId**).

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image2.png)

4.  Cliquez sur **Launch studio**.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image3.png)

5.  Sélectionnez **Compute** dans le volet gauche pour créer une
    instance de calcul. Sélectionnez **+ New**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image4.png)

6.  Fournissez les détails ci-dessous et cliquez sur **Review +
    Create**.

- Compute name - +++**auto-Compute+++**

- Virtual machine type – **CPU**

- Virtual Machine – **Standard E4ds_v4**

![](./media/image5.png)

7.  Sélectionnez **Create** pour créer l'instance de calcul (compute).

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image6.png)

### **Tâche 2 : Charger le bloc-notes dans AML Workspace**

1.  Cliquez sur **Notebooks** dans le volet de gauche. Cliquez sur les
    trois points à côté du **username** sous **Users** et sélectionnez
    **Upload folder**.

![](./media/image7.png)

2.  Sélectionnez Cliquez pour parcourir et sélectionnez le(s) dossier(s)
    et parcourez **C :\Labfiles** pour sélectionner le dossier
    **automl-regression-task-hardware-performance** et cliquez sur
    **Upload.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image8.png)

3.  Si vous obtenez une fenêtre contextuelle vous demandant de
    télécharger 3 fichiers sur ce site, cliquez sur **Upload**.

![Une image contenant du texte, une capture d'écran, un affichage, une
police Description générée automatiquement](./media/image9.png)

4.  Cochez la case **I trust contents of these files**, puis
    sélectionnez **Upload**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image10.png)

5.  Ouvrez Notebook(the .ipynb file),
    **automl-regression-task-hardware-performance**. Le bloc-notes est
    automatiquement connecté au calcul que nous avons créé précédemment.

![](./media/image11.png)

## **Exercice 1 : Se connecter à Azure Machine Learning Workspace**

### **Tâche 1 : Importer les bibliothèques requises**

1.  Exécutez la première cellule de la cellule sous **1.1** **Import the
    required libraries** pour importer les bibliothèques requises pour
    cette exécution de labo en cliquant sur le bouton Exécuter la
    cellule en haut à gauche de la cellule.

2.  Assurez-vous que l'exécution est réussie en recherchant un symbole
    de coche en bas à gauche de la cellule.

![Une capture d'écran d'un écran d'ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image12.png)

### **Tâche 2 : Configurer les détails de l'espace de travail et obtenir un handle pour l'espace de travail**

1.  Dans la cellule sous **1.2. Configure workspace details and get a
    handle to the workspace,** remplacer

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP : **Your assigned Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  Cliquez sur l'option Exécuter la cellule en haut à gauche de la
    cellule et assurez-vous d'obtenir une coche en bas à gauche une fois
    l'exécution réussie.

3.  Une sortie indiquant « **Found the config file in : /config.json** »
    s'affiche sous la cellule.

![](./media/image13.png)

### **Tâche 3 : Afficher les informations de l'espace de travail Azure ML**

1.  Exécutez la cellule suivante (la cellule située sous Afficher les
    informations de l'espace de travail Azure ML).

2.  Assurez-vous que les détails de l'espace de travail, de
    l'abonnement, de l'emplacement et du groupe de ressources
    répertoriés en tant que sortie sous la cellule sont tous corrects.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image14.png)

## **Exercice 2 : MLTable avec données d'entraînement d'entrée**

### **Tâche 1 : Créer une entrée de données MLTable**

1.  Exécutez la cellule suivante (celle sous **2.1 Create MLTable data
    input**).

2.  Assurez-vous que l'exécution est réussie.

![Une image contenant du texte, une police, une capture d'écran, un
logiciel Description générée automatiquement](./media/image15.png)

## **Exercice 3 : Configurer et exécuter le travail d'entraînement AutoML Regression**

1.  Exécutez les cellules sous **4.1 Configure and run the AutoML
    Regression training job** une par une et assurez-vous que chaque
    cellule est exécutée correctement.

2.  La cellule sous **4.2 Run the Command** soumet le travail AutoML.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image16.png)

3.  Vous pouvez vérifier l'état de la tâche en cliquant sur **Jobs**
    dans le volet gauche et en sélectionnant l'expérience qui est à
    l'état En cours d'exécution.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image17.png)

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image18.png)

**Remarque :** cela prend environ 10 à 15 minutes.

4.  La cellule suivante du bloc-notes attend que la tâche AutoML soit
    terminée.

5.  Exécutez-le et attendez la fin de l'exécution pour passer à la
    cellule suivante.

![](./media/image19.png)

6.  Ne passez à l'étape suivante qu'une fois l'exécution terminée.

![](./media/image20.png)

7.  Exécutez les 2 cellules suivantes une par une qui récupère l'url et
    le nom du travail.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image21.png)

## **Exercice 4 : Récupérer le meilleur essai (essai/course du meilleur modèle)**

1.  Ajoutez une cellule au-dessus de la première cellule sous cet
    exercice.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image22.png)

2.  Copiez le code ci-dessous. Cliquez sur **Run cell.**

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image23.png)

3.  Continuez à exécuter les 3 cellules suivantes une par une en
    analysant chaque code et sa sortie.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image24.png)

4.  Exécutez la cellule suivante pour **Get the parent run**.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image25.png)

5.  Exécutez la cellule suivante pour **print the parent tags**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image26.png)

6.  Exécutez la cellule suivante pour **Get the AutoML best child run**.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance moyen](./media/image27.png)

7.  Exécutez la cellule suivante pour **Get the best model run’s
    metrics**.

![Une capture d'écran d'une erreur informatique Description générée
automatiquement avec un niveau de confiance faible](./media/image28.png)

8.  Exécutez les 3 cellules suivantes pour **Download the best model
    locally**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image29.png)

## **Exercice 5 : Enregistrer le meilleur modèle et déployer**

### **Tâche 1 : Créer un point de terminaison en ligne géré**

1.  Exécutez les 2 premières cellules sous cette tâche.

![](./media/image30.png)

2.  Exécutez la cellule suivante avec le code,

**ml_client.begin_create_or_update(endpoint).result()**

Cela crée un point de terminaison en ligne nommé
**regression-\<Currentdate&time\>.**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image31.png)

3.  Vérifiez si la notification indique que **Endpoint
    "regression-\<Currentdate&time\>" update completed.**

> ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
> être incorrect.](./media/image32.png)

### **Tâche 2 : Enregistrer le meilleur modèle et le déployer**

1.  Exécutez la première cellule sous Enregistrer le meilleur modèle et
    déployez -\> **Register model** pour inscrire le modèle nommé
    **hardware-performance-model**.

2.  Une fois l'exécution réussie, exécutez la cellule suivante pour
    récupérer l'ID de modèle enregistré.

> ![](./media/image33.png)

### **Tâche 3 : Déployer**

1.  Dans la première cellule sous Déployer, remplacez la valeur
    **instance_type** par **Standard_E4s_v3.**

2.  Ensuite, exécutez la cellule pour déployer le meilleur modèle.

![Une capture d'écran d'un programme informatique Description générée
automatiquement](./media/image34.png)

3.  Exécutez la cellule suivante pour créer le déploiement.

![Une image contenant du texte, une capture d'écran, une ligne, une
police Description générée automatiquement](./media/image35.png)

4.  **Cela prendra environ 40 minutes**. Vous pouvez également vérifier
    l'état sous **Endpoints** (sélectionnez **Endpoints** dans le volet
    gauche, puis cliquez sur le point de **regression-XXXXXXX** que vous
    avez déployé précédemment).

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image36.png)

5.  Une fois l'exécution terminée et le déploiement réussi, la cellule
    génère les détails du déploiement.

![](./media/image37.png)

6.  De plus, dans la page de détails des points de terminaison, l'état
    du déploiement devient **Succeeded.**

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image38.png)

7.  Exécutez la cellule suivante dans le bloc-notes pour que le
    déploiement prenne 100 % du trafic.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance faible](./media/image39.png)

8.  Vérifiez que l'allocation du trafic en direct est de 100 % sur la
    page Détails des points de terminaison.

![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
être incorrect.](./media/image40.png)

## **Exercice 6 : Tester le déploiement**

1.  Exécutez la cellule sous Tester le déploiement.

2.  Vérifiez le résultat.

![](./media/image41.png)

3.  Suivez et exécutez les cellules restantes pour supprimer le point de
    terminaison.

![](./media/image42.png)

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image43.png)

4.  Vérifiez l'état du point de terminaison sous l'onglet Endpoints.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image44.png)

**Résumé**

Dans cet atelier, nous avons appris à

- Se connecter à votre espace de travail AML à partir du SDK Python

- Créez une tâche de régression AutoML avec 'regression()'
  factory-function.

- Entraînez le modèle à l'aide d'AmlCompute en soumettant/exécutant la
  tâche d'entraînement de régression AutoML

- Obtenir le modèle et les prédictions de score avec celui-ci
