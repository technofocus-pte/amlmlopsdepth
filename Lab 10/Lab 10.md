# **Atelier 10 - Utilisation du tableau de bord de l'IA responsable pour améliorer les performances des modèles d'apprentissage automatique**

**Objectif**

Cet atelier a pour but d'acquérir une formation pratique sur la façon
d'utiliser le tableau de bord de l'IA responsable pour déboguer les
modèles d'apprentissage automatique afin d'améliorer les performances du
modèle pour qu'il soit plus équitable, inclusif, sûr, fiable et
transparent.

Dans cet atelier, nous allons explorer comment utiliser la section
**Model Overview** du tableau de bord Azure Responsible AI (RAI). Nous
utiliserons les cohortes créées à partir du laboratoire d'analyse
d'erreurs pour déterminer pourquoi le comportement du modèle est
meilleur dans une cohorte par rapport à une autre.

Durée prévue – 60 minutes

## **Exercice 1 : Préparer les ressources**

### Tâche 1 : Cloner le dépôt pour cet cet atelier 

1.  À partir d'un navigateur, connectez-vous au portail Azure à
    l'adresse <https://portal.azure.com>

2.  Ouvrez **cloud** **Shell** en cliquant sur l'icône Cloud Shell sur
    le portail Azure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image1.png)

3.  Dans l'invite de commande Azure Cloud Shell, clonez le référentiel
    github du projet **Diabetes Hospital Readmission** en exécutant la
    commande ci-dessous.

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> Cela clonera le contenu du dépôt localement.
>
> ![](./media/image2.png)

4.  Accédez au répertoire du projet en exécutant la commande ci-dessous.

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### Tâche 2 : Se connecter à l'aide d'Azure CLI

1.  À partir du shell du cloud, exécutez la commande ci-dessous.

**az login**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image3.png)

2.  Ouvrez l'URL dans la console et tapez le code dans le navigateur.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image4.png)

3.  Sélectionnez les informations d'identification de **Azure login**.

> ![Une capture d'écran d'un téléphone Description générée
> automatiquement avec un niveau de confiance moyen](./media/image5.png)

4.  Cliquez sur **Continue**.

> ![Une capture d'écran d'une erreur informatique Description générée
> automatiquement avec un niveau de confiance moyen](./media/image6.png)

5.  Fermez le navigateur et revenez au portail Azure.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image7.png)

6.  Les détails de connexion sont affichés dans Cloud shell.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image8.png)

7.  Définissez par défaut votre environnement sur le **Resource Group
    attribué**.

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **Exercice 2 : Exécuter des tâches pour l'entraînement du modèle et la création du tableau de bord RAI**

1.  Exécutez la commande ci-dessous pour inscrire le **jeu de données
    d'entraînement (training dataset)** dans le workspace Azure Machine
    Learning.

> **az ml data create -f cloud/train_data.yml**

La ressource de données est créée et les détails sont affichés sur le
cloud shell.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image10.png)

2.  Exécutez la commande ci-dessous pour inscrire le **jeu de**
    **données de test (testing dataset)** dans l'espace de travail Azure
    Machine Learning.

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  Créez une **instance de calcul (compute instance)** pour exécuter
    les tâches. Ensuite, copiez le nom du calcul (par exemple,
    ***compute-xxxxxxxxxxxx)*** à la fin de l'exécution pour l'utiliser
    ultérieurement.

- Exécutez la commande ci-dessous pour **create** le **compute**.

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image12.png)

4.  Dans le menu Cloud Shell, cliquez sur le volet **Open editor { }**
    pour modifier certains fichiers.

> ![Ouvrir l'éditeur](./media/image13.png)

5.  Cliquez sur le dossier
    **RAI-Diabetes-Hospital-Readmission-classification** pour développer
    le répertoire.

![Développer le répertoire](./media/image14.png)

6.  Accédez au fichier **cloud/training_job.yml**. Remplacez ensuite
    l'espace réservé pour le nom du calcul par le **nom de votre
    instance de calcul (compute instance name)** que vous avez copié
    précédemment.

![Mise à jour du job de formation](./media/image15.png)

7.  Cliquez avec le bouton droit de la souris n'importe où dans le
    fichier, puis sélectionnez l'option **Save** pour enregistrer le
    fichier.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance moyen](./media/image16.png)

