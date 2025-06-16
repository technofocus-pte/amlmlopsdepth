# **ラボ 04 – Azure Machine Learning Studio でコード不要の AutoML を使用して分類モデルをトレーニングする**

**目的**

このラボでは、Azure Machine Learning Studio の Azure Machine Learning
Automated ML を用いて、コード不要の AutoML
で分類モデルをトレーニングする方法を学習します。この分類モデルは、顧客が金融機関の定期預金に加入するかどうかを予測します。Automated
Machine Learning
は、アルゴリズムとハイパーパラメータの様々な組み合わせを迅速に反復処理し、選択した成功指標に基づいて最適なモデルを見つけるのに役立ちます。

想定所要時間 – 60 分

現在、Azure Machine Learning
のモデルデプロイフェーズにいます。![](./media/image1.png)

## **エクササイズ1: Create an Azure Machine Learning workspace**

1\. \[Resources\] タブの資格情報を使用して、Azure Portal
(+++https://portal.azure.com+++) にサインインします。

2\. Azure Portal のホーム ページで、\[+Create a resource\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.リソースの作成で、検索バーを使用して「+++Azure Machine
Learning+++」を検索します。MarketplaceでAzure Machine
Learningを選択します。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)
>
> 4\.
> 「Marketplace」の下で、「Create」ドロップダウンをクリックし、「Azure
> Machine Learning」を選択します。![A screenshot of a software
> Description automatically generated](./media/image4.png)
>
> 5\. 新しいワークスペースを構成するには、次の情報を入力します。

- **Subscription**: 割り当てられたAzureサブスクリプションを選択します

- **Resource group**: 割り当てられたリソースグループを選択します

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

**Workspace Details:**

- **Workspace name: +++Azuremlws@lab.Lab InstanceId+++**

&nbsp;

- **Region**: • 地域を選択 ここではNorth Central USが使用されています

- **Container registry: Create newを選択します. +++Azuremlcr@lab.Lab
  InstanceId**+++ を入力します

![A screenshot of a computer Description automatically
generated](./media/image6.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

６. ワークスペースの構成が完了したら、\[Review + Create\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image8.png)

７. 検証に合格したら、「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

8\. 「Go to
resource」をクリックして、新しいワークスペースを表示します。![A
screenshot of a computer Description automatically
generated](./media/image10.png)

9\. Microsoft.MachineLEarningServices | 概要ページで、Azure Machine
Learning Studio でモデルを操作するの下の \[Launch studio\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image11.png)

## **エクササイズ2: 自動MLジョブを作成する**

1\. Azure Machine Learning Studio タブに移動します。

2\. 左側のペインで、「Authoring」セクションの「Automated
ML」を選択します。

3\. 「+ New Automated ML job」をクリックします。![](./media/image12.png)

### **タスク 1: Create data asset**

1\. \[Basic settings\] ページで、新しい実験名を
+++MarketingExperiment+++ と入力し、他のデフォルトを受け入れて \[Next\]
をクリックします。![](./media/image13.png)

2\. \[task type & data\] ページで、\[タスク タイプの選択\] で
\[Classification\] を選択し、\[Select task type\] で \[+ Create\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image14.png)

3\. 「Create data asset」ページで、以下の詳細を入力します。

- **Name** – +++marketingdata+++

- **Type** – **Tabular**

- **Next**をクリックします

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image15.png)

４. 「Data source」ペインで、「From local
files」を選択し、「Next」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

5\. 「Destination storage
type」で、ワークスペース作成時に自動的に設定されたデフォルトのデータストア（workspaceblobstore）を選択します。データファイルをこの場所にアップロードすると、ワークスペースで使用できるようになります。「Next」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

6\. 「File or folder selection」で、「Upload files or folder \> Upload
files」を選択します。C:/Labfiles から bankmarketing_train.csv
ファイルを選択します。「Next」を選択します。![A screenshot of a computer
Description automatically generated](./media/image18.png)

> 7.アップロードが完了すると、ファイルの種類に応じてデータプレビューエリアにデータが表示されます。設定フォームでデータの値を確認し、「Next」を選択します。

[TABLE]

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

8\.
スキーマフォームでは、この実験で使用するデータの詳細設定が可能です。この例では、day_of_week
のトグルスイッチをオンにして、データに含めないようにします。「Next」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

> 9.レビューフォームで情報を確認し、「Create」を選択してデータ資産の作成を完了します。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10\. 「Create a new Automated ML
job」ページに戻ると、データアセット作成の成功メッセージが表示されます。作成したマーケティングデータデータアセットを選択し、「Next」をクリックします。

注:
マーケティングデータが表示されない場合は、「更新」をクリックしてリストに表示してください。![A
screenshot of a computer Description automatically
generated](./media/image22.png)

### **タスク 2: ジョブの構成**

1.  タスク設定ページで、予測対象のターゲット列としてy（文字列）を選択します。この列は、顧客が定期預金に加入したかどうかを示します。

2.  「View additional configuration
    settings」を選択し、以下のフィールドに入力します。これらの設定は、トレーニングジョブをより適切に制御するためのものです。それ以外の場合は、実験の選択とデータに基づいてデフォルトが適用されます。

- Primary metric – AUCWeighted

- Explain best model – Enable

- Use all supported models - Enable

- Blocked models – None

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3\. \[Limits\] を選択し、\[Experiment timeout(minutes)\] フィールドに
+++60+++ と入力します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a test AI-generated content may be
incorrect.](./media/image25.png)

