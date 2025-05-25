# **實驗 10 - 使用負責任 AI 儀錶板提高機器學習模型的性能**

**目的**

這個實驗室是為了讓學生實踐學習如何使用負責任的AI儀錶板來調試機器學習模型，以提高模型的性能，使其更加公平、包容、安全可靠和透明。 

在本實驗中，我們將探討如何使用 Azure 負責任 AI （RAI） 儀錶板的“**Model
Overview**”部分。我們將使用從誤差分析實驗室創建的隊列來研究為什麼模型在一個隊列中的行為優於另一個隊列。

預期持續時間 – 60 分鐘

## **練習 1：準備資源**

### 任務 1：克隆此實驗室的存儲庫

1.  在瀏覽器中，通過 <https://portal.azure.com> 登錄到 Azure 門戶

2.  單擊 Azure 門戶上的 Cloud Shell 圖標打開 **Cloud Shell**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  在 Azure Cloud Shell 命令提示符下，通過執行以下命令克隆 **Diabetes
    Hospital Readmission** 項目 github 存儲庫。

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> 這將在本地克隆存儲庫的內容。
>
> ![](./media/image2.png)

4.  通過執行以下命令切換到項目目錄。

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### 任務 2：使用 Azure CLI 登錄

1.  在 Cloud Shell 中，執行以下命令。

**az login**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

2.  在控制台中打開 URL，然後在瀏覽器中鍵入代碼。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

3.  選擇 **Azure** **登錄**憑據。

> ![A screenshot of a phone Description automatically generated with
> medium confidence](./media/image5.png)

4.  單擊 **Continue**（繼續）。

> ![A screenshot of a computer error Description automatically generated
> with medium confidence](./media/image6.png)

5.  關閉瀏覽器並返回到 Azure 門戶。

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  登錄詳細信息將顯示在 Cloud Shell 中。

> ![A screenshot of a computer Description automatically
> generated](./media/image8.png)

7.  將環境默認值設置為**分配的 Resource group**。

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **練習 2：運行用於訓練模型和創建 RAI 控制面板的作業**

1.  執行以下命令，將**訓練數據集**註冊到 Azure 機器學習工作區。

> **az ml data create -f cloud/train_data.yml**

將創建數據資產，並在 Cloud Shell 上顯示詳細信息。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  執行以下命令，將**測試數據集**註冊到 Azure 機器學習工作區。

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  創建用於運行作業的 **compute instance**
    。然後，複製運行結束時的計算名稱（例如
    **compute-xxxxxxxxxxxx**）以供以後使用。

- 執行以下命令以**創建計算**。

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

4.  在 Cloud Shell 菜單上，單擊 **Open editor { }** 窗格以編輯某些文件。

> ![Open editor](./media/image13.png)

5.  單擊 **RAI-Diabetes-Hospital-Readmission-classification**
    文件夾以展開目錄。

![Expand directory](./media/image14.png)

6.  導航到 **cloud/training_job.yml**
    文件。然後將計算名稱的占位符替換為您之前複製的**計算實例名稱**。

![Training job update](./media/image15.png)

7.  右鍵單擊文件中的任意位置，然後選擇 **Save** 選項以保存文件。 

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8.  接下來，導航到 **cloud/rai_dashboard_pipeline.yml**
    文件。然後，使用您之前複製的計算實例名稱更新**計算名稱**的占位符。

![Rai pipeline update](./media/image17.png)

9.  右鍵單擊文件中的任意位置，然後選擇 **Save** 選項以保存文件。

10. 右鍵單擊文件中的任意位置，然後選擇 **Quit** 選項以關閉編輯器窗口。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image18.png)

11. 返回 Cloud Shell
    命令提示符，提交作業以訓練模型。在訓練期間，等待作業將其運行狀態更新為
    **Completed**。複製下面的代碼塊來執行此作。

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
> **注意：**如果此腳本未正確粘貼，請手動複製並粘貼
>
> **注意：**此腳本的執行大約需要 3 到 5 分鐘。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

12. （可選）可以從 **Azure Machine Learning Studio**
    **(**<https://ml.azure.com/>**)** -\> **Jobs**
    中檢查正在運行的作業的狀態

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image21.png)

13. 訓練作業成功完成後，將模型註冊到 Azure
    機器學習工作區。執行以下命令來執行此作。

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> 此命令將模型註冊到 AML 工作區，並在 Cloud Shell
> 中提供詳細信息，如下面的屏幕截圖所示。
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

14. 提交作業管道以創建 **RAI 控制面板**。執行以下命令來執行此作。

az ml job create --file cloud/rai_dashboard_pipeline.yml

此命令將提交作業，並且 **Cloud Shell** 將填充管道的初始階段，即
Preparing 狀態。

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

