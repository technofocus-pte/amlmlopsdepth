# ラボ 01- Azure Machine Learning Studio を使用して、データセットを準備し、分類モデルをトレーニングしてデプロイします。

**目的**

このラボでは、Azure Machine Learning
環境のセットアップ、データのアップロード、アクセスと探索、そして Azure
Machine Learning Studio
を使用した画像分類モデルのトレーニングとデプロイのプロセスをガイドすることに重点を置いています。

想定所要時間 - 45 分

## エクササイズ 1: Azure Machine Learning ワークスペースの設定

### タスク 1: VMクロックを同期する

> 1\. VMにログインしたら、画面右下の時計を右クリックします。
>
> 2\. 「Adjust date and time」を選択します。
>
> 3\. 開いたSettings画面で、「Sync now」の「Additional
> settings」をクリックします。![A screenshot of a computer Description
> automatically generated](./media/image1.png)
>
> 4\. 自動同期が機能しない場合に備えて、時刻の同期を行います。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image2.png)

### タスク 2: Azureリソースの準備

このタスクでは、Azure Machine
Learningワークスペースの作成に焦点を当てます。機械学習プロジェクトを効果的に整理・管理するための専用ワークスペースの設定方法を学びます。このワークスペースは、コラボレーション、実験、そしてデプロイメントのための中心的なハブとして機能します。

#### タスク 2.1: 必要なリソースプロバイダーを登録する

1\. Azure Portal のホーム
ページから、割り当てられたサブスクリプションに移動します。

2\. 左側のペインの \[Settings\] で \[Resource Providers\] を選択します。

3\. +++Microsoft.StreamAnalytics+++ を検索し、名前の横にある 3
つのドットを選択して \[Register\] をクリックします。![A screenshot of a
computer AI-generated content may be incorrect.](./media/image3.png)

####  タスク 2.2: Azure Machine Learning ワークスペースを作成する

> 1\. \[Resources\]
> タブのユーザー名とパスワードを使用して、+++https://portal.azure.com+++
> の Azure ポータルにサインインします。![A screenshot of a computer
> Description automatically generated](./media/image4.png)

2\. Azure ポータルのホーム ページから、\[+ Create a resource\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image5.png)

> 2\. \[Create a resource\] ページで、検索バーを使用して +++Azure
> Machine Learning+++ を検索し、\[Azure Machine Learning\]
> を選択します。![A screenshot of a computer Description automatically
> generated](./media/image6.png)
>
> 3\.
> 「Marketplace」の下で、「Create」ドロップダウンをクリックし、「Azure
> Machine Learning」を選択します。![A screenshot of a computer
> Description automatically generated](./media/image7.png)
>
> 4\.
> 新しいワークスペースを構成するために次の情報を入力し、「確認と作成」をクリックします。

- **Subscription**: 割り当てられたAzureサブスクリプションを選択します

- **Resource group**:.

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.labInstance.Id**+++

- **Region**:
  最寄りの地域を選択してください（ここでは米国中北部を選択しています）

&nbsp;

- **Container registry: Select Create new. Enter +++azuremlcr@lab.lab
  Instance.Id+++**

**注:**
リソース名に付加される番号は、ラボインスタンスIDです。これにより、リソースの一意性が確保されます。スクリーンショットには、リソースごとに異なる番号が付与されます。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5\. 検証に合格したら、「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6\. 「Go to
resource」をクリックして、新しいワークスペースを表示します。![A
screenshot of a computer Description automatically
generated](./media/image11.png)

7\. Microsoft.MachineLEarningServices | Overviewページで、Azure Machine
Learning Studio でモデルを操作するの下の \[Launch studio\]
を選択します。![A screenshot of a software update Description
automatically generated](./media/image12.png)

#### タスク 2.3: コンピューティングを作成する

このタスクでは、Azure でのコンピューティング
リソースの作成方法を説明します。仮想マシンやマネージド
コンピューティング クラスターなどのさまざまなコンピューティング
オプションを検討し、機械学習ワークロードを効率的に実行するためのリソースの構成とプロビジョニング方法を理解します。

1\. Azure Machine Learning Studio が開いたら、左側のペインの \[Manage\]
の下にある \[Compute\] をクリックします。![A screenshot of a computer
Description automatically generated](./media/image13.png)