4\. 「Validate and test」で以下の値を入力し、「Next」をクリックします。

- Validation type - **k-fold cross-validationを選択します。**

- Number of cross validations – **2を選択します。**

![A screenshot of a computer Description automatically
generated](./media/image26.png)

５. 「Compute」ページで、「Select compute type」として「Compute
cluster」を選択し、「+ New」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

6\. 「Create compute
cluster」ペインで、以下の詳細を選択し、「Next」をクリックします。

- Location – **North Central US** (Azure Machine Learning
  ワークスペースの場所と同じ)

- Virtual machine tier – **Dedicated**

- Virtual machine type - **CPU**

- Virtual machine size -**Standard_DS12_v2**

![A screenshot of a computer Description automatically
generated](./media/image28.png)

7\. 詳細設定で以下の詳細を入力し、「Create」を選択します。

- Compute name - +++automl-compute+++

- Minimum number of nodes - 0

- Maximum number of nodes – 1

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)
>
> 8\.
> コンピューティングのプロビジョニングが成功したら、「Next」を選択します。![A
> screenshot of a computer Description automatically
> generated](./media/image30.png)
>
> 9\. \[Review\] ページで、\[Submit the training job\] を選択します。![A
> screenshot of a computer Description automatically
> generated](./media/image31.png)

10\.
実験の準備が始まると、概要画面が開き、上部にステータスが表示されます。このステータスは実験の進行に合わせて更新されます。また、実験のステータスをお知らせする通知がスタジオ内に表示されます。![A
screenshot of a computer Description automatically
generated](./media/image32.png)

> 注: トレーニングの所要時間は約 40 分です。

## **エクササイズ3: モデルを探索する**

> トレーニングの進行中は、関連するモデルを調べることができます。
>
> 1\. 「Models + child
> jobs」タブに移動して、テストされたアルゴリズム（モデル）を確認します。![A
> screenshot of a computer Description automatically
> generated](./media/image33.png)

2\. StandardScalerWrapper、XGBoostClassifier モデルを選択します。![A
screenshot of a computer Description automatically
generated](./media/image34.png)

3\. 「Metrics」をクリックし、「Metrics」タブで詳細を確認します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

> 4\.
> すべての実験モデルが完了するまで、完了したモデルのアルゴリズム名を選択してパフォーマンスの詳細を確認してください。ジョブに関する情報を確認するには、「Overview」タブと「Metrics」タブを選択してください。
>
> 重要:
> モデルのトレーニングには約40分かかります。トレーニングが完了するまで、次のラボに進んでください。ステータスが「Completed」に変わったら、このラボを再開してください。