15. [*https://ml.azure.com/*](https://ml.azure.com/) 登錄到 **Azure
    Machine Learning studio**，以監視用於創建 RAI 儀錶板的管道作業。

16. 選擇 **Pipelines**。要查看創建 RAI
    控制面板的管道作業的進度，請單擊作業 **Display name**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

17. 實驗將處於 **Running** 狀態。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

18. 完成後，狀態將更改為 **Completed** （已完成） 並創建 RAI 控制面板。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

19. 單擊左側導航欄中的 **Models**
    選項卡。然後單擊模型的名稱以打開詳細信息頁面。

> ![](./media/image29.png)

20. 在頂部菜單中選擇 **Responsible AI** 選項。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

21. 現在，您可以開始使用 RAI 控制面板了。

## **練習 3：錯誤分析：**

RAI 控制面板的 Error Analysis （錯誤分析）
部分有助於提供影響模型錯誤率的特徵組的錯誤分佈。錯誤通常不會均勻分佈在不同的數據子組中，錯誤分析可幫助您識別錯誤率最高的特徵。

### 任務 1：查找模型錯誤：

在本任務中，我們將探索如何使用錯誤分析在經過訓練的模型中查找錯誤，以確定錯誤的位置。此外，我們還將學習如何創建數據隊列，以調查為什麼模型在某些隊列中表現不佳，而在其他隊列中表現不佳。

1.  點擊名稱 **Diabetes Hospital Readmission**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

2.  選擇 **Compute** （計算）。

![](./media/image32.png)

#### **任務 1.1：為錯誤最高的樹路徑識別並創建同類群組**

要開始分析，您可以觀察到根節點顯示，在評估模型時，在總共 994
個測試數據中發現了 168 個錯誤的預測。

1.  查找錯誤數最多的樹路徑。節點中的紅色陰影越深，錯誤率越高。

2.  在我們的例子中，紅色最深的樹路徑是倒數第二個葉節點。

![](./media/image33.png)

3.  **雙擊**此**節點**以選擇通向該節點的**entire
    path**。這將突出顯示路徑並顯示路徑中每個節點的特徵條件。

4.  通過單擊“錯誤分析”部分右上角的 **Save as a new cohort**
    按鈕，從所選路徑創建同期群。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)

5.  將 **Cohort name 輸入**為 **+++Err: Prior_Inpatient \>0; Num_meds
    \>11.50 & \<= 21.50+++**

**點擊 Save （保存）。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **任務 1.2：為錯誤最少的樹路徑識別並創建同類群組**

出於對比目的，使用錯誤數最少的樹路徑創建另一個隊列，看看我們是否可以深入瞭解為什麼模型在一個隊列中表現良好，而不是在另一個隊列中表現良好。特徵條件為
**num_lab_procedures ≤ 56.50**
的**葉節點**位於樹的最左側，是錯誤最少的樹路徑。

1.  **雙擊**該節點**。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

2.  單擊 **Save as a new
    cohort**（另存為新同期群）。此數據集中的**篩選器**為：num_lab_procedures
    \<= 56.50、number_diagnoses \<= 6.50、prior_inpatient \<= 0.00。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

3.  將同類群組**命名**為**：+++Prior_Inpatient = 0; num_diagnoses \<=
    6.50; lab_procedures \<= 56.50+++** ，然後單擊**Save**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **任務 1.3：使用特徵列表確定導致模型誤差的主要特徵**

1.  單擊 **Feature list**（功能列表）。

![](./media/image39.png)

2.  該列表根據特徵對錯誤的貢獻進行排序。特徵在此列表中的位置越高，它對模型誤差的貢獻重要性就越高。

3.  在我們的糖尿病醫院再入院模型中，**特徵列表**表明以下特徵是模型誤差的主要貢獻者之一。

    - 年齡

    - num_medications

    - 醫療

    - time_in_hospital

    - num_procedures

    - 胰島素

    - discharge_destination

### 任務 2：使用熱圖查找錯誤

從 Feature List 中，**Age** 是導致錯誤最多的因素之一。因此，我們將使用
Heat map （熱圖） 選項卡來探索哪個年齡組的患者導致模型表現不佳。

1.  在 **Error Analysis** （錯誤分析） 下選擇 **Heat map** （熱圖）。

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)

2.  在 Heat Map 選項卡下，選擇 **Rows ： Feature 1** 下拉菜單中的
    **Age** 以查看它在模型誤差中的作用因素。

3.  選擇 **Age**
    後，我們可以看到儀錶板如何具有內置智能，將功能劃分為具有可能條件的不同單元格。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

2.  將鼠標**懸停**在每個單元格上，您可以看到單元格中表示的數據組的正確與錯誤預測的數量、錯誤覆蓋率和錯誤率。

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)

3.  **超過 60 年**的單元格有 **536** 個正確的模型預測和 **126**
    個錯誤的模型預測。錯誤覆蓋率為 **73.81%**，錯誤率為 **18.79%**

