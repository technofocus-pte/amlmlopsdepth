# ラボ03 - マネージド フィーチャ ストアを使用してフィーチャ セットを開発および登録し、フィーチャを使用してモデルをトレーニングします。

このラボでは、カスタム変換を用いた特徴セット仕様の作成方法について説明します。次に、その特徴セットを使用してトレーニングデータを生成し、マテリアライゼーションを有効にし、バックフィルを実行します。マテリアライゼーションは、特徴ウィンドウの特徴値を計算し、それらの値をマテリアライゼーションストアに保存します。その後、すべての特徴クエリは、マテリアライゼーションストアからそれらの値を使用できます。

マテリアライゼーションを使用しない場合、特徴セットクエリは、値を返す前に、ソースに変換をオンザフライで適用して特徴を計算します。このプロセスはプロトタイピングフェーズではうまく機能します。ただし、本番環境でのトレーニングおよび推論操作では、信頼性と可用性を高めるために、特徴をマテリアライズすることをお勧めします。

想定所要時間：50分

## エクササイズ1: 必要な役割を割り当てます。

1\.
Azureポータルのホームページで、「Resources」タブから割り当て済みのリソースグループを選択します。左側のペインで「Access
control
(IAM)」を選択します。「Add」の横にあるドロップダウンをクリックし、「Add
role assignment」を選択します。![A screenshot of a computer Description
automatically generated](./media/image1.png)

![A screenshot of a computer Description automatically
generated](./media/image2.png)

> 2\. \[Members\] タブで \[+Select members\] をクリックし、ユーザー名
> +++@lab.CloudPortalCredential(User1).Username+++ を検索します。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

3\. ユーザー名を選択し、「Select」ボタンをクリックします。![A screenshot
of a computer Description automatically generated](./media/image4.png)

4\. 次の 2 つの画面で \[Review + assign\] をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image5.png)

5\.
割り当てが完了すると、追加されたロールの割り当てメッセージが表示されます。

6\. 同じ手順を繰り返して、ロール +++Storage Blob データ閲覧者+++ と
+++Storage Blob データ共同作成者+++ を追加します。

## エクササイズ2: 機能セットを開発し、マネージド機能ストアに登録する

このチュートリアルは、マネージド特徴量ストア チュートリアル
シリーズの最初の部分です。ここでは、以下の方法を学習します。

• 新しい最小限の特徴量ストア リソースを作成する。

• 特徴量変換機能を備えた特徴量セットを開発し、ローカルでテストする。

• 特徴量ストア エンティティを特徴量ストアに登録する。

• 開発した特徴量セットを特徴量ストアに登録する。

• 作成した特徴量を使用して、サンプルのトレーニング DataFrame
を生成する。

• 特徴量セットのオフライン
マテリアライゼーションを有効にし、特徴量データをバックフィルする。

### タスク 1: 環境を準備しましょう

1\. Azure Machine Learning Studio
の左側のペインで、「Authoring」の下にある「Notebooks」を選択します。ユーザー名の横にある
3 つの点をクリックし、「Upload folder」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2\. C:\Labfiles から featurestore
フォルダーを参照して選択し、「Upload」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3\. featurestore-\> notebooks-\>sdk_and_cli に移動し、ノートブック
1.Develop-feature-set-and-register.ipynb を開きます。

![](./media/image8.png)

4\. 「Compute」で「Serverless Spark Compute」を選択します。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5\. 「Configure
session」を選択して、前提条件を使用してセッションを構成します。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6\. Pythonパッケージを選択 -\>
Condaファイルをアップロードします。「Browse」をクリックし、C:\Labfilesからconda.ymlを選択して「Apply」を選択します。

