# **ラボ 10 - Responsible AIダッシュボードを使用して機械学習モデルのパフォーマンスを向上させる**

**目的**

このラボでは、Responsible AI
ダッシュボードを使用して機械学習モデルをデバッグし、モデルのパフォーマンスをより公平性、包括性、安全性、信頼性、透明性のあるものに向上させる方法を実践的に学習します。

このラボでは、Azure Responsible AI (RAI)
ダッシュボードのモデル概要セクションの使い方を学びます。エラー分析ラボで作成されたコホートを使用して、あるコホートと別のコホートでモデルの動作が優れている理由を調査します。

予定所要時間：60分

## **エクササイズ 1: リソースの準備**

### タスク1: このラボのリポジトリのクローンを作成します

1\.
ブラウザからAzureポータル（https://portal.azure.com）にログインします。

2\.
Azureポータルのクラウドシェルアイコンをクリックして、クラウドシェルを開きます。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Azure Cloud Shell コマンド
    プロンプトで、以下のコマンドを実行して、Diabetes Hospital
    Readmission プロジェクトの github リポジトリを複製します。

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> これにより、リポジトリの内容がローカルに複製されます。![](./media/image2.png)

４. 以下のコマンドを実行してプロジェクト ディレクトリに変更します。

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### タスク2: Azure CLIを使用してログインする

1.  クラウド シェルから以下のコマンドを実行します。

**az login**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

> **２. コンソールで URL を開き、ブラウザにコードを入力します。**![A
> screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> 3\. Azure ログイン資格情報を選択します。![A screenshot of a phone
> Description automatically generated with medium
> confidence](./media/image5.png)
>
> 4\. 「Continue」をクリックします。![A screenshot of a computer error
> Description automatically generated with medium
> confidence](./media/image6.png)
>
> 5.ブラウザを閉じてAzureポータルに戻ります
>
> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)
>
> 6\. ログインの詳細がクラウド シェルに表示されます。![A screenshot of a
> computer Description automatically generated](./media/image8.png)

7\. 割り当てられたリソース グループに環境のデフォルトを設定します。

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **エクササイズ 2: モデルをトレーニングし、RAIダッシュボードを作成するためのジョブを実行する**

1.  以下のコマンドを実行して、トレーニング データセットを Azure Machine
    Learning ワークスペースに登録します。

> **az ml data create -f cloud/train_data.yml**

データアセットが作成され、その詳細がクラウド シェルに表示されます。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  以下のコマンドを実行して、テスト データセットを Azure Machine
    Learning ワークスペースに登録します。

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

> 3\.
> ジョブを実行するためのコンピューティングインスタンスを作成します。実行後に、後で使用するためにコンピューティング名（例：compute-xxxxxxxxxxxx）をコピーします。
>
> • 以下のコマンドを実行してコンピューティングを作成します。

**az ml compute create --name compute@lab.Lab Instance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

> 4\. Cloud Shell メニューで、\[Open editor\] { }
> ペインをクリックして、いくつかのファイルを編集します。![Open
> editor](./media/image13.png)

5\. RAI-Diabetes-Hospital-Readmission-classification
フォルダをクリックしてディレクトリを展開します。

![Expand directory](./media/image14.png)

6\. cloud/training_job.yml
ファイルに移動します。コンピューティング名のプレースホルダーを、先ほどコピーしたコンピューティングインスタンス名に置き換えます。

![Training job update](./media/image15.png)

> 7.ファイル内の任意の場所を右クリックし、\[Save\]オプションを選択してファイルを保存します。 

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8\. 次に、cloud/rai_dashboard_pipeline.yml
ファイルに移動します。コンピューティング名のプレースホルダーを、先ほどコピーしたコンピューティングインスタンス名に更新します。

![Rai pipeline update](./media/image17.png)

9\. ファイル内の任意の場所を右クリックし、\[Save\]
オプションを選択してファイルを保存します。

10\. ファイル内の任意の場所を右クリックし、\[Quit\]
オプションを選択してエディター ウィンドウを閉じます。![A screenshot of a
computer program Description automatically generated with medium
confidence](./media/image18.png)