4.  **30-60 年**的單元格有 **273 個**正確的模型預測和 **25
    個**錯誤的模型預測。錯誤覆蓋率為 **25.60%**，錯誤率為 **13.61%**。

5.  **30 歲或更短**的單元格有 **17 個**正確和 **1 個**錯誤模型預測。

> 由於我們的觀察表明 **Age**
> 在模型的錯誤預測中起著重要作用，因此我們將為每個年齡組創建隊列，以便在下一個實驗中進一步分析。

#### ***任務 2.1：根據年齡組創建同類群組***

1.  單擊 **Over 60 years**
    單元格的百分比框。您將在方形單元格周圍看到一個藍色邊框。

2.  單擊 **Save as a new cohort**（另存為新同期群）。

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

3.  在 Save as a new cohort 對話框中，輸入

    - 群組名稱 - **+++Age==Over 60 year+++**

點擊 **Save** （保存）。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

4.  重複步驟 2 和 3，為其他兩個 Age 單元格中的每個單元格創建一個隊列。

- **隊列 \#4：**姓名 **- +++之前 == 30–60 歲+++**

- **隊列 \#5：**姓名 **- +++\<之前 = 30 歲+++**

### 任務 3：查看同期群列表

1.  單擊 Error Analysis 部分右上角的 **Settings** 齒輪圖標。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)

2.  這將打開 **Cohort Settings
    窗口窗格**，其中包含您創建的所有同類群組的列表。

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## 練習 4：使用 RAI 執行模型分析

在本實驗中，我們將探討如何使用 Azure 負責任 AI （RAI） 儀錶板的“**Model
Overview**”部分。我們將使用從誤差分析實驗室創建的隊列來研究為什麼模型在一個隊列中的行為優於另一個隊列。

## **練習 4.1：模型概述**

### 任務 1：查看和比較模型性能指標表

1.  向下滾動到 Error Analysis 下方，找到 Model Overview 部分。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

2.  在 Model Overview （模型概述） 下，選擇 **Dataset Cohorts**
    （數據集隊列） 窗格。這將顯示使用模型指標在表中創建的不同隊列。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

3.  比較錯誤最多的同期群 **Err： Prior_Inpatient \> 0;Num_Meds \> 11 and
    ≤ 21.50** 的誤差最小 **Prior_inpatient = 0;num_diagnose ≤
    6.50;lab_procedures \< 56.50**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4.  將鼠標懸停在圖表上的箱形圖線上可查看測量詳細信息。

![A screenshot of a computer Description automatically
generated](./media/image50.png)

5.  請注意，錯誤隊列的準確率分數為 0.806，這很糟糕。**False Positive**
    （假陽性率） **非常低**，而 **False Negative** （假陰性）
    值很**高**。這意味著，該模型預測的大多數患者在 30
    天內不會再次入院的預測率很高。

> ![A red line in a white sheet Description automatically
> generated](./media/image51.png)

6.  接下來，查看**誤差最小**的**隊列**的指標，準確率得分為
    0.94，這遠優於包含所有數據的模型的總體準確率得分。但是，此隊列的**假陽性**率也很低，為
    **0**。

![A picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### 任務 2：檢查概率分佈圖

1.  向下滾動以查看 **Probability** （概率） **分佈**。

2.  Probability distribution 圖表顯示模型的概率，用於預測隊列中的患者在
    30 天內是否會再次入院。

3.  比較所有 3 個隊列中患者未再次入院的概率。

4.  您將看到 **All data** cohort with all the patients test
    數據集顯示，大多數患者在 30
    天內不會再次入院，患者未再次入院的概率中位數為 0.854，上四分位數為
    0.986，這很好。

5.  接下來，錯誤率最高的同期群：**Err： Prior_Inpatient \>0;Num_meds
    \>11.50 & \<= 21.50**，顯示概率略低，為0.89，中位數為0.719。

6.  最後，錯誤率最低的隊列：**Prior_Inpatient = 0;num_diagnoses \<=
    6.50;lab_procedures \<= 56.50**，顯示患者未再次入院的概率的中位數為
    0.90，上四分位數為 0.986。

> ![A screenshot of a computer Description automatically
> generated](./media/image53.png)

7.  要更改圖表以顯示 3 個隊列中患者再次入院的概率，請單擊 x 軸上的
    **Choose Label**（選擇標簽）按鈕。

8.  選擇 **Probability： Readmitted** 單選按鈕。在彈出窗口窗格中。

9.  然後點擊 **Apply** 按鈕。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

10. 比較 3 個隊列中患者再次入院的概率

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image55.png)

9.  您會看到 3 個隊列再次入院的概率小於
    0.55。模型誤差數最少的隊列的概率最低，為
    0.179。錯誤最多的隊列的概率最高，為 0.543。

