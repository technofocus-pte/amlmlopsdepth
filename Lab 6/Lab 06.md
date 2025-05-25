# 實驗 06 - 為硬件數據集訓練最佳回歸模型

目的

在本實驗中，我們將介紹如何使用 AutoML 訓練回歸模型。我們將使用 Hardware
Performance
數據集來訓練和部署模型以用於推理場景。回歸的目標是預測硬件部件的某些組合的性能。

預期持續時間 – 60 分鐘

# 練習 0：準備好環境

### **任務 1：啟動 AML 工作區**

1.  登錄到 Azure 門戶，如果尚未登錄，請登錄
    +++[**https://portal.azure.com**](https://portal.azure.com)+++。

2.  從 Azure 門戶菜單中，選擇“**All resources**”。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  選擇 Azure 機器學習工作區 (**Azuemlws@lab.LabInstanceId**)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  單擊 **Launch studio**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  從左側窗格中選擇 **Compute** 以創建 Compute 實例。選擇 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

6.  提供以下詳細信息，然後單擊 Review + Create。

- 計算名稱 - +++**auto-compute**+++

- 虛擬機類型 – **CPU**

- 虛擬機 – **Standard E4ds_v4**

![](./media/image5.png)

7.  選擇 **Create** （創建） 以創建計算實例。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

### **任務 2：將筆記本上傳到 AML 工作區**

1.  單擊左側窗格中的 **Notebooks**。單擊 **Users**
    下**用戶名**旁邊的三個點，然後選擇 **Upload folder**。

![](./media/image7.png)

2.  選擇單擊以瀏覽並選擇文件夾，然後瀏覽 **C：\Labfiles** 以選擇文件夾
    **automl-regression-task-hardware-performance**，然後單擊
    **Upload**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image8.png)

3.  如果您看到一個彈出窗口，詢問 Upload 3 files to this site？，請單擊
    **Upload**。

![A picture containing text, screenshot, display, font Description
automatically generated](./media/image9.png)

4.  選中複選框 **I trust contents of these
    files**（我信任這些文件的內容），然後選擇 **Upload**（上傳）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

5.  打開 Notebook（. ipynb
    文件），**automl-regression-task-hardware-performance**。Notebook
    會自動連接到我們之前創建的計算。

![](./media/image11.png)

## **練習 1：連接到 Azure 機器學習工作區**

### **任務 1：導入所需的庫**

1.  執行 **1.1 Import the required libraries**
    下的單元格第一個單元格，通過單擊單元格左上角的 Run cell
    按鈕來導入此實驗室執行所需的庫。

2.  通過在單元格的左下角查找刻度線符號，確保執行成功。

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image12.png)

### **任務 2：配置工作區詳細信息並獲取工作區的句柄**

1.  在 1.2. 1.2. Configure workspace details and get a handle to the
    workspace 下的單元格中，替換

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP – **您分配的 Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  單擊單元格左上角的 Run cell
    選項，並確保執行成功後在左下角獲得一個刻度線符號。

3.  單元格下方顯示一個輸出，指出 **Found the config file in ：
    /config.json** 。

![](./media/image13.png)

### **任務 3：顯示 Azure ML 工作區信息**

1.  執行下一個單元格（顯示 Azure ML 工作區信息下方的單元格）。

2.  確保在單元格下方列為輸出的工作區、訂閱、位置和資源組的詳細信息都正確無誤。

![A screenshot of a computer program Description automatically
generated](./media/image14.png)

## **練習 2：使用輸入訓練數據的 MLTable** 

### **任務 1：創建 MLTable 數據輸入**

1.  執行下一個單元格（**2.1 Create MLTable data input**（2.1 創建
    MLTable 數據輸入）下的單元格）。

2.  確保執行成功。

![A picture containing text, font, screenshot, software Description
automatically generated](./media/image15.png)

## **練習 3：配置並運行 AutoML 回歸訓練作業**

1.  執行 **4.1 Configure 下的單元格並逐個運行 AutoML
    回歸訓練作業**，並確保每個單元格都成功執行。