2\. コンピューティング インスタンス画面で \[+ New\]
をクリックします。![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.「Create compute instance」画面で、以下の詳細を入力します。

> a\. コンピューティング名 – +++cpu-cluster-fs@lab.LabInstance.Id+++
>
> b\. 仮想マシンの種類 – CPU
>
> c\. 仮想マシンのサイズ – Standard_E4ds_v4 を選択
>
> 「Review + Create」をクリックします。

**注:**
後で使用するために、このコンピューティング名をメモしておいてください。![A
screenshot of a computer Description automatically
generated](./media/image15.png)

4\. 次の画面で「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

**注:** コンピューティングが実行状態になるまでに約 10 分かかります。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

重要：コンピューティングが起動したら、次のタスクに進むことができます。ただし、ラボの実行を中断する場合は、コンピューティングインスタンスを停止し、中断後に再開する際に再起動してください。

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

演習の概要:

この演習では、Azure Machine Learning
環境のセットアップに必要な基本的な手順を習得します。一連のタスクを通じて、参加者はストレージアカウントの作成、Machine
Learning SDK のインストール、Azure CLI を使用したログイン、Azure Machine
Learning ワークスペースの作成、コンピューティング
リソースのセットアップ方法を学習しました。この演習を完了することで、機能的な
Azure Machine Learning
環境を構築するために必要な基礎知識と実践的なスキルを習得し、自信を持って機械学習プロジェクトに着手できるようになります。

## エクササイズ 2 – Azure Machine Learning でのデータのアップロード、アクセス、探索

**目的**

この演習では、次の方法を学習します。

• データをクラウドストレージにアップロードする

• Azure Machine Learning データアセットを作成する

• インタラクティブな開発のためにノートブックでデータにアクセスする

• データアセットの新しいバージョンを作成する

機械学習プロジェクトの開始には、通常、探索的データ分析
(EDA)、データ前処理
(クリーニング、特徴量エンジニアリング)、そして仮説を検証するための機械学習モデルのプロトタイプ構築が含まれます。このプロトタイピングプロジェクトフェーズは非常にインタラクティブです。IDE
または Jupyter Notebook と Python
インタラクティブコンソールでの開発に適しています。このラボでは、これらのアイデアについて説明します。

現在、機械学習プロジェクトワークフローの「データ：探索と準備」段階にあります。![](./media/image19.png)

### タスク 1: Azureリソースの準備

重要：前回のエクササイズで作成したコンピューティングが稼働していることを確認してください。ラボの実行を中断する場合は、中断後に再開する際に必ず停止し、再起動してください。

#### タスク 1.1: ノートブックのアップロード

1.  Azure Machine Learning
    スタジオで、コンピューティングが起動したら、左側のペインから
    \[Notebooks\] オプションを選択します。![](./media/image20.png)

2\. 「What’s new in Notebooks」ダイアログを閉じます。![A screenshot of a
computer Description automatically generated with medium
confidence](./media/image21.png)

3\. ノートブックファイルペインが開き、「Users」→\<UserName
\>という構造が表示されます。ユーザー名の横にある3つの点をクリックし、「Create
new folder」を選択します。![A screenshot of a computer Description
automatically generated](./media/image22.png)

4\. フォルダー名に +++Azuremlnotebooks+++ と入力し、\[Create\]
をクリックします。![A screenshot of a computer Description automatically
generated](./media/image23.png)

> 5.フォルダーが作成されたら、Azuremlnotebooks フォルダーのメニュー
> オプション (フォルダー名の横にある 3 つのドット) をクリックし、Upload
> filesをクリックします。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

6\. 「Click to browse」を選択し、file(s)を選択します。C:\lab files の
explore-data.ipynb を参照し、「Open」をクリックします。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image25.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image26.png)

7\. 「Open file after upload」と「I trust the contents of this
file」のチェックボックスをオンにし、「Upload」をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image27.png)

8\. アップロードされたノートブックが開きます。![A screenshot of a
computer Description automatically generated](./media/image28.png)

> 9\.
> スタジオに初めてログインするため、スタジオから認証を求められた場合は、「Authenticate」をクリックします。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image29.png)

### タスク 2: データをアップロード、アクセス、探索する

#### タスク 2.1: データのダウンロード

1\. Notebooks の \[Files\] ペインで、フォルダー名 Azuremlnotebooks
の横にある 3 つのドットをクリックし、\[Create new folder\]
をクリックします。

![](./media/image30.png)

2\. フォルダーの名前を +++data+++
と入力し、「Create」をクリックします。.

![](./media/image31.png)

3\. フォルダーの作成が成功したら、フォルダー データのメニュー
オプションをクリックし、「Upload files」を選択します。![A screenshot of
a computer Description automatically generated](./media/image32.png)

