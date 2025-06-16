# ラボ 02 - Azure Machine Learning のデータ ラベル付けツールを使用してラベル付けされたデータセットを作成する

**目的**

このラボでは、Azure Machine Learning Studio の Azure Machine Learning
Data Tools
を使用して、ラベルなしデータのコレクションを、トレーニング済みの物体検出モデルによって検出されるクラスを含むラベル付きデータセットに管理する方法を学習します。

想定所要時間 - 40 分

## **エクササイズ 1: Azureリソースの準備**

### **タスク 1: Azure ストレージ アカウントを作成する**

1\. Azure ポータル (+++https://portal.azure.com+++) のホーム
ページから、検索バーに「+++storage account+++」と入力し、Storage
accountsを選択します。

![](./media/image1.png)

２. 「+Create」を選択します。![A screenshot of a computer Description
automatically generated with medium confidence](./media/image2.png)

> 3\. 「Create a storage account」ページで、以下の詳細を入力します。
>
> プロジェクトの詳細
>
> • サブスクリプション – サブスクリプションを選択します。
>
> • リソースグループ – 割り当てられたリソースグループを選択します。
>
> インスタンスの詳細
>
> • ストレージアカウント名 – +++imagestoreacc@lab.LabInstance.Id +++
>
> • リージョン – AML ワークスペースを作成したリージョンを選択します。
>
> • パフォーマンス – 「Standard」を選択します。
>
> • 冗長性 – 「Locally-redundant storage (LRS)」を選択します。
>
> \[Next\] を選択します。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4\. 「Advanced」タブで、「BLOB storage」セクションの「cross-tenant
replication」オプションがオフになっていることを確認します。その他のデフォルト設定はそのままにして、「Review +
create」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

> 5\. 検証に合格したら、「Create」をクリックします。![A screenshot of a
> computer error Description automatically
> generated](./media/image5.png)

6\. デプロイが完了したら、「Go to resource」をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image6.png)

7\.
ストレージアカウント名をメモしておいてください。ラボの後半で使用します。メモを取りながら次のタスクに進んでください。![A
screenshot of a computer Description automatically
generated](./media/image7.png)

### **タスク 2: Azure ストレージ コンテナーを作成する**

1\. ストレージ アカウント ページの左側のメニューから、\[Data Storage\]
セクションまでスクロールし、\[Containers\] を選択します。![A screenshot
of a computer Description automatically generated](./media/image8.png)

2\. + コンテナーを選択します。開いた「New
container」ペインで、コンテナーの名前に「+++imagedata+++」と入力し、「作成」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

3\. コンテナを作成したら、左ペインの「Security +
networking」の下にある「Access keys」を選択します。「Access
keys」ページで、キーの値の「Show」をクリックし、キーをコピーします。コピーした値は、後で参照できるようにメモ帳に保存してください。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4\. 左側のペインから「Containers」を選択して、コンテナー
ページに戻ります。![](./media/image11.png)

5\. 新しく作成したコンテナ「imagedata」を選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6\. 「Upload」をクリックします。「Upload blob」ペインで「Browse for
files」をクリックし、C:\Labfiles の下にある train_img
フォルダを開きます。![A screenshot of a computer Description
automatically generated with medium confidence](./media/image13.png)

7\. train_img
フォルダ内のすべてのファイルを選択し、「Open」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

8\. 「Upload blob」ページで「Upload」をクリックします。

![](./media/image15.png)

9\. アップロードが完了すると、「Successfully uploaded
blob(s)」というメッセージが表示されるので、「Upload
blob」ペインを閉じます。

![](./media/image16.png)

10\. 完了すると、242 個のイメージすべてが Azure ストレージ
コンテナーに追加されたことがわかります。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **エクササイズ 2: Azure Machine Learning のデータ ラベル付けプロジェクトを作成する**

1\. Azure Machine Learning Studio のホーム ページで、左側のペインの
\[Manage\] の下にある \[Data labeling\] を選択します。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

> 2\. \[+ Create\] を選択します。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)

3\. プロジェクトの詳細セクションで、以下の詳細を入力します。

a\. プロジェクト名 - +++soda+++

b\. メディアの種類 - 画像

c\. ラベリングタスクの種類 - オブジェクト識別（バウンディングボックス）

「Next」を選択します。![A screenshot of a computer Description
automatically generated with medium confidence](./media/image20.png)

> 4\. \[Add workforce (optional)\]
> 画面で、オプションを無効のままにして、\[Next\]
> を選択して続行します。![A screenshot of a computer Description
> automatically generated with medium confidence](./media/image21.png)
>
> 5\. 「Select or create data」ページで、「+ Create」をクリックします。
>
> ![](./media/image22.png)
>
> 6\. 「Create data asset」ページの「Data
> type」ペインで、以下の情報を入力します。
>
> a\. Name – +++sodaObjects+++
>
> b\. Description – +++Image labelling+++
>
> c\. Type – ファイル
>
> 「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)
>
> 7\. データ資産の作成ページのデータ ソース ペインで、Azure
> ストレージからオプションを選択し、次へをクリックします。![A screenshot
> of a computer Description automatically
> generated](./media/image24.png)
>
> 8\. 「Create data asset」ページの「Storage type」ペインで、「Create
> new datastore」を選択します。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image25.png)

