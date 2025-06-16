# ラボ07: Azure Machine Learning Studio からプロンプト フローを開発してテストする

**目的:**

このラボでは、Azure Machine Learning Studio
でプロンプトフローを使用する際の主なユーザージャーニーを学習します。Azure
Machine Learning
ワークスペースでプロンプトフローを有効にする方法、プロンプトフローを作成・開発する方法、フローをテスト・評価する方法、そして本番環境にデプロイする方法を学習します。

想定所要時間 – 60 分

## タスク 1: Azureリソースの準備

### タスク 1.1: Azure Machine Learning ワークスペースを作成する

このタスクでは、Azure Machine Learning
ワークスペースの作成に焦点を当てます。機械学習プロジェクトを効果的に整理・管理するための専用ワークスペースの設定方法を学習します。このワークスペースは、コラボレーション、実験、デプロイのための中心的なハブとして機能します。

1\. +++https://portal.azure.com+++ で Azure Portal
にサインインし、管理者テナントの資格情報を使用してログインします。

2\. Azure Portal のホームページで、\[+Create a resource\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image1.png)

３．リソースの作成ページで、検索バーを使用して +++Azure Machine
Learning+++ を検索し、Azure Machine Learning を選択します。

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)
>
> 4\.
> 「Marketplace」の下で、「Create」ドロップダウンをクリックし、「Azure
> Machine Learning」を選択します。![A screenshot of a computer
> Description automatically generated](./media/image3.png)
>
> 5\. 新しいワークスペースを構成するには、次の情報を入力します。

- **Subscription**: 割り当てられたAzureサブスクリプションを選択します

- **Resource group**: 割り当てられたリソース グループを選択します。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **ワークスペースの詳細:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: 最寄りの地域を選択してください（ここではNorth Central
  USを選択しています）

&nbsp;

- **Container registry: Create newを選択します。**
  [**+++**azuremlcr@lab.LabInstanceId](mailto:+++azuremlcr@lab.LabInstanceId)**+++を入力します。**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

６. ワークスペースの構成が完了したら、\[Review + Create\] を選択します。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

７. 検証に合格したら、「Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8\. 「Go to
resource」をクリックして、新しいワークスペースを表示します。![A
screenshot of a computer Description automatically
generated](./media/image9.png)

9\. Microsoft.MachineLEarningServices | 概要ページで、Azure Machine
Learning Studio でモデルを操作するの下の \[Launch studio\]
を選択します。![A screenshot of a computer Description automatically
generated](./media/image10.png)

### タスク 1.2: コンピューティングを作成する

このタスクでは、Azure でコンピューティング
リソースを作成する方法を説明します。仮想マシンやマネージド
コンピューティング クラスターなどのさまざまなコンピューティング
オプションを検討し、機械学習ワークロードを効率的に実行するためのリソースの構成とプロビジョニング方法を理解します。

1\. Azure Machine Learning Studio が開いたら、左側のペインの \[Manage\]
の下にある \[Compute\] をクリックします。![A screenshot of a computer
Description automatically generated](./media/image11.png)

2\. コンピューティング インスタンス画面で \[+ New\]
をクリックします。![A screenshot of a computer Description automatically
generated](./media/image12.png)

3\. 「Create compute instance」画面で、以下の詳細を入力します。

1.  Compute name – +++**pfcompute**+++

2.  Virtual machine type – **CPU**

3.  Virtual machine size – **Standard_E4ds_v4**を選択します。

> 「Review + Create」をクリックします。

**注:**
後で使用するために、このコンピューティング名をメモしておいてください。![A
screenshot of a computer Description automatically
generated](./media/image13.png)

４. 次の画面で「Create」をクリックしてコンピューティングを作成します。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**注:** コンピューティングが実行状態になるまでに約 10 分かかります。![A
screenshot of a computer Description automatically
generated](./media/image15.png)

重要：コンピューティングが起動したら、次のタスクを続行できます。ただし、ラボの実行を中断する場合は、コンピューティングインスタンスを停止し、中断後に再開するときに再起動してください。

### タスク 1.3: Azure OpenAI リソースを作成する

1\. Azure ポータル +++https://portal.azure.com+++
から、+++AzureOpenAI+++ を検索して選択します。![A screenshot of a
computer Description automatically generated](./media/image16.png)