8.  Ensuite, accédez au fichier **cloud/rai_dashboard_pipeline.yml**.
    Mettez ensuite à jour l'espace réservé pour le nom du calcul avec le
    **nom de votre instance de calcul (compute instance name)** que vous
    avez copié précédemment.

![Mise à jour du gazoduc Rai](./media/image17.png)

9.  Cliquez avec le bouton droit de la souris n'importe où dans le
    fichier, puis sélectionnez l' option **Save** pour enregistrer le
    fichier.

10. Cliquez avec le bouton droit de la souris n'importe où dans le
    fichier, puis sélectionnez l' option **Quit** pour fermer la fenêtre
    de l'éditeur.

![Une capture d'écran d'un programme informatique Description générée
automatiquement avec un niveau de confiance moyen](./media/image18.png)

11. De retour à l'invite de commande Cloud Shell, soumettez le travail
    pour entraîner le modèle. Attendez que le travail mette à jour son
    état d'exécution sur **Completed** pendant la formation. Copiez le
    bloc de code ci-dessous pour ce faire.

> **run_id=$(az ml job create --name my_training_job -f
> cloud/training_job.yml --query name -o tsv)**
>
> **\# wait for job to finish while checking for status**
>
> **if \[\[ -z "$run_id" \]\]**
>
> **then**
>
> **echo "Job creation failed"**
>
> **exit 3**
>
> **fi**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **if \[\[ -z "$status" \]\]**
>
> **then**
>
> **echo "Status query failed"**
>
> **exit 4**
>
> **fi**
>
> **running=("Queued" "Starting" "Preparing" "Running" "Finalizing")**
>
> **while \[\[ ${running\[\*\]} =~ $status \]\]**
>
> **do**
>
> **sleep 8**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **echo $status**
>
> **done**
>
> **Remarque :** Si ce script n'est pas collé correctement, copiez-le et
> collez-le manuellement
>
> **Remarque :** L'exécution de ce script devrait prendre environ 3 à 5
> minutes.
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image19.png)
>
> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image20.png)

12. Si vous le souhaitez, vous pouvez vérifier l'état du travail en
    cours d'exécution à partir des **Azure Machine Learning Studio
    (**<https://ml.azure.com/>**)** -\> **Jobs**

> ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
> être incorrect.](./media/image21.png)

13. Une fois le travail d'entraînement terminé, inscrivez le modèle dans
    l'espace de travail Azure Machine Learning. Exécutez la commande
    ci-dessous pour ce faire.

**az ml model create --name rai_hospital_model --path
« azureml://jobs/$run_id/outputs/model_output » --type mlflow_model**

> Cette commande inscrit le modèle dans l'espace de travail AML et
> fournit les détails dans le cloud shell, comme dans les captures
> d'écran ci-dessous.
>
> ![Une image contenant du texte, une capture d'écran, un logiciel, un
> logiciel multimédia Description générée
> automatiquement](./media/image22.png)
>
> ![Une image contenant du texte, une police, une capture d'écran
> Description générée automatiquement](./media/image23.png)

14. Soumettez le pipeline d'offres d'emploi pour créer le **RAI
    dashboard**. Exécutez la commande ci-dessous pour ce faire.

az ml job create --file cloud/rai_dashboard_pipeline.yml

Cette commande soumet le travail et le cloud shell est alimenté par
l'étape initiale du pipeline, à savoir l'état de **Préparation
(Preparing)**.

![Une image contenant du texte, une capture d'écran, un logiciel
Description générée automatiquement](./media/image24.png)

![Une image contenant du texte, une capture d'écran, un logiciel, une
police Description générée automatiquement](./media/image25.png)