2.  **4.2 Run the Command** 下的單元格提交 AutoML 作業。

![A screenshot of a computer program Description automatically
generated](./media/image16.png)

3.  您可以通過單擊左側窗格中的 **Jobs** 並選擇處於 Running
    狀態的實驗來檢查作業的狀態。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**注意：** 這大約需要 10 到 15 分鐘才能完成。

4.  Notebook 中的下一個單元格將等待 AutoMLjob 完成。

5.  執行它並等待執行完成，然後移動到下一個單元格。

![](./media/image19.png)

6.  只有在執行完成後，才能繼續執行下一步。

![](./media/image20.png)

7.  逐個執行接下來的 2 個單元格，用於檢索 url 和作業名稱。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

## **練習 4：檢索最佳試用 （最佳模型的試用/運行）**

1.  在此練習下的第一個單元格上方添加一個單元格。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

2.  複製以下代碼。單擊 **Run cell**。

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image23.png)

3.  繼續逐個執行接下來的 3 個單元格，分析每個代碼及其輸出。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image24.png)

4.  執行下一個單元格以**獲取父級運行**。

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image25.png)

5.  執行下一個單元格以**打印父標簽**。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image26.png)

6.  執行下一個單元格以**獲取 AutoML 最佳子運行**。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image27.png)

7.  執行下一個單元格以**獲取最佳模型運行的指標**。

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image28.png)

8.  執行接下來的 3 個單元格以**在本地下載最佳模型**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

## **練習 5：註冊最佳模型並部署**

### **任務 1：創建託管連線終端節點**

1.  執行此任務下的前 2 個單元格。

![](./media/image30.png)

2.  使用代碼執行下一個單元格

**ml_client.begin_create_or_update(endpoint).result()**

這將創建一個名為 **regression-\<Currentdate&time\>** 的在線終端節點。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image31.png)

3.  檢查是否有通知指出**端點 “regression-\<Currentdate&time\>”
    更新已完成**。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

### **任務 2：註冊最佳模型並部署**

1.  執行 Register best model 下的第一個單元並部署 -\> **Register
    model**，以註冊名為 **hardware-performance-model** 的模型。

2.  執行成功後，執行下一個單元格以檢索已註冊的模型 ID。

> ![](./media/image33.png)

### **任務 3：部署**

1.  在 Deploy （部署） 下的第一個單元格中，將值 **instance_type** 替換為
    **Standard_E4s_v3。**

2.  然後，執行 cell 以部署最佳模型。

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

3.  執行下一個單元以創建部署。

![A picture containing text, screenshot, line, font Description
automatically generated](./media/image35.png)

4.  **這大約需要 40 分鐘才能完成。**您還可以從 **Endpoints**
    （從左側窗格中選擇 **Endpoints ，**然後單擊您之前部署的
    **regression-XXXXXXX** 終端節點） 下檢查狀態。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  執行完成且部署成功後，該單元將輸出部署詳細信息。

![](./media/image37.png)

6.  此外，在 Endpoints details （終端節點）
    詳細信息頁面中，部署狀態將變為 **Succeeded**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

7.  執行筆記本中的下一個單元，以便部署獲取 100% 的流量。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image39.png)

8.  在 Endpoints details （終端節點詳細信息） 頁面中檢查 Live traffic
    allocation （實時流量分配） 為 100%。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

## **練習 6：測試部署**

1.  執行 Test the deployment（測試部署）下的單元。

2.  驗證輸出。

![](./media/image41.png)

3.  關注並執行其餘單元格以刪除終端節點。

![](./media/image42.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image43.png)

4.  從 Endpoints （終端節點） 選項卡下檢查終端節點的狀態。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

**總結**

在本實驗中，我們學習了如何

- 從 Python SDK 連接到 AML 工作區

- 使用 'regression（）' 工廠函數創建 AutoML 回歸作業。

- 通過提交/運行 AutoML 回歸訓練作業，使用 AmlCompute 訓練模型

- 獲取模型並使用它對預測進行評分