7\.
ノートブックの最初のセルを実行します。これにより、すべての依存関係がインストールされ、実行が完了します。完了まで約10分かかります。![A
screenshot of a computer Description automatically
generated](./media/image11.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8\.
Sparkセッションが開始したら、ユーザー名を自分のユーザー名に置き換えて次のセルを実行します。![A
screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9\. 次の 3 つのセルを実行して、Azure CLI をセットアップします。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10\. 次のセルで、出力の手順に従って Azure にログインします。![A
screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### タスク 2: 最小限の機能ストアを作成する

1\. フィーチャー
ストアの名前、場所、その他の値を設定するには、最初のセルを実行します。![A
screenshot of a computer program Description automatically
generated](./media/image19.png)

2\. 機能ストアを作成する次のセルを実行します。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

> 3\.
> 次のセルはAzureML機能ストアコアSDKクライアントを初期化します。実行してください。![A
> screenshot of a computer Description automatically
> generated](./media/image21.png)

### タスク 3: このノートブックでトランザクションローリング集約機能セットのプロトタイプを作成して開発します

1\. このセクションの最初のセルを実行して、トランザクション ソース
データを調査します。![A screenshot of a computer Description
automatically generated](./media/image22.png)

2\. 2
番目のセルを実行して、トランザクション機能セットをローカルで開発します。![A
screenshot of a computer code Description automatically
generated](./media/image23.png)

3\. 次のセルを実行して、機能セット仕様から Spark
データフレームを生成します。![A screenshot of a computer Description
automatically generated](./media/image24.png)

4\.
機能セット仕様を機能ストアに登録するには、特定の形式で保存する必要があります。生成されたトランザクションFeaturesetSpecを確認してください。仕様を確認するには、ファイルツリーからこのファイルを開いてください：featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml

次のセルを実行して、機能セット仕様としてエクスポートしてください。![A
screenshot of a computer program Description automatically
generated](./media/image25.png)

### タスク 4: フィーチャストアエンティティを登録する

> 1\.
> エンティティは、同じ論理エンティティを使用する機能セット全体で同じ結合キー定義を使用するというベストプラクティスを強制するのに役立ちます。セルを実行して、機能ストアエンティティを登録します。![A
> screen shot of a computer Description automatically
> generated](./media/image26.png)

### タスク 5: トランザクション機能セットを機能ストアに登録する

> 1\. Azure ポータル (+++https://portal.azure.com+++)
> から、割り当てられたリソース グループの featureset で始まるストレージ
> アカウントに移動します。![A screenshot of a computer AI-generated
> content may be incorrect.](./media/image27.png)
>
> 2\. 左側のペインから「Access Control (IAM)」を選択します。「Add -\>
> Add role assignment」を選択します。![A screenshot of a computer
> Description automatically generated](./media/image28.png)
>
> 3\. +++Storage Blob Data Reader+++ を検索して選択します。![A
> screenshot of a computer Description automatically
> generated](./media/image29.png)
>
> 4.演習 1 で行ったのと同様のロールの割り当てを完了します。
>
> 5\. 同様に、+++Storage BLOB データ共同作成者+++ ロールを追加します。
>
> 6\. Azure Machine Learning Studio に戻ります。
>
> 7\.
> 機能セットアセットを機能ストアに登録して、他のユーザーと共有および再利用できるようにします。また、バージョン管理や具体化などのマネージド機能も利用できます。機能セットアセットには、先ほど作成した機能セット仕様への参照と、バージョンや具体化設定などの追加プロパティが含まれます。

8.  次のセルを実行して、トランザクション機能セットを機能ストアに登録します。.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### タスク 6: フィーチャーストアのUIを探索する

1\. ブラウザーで新しいタブを開き、Azure ML グローバル ランディング
ページ (+++https://ml.azure.com/home+++) に移動します。

2\. 左側のナビゲーションで \[Feature stores\] をクリックします。![A
screenshot of a computer Description automatically
generated](./media/image31.png)

> 3\. featurestore をクリックします。
>
> 注: feature store
> アセット（機能セットとエンティティ）の作成と更新は、SDK と CLI
> を通じてのみ可能です。UI を使用して feature store
> を検索/参照できます。 ![A screenshot of a computer Description
> automatically generated](./media/image32.png)

### タスク 7: 登録された特徴量を使用してトレーニングデータのデータフレームを生成する

1.  まず、観測データの調査から始めます。観測データは通常、学習データと推論データで使用されるコアデータです。これを特徴データと結合して、完全な学習データを作成します。観測データは、イベント発生時に取得されたデータです。このケースでは、取引ID、アカウントID、取引金額などのコアとなる取引データが含まれています。また、学習用であるため、ターゲット変数（is_fraud）も追加されています。

2.  セルを実行し、出力データを観察します。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3\.
次のセルを実行して、登録されている機能セットを取得し、その機能を一覧表示します。![A
screenshot of a computer program Description automatically
generated](./media/image34.png)

4\. 次のセルを実行してサンプル値を出力します。![A screenshot of a
computer Description automatically generated](./media/image35.png)

5\.
次のセルを実行します。このステップでは、トレーニングデータに含める特徴量を選択し、Feature
Store SDKを使用してトレーニングデータを生成します。![A screenshot of a
computer program Description automatically
generated](./media/image36.png)

6 . 次のセルを実行して、特徴データと観測データを使用してトレーニング
データフレームを生成します。![A screenshot of a computer program
Description automatically generated](./media/image37.png)

### タスク 8: トランザクション機能セットでオフラインマテリアライゼーションを有効にする

機能セットでマテリアライゼーションを有効にすると、バックフィルを実行したり、定期的なマテリアライゼーション
ジョブをスケジュールしたりできるようになります。

> 1\. 次のセルを実行し、yaml ファイルの spark.sql.shuffle.partitions
> を特徴データのサイズに応じて設定します。
>
> 2\. Spark 構成の spark.sql.shuffle.partitions
> は、特徴セットがオフラインストアにマテリアライズされる際に生成される
> parquet ファイルの数（1 日あたり）に影響を与える可能性のあるオプション
> パラメーターです。このパラメーターのデフォルト値は 200
> です。ベストプラクティスとしては、小さな parquet
> ファイルを多数生成しないようにすることです。特徴セットがマテリアライズされた後にオフライン特徴の取得が遅くなる場合は、オフラインストアの対応するフォルダーに移動し、小さな
> parquet ファイル（1
> 日あたり）が多すぎることが原因かどうかを確認し、このパラメーターの値を調整してください。
>
> 注:
> このノートブックで使用されているサンプルデータは小さいため、featureset_asset_offline_enabled.yaml
> ファイルではこのパラメーターは 1 に設定されています。 ![A screenshot
> of a computer Description automatically
> generated](./media/image38.png)

3\.
マテリアライゼーションとは、特定の特徴ウィンドウの特徴値を計算し、それをマテリアライゼーションストアに保存するプロセスです。特徴をマテリアライゼーションすることで、信頼性と可用性が向上します。すべての特徴クエリは、マテリアライゼーションストアからマテリアライゼーションされた値を使用します。このステップでは、18か月の特徴ウィンドウに対して1回限りのバックフィルを実行します。

4\.
次のコードセルは、定義された特徴ウィンドウの現在のステータス（なしまたは不完全）に基づいてデータをマテリアライゼーションします。実行してください。![A
screenshot of a computer Description automatically
generated](./media/image39.png)

5\.
次のセルに特徴セットのサンプルデータを出力してみましょう。実行してみましょう。出力情報から、データがマテリアライゼーションストアから取得されたことがわかります。トレーニング/推論データの取得に使用される
get_offline_features()
メソッドも、デフォルトでマテリアライゼーションストアを使用します。![A
screenshot of a computer Description automatically
generated](./media/image40.png)

## エクササイズ3: 特徴量を使用してモデルを実験およびトレーニングする

> このノートブックでは、以下の方法を学習します。
>
> •
> 既存の事前計算済み値を特徴量として使用し、新しいアカウント特徴量セット仕様のプロトタイプを作成します。次に、ローカルの特徴量セット仕様を特徴量ストアに特徴量セットとして登録します。このプロセスは、カスタム変換を含む特徴量セットを作成した最初のチュートリアルとは異なります。
>
> •
> 取引とアカウントの特徴量セットからモデルの特徴量を選択し、特徴量取得仕様として保存します。
>
> • 特徴量取得仕様を使用して新しいモデルをトレーニングするトレーニング
> パイプラインを実行します。このパイプラインは、組み込みの特徴量取得コンポーネントを使用してトレーニング
> データを生成します。

### タスク 1: 環境をセットアップする

1.  「Notebooks」ペインから、「Experiment and train models using
    features」ノートブックを開きます。

2.  「Configure
    session」をクリックし、以前のノートブックと同様にconda.yamlをアップロードします。

3.  最初のセルを実行してセッションを開始します。これには約10分かかります。

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4\. 次のセルで、\< your_user_alias \>
のプレースホルダーをフォルダー構造内のユーザー名に置き換えて、セルを実行します。![A
screenshot of a computer program Description automatically
generated](./media/image42.png)

5\. 次の3つのセルを実行してCLIをセットアップします。

6\.
次のセルはプロジェクトワークスペース変数を初期化します。これを実行して変数を初期化します。![A
screenshot of a computer Description automatically
generated](./media/image43.png)

7\.
次のセルはフィーチャストアの変数を初期化します。実行してください。![A
screenshot of a computer Description automatically
generated](./media/image44.png)

> 8.次のセルを実行して、フィーチャー
> ストア消費クライアントを初期化します。

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

### タスク 2: 事前計算されたデータからローカルでアカウント機能セットを作成する

事前計算済み特徴量をオンボーディングする場合、変換コードを記述することなく、特徴セット仕様を作成できます。特徴セット仕様とは、featurestore
に接続することなく、完全にローカル/開発環境で特徴セットを開発およびテストするための仕様です。このステップでは、特徴セット仕様をローカルで作成し、そこから値をサンプリングします。

1\. 以下のセルを実行して、アカウントのソースデータを探索します。

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2\.
次のセルを実行して、これらの事前計算された機能からローカルにアカウント機能セット仕様を作成します。![A
screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3\. 次のセルを実行して、機能セット仕様から Spark
データフレームを生成します。![A screenshot of a computer Description
automatically generated](./media/image49.png)

> 4.機能セット仕様を機能ストアに登録するには、特定の形式で保存する必要があります。アクション：以下のセルを実行した後、生成されたアカウントのFeatureSetSpecを確認してください。仕様を確認するには、ファイルツリーからこのファイルを開いてください：featurestore/featuresets/accounts/spec/FeatureSetSpec。次のセルを実行してください。![A
> screenshot of a computer program Description automatically
> generated](./media/image50.png)

### タスク 3: 未登録の機能をローカルで試し、準備ができたら機能ストアに登録する

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

1\. 次の 2 つのセルを実行して、ローカルでトレーニング
データを生成します。![A close-up of a computer code Description
automatically generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

> 2.次のセルを実行して、アカウントのフィーチャセットをフィーチャストアに登録します。ローカルでさまざまなフィーチャ定義を試し、サニティテストを行ったら、フィーチャストアに登録できます。そのためには、フィーチャセットのアセット定義をフィーチャストアに登録します。.

![A screenshot of a computer Description automatically
generated](./media/image54.png)

3\. 次の 2
つのセルを実行して、登録された機能セットと健全性テストを取得します。![A
screenshot of a computer Description automatically
generated](./media/image55.png)

### タスク 4: トレーニング実験を実行する

1\. 次のセルを実行して、SDK から機能を検出します。![A screenshot of a
computer Description automatically generated](./media/image56.png)

2\.
前の手順では、ローカルでの実験とテストのために、未登録の特徴量セットと登録済みの特徴量セットの組み合わせから特徴量を選択しました。これでクラウドでの実験の準備が整いました。選択した特徴量を特徴量取得仕様として保存し、学習/推論用の
mlops/cicd
フローで使用することで、モデルのリリース時の俊敏性が向上します。

3.次のセルを実行して、モデルの特徴量を選択します。![A screenshot of a
computer program Description automatically
generated](./media/image57.png)

4\.
次のセルを実行し、選択した機能を機能取得仕様としてエクスポートします。![A
screenshot of a computer program Description automatically
generated](./media/image58.png)

### タスク 5: パイプラインを使用してクラウドでトレーニングし、問題がなければモデルを登録します。

このステップでは、トレーニング
パイプラインを手動でトリガーします。本番環境では、ソース
リポジトリの特徴量取得仕様の変更に基づいて、CI/CD
パイプラインによってトリガーされる可能性があります。

1\. 次のセルを実行して、トレーニング パイプラインを実行します。![A
screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2\.
Studioの左側のペインで「Jobs」を右クリックし、新しいタブで開きます。実験「training_on_fraud_model」を選択します。![A
screenshot of a computer Description automatically
generated](./media/image61.png)

3\.
トレーニングジョブをクリックして詳細を確認します。実験は完了するまで約5～15分かかります。![A
screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4\.
完了するまでお待ちください。完了したら、左側のペインから「Models」を選択します。リストから「fraud_model」を選択します。これが今作成されたモデルです。![A
screenshot of a computer Description automatically
generated](./media/image64.png)

> 5.機能セットタブを選択します。ここでは、このモデルが依存するトランザクションとアカウントの機能セットの両方が表示されます。

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6\. +++https://ml.azure.com/home+++
でフィーチャーストアのUIを開きます。「フィーチャーストア」-\>「featurestore」を選択します。![A
screenshot of a computer Description automatically
generated](./media/image66.png)

7\. 左側のペインから「Feature
sets」を選択し、いずれかの機能セットを選択します。![A screenshot of a
computer Description automatically generated](./media/image67.png)

8\.
「Models」タブをクリックします。モデル登録時に指定された特徴量取得仕様に基づいて、特徴量セットを使用しているモデルのリストが表示されます。![A
screenshot of a computer Description automatically
generated](./media/image68.png)

概要:

このラボでは、マネージド フィーチャ ストアを使用してフィーチャ
セットを開発および登録し、フィーチャを使用してモデルをトレーニングする方法を学習しました。