9.「New datastore」ペインで、以下の詳細を入力します。

1.  **Datastore name** – +++**sodadatastore**+++

2.  **Datastore type** – **Azure Blob Storageを選択**

3.  **Account selection method –From Azure subscriptionを選択**

4.  **Subscription ID – サブスクリプションを選択してください**

5.  **Storage account –imagestoreaccを選択**

6.  **Blob container –imagedataを選択**

7.  **Authentication type –Account Key** を選択

8.  **Account key –演習 1 で保存したアカウント キーを入力します。**

> 「Create」をクリックします。
>
> ![](./media/image26.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image27.png)

10\. 「Select a
datastore」ページに作成成功メッセージが表示されます。作成された
sodadatastore を選択します。「Next」をクリックします。

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)
>
> 11\. 「Choose a storage」で「Enter storage path
> manually」を選択し、「Storage path」に「/」と入力し、「Skip data
> validation」を有効にします。「」をNextクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)
>
> 12\. 詳細を確認し、「Create」をクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)
>
> 13\. 「Select or create
> data」ペインに戻り、「sodaObjects」を選択します。「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)
>
> 14\. 「Incremental refresh」ページで、「Enable incremental refresh at
> regular intervals」を選択します。「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)
>
> 15\. label categoriesページで、\[Add label category\] を 2
> 回クリックして、既存のカテゴリ名プレースホルダーに加えてさらに 2
> つのカテゴリ名プレースホルダーを追加します。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)
>
> 16\.
> 追加後、各ラベルカテゴリーのプレースホルダーに「+++coke+++」、「+++diet_coke+++」、「+++sprite+++」と入力します。「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)
>
> 17\. ラベル付けの指示を空白のままにして、「Next」をクリックします。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)
>
> 18\. 品質管理（プレビュー）ページで「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)
>
> 19\. 「Enable ML assisted labelling
> option」オプションを無効にして、「Create
> project」をクリックします。![A screenshot of a computer Description
> automatically generated with medium confidence](./media/image37.png)
>
> 20\.
> 成功：Sodaデータラベリングプロジェクトの作成に成功しました。データラベリング画面に「Project
> is
> initializing」というメッセージが表示されます。Sodaプロジェクトをクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

1.  **Label dataをクリックします。**

> ![](./media/image39.png)
>
> 3\. 右上のショートカット
> キーには、使用可能なさまざまなショートカットが表示されます。
>
> ![A group of soda cans on a table Description automatically generated
> with medium confidence](./media/image40.png)
>
> 4\.
> 上部のメニューバーには、利用可能なさまざまなオプションが表示されます。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)
>
> 4\.
> 最初の画像が画面に表示されます。左側の「Tags」パネルから適切なタグを選択します。画像をクリックして少しドラッグすると、画像にラベルが付けられます。「Submit」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image42.png)
>
> 5\.
> 現在の画像を送信したときに表示される次の画像に対して同じプロセスを繰り返します。少なくとも
> 10 枚の画像にラベルを付けます。
>
> ![](./media/image43.png)
>
> 6\.
> 画像の最後までアップロードが続けられます。10枚を超えたらアップロードを中止するか、すべての画像のラベル付けを完了させてください。
>
> 7\.
> 上部のナビゲーションパスにある「Soda」をクリックしてダッシュボードに戻ります。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image44.png)
>
> 8.ダッシュボードには、ラベル付けされた資産とラベルの分布に関する詳細が表示されます。
>
> ![](./media/image45.png)
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image46.png)
>
> 9\. 「Export」をクリックします。
>
> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image47.png)
>
> 10\. 「Export data」ペインで、次の項目を選択します。
>
> • Asset type - labeled
>
> • Export format - Azure ML dataset
>
> \[Submit\] をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image48.png)
>
> 11\. エクスポートが完了すると、ダッシュボードページに「Labels
> successfully
> exported」というメッセージが表示されます。成功メッセージ内のファイルリンクをクリックすると、エクスポートされたファイルの詳細が表示されます。
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image49.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image50.png)
>
> 12\. \[Datasources -\> Actions\] セクションの \[View in datastores\]
> または \[View in Azure portal\] リンクをクリックします。
>
> ![](./media/image51.png)
>
> 13\. データストアで表示します。
>
> ![A picture containing text, number, software, font Description
> automatically generated](./media/image52.png)

**概要**

このラボでは、Azure
ストレージからデータアセットを作成する方法、画像にラベルを付けてラベル付きデータセットを作成する方法を学習しました。

この一連のタスク全体は、機械学習プロジェクトワークフローの「Data:
Explore & prepare」ステージにも属します。
