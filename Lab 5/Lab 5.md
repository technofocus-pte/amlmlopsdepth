# **ラボ 05 - Azure Machine Learning スタジオでコード不要の自動機械学習を使用して需要を予測する**

**目的**

このラボでは、Azure Machine Learning Studio
の自動機械学習を使用して、コードを1行も書かずに時系列予測モデルを作成する方法を学びます。このモデルは、自転車シェアリングサービスのレンタル需要を予測します。

このラボではコードを一切記述せず、Studio
インターフェイスを使用してトレーニングを実行します。

想定所要時間 – 60分

## **エクササイズ 1: 環境を準備する**

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

## **エクササイズ 2: 自動MLジョブを作成する**

1\. Azure Machine Learning Studio
の左側のペインで、「Author」セクションの「Automated
ML」をクリックします。

2\. 「+ New Automated ML job」を選択します。![](./media/image4.png)

### **タスク 1: データ資産の作成**

> 1\. 実験名を +++experiment_forecast+++
> とし、他のデフォルトを受け入れて \[Next\] を選択します。![A screenshot
> of a computer Description automatically generated](./media/image5.png)

2\. タスク タイプとして「Time series forecasting」を選択し、「+
Create」をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3\. 「Create data asset」ページで、次の詳細を入力します。

1.  Name – +++**bikedata**+++

2.  Type – Tabular

> 「Next」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)
>
> 4.\[Data source\] ペインで \[From local files\] を選択し、\[Next\]
> をクリックします。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5\. \[Destination storage type\] で、workspaceblob を選択し、\[Next\]
を選択します。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  File or folderの選択で、Upload filesを選択し、C:\Lab
    filesフォルダからbike-no.csvを選択してをクNextリックします。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  設定とプレビュー
    フォームが次のように入力されていることを確認し、\[Next\]
    を選択します。

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

> 8\.
> スキーマフォームでは、この実験のデータをさらに設定できます。この例では、以下のトグルスイッチをオフにします。 

1.  **casual** と 

2.  **registered** 列

> 「Next」をクリックします。

これらの列は cnt 列の内訳であるため、含めません。

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)
>
> 9\.
> 「Review」フォームで情報を確認し、「Create」をクリックしてデータ資産の作成を完了します。![](./media/image13.png)
>
> 9\. 「Create a new Automated ML
> job」ページに戻ると、データアセット作成の成功メッセージが表示されます。
>
> 10\. 新しく作成したbikedataを選択し、「Next」をクリックします。
>
> 注: bikedata が表示されない場合は、データ アセット
> ペインを更新してください。
>
> ![](./media/image14.png)

### **タスク 2: ジョブの構成**

> 1\. タスク設定ページで以下の詳細を入力し、「View additional
> configuration settings」を選択します。
>
> Target column – **cnt(Integer)**
>
> Time column **– date (Date)**
>
> Autodetect forecast horizonを解除し、値を +++14+++ と入力します。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)
>
> 2\. 「Additional
> configuration」ペインで以下の詳細を入力し、「Save」をクリックします。

- Primary metric - **Normalized root mean squared error**

- Explain best model – **Enable**

- Blocked algorithms - **Extreme Random Trees**

> 追加の予測設定を展開します

- Autodetect Forecast target lags – **UnSelected**

- Autodetect Target rolling window size – **UnSelected**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3\. \[Limits\] を選択し、\[Experiment timeout(minutes)\] フィールドに
+++60+++ と入力します。![A screenshot of a test AI-generated content may
be incorrect.](./media/image17.png)

> 4\. 「Validate and test」で以下の値を選択し、「Next」を選択します。
>
> Validation type – **k-fold cross-validation**
>
> Number of cross validations – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)
>
> 5\.
> automl-compute（前のラボで作成したもの）を選択します。「Next」をクリックします。![A
> screenshot of a computer Description automatically
> generated](./media/image19.png)
>
> 6\. 詳細を確認し、「Submit training job」を選択します。![A screenshot
> of a computer Description automatically
> generated](./media/image20.png)
>
> 7.ステータスページには、初期ステータスとして「Running」と表示されます。ステータスを確認するには、ページを更新してください。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8\. トレーニングが完了すると、ステータスが「Completed」に変わります。

**注:** トレーニングの完了には約 30 ～ 45 分かかります。

## **エクササイズ 3: モデルを探索する**

> 1\.
> 「Models」タブに移動して、テスト済みのアルゴリズム（モデル）を確認します。デフォルトでは、モデルは完了するとメトリックスコアの順に並べられます。
>
> 2\.
> このチュートリアルでは、選択した正規化二乗平均平方根誤差メトリックに基づいて最も高いスコアを獲得したモデルがリストの一番上に表示されます。
>
> 3\.
> すべての実験モデルが終了するまで待つ間、完了したモデルのアルゴリズム名を選択して、そのパフォーマンスの詳細を確認します。![A
> screenshot of a computer Description automatically
> generated](./media/image22.png)
>
> 4\. Overviewをクリックして詳細を表示します。![A screenshot of a
> computer Description automatically generated](./media/image23.png)
>
> 5\. 「Metrics」タブをクリックして詳細を確認します。![A screenshot of a
> computer Description automatically generated with medium
> confidence](./media/image24.png)
>
> **重要：**このトレーニングが完了するまで、次のラボを実行し続けてください。トレーニングが完了したら、ここからこのラボを再開してください。![A
> screenshot of a computer Description automatically
> generated](./media/image25.png)

## **エクササイズ 4: 最適なモデルを特定する**

Azure Machine Learning Studio
の自動機械学習を使用すると、最適なモデルを数ステップで Web
サービスとしてデプロイできます。デプロイとは、モデルを統合し、新しいデータに基づいて予測を行い、潜在的な機会領域を特定できるようにすることです。

1.  ジョブが完了したら、画面上部のジョブ名を選択して親ジョブ
    ページに戻ります。

![](./media/image26.png)

2\. 「Best model
summary」セクションでは、正規化された二乗平均平方根誤差メトリックに基づいて、この実験のコンテキストにおける最適なモデルが選択されます。![A
screenshot of a computer Description automatically
generated](./media/image27.png)

3\. アルゴリズム名をクリックして開き、詳細を確認します。

4\. モデルはWebサービスとしてデプロイすることもできます。

**概要**

このラボでは、Azure Machine Learning スタジオの自動 ML
を使用して、自転車シェアのレンタル需要を予測する時系列予測モデルを作成しました。