> 4\. \[Click to browse and select file(s)\] を選択し、C:\labfiles
> に移動して default_of_credit_card_clients.csv
> ファイルを選択し、\[Open\] をクリックします。![A screenshot of a
> computer Description automatically generated with medium
> confidence](./media/image33.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image34.png)

5\. アップロードが完了すると、「Notifications」の下に「File uploaded
successfully」というメッセージが表示されます。

![A close-up of a computer screen Description automatically generated
with low confidence](./media/image35.png)

#### タスク 2.2: Create handle to workspace

1\. ノートブック（explore-data）に戻ります。

2\.
コードに進む前に、ワークスペースを参照する方法が必要です。ワークスペースへのハンドルとして
ml_client を作成します。ml_client
を使用してリソースとジョブを管理します。

3\.
「ワークスペースへのハンドルを作成」​​の下の最初のセルで、\<SUBSCRIPTION_ID\>、\<RESOURCE_GROUP\>、\<AML_WORKSPACE_NAME\>
のプレースホルダーを置き換えます。

4\. \<RESOURCE_GROUP\>
を、割り当てたリソースグループの名前に置き換えます。

5\. \<AML_WORKSPACE_NAME\> を +++Azuremlws@ラボ.ラボInstance.Id+++
に置き換えます。

6\. \<SUBSCRIPTION_ID\> を +++@Lab.CloudSubscription.Id+++
に置き換えます。

7\. セルの左上にある「Run
cell」ボタンをクリックします。実行が成功したら、セルの下部にチェックマークが表示されます。![A
screenshot of a computer Description automatically
generated](./media/image36.png)

#### タスク 2.3: クラウドストレージにデータをアップロードする

1.  Azure Machine Learning のデータ資産は、Web ブラウザーのブックマーク
    (お気に入り) に似ています。頻繁に使用するデータを指す長いストレージ
    パス (URI)
    を覚える代わりに、データ資産を作成し、フレンドリ名でその資産にアクセスできます。

2.  次のノートブック セルでデータ資産が作成されます。コード
    サンプルでは、​​指定されたクラウド ストレージ リソースに生データ
    ファイルをアップロードします。

3.  データ資産を作成するたびに、そのデータ資産の一意のバージョンが必要です。同じバージョンが既に存在する場合はエラーが発生します。このコードでは、セルが実行されるたびに時間を使用して一意のバージョンを生成しています。

4\. セルの左上にある \[Execute\]
ボタンをクリックして、次のセルを実行します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

5\. 「Data asset created. Name: credit-card, version:
YYYY:MM:DD.xxxxxx」がセルの下に表示される出力です。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image38.png)

6\.
左ペインから「Data」をクリックし、上記の手順で実行した実行によって作成されたクレジットカードデータアセットをクリックします。詳細を確認し、「Notebooks」ペインに戻ります。

![](./media/image39.png)

#### タスク 2.4: ノートブック内のデータにアクセスする

1.  ノートブックに戻り、%pip コマンドでセルを実行して、Jupyter
    カーネルに azureml-fsspec Python ライブラリをインストールします。

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image40.png)

2\. 次のセルを実行して、Pandas で CSV ファイルにアクセスします。

3\. セルの下部にデータアセットの URI
が表示され、データも表示されます。![A screenshot of a computer code
Description automatically generated with low
confidence](./media/image41.png)

#### タスク 2.5: データ資産の新しいバージョンを作成する

1\.
機械学習モデルの学習に適したデータにするには、少しクリーニングする必要があることにお気づきかもしれません。データには以下の点があります。

a\. 2つのヘッダー

b\. クライアントID列（機械学習ではこの機能は使用しません）

c\. レスポンス変数名にスペースが含まれています

2\.
また、CSV形式と比較して、Parquetファイル形式はこのデータを保存するのに適しています。Parquetは圧縮機能を備え、スキーマも維持します。そのため、データをクリーニングしてParquetに保存するには、次のセルを実行してください。

3\.
セルの下部にあるチェックマークで、実行が成功したことを確認してください。![](./media/image42.png)

> 4.この表は、前のステップでダウンロードした元のdefault_of_credit_card_clients.csvファイル（.CSVファイル）のデータ構造を示しています。アップロードされたデータには、以下に示すように、23個の説明変数と1個の応答変数が含まれています。

[TABLE]

５.
次のセルを実行して、データアセットの新しいバージョンを作成します（データは自動的にクラウドストレージにアップロードされます）。

