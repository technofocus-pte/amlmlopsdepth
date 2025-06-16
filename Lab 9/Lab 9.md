# **ラボ 09 - GitHub で MLOps を設定する**

**目的:**

Azure Machine Learning を使用すると、GitHub Actions
と統合して機械学習のライフサイクルを自動化できます。

このラボでは、Azure Machine Learning
を使用して、線形回帰分析を実行し、ニューヨーク市のタクシー料金を予測するエンドツーエンドの
MLOps
パイプラインを構築する方法を学習します。このパイプラインは、それぞれ異なる機能を提供するコンポーネントで構成されています。これらのコンポーネントはワークスペースに登録し、バージョン管理を行い、さまざまな入出力で再利用できます。

予定所要時間: 60 分

現在、Azure Machine Learning の MLOps
フェーズにいます。![](./media/image1.png)

## **エクササイズ 1: Azureリソースの準備**

### **タスク 1: Azure Machine Learning ワークスペースを作成する**

1\. まだログインしていない場合は、+++https://portal.azure.com+++ で
Azure Portal にサインインします。

2\. Azure Portal のホーム ページで、\[+Create a resource\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image2.png)

> ３. 「Create a resource」ページで、検索バーを使用して「+++Azure
> Machine Learning+++」を検索します。
>
> ４. 「Machine Learning」を選択します。![](./media/image3.png)
>
> 5\.
> 「Marketplace」の下で、「Create」ドロップダウンをクリックし、「Azure
> Machine Learning」を選択します。![A screenshot of a computer
> Description automatically generated](./media/image4.png)
>
> 6\. 新しいワークスペースを構成するには、次の情報を入力します。

- **Subscription**: 割り当てられたAzureサブスクリプションを選択します

- **Resource group**: 割り当てられたリソース グループを選択します。

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.Lab Instance.Id**+++

- **Region**: 最寄りの地域を選択してください（ここではNorth Central
  USを選択しています）

&nbsp;

- **Container registry: Create newを選択します。+++azuremlcr@lab.Lab
  Instance.Id+++を入力します。**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7\. 検証に合格したら、「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

8\. 「Go to resource」をクリックして、新しいワークスペースを表示します。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9\. Microsoft.MachineLEarningServices | 概要ページで、Azure Machine
Learning Studio でモデルを操作するの下の \[Launch studio\]
を選択します。

![A screenshot of a software update Description automatically
generated](./media/image9.png)

### **タスク 2: コンピューティングを作成する**

![A screenshot of a computer Description automatically
generated](./media/image10.png)

1\. 「Compute clusters」タブを選択し、「+New」をクリックします。

![](./media/image11.png)

2\. 「Create compute cluster」画面で、以下の詳細を入力します。

1.  Location – Azure Machine Learning
    ワークスペースを作成したリージョンを選択します

2.  Virtual machine tier – **Dedicated**

3.  Virtual machine type – **CPU**

4.  Virtual machine size – **Standard_E4s_v3** を選択します (VM
    サイズを見つけるには、すべてのオプションから選択をチェックします)

> 「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

3\. 「Advanced Settings」ページで、以下の詳細を入力します。

1.  Compute name – +++**cpu-cluster@lab.Lab InstanceId**+++

2.  Minimum number of nodes – 0

3.  Maximum number of nodes – 1

> 「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

**注:** コンピューティングが実行状態になるまでに約 10 分かかります。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

## **エクササイズ 2: Azureリソースを取得する**

1.  Azureポータル（https://portal.azure.com）からリソースグループを開き、以下のリソースの名前をメモします。

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage account**

> そして、設定ファイルで更新されるように、メモ帳にローカルに保存します。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

## **エクササイズ 3: GitHubアカウントとリソースを準備する**

**注:** GitHub
のアカウントをまだお持ちでない場合は、ここから作成してください
+++https://github.com/+++ -\> サインアップ。

### **タスク 2: mlopsデモのリポジトリをGitHubアカウントにフォークします**

1\. ブラウザを開き、このリンクを入力します -
+++https://github.com/getazureready/mlops-v2-gha-demo+++

2\. 右上の「Fork」をクリックします。![A screenshot of a chat Description
automatically generated with medium confidence](./media/image16.png)

3\. 「Create a new fork」ページが開きます。「Create
fork」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

> 4.GitHubプロジェクトから設定を選択します

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.png)

**5. 「Secrets and variables」の下の「Actions」を選択します。**

![A screenshot of a computer Description automatically
generated](./media/image19.png)

6\. 「New repository secret」を選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

7\.
このシークレットに「+++AZURE_CREDENTIALS+++」という名前を付け、以下のサービスプリンシパルの出力をシークレットの内容として貼り付けます。このサービスプリンシパルは事前に作成されています。「Add
secret」を選択してください。

> {
>
> "clientId": "+++@ラボ .Variable(spAppId)+++",
>
>   "clientSecret": "+++@ラボ .Variable(spClientSecret)+++",
>
>   "subscriptionId": "+++@ラボ.CloudSubscription.Id+++",
>
>   "tenantId": "+++@ラボ.CloudSubscription.TenantId+++",
>
>   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
>   "resourceManagerEndpointUrl": "https://management.azure.com/",
>
>   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
>   "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
>   "galleryEndpointUrl": "https://gallery.azure.com/",
>
>   "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![A screen shot of a computer Description automatically generated with
> low confidence](./media/image21.png)