10. Cloud Shell
    コマンドプロンプトに戻り、モデルをトレーニングするジョブを送信します。トレーニング中にジョブの実行ステータスが「Completed」に更新されるまで待ちます。そのためには、以下のコードブロックをコピーしてください。

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
> 注:
> このスクリプトが正しく貼り付けられない場合は、手動でコピーして貼り付けてください。
>
> 注: このスクリプトの実行には約3～5分かかります。![A screenshot of a
> computer Description automatically generated with medium
> confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)
>
> 11\. オプションとして、Azure Machine Learning Studio
> (https://ml.azure.com/) -\>
> ジョブから実行中のジョブのステータスを確認できます。![A screenshot of
> a computer AI-generated content may be
> incorrect.](./media/image21.png)
>
> 12\. トレーニングジョブが正常に完了したら、モデルをAzure Machine
> Learningワークスペースに登録します。以下のコマンドを実行します。

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> このコマンドは、モデルを AML
> ワークスペースに登録し、以下のスクリーンショットのようにクラウド
> シェルに詳細を提供します。
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

13\.
RAIダッシュボードを作成するためのジョブパイプラインを送信します。以下のコマンドを実行します。

az ml job create --file cloud/rai_dashboard_pipeline.yml

このコマンドはジョブを送信し、クラウド
シェルにパイプラインの初期ステージである「Preparing」状態が入力されます。

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

> 14\. Azure Machine Learning
> Studio（https://ml.azure.com/）にログインし、RAIダッシュボードを作成するパイプラインジョブを監視します。
>
> 15\.
> 「Pipelines」を選択します。RAIダッシュボードを作成するパイプラインジョブの進行状況を表示するには、ジョブの表示名をクリックします。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

16. 実験は実行状態になります。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

17. 完了するとステータスが「Completed」に変わり、RAI
    ダッシュボードが作成されます。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

18. 左側のナビゲーションにある「Models」タブをクリックします。次に、モデル名をクリックして詳細ページを開きます。

> ![](./media/image29.png)
>
> 19\. 上部のメニューで「Responsible AI」オプションを選択します。![A
> screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)
>
> 20\. これで、RAI ダッシュボードの使用を開始する準備が整いました。

## **エクササイズ 3: エラー分析:**

RAIダッシュボードのエラー分析セクションは、モデルのエラー率に寄与する特徴量グループのエラー分布を示すのに役立ちます。エラーは多くの場合、異なるデータサブグループ間で均等に分布していないため、エラー分析はエラー率が最も高い特徴量を特定するのに役立ちます。

### タスク1: モデルのエラーを検索します。

> このタスクでは、エラー分析を用いて学習済みモデルのエラーを検出し、その発生箇所を特定する方法を学びます。さらに、データのコホートを作成し、モデルのパフォーマンスが一部のコホートでは低く、他のコホートでは低い理由を調査する方法も学びます。
>
> 1\. 「Diabetes Hospital
> Readmission（糖尿病入院再入院）」をクリックします。![A screenshot of a
> computer Description automatically generated with medium
> confidence](./media/image31.png)
>
> 2.コンピューティングを選択します。

![](./media/image32.png)

#### **タスク1.1: 最もエラーが多いツリーパスのコホートを特定して作成する**

分析を始めるには、ルートノードを見ると、合計994個のテストデータのうち、モデルの評価中に168個の誤った予測が見つかったことがわかります。

1\.
エラー数が最も多いツリーパスを見つけます。ノードの赤色が濃いほど、エラー率が高くなります。

2\.
この場合、最も濃い赤色のツリーパスは、右下から2番目のリーフノードです。

![](./media/image33.png)

> ３.
> このノードをダブルクリックして、そのノードまでのパス全体を選択します。パスがハイライト表示され、パス内の各ノードのフィーチャ条件が表示されます。
>
> ４. エラー分析セクションの右上にある「Save as a new cohort
> button」ボタンをクリックして、選択したパスからコホートを作成します。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)
>
> ５. コホート名を「+++Err: Prior_Inpatient \>0; Num_meds \>11.50 & \<=
> 21.50+++」と入力します。
>
> 「Save」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **タスク1.2: エラーが最も少ないツリーパスのコホートを特定して作成する**