15. Connectez-vous à **Azure Machine Learning Studio** à
    l'https://ml.azure.com/ pour surveiller le travail de pipeline pour
    la création du tableau de bord RAI.

16. Sélectionnez **Pipelines**. Pour afficher la progression de la tâche
    de pipeline en créant le tableau de bord RAI, cliquez sur la tâche
    **Display name**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image26.png)

17. L'expérience sera à l’état **Running**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image27.png)

18. Le statut passe à **Completed** une fois que l'opération est
    terminée et que le tableau de bord RAI est créé.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image28.png)

19. Cliquez sur l'onglet **Models** dans le volet de navigation de
    gauche. Cliquez ensuite sur le nom du modèle pour ouvrir la page de
    détails.

> ![](./media/image29.png)

20. Sélectionnez l'option **Responsible AI** dans le menu supérieur.

> ![Une capture d'écran d'un ordinateur Le contenu généré par l'IA peut
> être incorrect.](./media/image30.png)

21. Vous êtes maintenant prêt à commencer à utiliser **RAI dashboard**.

## **Exercice 3 : Analyse des erreurs :**

La section Analyse des erreurs du tableau de bord RAI permet de fournir
une distribution des erreurs des groupes de caractéristiques contribuant
au taux d'erreur du modèle. Les erreurs ne sont souvent pas réparties
uniformément entre les différents sous-groupes de données, et l'analyse
des erreurs vous aide à identifier les entités présentant les taux
d'erreur les plus élevés.

### Tâche 1 : Trouver les erreurs de modèle :

Dans cette tâche, nous allons explorer comment utiliser l'analyse
d'erreurs pour rechercher des erreurs dans le modèle formé afin
d'identifier où se trouvent les erreurs. En outre, nous allons apprendre
à créer des cohortes de données pour déterminer pourquoi un modèle
fonctionne mal dans certaines cohortes et pas dans d'autres.

1.  Cliquez sur le nom **Diabetes Hospital Readmission.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image31.png)

2.  Sélectionnez l'icône **Compute**.

![](./media/image32.png)

#### **Tâche 1.1 : Identifier et créer une cohorte pour le chemin d'arborescence avec le plus d'erreurs**

Pour commencer l'analyse, vous pouvez observer que le nœud racine montre
que sur 994 données de test totales, 168 prédictions incorrectes ont été
trouvées lors de l'évaluation du modèle.

1.  Trouvez le chemin d'accès à l'arborescence avec le plus grand nombre
    d'erreurs. Plus la teinte rouge du nœud est foncée, plus le taux
    d'erreur est élevé.

2.  Dans notre cas, le chemin de l'arbre avec la couleur rouge la plus
    foncée est le nœud feuille qui est le deuxième en partant du bas à
    droite.

![](./media/image33.png)

3.  **Double-click** sur ce **nœud (node)** pour sélectionner
    l'**intégralité du chemin (entire path)** menant au nœud. Cela met
    en surbrillance le chemin d'accès et affiche la condition de
    fonctionnalité pour chaque nœud du chemin d'accès.

4.  Créez une cohorte à partir du chemin sélectionné en cliquant sur le
    bouton **Save as a new cohort** dans le coin supérieur droit de la
    section Analyse des erreurs.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image34.png)

5.  Entrez le nom de la cohorte **(Cohort name)** sous la forme
    **+++Err : Prior_Inpatient \>0 ; Num_meds \>11,50 \< = 21,50+++**

**Cliquez sur Save.**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image35.png)

#### **Tâche 1.2 : Identifier et créer une cohorte pour le chemin d'arborescence avec le moins d'erreurs**

À des fins de contraste, créez une autre cohorte avec le chemin
d'arborescence avec le moins d'erreurs pour voir si nous pouvons obtenir
des informations sur les raisons pour lesquelles le modèle fonctionne
bien dans une cohorte par rapport à une autre. Le **nœud terminal** avec
la condition de caractéristique **num_lab_procedures ≤ 56.50*,*** à
l'extrême gauche de l'arbre, est le chemin de l'arbre avec le moins
d'erreurs.

