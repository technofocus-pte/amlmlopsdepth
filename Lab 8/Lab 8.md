# ラボ 08 – プロンプトフローを使用して RAG で QA データ生成を実装する

**目的:**

QAデータ生成は、RAG（Retrieval Augmented
Generation）作成プロセスの一部であり、自動生成されたQAデータセットを使用して、RAGに最適なプロンプトとRAGの評価指標を取得します。

このラボでは、データからQAデータセットを作成する方法を学習します。

想定所要時間：60分

## エクササイズ 1: AOAIデプロイメントを作成する

この演習では、前のラボで作成したAzure
OpenAIリソースを使用して、gpt-35-turboモデルのデプロイメントを作成します。

1\. Azure Machine Learning Studioの左側のペインから「Model
Catalog」を選択します。「+++gpt-35-turbo+++」を検索し、モデルリストからgpt-35-turboを選択します。![A
screenshot of a computer Description automatically
generated](./media/image1.png)

2\. Azure OpenAIリソースフィールドでAOAIリソース（AOAI-PF@lab.Lab
InstanceId）が選択されていることを確認します。「Deploy」を選択してモデルをデプロイします。![A
screenshot of a computer Description automatically
generated](./media/image2.png)

3\. デプロイメント名を承認し、「Deploy」を選択します。![A screenshot of
a computer Description automatically generated](./media/image3.png)

4\. デプロイメント名を +++text-embedding-ada-002-2+++
として、text-embedding-ada-002 のモデルデプロイメントを繰り返します。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## エクササイズ 2: 環境をセットアップする

1\.
Studioの左側のパネルから「Notebooks」を選択します。ユーザー名の横にある3つの点をクリックし、「Upload
files」を選択します。![A screenshot of a computer Description
automatically generated](./media/image5.png)

2\. C:\Lab Files を参照し、qa_data_generation.ipynb
ファイルを選択します。「I trust contents of this file
checkbox」チェックボックスをオンにして、「Upload」をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image6.png)

3\. ノートブックを開き、「Compute」オプションで「Serverless Spark
Compute」を選択します。![A screenshot of a computer program Description
automatically generated](./media/image7.png)

4\. コンピューティングが接続されたら、「Configure session」を選択して
conda.yml
ファイルをアップロードし、それを使用して実行環境を設定します。![A
screenshot of a computer Description automatically
generated](./media/image8.png)

5\. Python パッケージを選択 -\> Conda ファイルをアップロード -\>
参照をクリックします。![A screenshot of a computer Description
automatically generated](./media/image9.png)

6\. C:\Lab Files から conda.yml を選択し、「Apply」を選択します。

> ![A screenshot of a computer program Description automatically
> generated](./media/image10.png)

## エクササイズ 3: AzureML ワークスペースのクライアントを取得する

1\.
ノートブックの最初のセルを実行して依存関係をインストールします![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

**注:** 完了するまでに10～15分かかります

2\. az login で次のセルを実行して、Azure CLI にログインします。![A
screenshot of a computer Description automatically
generated](./media/image13.png)

1\. ワークスペースはAzure Machine Learningの最上位リソースであり、Azure
Machine
Learningの使用時に作成するすべての成果物を一元的に操作できる場所を提供します。このセクションでは、ジョブが実行されるワークスペースに接続します。MLClientはAzureMLと対話するための手段です。

2\.
次のセルで、サブスクリプションIDを+++@ラボ.Subscription()+++、リソースグループをリソースグループ名、Azure
MLワークスペースを+++Azuremlws@lab.Lab
InstanceId+++に置き換えて、MClientを作成します。 ![A screenshot of a
computer Description automatically generated](./media/image14.png)

![A screenshot of a computer Description automatically
generated](./media/image15.png)

3\.
次のセルを実行して接続名を設定します。接続の作成時に別の名前を使用した場合は、このセルにその値を入力して実行してください。![A
screenshot of a computer Description automatically
generated](./media/image16.png)

4\. キー値を Azure openAI キーに置き換え、ターゲット値を先ほど保存した
Azure OpenAI リソースのエンドポイント値に置き換えます。

値を置き換えた後、セルを実行します。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

5\. ワークスペースがAzure
OpenAIに接続されたので、gpt-35-turboモデルがデプロイされ、推論の準備が整っていることを確認します。

6\.
次のセルを実行して、モデル名とデプロイメント名を設定します。モデルとデプロイメントの作成時に異なる名前を付けた場合は、モデル名とデプロイメント名の値を置き換えてください。![A
screenshot of a computer code Description automatically
generated](./media/image18.png)

7\.
最後に、デプロイメント情報とモデル情報を、AzureML埋め込みコンポーネントが入力として期待するURI形式に結合します。次のセルを実行してこれを実行します。![A
screenshot of a computer Description automatically
generated](./media/image19.png)

## エクササイズ 4: セットアップ・パイプライン

AzureML
パイプラインは複数のコンポーネントを接続します。各コンポーネントは入力と、その入力を使用するコード、そしてそのコードから生成される出力を定義します。パイプライン自体にも入力があり、個々のサブコンポーネントを接続することで出力が生成されます。埋め込みとインデックス作成のためにデータを処理するために、ワークフローの各ステップを実行する複数のコンポーネントを連結します。

コンポーネントはレジストリ azureml
に公開されます。このレジストリにはデフォルトでアクセスでき、どのワークスペースからでもアクセスできます。以下のセルでは、azureml
レジストリからコンポーネント定義を取得します。

1\. 次のセルを実行し、問題なく実行されることを確認します。![A screenshot
of a computer code Description automatically
generated](./media/image20.png)

２.
各コンポーネントには、コンポーネントの目的と各入出力の全体的な説明を提供するドキュメントがあります。例えば、コンポーネント定義を調べることで、data_generation_component
が何をするかを理解できます。次のセルを実行し、出力を確認してください。![A
screenshot of a computer Description automatically
generated](./media/image21.png)

３.
以下では、上記のコンポーネントの入力と出力を連結するPython関数を定義することで、パイプラインを構築します。関数の引数はパイプライン自体への入力であり、戻り値はパイプラインの出力を定義する辞書です。次のセルが正常に実行されることを確認してください。![A
screenshot of a computer code Description automatically
generated](./media/image22.png)

![A screenshot of a computer program Description automatically
generated](./media/image23.png)

４. 以下の設定は、さまざまな git および data_source
パラメーターを設定して、より大きな AzureDocs git リポジトリからの
AzureML ドキュメントのみを処理し、各ドキュメントのソース URL が git URL
ではなくパブリックにホストされている URL
にリンクされるようにする方法を示しています。

５. 次の 2
つのセルを実行し、正常に実行されることを確認します。![](./media/image24.png)

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

## エクササイズ 5: パイプラインの送信

1\. パイプラインの各ステップの出力は、Workspace UI
で確認できます。以下のセルを実行した後、「詳細ページ」のリンクをクリックしてください。

2\.
次のセルを実行し、出力内のリンクをクリックしてフローのステータスを確認します。![A
screenshot of a computer Description automatically
generated](./media/image26.png)

３.
プロンプトフローで実行が開始されます。フローの各ステージを確認します。![A
screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

> ４．フローが成功したら次のステップに進みます

## エクササイズ 6: 生成されたQAデータを確認する

1\. 次の 2 つのセルを実行し、QA データの出力を確認します。![A screenshot
of a computer code Description automatically
generated](./media/image29.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image30.png)

概要:

このラボでは、データから QA データセットを作成する方法を学びました。