６. 実行が成功すると、セルの後に「Data asset created. Name: credit_card,
version: YYYY.MM.DD.xxxxxx_cleaned」という出力が表示されます。![A
screenshot of a computer code Description automatically generated with
low confidence](./media/image43.png)

![A screenshot of a computer Description automatically generated with
low confidence](./media/image44.png)

重要:

この Python
コードセルは、作成するデータアセットの名前とバージョン値を設定します。そのため、これらの値を変更せずにこのセル内のコードを複数回実行すると、エラーが発生します。名前とバージョン値を固定することで、自動生成値やランダム生成値を気にすることなく、特定の状況に適した値を渡すことができます。

７.
クリーンアップされたParquetファイルは最新バージョンのデータソースです。次のセルのコードは、最初にCSVバージョンの結果セットを表示し、次に実行時にParquetバージョンを表示します。

８．次のセルを実行し、以下の結果を確認してください。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image45.png)

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image46.png)

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image47.png)

![A picture containing text, screenshot, number, display Description
automatically generated](./media/image48.png)

> ９.
> 「Data」の下でクリーンアップされたデータを探します。![](./media/image49.png)

重要:
ここから次のエクササイズに進むことができます。ただし、ラボの実行を中断する場合は、コンピューティングインスタンスを停止し、再開時に再起動してください。![A
screenshot of a computer Description automatically generated with medium
confidence](./media/image50.png)

エクササイズの概要

この演習では、データをクラウド ストレージにアップロードする方法、Azure
Machine Learning
データ資産を作成する方法、対話型開発のためにノートブック内のデータにアクセスする方法、およびデータ資産の新しいバージョンを作成する方法を学習しました。

## エクササイズ 3 – Azure Machine Learning Studio で画像分類モデルをトレーニングしてデプロイする

**目的**

この演習では、次のことを学びます

1.  ワークスペースに接続し、Azure Machine Learning Studio Notebook UI
    を使用してコンピューティングリソースをセットアップする

2.  データを取り込んでトレーニング用に準備する

3.  画像分類用にモデルをトレーニングする

4.  モデルを最適化するためのメトリックを表示および分析する

5.  モデルをオンラインでデプロイしてテストする

私たちは、機械学習プロジェクト ワークフローのTrain & validate
modelの段階にあります。

### ![A picture containing text, font, number, screenshot Description automatically generated](./media/image51.png)タスク 1: ノートブックのアップロード

1\. Azure Machine Learning Studio の Notebooks
ページで、AzureMLnotebooks フォルダーのメニュー
オプションをクリックし、ファイルのUploadをクリックします。

> ![A screenshot of a computer Description automatically
> generated](./media/image52.png)
>
> 2\. 選択し、クリックしてファイルを参照して選択し、C:\labfiles
> を参照して、azureml-getting-started-studio (Jupyter ソース ファイル)
> ファイルを選択します。![A screenshot of a computer screen Description
> automatically generated with medium confidence](./media/image53.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)
>
> 3\. 「Open file after
> upload」チェックボックスをオンにして、「Upload」をクリックします。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image55.png)
>
> 4\.
> ファイルのアップロードが成功すると、スタジオでファイルが開かれ、実行状態にあるコンピューティング
> (cpu-cluster-fs) に自動的に接続されます。![A screenshot of a computer
> Description automatically generated with medium
> confidence](./media/image56.png)

### タスク 2: Azure Machine Learning ワークスペースに接続する

*コードに進む前に、ワークスペースに接続する必要があります。ワークスペースはAzure
Machine Learningの最上位リソースであり、Azure Machine
Learningの使用時に作成したすべての成果物を一元的に操作できる場所を提供します。*

*ワークスペースへのアクセスにはDefaultAzureCredentialを使用しています。DefaultAzureCredentialはほとんどのシナリオに対応できるはずです。*

*\# Handle to the workspace*

**from** azure.ai.ml **import** MLClient

*\# Authentication package*

**from** azure.identity **import** DefaultAzureCredential

credential **=** DefaultAzureCredential()

*\# Get a handle to the workspace. You can find the info on the
workspace tab on ml.azure.com*

ml_client **=** MLClient(

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1\.
上記のコード（ノートブックの最初のセル）で、SUBSCRIPTION_ID、RESOURCE_GROUP名、AML_WORKSPACE_NAMEプレースホルダーを、前の演習で保存した値に置き換えます。

2\.
ノートブックの最初のセルは次のようになります。最初のセルの左上にある「Run」ボタンをクリックします。

![A screenshot of a computer Description automatically
generated](./media/image57.png)

3\.
セルの下部にあるステータスを確認して、セルが正常に実行されたことを確認します。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image58.png)