1.  **Double-click** sur le nœud.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image36.png)

2.  Cliquez sur **Save as a new cohort**. Le **filtre (Filter)** de ce
    jeu de données est le suivant : num_lab_procedures \< = 56,50,
    number_diagnoses \< = 6,50, prior_inpatient \< = 0,00.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image37.png)

3.  **Nommez** la cohorte : **+++Prior_Inpatient = 0 ; num_diagnoses \<
    = 6,50 ; lab_procedures \< = 56,50+++** et cliquez sur **Save**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image38.png)

#### **Tâche 1.3 : Utiliser la liste des fonctionnalités pour identifier la principale caractéristique contribuant aux erreurs de modèle**

1.  Cliquez sur **Feature list**.

![](./media/image39.png)

2.  La liste est triée en fonction de la contribution des
    fonctionnalités aux erreurs. Plus une fonctionnalité est élevée dans
    cette liste, plus son importance dans les erreurs de votre modèle
    est élevée.

3.  Dans notre modèle de réadmission à l'hôpital pour diabétiques, la
    **Feature list** indique que les caractéristiques suivantes figurent
    parmi les principaux contributeurs aux erreurs du modèle.

    - Âge

    - num_medications

    - Medicare

    - time_in_hospital

    - num_procedures

    - insuline

    - discharge_destination

### Tâche 2 : Trouver des erreurs à l'aide de la carte thermique

D'après la liste des fonctionnalités, **Age** était l'un des principaux
contributeurs d'erreurs. Nous allons donc utiliser l'onglet Carte
thermique pour explorer le groupe d'âge des patients qui entraîne une
mauvaise performance du modèle.

1.  Sélectionnez **Heat map** sous **Error Analysis**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image40.png)

2.  Sous l'onglet Carte thermique, sélectionnez **Âge** dans le menu
    déroulant **Rows: Feature 1** pour voir quel facteur cela joue dans
    les erreurs du modèle.

3.  Après avoir sélectionné l'**Age**, nous pouvons voir comment le
    tableau de bord dispose d'une intelligence intégrée pour diviser la
    fonctionnalité en différentes cellules avec les conditions
    possibles.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image41.png)

2.  **Passez (Hover)** votre souris sur chaque cellule, vous pouvez voir
    le nombre de prédictions correctes et incorrectes, la couverture des
    erreurs et le taux d'erreur pour le groupe de données représenté
    dans la cellule.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image42.png)

3.  La cellule avec **Over 60 years** a **536** prédictions de modèle
    correctes et **126** incorrectes. La couverture d'erreurs est de
    **73,81 %** et le taux d'erreur **de 18,79 %**

4.  La cellule avec **30-60 years** a **273** prédictions de modèle
    correctes et **25** prédictions incorrectes. La couverture d'erreur
    est de **25,60 %** et le taux d’erreur de **13,61 %.**

5.  La cellule de **30 years or younger** a **17** prédictions de modèle
    correctes et **1** prédiction incorrecte**.**

> Étant donné que notre observation montre que **L’âge** joue un rôle
> important dans les prédictions erronées du modèle, nous allons créer
> des cohortes pour chaque groupe d'âge pour une analyse plus
> approfondie dans le prochain laboratoire.

#### ***Tâche 2.1 : Créer des cohortes en fonction des groupes d'âge***

1.  Cliquez sur la case de pourcentage de la cellule **Plus de 60 ans**.
    Vous verrez une bordure bleue autour de la cellule carrée.

2.  Cliquez sur **Save as a new cohort**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image43.png)

3.  Dans la boîte de dialogue Enregistrer en tant que nouvelle cohorte,
    entrez

    - Cohort name - **+++Age==Over 60 year+++**

Cliquez sur **Save**.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image44.png)

4.  Répétez les étapes 2 et 3 pour créer une cohorte pour chacune des
    deux autres cellules Age.