8\. 追加されたシークレット AZURE_CREDENTIALS は、リポジトリ
シークレットの下に表示されます。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

9\. 「New repository secret」をクリックします。![](./media/image23.png)

10\. 以下の詳細を入力してください。

1.  Name – +++ARM_CLIENT_ID+++

2.  Secret – +++@lab.Variable(spAppId)+++

> ![A screenshot of a computer secret Description automatically
> generated with low confidence](./media/image24.png)

11\. 次の値に対して手順 9 と 10 を繰り返して、追加の GitHub
シークレットを作成します。

- +++ARM_CLIENT_SECRET+++ - +++@lab .Variable(spClientSecret)+++

- +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

- +++ARM_TENANT_ID+++ - +++@lab.CloudSubscription.TenantId+++

## **エクササイズ 4: 機械学習環境パラメータを構成する**

1\. シークレット ページから、左上にある GitHub ID の横にある
mlops-v2-gha-demo をクリックして、リポジトリ
ページに移動します。![](./media/image25.png)

2\.
ルートにあるconfig-infra-prod.ymlファイルを選択し、「Edit」（鉛筆アイコン）をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image26.png)

> 3\. 値を変更する

6.  **Namespace** –
    **mlopsliteXX**（XXをランダムな数字に置き換えてください）

7.  **Postfix** – **c**

8.  **location** – **Same as your workspace region**

> 「Commit changes」をクリックします。
>
> 「pipeline reference」セクションで、Azure リソースの値を、演習 2
> で取得して保存した値に置き換えます。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

4\. 変更のコミット ペインで変更のコミットをクリックします。![A
screenshot of a computer Description automatically generated with medium
confidence](./media/image28.png)

5..github/workflows から deploy-model-training-pipeline-classical.yml
を開きます。編集（鉛筆アイコン）をクリックします。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

> 6\. ファイルの内容で、Size の値を +++Standard_E4s_v3+++
> に置き換えます。
>
> 「変更をコミット」を選択します。 ![A screenshot of a computer
> Description automatically generated with medium
> confidence](./media/image30.png)

7\. mlops/azureml/deploy/online から online-deployment.yml
ファイルを開きます。「Edit」（鉛筆アイコン）をクリックします。

![](./media/image31.png)

8\. instance_typeの値を+++Standard_E4s_v3+++に置き換えます。「Commit
changes」をクリックします。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image32.png)

> 9\. github/workflows にある tf-gha-deploy-infra.yml
> ファイルを開きます。「Edit」をクリックし、9行目と14行目の「Azure」を「+++CoursesTF+++」に置き換えます。
>
> \[Commit changes\]を選択します。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

10\. 上部のメニューバーから「Actions」を選択します。「I understand my
workflows」をクリックし、有効化します。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

11\. プロジェクトに関連付けられた定義済みの GitHub
ワークフローが表示されます。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

## **エクササイズ 5: 機械学習インフラストラクチャを導入する**

**1. tf-gha-deploy-infra.yml
を選択します。「Runworkflow」をクリックします。**

**• ブランチ – main を選択します。**

**「Run workflow」を選択します。**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image36.png)

2\. これにより、GitHub Actions と Terraform
を使用して機械学習インフラストラクチャがデプロイされます。

3\. ジョブのステータスを追跡し、実行が成功したことを確認します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

**注:** このワークフローは完了までに約5分かかります.

## **エクササイズ 6: モデルトレーニングパイプラインのデプロイ**

次に、モデルトレーニングパイプラインを新しい機械学習ワークスペースにデプロイします。

このパイプラインは、コンピューティングクラスターインスタンスを作成し、必要な
Docker イメージと Python
パッケージを定義するトレーニング環境を登録し、トレーニングデータセットを登録し、前のセクションで説明したトレーニングパイプラインを開始します。 

1\. tf-gha-deploy-infra.yml ワークフロー ページから、\[Actions\]
をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

> 2.プロジェクトに関連付けられた定義済みのGitHubワークフローが表示されます。リストからdeploy-model-training-pipelineを選択してください。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

3\. 「Run workflow -\> Run workflow」をクリックします。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image40.png)

4\. 開始したばかりのパイプラインをクリックして、進行状況を追跡します。

![A picture containing text, software, web page, font Description
automatically generated](./media/image41.png)

5\. このパイプラインが完了するまでに約 15 ～ 45 分かかります。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image42.png)

6\. パイプラインの実行が成功したスクリーンショットを以下に示します。

![](./media/image43.png)

7\. この実行により、モデルが機械学習ワークスペースに登録されます。

8\. https://ml.azure.com/ にあるAzureMachineLearning
Studioにログインし、左側のペインで「Data」をクリックして、タクシーデータが追加されていることを確認します。これは、ワークフローのregister-datasetジョブの一部として実行されます。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

9\.
左ペインから「Jobs」をクリックし、「taxi-fare-training」を選択します。これはワークフローのrun-pipelineジョブで実行されます。

![](./media/image45.png)

10\. 最新の実行の表示名を選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

11\. トレーニングに含まれる段階と詳細を確認します。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

トレーニング済みのモデルが機械学習ワークスペースに登録されたら、スコアリング用にモデルをデプロイする準備が整います。

**概要**

このラボでは、Azure Machine Learning を使用してエンドツーエンドの MLOps
パイプラインを設定し、データを準備してモデル トレーニング
パイプラインを新しい Machine Learning
ワークスペースにデプロイする方法を学習しました。