In \[ \]:

### タスク 3: データをアップロードする

Azure Machine Learning のトレーニング
ジョブを実行するには、環境が必要です。

このラボでは、必要なすべてのライブラリ (python、MLflow、numpy、pip など)
が含まれる、AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest
という既製の環境を使用します。

1\. 次のセルのコードを実行してデータをアップロードします。

2\. セルの出力として「Data asset
created」というメッセージが表示されていることを確認します。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image59.png)

###  タスク 4: トレーニング用のコマンドジョブを構築する

ジョブの実行に必要なアセットがすべて揃ったので、Azure ML Python SDK v2
を使用してジョブ自体を構築します。ここではコマンドジョブを作成します。

AzureML コマンドジョブは、クラウドでトレーニング
コードを実行するために必要なすべての詳細（入力と出力、使用するハードウェアの種類、インストールするソフトウェア、コードの実行方法）を指定するリソースです。コマンドジョブには、単一のコマンドを実行するための情報が含まれています。

#### タスク 4.1 : トレーニングスクリプトを作成する

1\. まず、トレーニングスクリプト（main.py Python
ファイル）を作成します。

2\. 次のセルを実行し、正常に実行されることを確認します。![A picture
containing text, font, line, screenshot Description automatically
generated](./media/image60.png)

3\.
次のセルのスクリプトはデータの前処理を行い、テストデータとトレーニングデータに分割します。その後、このデータを使用してツリーベースのモデルをトレーニングし、出力モデルを返します。パイプライン実行中のパラメータとメトリクスのログにはMLFlowが使用されます。

4\. セルを実行し、出力が正常に実行されることを確認します。

**Writing ./src/main.py**

> ![A screenshot of a computer program Description automatically
> generated with low confidence](./media/image61.png)
>
> ![A screenshot of a computer program Description automatically
> generated with medium confidence](./media/image62.png)

5\.
このスクリプトからわかるように、モデルのトレーニングが完了すると、モデルファイルが保存され、ワー​​クスペースに登録されます。これで、登録されたモデルを推論エンドポイントで使用できるようになります。

#### タスク 4.2: コマンドを設定する

目的のタスクを実行できるスクリプトが完成したので、次はコマンドラインアクションを実行できる汎用コマンドを使用します。このコマンドラインアクションは、システムコマンドを直接呼び出したり、スクリプトを実行したりすることができます。

1\.
ここでは、入力データ、分割比率、学習率、登録モデル名を入力変数として使用します。

2\. 左側のペインから「Data」を選択し、「credit-card-data」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image63.png)

3\. 「Data sources」セクションで、「Datastore
URI」の値を探してコピーします。次のステップで使用するために保存します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image64.png)

4\. 次のセルで、次の値を置き換えます。

a\. path の値を、前の手順で保存したデータストア URI に置き換えます。

b\. compute の値を +++cpu-cluster-fs@ラボ.ラボInstance.Id+++ (ラボ 1
で保存したクラスターの名前) に置き換えます。

5\. 「Run」をクリックします。セルが正常に実行されたことを確認します。![A
screenshot of a computer program Description automatically
generated](./media/image65.png)

### タスク 6: ジョブを送信する

AzureMLで実行するジョブを送信します。ジョブの実行には2～3分かかります。コンピューティングインスタンスがゼロノードにスケールダウンされ、カスタム環境の構築中の場合は、さらに時間がかかる場合があります（最大10分）。

1.  以下のコマンドでセルを実行してジョブを送信します。

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  「Run」をクリックします。実行が成功し、「Details
    Page」列に結果へのリンクがあることを確認します。

注: 完了までに約2分かかります。

.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image66.png)

3.  結果の「Details Page」列の下にあるリンクを新しいタブで開きます。

### タスク 7: トレーニングジョブの結果を表示する

1.  ジョブを送信した後に生成された URL をクリックすると、トレーニング
    ジョブの結果を表示できます。

> ![A screenshot of a computer Description automatically
> generated](./media/image67.png)

2.  または、左側のナビゲーションメニューで「Jobs」をクリックすることもできます。ジョブとは、指定されたスクリプトまたはコードから実行される複数の実行をグループ化したものです。実行に関する情報は、そのジョブに保存されます。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image68.png)