- **Cohort \#4:** Name - **+++Age == 30–60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### Tâche 3 : Afficher les listes de cohortes

1.  Cliquez sur l'icône d'engrenage **Settings** dans le coin supérieur
    droit de la section Analyse des erreurs.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image45.png)

2.  Cela ouvrira un volet **Cohort Settings** **window pane** de cohorte
    avec la liste de toutes les cohortes que vous avez créées.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image46.png)

## Exercice 4 : Utilisation de RAI pour effectuer une analyse de modèle

Dans cet atelier, nous allons explorer comment utiliser la section
**Model Overview** du tableau de bord Azure Responsible AI (RAI). Nous
utiliserons les cohortes créées à partir du laboratoire d'analyse
d'erreurs pour déterminer pourquoi le comportement du modèle est
meilleur dans une cohorte par rapport à une autre.

## **Exercice 4.1 : Présentation du modèle**

### Tâche 1 : Examiner et comparer le tableau des indicateurs de performance du modèle

1.  Faites défiler la page sous l'analyse des erreurs pour trouver la
    section Model Overview.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image47.png)

2.  Sous Model Overview, sélectionnez le volet **Dataset Cohorts**. Cela
    affiche les différentes cohortes créées dans un tableau avec les
    métriques du modèle.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image48.png)

3.  Comparer la cohorte avec le plus d'erreurs **Err: Prior_Inpatient \>
    0; Num_Meds \> 11 and ≤ 21.50** verse the least errors
    **Prior_inpatient = 0; num_diagnose ≤ 6.50; lab_procedures \<
    56.50..**

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image49.png)

4.  Passez la souris sur la boîte à moustaches sur le graphique pour
    voir les détails de la mesure.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image50.png)

5.  Observez que le score de précision pour la **erroneous cohort** est
    de 0,806, ce qui est mauvais. Le taux **de faux positifs** est
    **très faible** et la valeur de **faux négatifs** est **élevé**.
    Cela signifie que la majorité des patients que le modèle prédit ont
    un taux élevé de prédiction des patients qui ne seront pas réadmis
    comme réadmis dans 30 jours à l'hôpital.

> ![Une ligne rouge dans une feuille blanche Description générée
> automatiquement](./media/image51.png)

6.  Ensuite, examinez les mesures de la **cohort** avec le **moins
    d’erreurs** a un score de précision de 0,94, ce qui est bien
    meilleur que le score de précision global du modèle avec toutes les
    données. Cependant, cette cohorte a également un faible taux **de
    faux positifs** à **0**.

![Une image contenant du texte, une capture d'écran, une ligne, un
numéro Description générée automatiquement](./media/image52.png)

### Tâche 2 : Examiner le graphique de distribution des probabilités

1.  Faites défiler vers le bas pour voir la **distribution des
    probabilités (Probability distribution)**.

2.  Le graphique de distribution des probabilités montre la probabilité
    du modèle, prédisant si les patients des cohortes seront réadmis ou
    non réadmis à l'hôpital dans les 30 jours.

3.  Comparez la probabilité que les patients ne soient pas réadmis pour
    les 3 cohortes.

4.  Vous verrez que la cohorte **de toutes les données (All data)** avec
    l'ensemble de données de test de tous les patients montre que la
    majorité des patients ne seront pas réadmis à l'hôpital dans les 30
    jours, avec une probabilité médiane de patients non réadmis à 0,854
    et un quartile supérieur à 0,986, ce qui est bien.

5.  Ensuite, la cohorte avec le taux d'erreur le plus élevé : ***Err :
    Prior_Inpatient \>0 ; Num_meds \>11,50 & \<= 21,50***, montre une
    probabilité légèrement plus faible à 0,89 et une médiane à 0,719.

6.  Enfin, la cohorte ayant le moins d'erreurs : ***Prior_Inpatient =
    0* ;*num_diagnoses \<= 6,50* ; *lab_procedures \<= 56,50***,
    montrent une probabilité que les patients ne soient pas réadmis a
    une médiane de 0,90 et un quartile supérieur de 0,986.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image53.png)

