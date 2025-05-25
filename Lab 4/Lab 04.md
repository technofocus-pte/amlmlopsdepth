# **實驗 04 - 在 Azure 機器學習工作室中使用無代碼 AutoML 訓練分類模型**

**目的**

在本實驗室中，我們將學習如何在 Azure 機器學習工作室中使用 Azure
機器學習自動化 ML 通過無代碼 AutoML
訓練分類模型。此分類模型預測客戶是否會在金融機構認購定期存款。自動化機器學習可快速迭代算法和超參數的多種組合，以幫助您根據所選的成功指標找到最佳模型。

預期持續時間 – 60 分鐘

我們正處於 Azure 機器學習的 **Deploy Model** 階段。

![](./media/image1.png)

## **練習 1：創建 Azure 機器學習工作區**

1.  登錄到 Azure 門戶 – +++**https://portal.azure.com**+++
    使用“**Resources**”選項卡中的憑據。

2.  在 Azure 門戶主頁中，選擇“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在“**Create a resource**”中，使用搜索欄查找 +++**Azure Machine
    Learning**+++。在“**Marketplace**”下選擇“**Azure Machine
    Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  在“**Marketplace**”下，單擊“**Create**”下拉列表，然後選擇“**Azure
    Machine Learning**”。

> ![A screenshot of a software Description automatically
> generated](./media/image4.png)

5.  提供以下信息以配置您的新工作區：

    - **訂閱：**選擇已**分配的 Azure 訂閱**

    - **資源組：**選擇分配的資源組

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

**工作區詳細信息：**

- **工作區名稱：+++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **區域：**在此處選擇“**North Central US** ”區域

- **容器註冊表：**選擇**Create new** （新建）。
  輸入**+++Azuremlcr@lab.LabInstanceId**+++

![A screenshot of a computer Description automatically
generated](./media/image6.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  配置完工作區後，選擇“**Review + Create**”。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

7.  驗證通過後，單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

8.  單擊 **Go to resource**（轉到資源）以查看新工作區。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

9.  **在 Microsoft.MachineLEarningServices | Overview
    頁上，**選擇**“Work with your model in Azure Machine Learning
    studio”**下的**“Launch studio”。**

![A screenshot of a computer Description automatically
generated](./media/image11.png)

## **練習 2：創建自動化 ML 作業**

1.  導航到 Azure 機器學習工作室選項卡。

2.  在左側窗格中，選擇 **Authoring** （創作） 部分下的 **Automated ML**
    （自動化 ML）。

3.  單擊 **+ New Automated ML job**（新建自動化 ML 作業）。

![](./media/image12.png)

### **任務 1：創建數據資產**

1.  在 **Basic settings** （基本設置） 頁面上，將 New experiment name
    （新實驗名稱） 指定為
    +++MarketingExperiment+++，接受其他默認值，然後單擊 **Next**
    （下一步）。

![](./media/image13.png)

2.  在任務類型和數據頁面中，**Select task
    type**下的**Classification**，然後在 **Select data**下選擇 **+
    Create。**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  在 Create data asset （創建數據資產） 頁面中，提供以下詳細信息。

- **名你** – +++marketingdata+++

- **類型** – **Tabular**

- 單擊 **Next**（下一步）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image15.png)

4.  在 **Data source** （數據源） 窗格中，選擇 **From local files**
    （從本地文件） 並單擊 **Next** （下一步）。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

5.  在 **Destination storage type** （目標存儲類型）
    中，選擇在創建工作區期間自動設置的默認數據存儲：**workspaceblobstore**。您可以將數據文件上傳到此位置，以使其可用於您的工作區。選擇
    **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

6.  在 **File or folder selection** （文件或文件夾選擇） 中，選擇
    **Upload files or folder** （上傳文件或文件夾） **\> Upload
    files。**從 **C：/Labfiles** 中選擇 **bankmarketing_train.csv**
    文件。選擇 **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

7.  上傳完成後，將根據文件類型填充 **Data preview** 區域。在
    **Settings** 窗體中，查看數據的值。然後選擇 **Next**（下一步）。

[TABLE]

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

8.  **Schema** 表單允許為此實驗進一步配置數據。對於此示例，請選擇
    **day_of_week** 的切換開關，以便不包含它。選擇 **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

9.  在 **Review** 表單中，驗證信息並選擇 **Create**
    以完成**數據資產**的創建。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. 返回 **Create a new Automated ML job** （創建新的自動化 ML 作業）
    頁面，將顯示數據資產創建的 **success** 消息。選擇創建的
    **marketingdata** 數據資產，然後單擊 **Next**。

> **注意：**如果未顯示 **marketingdata**，請單擊 Refresh 將其列出。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

### **任務 2：配置作業**

1.  在 **Task Settings** 頁面中，選擇 **y （String）** 作為 **Target
    列**，這是您要預測的內容。此欄顯示客戶是否申購定期存款。

2.  選擇 **View additional configuration settings**
    並填充字段，如下所示。這些設置是為了更好地控制訓練作業。否則，將根據試驗選擇和數據應用默認值。

- 主要指標 – AUCWeighted

- 解釋最佳模型 – 啟用

- 使用所有支持的模型 - 啟用

- 阻止的模型 – 無

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3.  選擇 **Limits**，然後為 **實驗超時（分鐘）** 字段輸入 +++**60**+++。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a test AI-generated content may be
incorrect.](./media/image25.png)

4.  在 **Validate and test** 下，提供以下值，然後單擊 **Next**。

- 驗證類型 - 選擇**k-fold cross-validation**

- 交叉驗證數量 – 選擇 **2**

![A screenshot of a computer Description automatically
generated](./media/image26.png)

5.  在 Compute 頁面中，選擇 Select compute type （選擇計算類型） 作為
    **Compute cluster**（計算集群），然後單擊 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

