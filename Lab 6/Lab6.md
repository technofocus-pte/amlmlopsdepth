# ラボ 06 - ハードウェアデータセットに最適な回帰モデルのトレーニング

目的

このラボでは、AutoML
を用いて回帰モデルをトレーニングする方法を学習します。ハードウェアパフォーマンスデータセットを用いてモデルをトレーニングし、推論シナリオで使用できるようにデプロイします。回帰の目標は、特定のハードウェアパーツの組み合わせにおけるパフォーマンスを予測することです。

想定所要時間 – 60分

# エクササイズ 0: 環境を準備しましょう

### **タスク 1: AML ワークスペースを起動する**

1\. Azure ポータルにログインします
(まだログインしていない場合は、+++https://portal.azure.com+++)。

2\. Azure ポータル メニューから、\[All resources\] を選択します。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3\. Azure Machine Learning ワークスペース (Azuemlws@lab.LabInstanceId)
を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4\. 「Launch studio」をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5\.
左ペインから「Compute」を選択し、コンピューティングインスタンスを作成します。「+
New」を選択します。![A screenshot of a computer Description
automatically generated](./media/image4.png)

6\. 以下の詳細を入力し、「Review + Create」をクリックします。

- Compute name - +++**auto-compute**+++

- Virtual machine type – **CPU**

- Virtual Machine – **Standard E4ds_v4**

![](./media/image5.png)

> 7.コンピューティングインスタンスを作成するには、\[Create\]
> を選択します。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

### **タスク 2: ノートブックをAML Workspaceにアップロードする**

1.  左ペインから「Notebooks」をクリックします。「Users」の下にあるユーザー名の横にある3つの点をクリックし、「Upload
    folder」を選択します。

![](./media/image7.png)

**2. \[Click to browse\] を選択してフォルダーを選択し、C:\Lab
filesを参照して automl-regression-task-hardware-performance
フォルダーを選択し、\[Upload\] をクリックします。**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image8.png)

3\. 「If you get a pop up asking Upload 3 files to this
site?」というポップアップが表示されたら、「Upload」をクリックします。

![A picture containing text, screenshot, display, font Description
automatically generated](./media/image9.png)

4\. 「I trust contents of these
files」チェックボックスを選択し、「Uplaod」を選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

5\. ノートブック（.ipynb
ファイル）「automl-regression-task-hardware-performance」を開きます。このノートブックは、先ほど作成したコンピューティングに自動的に接続されます。

![](./media/image11.png)

## **エクササイズ 1: Azure Machine Learning ワークスペースに接続する**

### **タスク 1: 必要なライブラリをインポートする**

1\. セルの左上にある \[セルの実行\] ボタンをクリックして、「1.1 Import
the required
libraries」の下の最初のセルを実行し、このラボ実行に必要なライブラリをインポートします。

2\. セルの左下にあるチェック
シンボルを確認して、実行が成功したことを確認します。

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image12.png)

### **タスク 2: ワークスペースの詳細を設定し、ワークスペースへのハンドルを取得します**

1\. 「1.2. Configure workspace details and get a handle to the
workspace」の下のセルで、以下の値を置き換えます。

• SUBSCRIPTION_ID - +++@Lab.CloudSubscription.Id+++

• RESOURCE_GROUP - 割り当てたリソースグループ名

• AML_WORKSPACE_NAME - +++Azuremlws@lab.Lab InstanceId+++

2\. セルの左上にある「Run
cell」オプションをクリックし、実行が成功すると左下にチェックマークが表示されていることを確認します。

3\. セルの下に、「Found the config file in :
/config.json」という出力が表示されます。

![](./media/image13.png)

### **タスク 3: Azure ML ワークスペース情報を表示する**

1\. 次のセル (Azure ML ワークスペース情報の表示の下のセル)
を実行します。

2\.
セルの下に出力として表示されるワークスペース、サブスクリプション、場所、リソース
グループの詳細がすべて正しいことを確認します。

![A screenshot of a computer program Description automatically
generated](./media/image14.png)

## **エクササイズ 2: 入力トレーニングデータを含む MLTable**

### **タスク 1: MLTableデータ入力を作成する**

1\. 次のセル（2.1 MLTable データ入力の作成の下のセル）を実行します。

2\. 実行が成功したことを確認します。

![A picture containing text, font, screenshot, software Description
automatically generated](./media/image15.png)

## **エクササイズ 3: AutoML 回帰トレーニング ジョブを構成して実行する**