7.  Pour modifier le graphique afin d'afficher la probabilité que les
    patients soient réadmis pour les 3 cohortes, cliquez sur le bouton
    **Choose Label** sur l'axe des abscisses.

8.  Sélectionnez la case d'option **Probability : Readmitted**. Dans le
    volet de la fenêtre contextuelle.

9.  Cliquez ensuite sur le bouton **Apply**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image54.png)

10. Comparer la probabilité de réadmission des patients pour les 3
    cohortes

> ![Une capture d'écran d'un graphique Description générée
> automatiquement avec un niveau de confiance
> faible](./media/image55.png)

9.  Vous voyez que la cohorte 3 a une probabilité d'être réadmise
    inférieure à 0,55. La cohorte ayant le moins d'erreurs de modèle a
    la probabilité la plus faible de 0,179. La cohorte avec le plus
    d'erreurs a la probabilité la plus élevée à 0,543.

### Tâche 3 : Examiner le graphique de visualisation des mesures

Allons maintenant mieux comprendre les performances du modèle en passant
au volet Visualisations de métriques.

1.  Cliquez sur l'onglet Visualisations de mesures.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image56.png)

2.  Pour choisir une autre mesure, cliquez sur **Choose metric** sur
    l'axe des x pour choisir **Precision score** dans la liste des
    autres mesures disponibles. Cliquez ensuite sur le bouton **Apply**.

> **Remarque** : Étant donné que le modèle entraîné est un problème de
> classification, le tableau de bord RAI n'affichera que les mesures de
> classification.
>
> ![](./media/image57.png)

3.  En examinant le graphique, vous verrez que les performances du
    modèle pour toutes les cohortes de données de test et les cohortes
    erronées sont correctes dans ~70 % des cas.

4.  Le taux de **score de précision (Precision score)** pour la cohorte
    la **moins erronée (least erroneous cohort)** est **de 0,94** pour
    les patients sans hospitalisation antérieure et le nombre de
    diagnostics est inférieur à 7. Ceci est cohérent avec le score de
    précision.

![Une capture d'écran d'un ordinateur Description générée
automatiquement avec un niveau de confiance moyen](./media/image58.png)

5.  Enfin, remplacez la mesure par **Recall** pour voir dans quelle
    mesure le modèle a pu prédire correctement que les patients des
    cohortes seront réadmis à l'hôpital dans 30 jours.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image59.png)

6.  Le rappel montre que la **prédiction du modèle (model's
    prediction)** était **correcte moins de 25 % (correct less than
    25%)** du temps pour toutes les cohortes de patients réadmis. Cela
    révèle que les prédictions du modèle ne sont pas correctes la
    plupart du temps lorsqu'il s'agit de prédire les patients qui seront
    réadmis dans les 30 jours.

![Une capture d'écran d'un graphique Description générée automatiquement
avec un niveau de confiance faible](./media/image60.png)

### Tâche 4 : Examiner la matrice de confusion

La matrice de confusion est utile pour vérifier le taux du modèle en
faisant correctement la bonne prédiction. Cela révélera dans quelle
mesure le modèle apprend dans les cas où le patient est réadmis à
l'hôpital dans les 30 jours par rapport aux cas non réadmis.

1.  Cliquez sur l'onglet **Confusion matrix**.

&nbsp;

2.  Vous observerez que le **model** fonctionne **mieux** avec les
    patients qui ne sont **pas Readmitted** par rapport aux
    **Readmitted**.

3.  Le nombre de faux négatifs doit être inférieur à celui de vrais
    négatifs. Cela signifie que sur toutes les données des patients, le
    modèle n'a pu prédire correctement que 24 patients seraient réadmis
    à l'hôpital dans \< 30 jours.

- Le nombre de vrais positifs (TP) est : **802**

- Le nombre de faux négatifs (FN) est de : **159**

- Le nombre de faux positifs (FP) est de : **9**

