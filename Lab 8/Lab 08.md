# 實驗 08 – 使用提示流通過 RAG 實現 QA 數據生成

**目的：**

QA 數據生成是 RAG（檢索增強生成）創建過程的一部分，其中自動生成的 QA
數據集用於獲取 RAG 的最佳提示並獲取 RAG 的評估指標

在本實驗中，您將學習如何根據數據創建 QA 數據集。

預計持續時間 – 60 分鐘

## 練習 1：創建 AOAI 部署

在本練習中，我們將使用在上一個實驗室中創建的 Azure OpenAI 資源創建
gpt-35-turbo 模型部署。

1.  在 Azure 機器學習工作室中，從左窗格中選擇 “**Model Catalog**” 。搜索
    +++**gpt-35-turbo**+++ 並從模型列表中選擇 **gpt-35-turbo**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  確保在 **Azure OpenAI 資源**字段中選中 AOAI 資源
    **AOAI-PF@lab.LabInstanceId**。選擇 **Deploy** 以部署模型。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  接受 **Deployment name** 並選擇 **Deploy**。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  對 **text-embedding-ada-002** 重複模型部署，部署名稱為
    +++**text-embedding-ada-002-2**+++

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## 練習 2：設置環境

1.  從 Studio 的左側窗格中，選擇
    **Notebooks**。單擊用戶名旁邊的三個點，然後選擇 **Upload
    files**（上傳文件）。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

2.  瀏覽到 **C：\LabFiles** 並選擇 **qa_data_generation.ipynb**
    文件。選中 **I trust contents of this file** 複選框，然後單擊
    **Upload** 。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  打開筆記本，然後在 **Compute** （計算） 選項中選擇 **Serverless
    Spark Compute** （無服務器 Spark 計算）。

![A screenshot of a computer program Description automatically
generated](./media/image7.png)

4.  附加計算後，選擇 **Configure session** （配置會話） 以上傳 conda.yml
    文件並設置環境以使用它執行。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  選擇 **Python packages** -\>**Upload Conda file** -\>然後單擊
    **Browse**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  從 **C：\LabFiles** 中選擇 **conda.yml**，然後選擇 **Apply**。

> ![A screenshot of a computer program Description automatically
> generated](./media/image10.png)

## 練習 3：獲取 AzureML 工作區的客戶端

1.  執行 notebook 的第一個單元格以安裝依賴項

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

**注意：**這將需要 10 到 15 分鐘才能完成

2.  使用 az login 執行下一個單元格以**登錄**到 **Azure** CLI。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

3.  工作區是 Azure 機器學習的頂級資源，提供了一個集中位置來處理您在使用
    Azure 機器學習時創建的所有項目。在本節中，我們將連接到將在其中運行
    Job 的工作區。 MLClient 是您與 AzureML 交互的方式

4.  將 **Subscription ID** 的占位符替換為 +++@lab.Subscription
    ()+++、具有**Resource group name** 的 **Resource group**
    和下一個單元格中具有 +++**Azuremlws@lab.LabInstanceId+++** 的
    **Azure ML Workspace**，用於創建 MClient。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer Description automatically
generated](./media/image15.png)

5.  **執行**設置**連接名稱**的下一個單元格。如果您在創建連接時使用了任何其他名稱，請在此單元格中指定該值，然後執行。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

6.  將 **key** 值替換為 **Azure openAI key**，將 **target**
    值替換為我們之前保存的 Azure OpenAI 資源的 **Endpoint** 值。

替換值後**執行**單元格。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

7.  現在，您的工作區已連接到 Azure OpenAI，我們將確保 gpt-35-turbo
    模型已部署，可供推理使用。

8.  **執行**下一個單元格以設置模型和**部署**名稱。如果您在創建模型和部署時指定了不同的名稱，請替換
    model name （模型名稱） 和 deployment name （部署名稱） 的值。

![A screenshot of a computer code Description automatically
generated](./media/image18.png)

9.  最後，我們將部署和模型信息合併到 Azure ML 嵌入組件期望作為輸入的 uri
    形式中。**執行** next cell 以執行此作。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

## 練習 4：設置管道

AzureML
管道將多個組件連接在一起。每個組件定義輸入，即使用代碼生成的輸入和輸出的代碼。管道本身可以具有通過將各個子組件連接在一起而生成的輸入和輸出。為了處理您的數據以進行嵌入和索引，我們將多個組件鏈接在一起，每個組件都執行自己的工作流程步驟。

組件將發佈到註冊表
azureml，默認情況下，該註冊表應具有訪問權限，可以從任何工作區訪問它。在下面的單元格中，我們從
azureml 註冊表獲取組件定義。

1.  執行下一個單元格並確保它被執行而沒有任何問題。

![A screenshot of a computer code Description automatically
generated](./media/image20.png)

2.  每個組件都有文檔，這些文檔提供了組件用途和每個輸入/輸出的總體描述。例如，我們可以通過檢查
    Component 定義來瞭解 **data_generation_component**
    的作用。為此**執行**下一個單元格並觀察輸出。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

3.  下面是一個 Pipeline 是通過定義一個 python
    函數來構建的，該函數將上述組件、輸入和輸出鏈接在一起。該函數的參數是
    Pipeline 本身的輸入，返回值是定義 Pipeline 輸出的字典。確保 **next
    cell** 成功**執行**。

![A screenshot of a computer code Description automatically
generated](./media/image22.png)

![A screenshot of a computer program Description automatically
generated](./media/image23.png)

4.  下面的設置顯示了如何將不同的 git 和 data_source
    參數設置為僅處理來自較大的 AzureDocs git 存儲庫的 AzureML
    文檔，並確保處理每個文檔的源 url 以鏈接到公共託管的 URL 而不是 git
    url。

5.  執行接下來的兩個單元格並確保它們成功執行。

![](./media/image24.png)

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

## 練習 5：提交管道

1.  管道中每個步驟的輸出都可以通過 Workspace UI
    檢查，運行以下單元格後，單擊“詳細信息頁面”下的鏈接。

2.  執行下一個單元格並單擊輸出中的鏈接以查看流狀態

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  執行將在提示流中打開。探索流程的每個階段。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

6.  流程成功後，進入下一步。

## 練習 6：查看生成的 QA 數據

1.  執行接下來的 2 個單元格並查看 QA 數據的輸出。

![A screenshot of a computer code Description automatically
generated](./media/image29.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image30.png)

總結：

在本實驗中，我們學習了如何根據您的數據創建 QA 數據集。