> 比較のため、エラー数が最も少ないツリーパスを持つ別のコホートを作成し、あるコホートでモデルが他のコホートと比較して優れたパフォーマンスを発揮する理由について洞察を得られるようにします。ツリーの左端にある、特徴量条件
> num_lab_procedures ≤ 56.50
> を持つリーフノードが、エラー数が最も少ないツリーのパスです。
>
> 1\. ノードをダブルクリックします。![A screenshot of a computer
> Description automatically generated with medium
> confidence](./media/image36.png)
>
> 2\. 「Save as a new
> cohort」をクリックします。このデータセットのフィルターは、「num_lab_procedures
> \<= 56.50、number_diagnoses \<= 6.50、prior_inpatient \<=
> 0.00」です。![A screenshot of a computer Description automatically
> generated with medium confidence](./media/image37.png)
>
> 3\. コホートに名前を付けます: +++Prior_Inpatient = 0; num_diagnoses
> \<= 6.50; ラボ_procedures \<= 56.50+++ し、「Save」をクリックします。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **タスク1.3: 特徴リストを使用して、モデルエラーに寄与する主な特徴を特定します。**

1.  Feature listをクリック

![](./media/image39.png)

2.  リストは、特徴量のエラーへの寄与度に基づいてソートされています。リストの上位に位置する特徴量ほど、モデルエラーへの寄与度が高くなります。

3.  当社の糖尿病入院再入院モデルでは、特徴量リストには、以下の特徴量がモデルのエラーの上位に寄与していることが示されています。

    - Age

    - num_medications

    - medicare

    - time_in_hospital

    - num_procedures

    - insulin

    - discharge_destination

### タスク2: ヒートマップを使用してエラーを見つける

> 特徴量リストから、年齢がエラーの要因として上位にランクインしていることがわかりました。そこで、「ヒートマップ」タブを使用して、どの年齢層の患者がモデルのパフォーマンスを低下させているのかを調べてみましょう。
>
> 1\. 「Error Analysis」の「Heat map」を選択します。![A screenshot of a
> computer Description automatically generated](./media/image40.png)
>
> 2\. 「Heat Map」タブの「Rows: Feature
> 1」ドロップダウンメニューで「Age」を選択し、モデルの誤差に年齢がどのような要因として作用しているかを確認します。
>
> 3\.
> 「Age」を選択すると、ダッシュボードにインテリジェンスが組み込まれており、考えられる条件に基づいて特徴量を複数のセルに分割していることがわかります。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)
>
> 4\. 各セルの上にマウスを移動すると、セルに表示されているデータ
> グループの正しい予測と誤った予測の数、エラー範囲、エラー率が表示されます。![A
> screenshot of a computer Description automatically
> generated](./media/image42.png)

5\.
60歳以上のセルでは、モデル予測が536件正解、126件誤答です。エラーカバレッジは73.81%、エラー率は18.79%です。

6\.
30～60歳のセルでは、モデル予測が273件正解、25件誤答です。エラーカバレッジは25.60%、エラー率は13.61%です。

7\. 30歳以下のセルでは、モデル予測が17件正解、1件誤答です。

今回の観察結果から、年齢がモデルの誤答に大きな役割を果たしていることが示されたため、次のラボでさらに分析を行うために、各年齢層ごとにコホートを作成します。

#### ***タスク2.1: 年齢層に基づいてコホートを作成する***

> 1\.
> 60歳以上のセルのパーセンテージボックスをクリックします。四角いセルの周囲に青い枠線が表示されます。
>
> 2\. 「Save as a new cohort」をクリックします。![A screenshot of a
> computer Description automatically generated](./media/image43.png)

3\. 「Save as a new cohort」ダイアログで、次の情報を入力します。

• コホート名 - +++年齢==60歳以上+++

「Save」をクリックします。![A screenshot of a computer Description
automatically generated](./media/image44.png)

4\. 手順 2 と 3 を繰り返して、他の 2 つの Age
セルごとにコホートを作成します。

- **Cohort \#4:** Name - **+++Age == 30–60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### タスク3: コホートリストを表示する

> 1\. Error
> Analysisセクションの右上隅にある設定歯車アイコンをクリックします。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)
>
> 2\. 作成したすべてのコホートのリストを含むCohort Settingsウィンドウ
> ペインが開きます。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## エクササイズ 4: RAIを使用してモデル分析を実行する

**このラボでは、Azure Responsible AI (RAI)
ダッシュボードのモデル概要セクションの使い方を学びます。エラー分析ラボで作成されたコホートを用いて、あるコホートと別のコホートでモデルの動作が優れている理由を調査します。**

## **エクササイズ 4.1: モデルの概要**

### タスク1: モデルのパフォーマンス メトリック テーブルを確認して比較する