- Le nombre de vrais négatifs (TN) est de : **24**

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image61.png)

## **Exercice 2 : Cohorte de fonctionnalités**

Étant donné que la cohorte avec l'erreur la plus élevée a des patients
avec un nombre de Prior_Inpatient \> *0* jours et un nombre de
médicaments compris entre 11 et 22 était l'endroit où le modèle avait un
taux d'erreur plus élevé, un examen plus approfondi des
*Prior_Inpatient* et des *Num_medications* aidera à isoler où il y a des
problèmes. Pour cet atelier, nous n'analyserons que *Prior_Inpatient*.

1.  Cliquez sur l'onglet **Feature Cohorts**.

2.  Dans le menu déroulant **Feature(s),** faites défiler la liste et
    cochez la **case prior_inpatient**. Cela affichera 3 cohortes de
    fonctionnalités différentes et les mesures de performance du modèle.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement](./media/image62.png)

3.  La cohorte **prior_inpatient *\< 3*** a un échantillon de **943**
    personnes. Cela signifie que la majorité des patients dans les
    données de test ont été hospitalisés moins de 3 fois dans le passé.
    Le **taux de précision du modèle (model's accuracy rate)** pour
    cette cohorte est **de 0,838**, ce qui est bien.

4.  Seuls 39 patients des données de test appartiennent à la cohorte
    ***prior_inpatient ≥ 3 et \< 6***. Le taux de précision du modèle
    est de **0,692**, ce qui n'est pas bon.

5.  Enfin, seuls 12 patients d'après les données du test ont une
    hospitalisation antérieure supérieure ou égale à 6 jours. La
    **précision du modèle (model accuracy)** de **0,75** pour cette
    cohorte est correcte.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image63.png)

### Tâche 1 : Distribution de probabilité de caractéristique

À l'instar de la cohorte de l'ensemble de données, vous avez la
possibilité d'afficher la « distribution des probabilités ».

1.  Vous pouvez voir que moins le nombre d'hospitalisations
    prior_inpatient du patient diabétique est élevé, plus il est
    probable que le patient ne sera pas réadmis dans les 30 jours.

![Une capture d'écran d'un ordinateur Description générée
automatiquement](./media/image64.png)

### Tâche 2 : Visualisations des métriques de fonctionnalité

1.  Sélectionnez **Metrics visualization**. Sur l'axe des x, cliquez sur
    le bouton **Choose metric**. Sélectionnez ensuite la mesure **Score
    de précision (Precision score)**.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image65.png)

2.  Vous voyez que le score de précision pour les patients avec
    **prior_inpatient \< 3** est de 0,40, ce qui est très mauvais. Cela
    signifie que de toutes les prédictions faites par le modèle,
    seulement 40 % étaient correctes pour cette cohorte.

> ![Un graphique à barres bleu et blanc Description générée
> automatiquement](./media/image66.png)

3.  Le score de précision pour les 2 autres cohortes est bon.

4.  Ensuite, sélectionnez **Recall score** pour l'axe x.

> ![Une capture d'écran d'un ordinateur Description générée
> automatiquement avec un niveau de confiance
> moyen](./media/image67.png)

5.  Au contraire, vous verrez que le score de rappel pour les patients
    avec **prior_inpatient \< 3** est de 0,013. Cela signifie que pour
    la majorité des patients dans les données de test, le modèle a du
    mal à prédire correctement si le patient sera réadmis dans les 30
    jours ou non.

> ![Une image contenant une capture d'écran, un logiciel, une ligne, du
> texte Description générée automatiquement](./media/image68.png)
>
> **Résumé**
>
> Cet atelier montre à quel point les mesures de performance
> traditionnelles des modèles (par exemple, la précision, la
> mémorisation, la matrice de confusion, etc.) sont toujours très
> importantes. En combinant les informations RAI et les mesures de
> performance traditionnelles, le tableau de bord nous offre un outil
> holistique pour analyser et déboguer le modèle à un niveau plus
> granulaire.