2\. 「+ Create」をクリックします。![A screenshot of a computer
Description automatically generated](./media/image17.png)

3\. 以下の詳細を入力し、「Next」をクリックします。

- Resource group - 割り当てられたリソースグループを選択します

- Region – 地域を選択してください（ここではNorth Central
  USを使用しています）

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4\. 次のページでデフォルトを受け入れ、「Review +
submit」ページで「Create」をクリックします。![A screenshot of a computer
Description automatically generated](./media/image19.png)

5\. デプロイが完了したら、「Go to resource」をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image20.png)

6\. 左側のペインから「Keys and Endpoint」を選択します。![A screenshot of
a computer Description automatically generated](./media/image21.png)

7\.
キーとエンドポイントをコピーし、ラボの後半で使用するためにメモ帳に保存します。![A
screenshot of a computer Description automatically
generated](./media/image22.png)

8\. Azure Machine Learning Studio の左側のペインでモデル
カタログを選択し、gpt-4o を選択します。![A screenshot of a computer
Description automatically generated](./media/image23.png)

9\. 「Deploy」をクリックしてモデルをデプロイします。![A screenshot of a
computer Description automatically generated](./media/image24.png)

10\.
デプロイメント名を承認し、「Deploy」を選択します。この名前は後で使用するためにメモしておいてください。![A
screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## タスク 2: プロンプトフロー接続を設定する

1\. Azure Machine Learning Studio の左側のナビゲーション
ペインから「Prompt flow」を選択します。メニュー
バーから「Connections」を選択します。「Create」の横にあるドロップダウンから「Azure
OpenAI」を選択します。![A screenshot of a computer Description
automatically generated](./media/image27.png)

2\. Azure OpenAI 接続の追加ウィザードで、以下の詳細を入力し、\[Save\]
を選択します。

- Name – +++**AoaiML_pf**+++

- Provider – **Azure OpenAI**を選択します。

- Subscription ID – 割り当てられたサブスクリプションを選択します

- Azure OpenAI Account Name –
  [**AOAI-PF@lab.LabInstanceId**](mailto:AOAI-PF@lab.LabInstanceId)を選択します。

- Auth Mode – **API Key**を選択します。

- API Key – Azure OpenAIリソースを保存したキーを入力します

- API base – Azure OpenAIリソースから保存したエンドポイントを提供します

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

> ３．接続の作成が成功したことを確認します

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## タスク 3: プロンプトフローを作成し、開発する

1.  プロンプトフローのホームページの「Flows」タブで、「Create」を選択してプロンプトフローを作成します。「Create
    a new
    flow」ページには、作成可能なフローの種類、フロー作成のために複製できる組み込みサンプル、フローをインポートする方法が表示されます。

![A screenshot of a computer Description automatically
generated](./media/image31.png)

1\. WebClassification カテゴリの「Clone」を選択します。

Explore ギャラリーでは、組み込みサンプルを参照し、任意のタイルで「View
detail」を選択して、シナリオに適しているかどうかをプレビューできます。

このラボでは、Web Classification
サンプルを使用して、主要なユーザージャーニーを順に説明します。

Web Classification は、LLM
を使用した多クラス分類のデモンストレーションフローです。URL
を入力すると、このフローは、わずかなショット、簡単な要約、分類プロンプトだけで、URL
を Web カテゴリに分類します。例えば、https://www.imdb.com という URL
を入力すると、この URL を Movie に分類します。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

2\. フォルダー名に入力された名前を受け入れて、「Clone」を選択します。![A
screenshot of a computer Description automatically
generated](./media/image33.png)

3\.
フロー実行にはコンピューティングセッションが必要です。コンピューティングセッションは、アプリケーションの実行に必要なコンピューティングリソース（必要なすべての依存パッケージを含むDockerイメージなど）を管理します。

4\. フロー作成ページで、「Start compute
session」を選択してコンピューティングセッションを開始します。![A
screenshot of a computer Description automatically
generated](./media/image34.png)

**注:** コンピューティング セッションが実行状態になるまでには約 10
分かかります。![A screenshot of a computer Description automatically
generated](./media/image35.png)

## タスク 4: フロー作成ページを検査する

コンピューティングセッションの開始には数分かかる場合があります。コンピューティングセッションの開始中に、フロー作成ページの各部分を確認してください。

•
ページの左側にあるフローまたはフラット化ビューはメインの作業領域で、ノードの追加または削除、ノードのインライン編集と実行、プロンプトの編集などを行ってフローを作成できます。\[Inputs\]
セクションと \[Outputs\]
セクションでは、入力と出力の表示、追加、削除、編集を行うことができます。

現在の Web Classification
サンプルを複製した時点で、入力と出力は既に設定されています。フローの入力スキーマは、name:
url; type: string（文字列型の
URL）です。プリセットの入力値は、https://www.imdb.com
などの別の値に手動で変更できます。

• 右上の \[Files\]
には、フローのフォルダとファイル構造が表示されます。各フローフォルダには、flow.dag.yaml
ファイル、ソースコードファイル、システムフォルダが含まれています。テスト、デプロイ、またはコラボレーション用のファイルを作成、アップロード、またはダウンロードできます。

• 右下の \[Graph\]
ビューは、フローの外観を視覚化するためのものです。拡大・縮小したり、自動レイアウトを使用したりできます。

フロー表示またはフラット化表示でファイルをインラインで編集したり、Raw
ファイルモードの切り替えをオンにしてファイルからファイルを選択し、タブで開いて編集したりすることもできます。

フロー表示またはフラット化表示でファイルをインラインで編集したり、Raw
ファイルモードの切り替えをオンにしてファイルからファイルを選択し、タブで開いて編集したりすることもできます。![A
screenshot of a computer Description automatically
generated](./media/image36.png)

## タスク 5: LLMノードを設定する

各LLMノードについて、LLM
APIキーを設定するための接続を選択する必要があります。Azure
OpenAI接続を選択してください。接続の種類に応じて、ドロップダウンリストからdeployment_nameまたはモデルを選択する必要があります。Azure
OpenAI接続の場合は、deploymentを選択してください。 

1.  summary_text_content には、以下の詳細を入力します。

Connection –**AoaiML_pf**を選択します。

Api – **chat**を選択します。

deployment name – **gpt-4o-2024-11-20**を選択します。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

２. LLM ノード classify_with_llm に対しても同様に接続を設定します。![A
screenshot of a computer Description automatically
generated](./media/image38.png)

３.
単一のノードをテストおよびデバッグするには、「フロー」ビューでノード上部の「実行」アイコンを選択します。「入力」を展開し、フローの入力URLを変更することで、異なるURLに対するノードの動作をテストできます。

４.
実行ステータスはノード上部に表示されます。実行が完了すると、実行出力がノードの「出力」セクションに表示されます。

５. フローの先頭に移動し、fetch_text_content_from
URLを実行してブロックを実行します。![A screenshot of a computer
Description automatically generated](./media/image39.png)

グラフビューには、単一の実行ノードのステータスも表示されます。

６.
「Inputs」セクションで、「Value」フィールドに「+++https://play.google.com/store/apps/details?id=com.spotify.music+++」と入力します。

右上の「Run」を選択して、フロー全体をテストおよびデバッグします。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## タスク 5: フロー出力の表示

フロー出力を設定することで、複数のノードの出力を一箇所で確認することもできます。フロー出力を使用すると、次のことが可能になります。

• 一括テスト結果を単一のテーブルで確認する。

• 評価インターフェースのマッピングを定義する。

• デプロイメント応答スキーマを設定する。

1\. 上部のバナーまたはメニューバーで「View
outputs」を選択すると、詳細な入力、出力、フロー実行、オーケストレーション情報が表示されます。![A
screenshot of a computer Description automatically
generated](./media/image41.png)

2.\[Outputs\] 画面の \[Outputs\]
タブで、フローがカテゴリと証拠を使用して入力 URL
を予測していることに注意してください。![A screenshot of a computer
Description automatically generated](./media/image42.png)

3\.
「Outputs」画面で「Trace」タブを選択し、ノード名の下の「Flow」を選択すると、右側のペインにフローの詳細な概要情報が表示されます。フローを展開し、任意のステップを選択すると、そのステップの詳細情報が表示されます。![A
screenshot of a computer Description automatically
generated](./media/image43.png)

**概要:**

このラボでは、Azure Machine Learning Studio のプロンプト
フローを使用して、簡単な要約と分類プロンプトを使用して URL を Web
カテゴリに分類する方法を学習しました。