1\. 「Error Analysis」の下までスクロールして、「Model
Overview」セクションを見つけます。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

> 2\. 「Model Overview」で、「Dataset
> Cohorts」ペインを選択します。作成された様々なコホートが、モデルメトリクスとともに表形式で表示されます。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

**3.エラーが最も多いコホート（Err: Prior_Inpatient \> 0; Num_Meds \> 11
and ≤ 21.50）とエラーが最も少ないコホート（Prior_inpatient = 0;
num_diagnose ≤ 6.50; Lab_procedures \< 56.50）を比較します。**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4\. グラフ上のボックス
プロット線の上にマウスを移動すると、測定の詳細が表示されます。![A
screenshot of a computer Description automatically
generated](./media/image50.png)

> 5\.
> 誤ったコホートの精度スコアが0.806であることに注目してください。これは悪い値です。偽陽性率は非常に低く、偽陰性率が高いです。つまり、モデルが予測する患者の大多数は、30日以内に再入院する患者ではなく、再入院しない患者を高い確率で予測していることになります。![A
> red line in a white sheet Description automatically
> generated](./media/image51.png)

6\.
次に、エラーが最も少ないコホートの指標を見てみましょう。精度スコアは0.94で、これはすべてのデータを含むモデルの全体的な精度スコアよりもはるかに優れています。ただし、このコホートの偽陽性率は0と低くなっています。![A
picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### タスク2: 確率分布図を調べる

> 1\. 下にスクロールして確率分布を確認します。
>
> 2\.
> 確率分布チャートは、各コホート内の患者が30日以内に再入院するかどうかを予測するモデルの確率を示しています。
>
> 3\. 3つのコホートすべてで、患者が再入院しない確率を比較します。
>
> 4\.
> 全患者のテストデータセットを含む「全データ」コホートを見ると、大多数の患者が30日以内に再入院しないことがわかります。再入院しない確率の中央値は0.854、上位四分位値は0.986と良好です。
>
> 5\. 次に、エラー率が最も高いコホート（Err: Prior_Inpatient \>0;
> Num_meds \>11.50 & \<=
> 21.50）では、確率はわずかに低く0.89、中央値は0.719です。
>
> 6\. 最後に、エラー率が最も低いコホート（Prior_Inpatient =
> 0、num_diagnoses \<= 6.50、ラボ_procedures \<=
> 56.50）では、患者が再入院しない確率の中央値は 0.90、上位四分位数は
> 0.986 です。![A screenshot of a computer Description automatically
> generated](./media/image53.png)
>
> 7\.
> 3つのコホートにおける患者の再入院確率を表示するようにグラフを変更するには、X軸の「ラベルを選択」ボタンをクリックします。
>
> 8\.
> ポップアップウィンドウペインで「確率：再入院」ラジオボタンをクリックします。
>
> 9\. 次に「Apply」ボタンをクリックします。![A screenshot of a computer
> Description automatically generated with medium
> confidence](./media/image54.png)
>
> 10\. 3つのコホートにおける患者の再入院確率を比較する![A screenshot of
> a graph Description automatically generated with low
> confidence](./media/image55.png)
>
> 11\.
> 3つのコホートの再入学確率は0.55未満であることがわかります。モデルエラーが最も少ないコホートの再入学確率は0.179と最も低く、エラーが最も多いコホートの再入学確率は0.543と最も高くなります。

### タスク3: メトリック視覚化チャートを確認する

> それでは、「Metric
> visualizations」ペインに切り替えて、モデルのパフォーマンスをより深く理解してみましょう。
>
> 1\. 「Metric visualizations」タブをクリックします。![A screenshot of a
> computer Description automatically generated with medium
> confidence](./media/image56.png)
>
> 2\. 別の指標を選択するには、X軸の「Choose
> metric」をクリックし、利用可能な指標のリストから「Precision
> score」を選択します。「Apply」ボタンをクリックします。
>
> 注:
> トレーニング済みモデルは分類問題であるため、RAIダッシュボードには分類指標のみが表示されます。
> ![](./media/image57.png)

3\.
グラフを見ると、すべてのテストデータコホートとエラーコホートにおいて、モデルのパフォーマンスが約70%の確率で正確であることがわかります。

4\.
入院歴がなく、診断数が7未満の患者の場合、エラーが最も少ないコホートの適合率スコアは0.94です。これは、正確性スコアと一致しています。![A
screenshot of a computer Description automatically generated with medium
confidence](./media/image58.png)

> 5\. 最後に、メトリックを「Recall」に変更して、コホート内の患者が 30
> 日以内に再入院することをモデルがどの程度正確に予測できたかを確認します。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6\.
再現率を見ると、再入院する患者について、全コホートにおいてモデルの予測が25%未満の確率で正しかったことがわかります。これは、30日以内に再入院する患者を予測する場合、モデルの予測が大部分の確率で正しくないことを示しています。

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### タスク4: 混同マトリックスを見てください

> 混同行列は、モデルが正しい予測を行っている割合を確認するのに役立ちます。これにより、患者が30日以内に再入院した場合と再入院しなかった場合のモデル学習状況が明らかになります。
>
> 1\. 「Confusion Matrix」タブをクリックします。
>
> 2\.
> 再入院していない患者の方が再入院した患者よりもモデルのパフォーマンスが向上していることがわかります。
>
> 3\.
> 偽陰性の数は真陰性の数より少なくなるはずです。これは、すべての患者データのうち、モデルが30日以内に再入院すると正しく予測できたのは24人だけだったことを意味します。

- The number of True Positive (TP) is: **802**

- The number of False Negative (FN) is: **159**

- The number of False Positive (FP) is: **9**

- The number of True Negative (TN) is: **24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **エクササイズ 2: 特集コホート**

> エラー率が最も高かったコホートでは、Prior_Inpatient
> の数が0日を超え、投薬数が11～22の患者が含まれ、モデルのエラー率が高かったため、Prior_Inpatient
> と Num_medications
> を詳しく調べることで、問題箇所を特定するのに役立ちます。このラボでは、Prior_Inpatient
> のみを分析します。
>
> 1\. 「Feature Cohorts」タブをクリックします。
>
> 2\.
> 「Feature(s)」ドロップダウンメニューで、リストを下にスクロールし、「prior_inpatient」チェックボックスをオンにします。これにより、3つの異なる特徴コホートとモデルのパフォーマンス指標が表示されます。![A
> screenshot of a computer Description automatically
> generated](./media/image62.png)
>
> 3\. prior_inpatient \<
> 3コホートのサンプルサイズは943です。これは、テストデータに含まれる患者の大多数が過去に3回未満しか入院していないことを意味します。このコホートにおけるモデルの精度は0.838で、良好です。
>
> 4\. テストデータでは、prior_inpatient ≥ 3かつ\<
> 6コホートに該当する患者はわずか39人です。モデルの精度は0.692で、良好ではありません。
>
> 5\.
> 最後に、テストデータでは、過去に6日以上の入院歴がある患者はわずか12人です。このコホートにおけるモデルの精度は0.75で、良好です。![A
> screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

### タスク1: 特徴確率分布

データセットコホートと同様に、「Probability
Distribution」を表示できます。

1\.
糖尿病患者の過去の入院回数が少ないほど、30日以内に再入院する可能性が低いことがわかります。

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### タスク2: 特徴メトリクスの視覚化

> 1\. 「Metrics visualization」を選択します。X軸で「Choose
> metric」ボタンをクリックします。次に、「Precision
> score」メトリクスを選択します。![A screenshot of a computer
> Description automatically generated with medium
> confidence](./media/image65.png)
>
> 2\. prior_inpatient \< 3
> の患者の適合率スコアは0.40と非常に悪いことがわかります。これは、モデルが行ったすべての予測のうち、このコホートにおいて正しかったのはわずか40%だったことを意味します。![A
> blue and white bar graph Description automatically
> generated](./media/image66.png)
>
> 3\. 他の2つのコホートの適合率スコアは良好です。
>
> 4\. 次に、X軸に「Recall score」メトリックを選択します。![A screenshot
> of a computer Description automatically generated with medium
> confidence](./media/image67.png)
>
> 5\. 一方、prior_inpatient \< 3
> の患者の再現スコアは0.013です。これは、テストデータ内の患者の大多数について、モデルが30日以内に患者が再入院するかどうかを正しく予測することが困難であることを意味します。![A
> picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **概要**

このラボでは、従来のモデルパフォーマンス指標（例：精度、再現率、混同行列など）が依然として非常に重要であることが示されます。RAIの洞察と従来のパフォーマンス指標を組み合わせることで、ダッシュボードはより詳細なレベルでモデルを分析およびデバッグするための包括的なツールを提供します。