6.  在 **Create compute cluster** 窗格中，選擇以下詳細信息，然後單擊
    **Next**。

- 位置 – **美國中北部**（與 Azure 機器學習工作區的位置相同）

- 虛擬機層 – **Dedicated**

- 虛擬機類型 - **CPU**

- 虛擬機大小 - 選擇 **Standard_DS12_v2**

![A screenshot of a computer Description automatically
generated](./media/image28.png)

7.  在 Advanced settings （高級設置） 中，提供以下詳細信息並選擇
    **Create** （創建）。

- 計算名稱 - +++automl-compute+++

- 最小節點數 - 0

- 最大節點數 – 1

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

8.  計算預置成功後，選擇 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

9.  在 **Review** （查看） 頁面中，選擇 **Submit the training job**
    （提交訓練作業）。

> ![A screenshot of a computer Description automatically
> generated](./media/image31.png)

10. 當實驗準備開始時，**Overview** （概述） 屏幕將打開，其中 **Status**
    （狀態）
    位於頂部。此狀態會隨著實驗的進行而更新。工作室中還會顯示通知，以告知您實驗的狀態。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

> **注意：**完成培訓大約需要 40 分鐘。

## **練習 3：探索模型**

在訓練過程中，您可以瀏覽與之關聯的模型。

1.  導航到 **Models + child** jobs 選項卡，查看測試的算法（模型）。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

2.  選擇 **StandardScalerWrapper, XGBoostClassifier** 模型。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

3.  單擊 **Metrics** 並瀏覽 Metrics 選項卡下的詳細信息。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

4.  在等待所有實驗模型完成時，請選擇已完成模型的 **Algorithm name**
    （算法名稱） 以瀏覽其性能詳細信息。選擇 **Overview** 和 **Metrics**
    選項卡以獲取有關作業的信息。

> **重要提示：**模型訓練大約需要 40
> 分鐘才能完成。在進行中，請繼續進行下一個實驗。在狀態更改為
> **Completed**（已完成）後，繼續執行此實驗室。

## **練習 4：模型解釋**

模型解釋可以按需生成。模型解釋儀錶板是 **Explanations （preview）**
（解釋（預覽）） 選項卡的一部分，它總結了這些解釋。

1.  在 Models + child jobs （模型 + 子作業）
    選項卡（從父作業中）下，選擇 **MaxAbsScaler, LightGBM。**

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  選擇 **Explain model** （解釋模型） 選項卡。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

3.  在打開的 Explain model （解釋模型） 窗格中，選擇

    1.  選擇計算類型 - **Compute cluster**

    2.  選擇 AzureML 計算實例 - 選擇 **automl-compute**

選擇 **Create**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  此時將顯示 Success 消息。選擇 **Explanations（preview）**
    選項卡。此選項卡在可解釋性運行完成後填充。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

5.  展開左窗格。在 **Features** （功能） 下，選擇顯示 **raw** 的行。選擇
    **Aggregate** **feature importance**
    選項卡。此圖表顯示哪些數據特徵影響了所選模型的預測。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

在此示例中，持續**時間**似乎對此模型的預測影響最大。

## **練習 5：部署最佳模型**

自動化機器學習界面允許您將最佳模型部署為 Web
服務。部署是模型的集成，以便它可以預測新數據並確定潛在的機會領域。對於此實驗，部署到
Web 服務意味著金融機構現在擁有用於識別潛在定期存款客戶的迭代和可縮放 Web
解決方案。

試驗運行完成後，**Details** （詳細信息） 頁面將填充 **Best model
summary** （最佳模型摘要） 部分。在此實驗上下文中，**VotingEnsemble**
被認為是基於 **AUCWeighted** 指標的最佳模型。

1.  從左側窗格中選擇 **Jobs**，然後選擇您創建的實驗。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image41.png)

2.  單擊實驗的顯示名稱。

![](./media/image42.png)

3.  檢查狀態是否為 **Completed**。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

4.  試驗運行完成後，**Details** （詳細信息） 頁面將填充 **Best model
    summary** （最佳模型摘要） 部分。在此實驗上下文中，根據
    **AUC_weighted** 指標，**VotingEnsemble** 被認為是最佳模型。

> ![A screenshot of a computer Description automatically
> generated](./media/image44.png)

我們部署了此模型，但請注意，部署大約需要 20
分鐘才能完成。部署過程包括幾個步驟，包括註冊模型、生成資源以及為 Web
服務配置資源。

5.  選擇 **VotingEnsemble** 以打開特定於模型的頁面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

6.  選擇左上角的 **Deploy** 菜單，然後選擇 **Deploy to web service**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

7.  填充 **Deploy a model** （部署模型） 窗格，如下所示：

[TABLE]

> 單擊 **Deploy**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image47.png)

8.  模型屏幕上會顯示一條成功消息，指出已 **Model deployment is
    successfully triggered**，狀態為正在**Running**。

![](./media/image48.png)

9.  部署完成後，狀態將更改為 **Completed** （已完成）。

![A screenshot of a computer Description automatically
generated](./media/image49.png)

> 現在，您有一個可作的 Web 服務來生成預測。

## **練習 6：刪除資源**

### **任務 1：刪除端點**

1.  在 AML Studio 的左側窗格中，單擊 **Endpoints**（終端節點）。

2.  選擇終端節點 **my-automl-deploy**，然後單擊 **Delete**。

![](./media/image50.png)

3.  在 Delete real-time endpoint 對話框中選擇 **Delete**。

4.  刪除終端節點後，您應該會收到一條成功消息。

**總結**

在本實驗室中，我們學習了如何在 Azure 機器學習工作室中訓練沒有代碼的
AutoML 分類模型，並將最佳模型部署為 Web 服務。