## **エクササイズ4: モデルの説明**

モデルの説明はオンデマンドで生成できます。「説明（プレビュー）」タブに含まれるモデルの説明ダッシュボードには、これらの説明の概要が表示されます。

1\. \[Models + child jobs\] タブ (親ジョブから)
で、MaxAbsScaler、LightGBM を選択します。![A screenshot of a computer
Description automatically generated](./media/image36.png)

2\. 「Explain model」タブを選択します。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

> 3\. 開いたモデルの説明ペインで、

1.  Select compute type - **Compute cluster**

2.  Select AzureML compute instance - Select **automl-compute**

「Create」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4\.
成功メッセージが表示されます。「説明（プレビュー）」タブを選択します。このタブは、説明可能性の実行が完了すると表示されます。![A
screenshot of a computer Description automatically generated with medium
confidence](./media/image39.png)

5\.
左側のペインを展開します。「Features」の下にある「raw」と表示されている行を選択します。「Aggregate
feature
importance」タブを選択します。このグラフには、選択したモデルの予測に影響を与えたデータ特徴が表示されます。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

この例では、期間がこのモデルの予測に最も影響を与えるようです。

## **エクササイズ5: 最適なモデルを導入する**

自動機械学習インターフェースを使用すると、最適なモデルをWebサービスとしてデプロイできます。デプロイとは、モデルを統合して新しいデータに基づいて予測を行い、潜在的な機会領域を特定できるようにすることです。この実験では、Webサービスへのデプロイは、金融機関が潜在的な定期預金顧客を特定するための反復的でスケーラブルなWebソリューションを手に入れることを意味します。

実験の実行が完了すると、「Details」ページに「Best model
summary」セクションが表示されます。この実験では、AUCWeightedメトリックに基づいて、VotingEnsembleが最適なモデルと見なされます。

1\. 左側のペインで「Jobs」を選択し、作成した実験を選択します。![A
screenshot of a computer Description automatically generated with medium
confidence](./media/image41.png)

2\. 実験の表示名をクリックします。![](./media/image42.png)

3\. ステータスが「Completed」になっているかどうかを確認します。![A
screenshot of a computer Description automatically
generated](./media/image43.png)

> 4\. 実験の実行が完了すると、「Details」ページに「Best model
> summary」セクションが表示されます。この実験では、AUC_weighted
> 指標に基づいて、VotingEnsemble が最適なモデルと判断されています。![A
> screenshot of a computer Description automatically
> generated](./media/image44.png)

このモデルをデプロイしますが、デプロイには約20分かかりますのでご注意ください。デプロイプロセスには、モデルの登録、リソースの生成、Webサービス用の設定など、いくつかの手順が含まれます。

> 5\. VotingEnsemble を選択して、モデル固有のページを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

> 6.左上の\[Deploy\]メニューを選択し、\[Deploy to web
> service\]を選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

> 7\. \[Deploy a model\] ペインに次のように入力します。

[TABLE]

> 「Deploy」をクリックします

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image47.png)

> 8\. Model deployment is successfully
> triggeredメッセージがモデル画面に表示され、ステータスはRunningになります。

![](./media/image48.png)

9\. デプロイが完了すると、ステータスが「Completed」に変わります。![A
screenshot of a computer Description automatically
generated](./media/image49.png)

> これで、予測を生成するための運用可能な Web サービスができました。

## **エクササイズ6: リソースを削除する**

### **タスク 1: エンドポイントの削除**

1\. AML Studio の左側のペインで、「Endpoints」をクリックします。

2\.
エンドポイント「my-automl-deploy」を選択し、「Delete」をクリックします。

![](./media/image50.png)

3\. 「Delete real-time endpoint」ダイアログで「Delete」を選択します。

4\. エンドポイントが削除されると、成功メッセージが表示されます。

**概要**

このラボでは、Azure Machine Learning スタジオでコード不要の AutoML
を使用して分類モデルをトレーニングし、最適なモデルを Web
サービスとしてデプロイする方法を学習しました。