3.  概要ページでは、まず「Properties」ペインの「ステータス」が「Running」と表示されます。

4.  準備が完了すると、ステータスが「Completed」に変わります。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image69.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image70.png)

5.  メトリックを表示するには、「Metrics」ペインを選択します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image71.png)

6\. \[画像\] タブを選択して、training_confusion
マトリックス、精度リコール曲線、および roc 曲線を表示します。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image72.png)

1)  「Overview」では、ジョブのステータスを確認できます。

2)  「Metrics」では、スクリプトで指定したメトリクスのさまざまな視覚化が表示されます。

3)  「Images」では、MLflow
    で記録した画像アーティファクトを確認できます。

4)  「Child jobs」では、子ジョブを追加した場合に表示されます。

5)  「Outputs +
    logs」では、トラブルシューティングやその他のモニタリングに必要なログファイルが表示されます。

6)  「Code」では、ジョブで使用されるスクリプト/コードが表示されます。

7)  「Explanations」と「Fairness」では、モデルが責任ある AI
    標準にどのように適合しているかを確認できます。これらは現在プレビュー機能であり、追加のパッケージのインストールが必要です。

8)  「Monitoring」では、コンピューティングリソースのパフォーマンスに関するメトリクスを確認できます。

### タスク 8: モデルをオンラインエンドポイントとしてデプロイする

機械学習モデルをトレーニングした後は、他のユーザーが推論に使用できるようにデプロイする必要があります。Azure
Machine Learning
では、エンドポイントを作成し、そこにデプロイを追加することができます。

ここで言うエンドポイントとは、クライアントがトレーニング済みモデルにリクエスト（入力データ）を送信し、モデルから推論（スコアリング）結果を受け取るためのインターフェイスを提供する
HTTPS パスです。エンドポイントは以下の機能を提供します。

• キーまたはトークンベースの認証

• TLS（SSL）ターミネーション

• 安定したスコアリング
URI（endpoint-name.region.inference.ml.azure.com）

デプロイとは、実際に推論を行うモデルをホストするために必要なリソースのセットです。

#### タスク 8.1: オンラインエンドポイントを作成する

1\. 機械学習モデルを、Azure クラウドのオンライン エンドポイントである
Web サービスとしてデプロイします。

2\. 左側のペインから \[Endpoints\] を選択します。

![A screenshot of a computer Description automatically
generated](./media/image73.png)

3\. リアルタイムエンドポイントの作成を選択![A screenshot of a computer
Description automatically generated](./media/image74.png)

![A screenshot of a computer Description automatically
generated](./media/image75.png)

4\.
仮想マシンで「Standard_E4s_v3」を選択します。インスタンス数を1と入力します。

エンドポイント名とデプロイメント名はデフォルトのままとし、「Deploy」を選択します。![A
screenshot of a computer Description automatically
generated](./media/image76.png)

注: エンドポイントの作成には約 20 分かかります。

5\. 完了すると、プロビジョニングの状態が「Succeeded」に変わります。![A
screenshot of a computer Description automatically
generated](./media/image77.png)

#### タスク 8.2: サンプルクエリでテストする

1.  エンドポイントページで、「Test」タブを選択します。

2.  次のサンプルリクエストファイルをコピーして、「Input data to test
    real-time endpoint
    field」フィールドに貼り付け、既存のコードを置き換えます。

> **{**
>
> **"input_data": {**
>
> **"columns":
> \[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22\],**
>
> **"index": \[0, 1\],**
>
> **"data": \[**
>
> **\[20000,2,2,1,24,2,2,-1,-1,-2,-2,3913,3102,689,0,0,0,0,689,0,0,0,0\],**
>
> **\[10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10,
> 9, 8\]**
>
> **\]**
>
> **}**
>
> **}**

3.  「Test」を選択し、「Test result」で結果を表示します。

> ![A screenshot of a computer Description automatically
> generated](./media/image78.png)

### タスク 9: エンドポイントを削除する

1\.
左側のペインからエンドポイントを選択します。作成したエンドポイントを選択し、「Delete」をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image79.png)

2\. 確認ダイアログボックスで「Delete」をクリックします。![A screenshot
of a computer error Description automatically generated with low
confidence](./media/image80.png)

3\. 削除が成功したことを示す通知を探します。![A picture containing text,
screenshot, font, line Description automatically
generated](./media/image81.png)

**概要**

このラボでは、Azure Machine Learning Studio
で画像分類モデルをトレーニングし、それを Web
サービスとしてデプロイする方法を学習しました。