### 任務 3：查看指標可視化圖表

現在，讓我們通過切換到 Metric visualizations （指標可視化）
窗格來更深入地瞭解模型的性能。 

1.  單擊 Metric visualizations 選項卡。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

2.  要選擇其他指標，請單擊 x 軸上的 **Choose metric** （選擇指標）
    ，從其他可用指標列表中選擇 **Precision score**
    （精度分數）。然後點擊 **Apply** 按鈕。 

> **注意：**由於經過訓練的模型是一個分類問題，因此 RAI
> 控制面板將僅顯示分類指標。
>
> ![](./media/image57.png)

3.  通過查看圖表，您會發現所有測試數據同期群和錯誤同期群的模型性能在
    ~70% 的時間內都是正確的。 

4.  對於既往無住院史且診斷數量少於 7 的患者，**錯誤最少隊列**的
    **Precision 評分**率為 **0.94**。這與準確率分數一致。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image58.png)

5.  最後，將指標更改為 **Recall**
    （召回率），以查看模型正確預測隊列中的患者將在 30
    天內再次入院的程度。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6.  召回表明，對於再次入院的患者的所有隊列，**模型的預測正確率不到
    25%**。這表明，在嘗試預測將在 30
    天內再次入院的患者時，模型的預測在大多數情況下都是不正確的。

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### 任務 4：查看混淆矩陣

混淆矩陣有助於檢查模型正確做出正確預測的速率。這將揭示模型對患者在 30
天內再次入院與未再次入院的情況的學習效果。

1.  單擊 **Confusion matrix** 選項卡。

&nbsp;

2.  您將觀察到，與 **Readmitted** 相比，該**模型**對 **Not Readmitted**
    的患者表現**更好**。

3.  False Negative 的數量應小於 True
    Negative。這意味著在所有患者數據中，該模型只能正確預測 24 名患者在
    30 天內\<再次入院。

- 真陽性 （TP） 的數量為：**802**

- 假陰性 （FN） 的數量為：**159**

- 誤報 （FP） 數為：**9**

- 真陰性 （TN） 的數量為：**24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **練習 2：特徵同類群組**

由於錯誤最高的隊列中有 Prior_Inpatient \> 0 天的患者，並且藥物數量在 11
到 22
之間是模型錯誤率較高的地方，因此仔細研究Prior_Inpatient和Num_medications將有助於隔離存在問題的地方。在本實驗中，我們將僅分析
Prior_Inpatient。

1.  單擊 **Feature Cohorts** 選項卡。

2.  在 **Feature（s）** 下拉菜單下，向下滾動列表並選中
    **prior_inpatient** 複選框。這將顯示 3
    個不同的特徵隊列和模型性能指標。

> ![A screenshot of a computer Description automatically
> generated](./media/image62.png)

3.  **prior_inpatient \< 3** 隊列的樣本量為
    **943**。這意味著測試數據中的大多數患者過去住院時間少於 3
    次。該**模型**對該隊列的**準確率**為 **0.838**，這很好。

4.  測試數據中只有 39 名患者屬 **prior_inpatient ≥ 3 和 \< 6**
    隊列。該模型的準確率為 **0.692**，這並不好。

5.  最後，測試數據中只有 12 名患者的既往住院時間大於或等於 6
    天。此隊列的**模型準確率**為 **0.75** 是可以的。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

### 任務 1：特徵概率分佈

與 Dataset 同類群組類似，您可以查看 “Probability Distribution” 。

1.  您可以看到，糖尿病患者的prior_inpatient住院次數越少，患者在 30
    天內不再次入院的可能性就越大。 

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### 任務 2：功能量度可視化

1.  選擇 **Metrics visualization**（指標可視化）。在 x 軸上，單擊
    **Choose metric** 按鈕。然後選擇 **Precision score** （精度分數）
    指標。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image65.png)

2.  您看到**prior_inpatient \< 3** 患者的精確率評分為
    0.40，這非常糟糕。這意味著，在模型所做的所有預測中，只有 40%
    的預測對這個隊列是正確的。

> ![A blue and white bar graph Description automatically
> generated](./media/image66.png)

3.  其他 2 個隊列的精確率分數很好。

4.  接下來，為 x 軸選擇 **Recall score** metric （召回率指標）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image67.png)

5.  相反，您將看到**prior_inpatient \< 3** 的患者的召回率得分為
    0.013。這意味著，對於測試數據中的大多數患者，該模型難以正確預測患者是否會在
    30 天內再次入院。

> ![A picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **總結**
>
> 本實驗展示了傳統模型性能指標（例如準確率、召回率、混淆矩陣等）仍然非常重要。通過將
> RAI
> 洞察和傳統性能指標相結合，該控制面板為我們提供了一個全面的工具，可以在更精細的級別上分析和調試模型。