1\. 4.1 AutoML 回帰トレーニング ジョブを構成して実行の下のセルを 1
つずつ実行し、各セルが正常に実行されることを確認します。

2\. 4.2 コマンドの実行の下のセルが AutoML ジョブを送信します。

![A screenshot of a computer program Description automatically
generated](./media/image16.png)

3\.
左側のペインからジョブをクリックし、実行中の状態にある実験を選択すると、Jobsのステータスを確認できます。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**注:** 完了するまでに約 10 ～ 15 分かかります。

4\. ノートブックの次のセルは、AutoMLジョブが終了するまで待機します。

5\. それを実行し、実行が完了するまで待ってから次のセルに移動します。

![](./media/image19.png)

6\. 実行が完了したら、次のステップに進みます。

![](./media/image20.png)

7\. 次の 2 つのセルを 1 つずつ実行して、URL とジョブ名を取得します。![A
screenshot of a computer Description automatically
generated](./media/image21.png)

## **エクササイズ 4: ベストトライアル（ベストモデルのトライアル/実行）を取得する**

1\. この演習の最初のセルの上にセルを追加します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

**2. 以下のコードをコピーし、「Run cell」をクリックします。**

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image23.png)

3\. 次の 3 つのセルを 1 つずつ実行し、各コードとその出力を分析します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image24.png)

4\. 次のセルを実行して親実行を取得します。![A screenshot of a computer
program Description automatically generated with low
confidence](./media/image25.png)

> 5.次のセルを実行して親タグを出力します。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image26.png)

> 6.次のセルを実行して、AutoML の最適な子実行を取得します。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image27.png)

7\. 次のセルを実行して、最適なモデル実行のメトリックを取得します。

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image28.png)

8\. 次の 3
つのセルを実行して、最適なモデルをローカルにダウンロードします。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

## **エクササイズ 5: 最適なモデルを登録してデプロイする**

### **タスク 1: 管理対象オンライン エンドポイントを作成する**

1\. このタスクの最初の 2 つのセルを実行します。

![](./media/image30.png)

2\. 次のセルをコードで実行します。

**ml_client.begin_create_or_update(endpoint).result()**

これにより、regression-\<Currentdate&time\> という名前のオンライン
エンドポイントが作成されます。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image31.png)

3\.
エンドポイント「regression-\<Currentdate&time\>」の更新が完了したことを示す通知を確認します。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

### **タスク 2: 最適なモデルを登録してデプロイする**

> 1\. 「Register best model and deploy -\> Register
> model」の最初のセルを実行して、hardware-performance-model
> という名前のモデルを登録します。
>
> 2\. 実行が成功したら、次のセルを実行して登録されたモデル ID
> を取得します。![](./media/image33.png)

### **タスク 3: デプロイ　**

1\. 「Deploy」の下の最初のセルで、instance_type の値を Standard_E4s_v3
に置き換えます。

2\. 次に、セルを実行して最適なモデルをデプロイします。

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

3\. 次のセルを実行してデプロイメントを作成します。

![A picture containing text, screenshot, line, font Description
automatically generated](./media/image35.png)

4\.
完了までに約40分かかります。エンドポイントのステータスからも確認できます（左ペインからエンドポイントを選択し、先ほどデプロイした
regression-XXXXXXX エンドポイントをクリックします）。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5\.
実行が完了し、デプロイメントが成功すると、セルはデプロイメントの詳細を出力します。

![](./media/image37.png)

6\.
また、エンドポイントの詳細ページで、デプロイメントのステータスが「Succeeded」になります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

7\. デプロイメントが 100%
のトラフィックを処理するように、ノートブックの次のセルを実行します。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image39.png)

8\. エンドポイントの詳細ページで、ライブ トラフィックの割り当てが 100%
になっていることを確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

## **エクササイズ 6: デプロイメントをテストする**

1\. デプロイメントのテストの下のセルを実行します。

2\. 出力を確認します。

![](./media/image41.png)

３. 残りのセルに従って実行し、エンドポイントを削除します。

![](./media/image42.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image43.png)

４. 「Endpoints」タブからエンドポイントのステータスを確認します。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

**概要**

**このラボでは、以下の方法を学習しました。**

**• Python SDK から AML ワークスペースに接続する**

**• ファクトリー関数「regression()」を使用して AutoML
回帰ジョブを作成する**

**• AutoML 回帰トレーニングジョブを送信/実行し、AmlCompute
を使用してモデルを　　　トレーニングする**

**• モデルを取得し、それを使用して予測スコアを計算する**
