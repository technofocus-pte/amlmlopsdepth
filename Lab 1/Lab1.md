# **實驗 01 - 使用 Azure 機器學習工作室準備數據集、訓練和部署分類模型**

**目的**

此實驗室重點介紹指導你完成設置 Azure
機器學習環境、上傳、訪問和瀏覽數據以及使用 Azure
機器學習工作室訓練和部署圖像分類模型的過程。

預計持續時間 - 45 分鐘

## **練習 1：設置 Azure 機器學習工作區**

### **任務 1：同步 VM 時鐘**

1.  登錄到 VM 後，右鍵單擊屏幕右下角的時鐘。

2.  選擇 **Adjust date and time**（調整日期和時間）。

&nbsp;

3.  在打開的 個人設置 屏幕上，單擊 **Sync now** 下 其他設置。

> ![A screenshot of a computer Description automatically
> generated](./media/image1.png)

4.  這負責同步時間，以防自動同步不起作用。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image2.png)

### 任務 2：準備 Azure 資源

此任務側重於創建 Azure
機器學習工作區。您將瞭解如何設置專用工作區來有效地組織和管理他們的機器學習項目。此工作區充當協作、試驗和部署的中心樞紐。

#### 任務 2.1：註冊所需的資源提供程序 

1.  從 Azure 門戶主頁導航到分配的**訂閱**。

2.  在左側窗格中的 **Settings** （設置） 下選擇 Resource
    Providers（資源提供程序）。

3.  搜索
    +++Microsoft.StreamAnalytics+++，選擇名稱對應的三個點，然後單擊“**Register**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  重複上述步驟以註冊 +++Microsoft.Cdn+++ 和
    +++Microsoft.PolicyInsights+++

#### 任務 2.2：創建 Azure 機器學習工作區

1.  使用 **Resources**（資源）選項卡中的 **Username** （用戶名） 和
    **Password**（密碼）登錄到 Azure 門戶（地址為
    +++https://portal.azure.com+++）。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

2.  在 Azure 門戶主頁中，選擇“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

3.  在“**Create a resource** ”頁上，使用搜索欄查找 +++**Azure Machine
    Learning**+++，然後選擇“**Azure Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image6.png)

4.  在“**Marketplace**”下，單擊 **Create dropdown** ，然後選擇“**Azure
    Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

5.  提供以下信息以配置您的新工作區，然後單擊 **Review + create**。

    - **訂閱**: 選擇已**分配的 Azure 訂閱**

    - **資源組**: 選擇分**配給您的 Resource Group**。

> **工作區詳細信息：**

- **工作區名稱：**+++**Azuremlws@lab.LabInstance.Id**+++

- **區域：**選擇離您最近的區域（此處選擇**美國中北部**）

&nbsp;

- **容器註冊表：選擇 Create new （新建）。輸入
  +++azuremlcr@lab.LabInstance.Id+++**

**注意：**附加到資源名稱的數字是您的 Labinstance
ID，以確保唯一性。屏幕截圖將具有不同的編號，因為它們是唯一的。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  驗證通過後，單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  單擊 **Go to resource**（轉到資源）以查看新工作區。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **在 Microsoft.MachineLEarningServices |“概述”頁上，**選擇**“在
    Azure 機器學習工作室中使用模型”**下的**“啟動工作室”。**

![A screenshot of a software update Description automatically
generated](./media/image12.png)

#### 任務 2.3：創建計算

此任務演示了如何在 Azure
中創建計算資源。您將探索不同的計算選項，例如虛擬機或託管計算集群，並瞭解如何配置和預置資源以高效執行機器學習工作負載。

1.  **Azure Machine Learning Studio**
    打開後，單擊左側窗格中“**Manage**”下的“**Compute**”。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

2.  單擊 **Compute instances** 屏幕上的 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  在 Create compute instance （創建計算實例）
    屏幕上，輸入以下詳細信息。

    1.  計算名稱 – +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  虛擬機類型 – **CPU**

    3.  虛擬機大小 – 選擇 **Standard_E4ds_v4**

> 單擊**Review + Create**。

**注意：**記下此計算名稱以供以後使用。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

4.  單擊下一個屏幕中的 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

**注意：**計算大約需要 10 分鐘才能達到 Running 狀態。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

**重要提示：**Compute
啟動並運行後，您可以繼續執行下一個任務。但是，如果您要從實驗室執行中休息，請確保**停**止計算實例，並在休息後啟動時重新啟動它。

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

**練習總結：**

本練習使參與者熟悉設置 Azure
機器學習環境所涉及的基本步驟。通過這一系列任務，參與者學習了如何創建存儲帳戶、安裝機器學習
SDK、使用 Azure CLI 登錄、創建 Azure
機器學習工作區以及設置計算資源。通過完成本練習，您已獲得建立功能性 Azure
機器學習環境所需的基礎知識和實踐技能，使您能夠自信地開始機器學習項目。

## **練習 2 - 在 Azure 機器學習中上傳、訪問和瀏覽數據**

**目的**

在本練習中，您將學習如何：

- 將數據上傳到雲存儲

- 創建 Azure 機器學習數據資產

- 在 Notebook 中訪問數據以進行交互式開發

- 創建數據資產的新版本

機器學習項目的啟動通常涉及探索性數據分析
（EDA）、數據預處理（清理、特徵工程）以及構建機器學習模型原型以驗證假設。此原型設計項目階段具有高度交互性。它適合在
IDE 或 Jupyter 筆記本中進行開發，並帶有 Python
交互式控制台。本實驗介紹了這些想法。

我們正處於**機器學習項目工作流程**的**數據：探索和準備**階段。

![](./media/image19.png)

### 任務 1：準備 Azure 資源

**重要提示：**確保我們在上一個練習中創建的計算已啟動並正在運行。如果您要從實驗室執行中休息，請確保在休息後啟動時**停**止並重新啟動它。

#### 任務 1.1：上傳筆記本

1.  在 Azure
    機器學習工作室中，計算啟動並運行後，從左窗格中選擇“**Notebooks**”選項。
    ![](./media/image20.png)

2.  關閉 **What's new in Notebooks** 對話框。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image21.png)

3.  Notebook Files （筆記本文件） 窗格打開，結構為 **Users -\> \<
    UserName \>**。單擊用戶名旁邊的三個點，然後選擇 **Create new
    folder**（創建新文件夾）。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

4.  將文件夾名稱輸入為
    +++**Azuremlnotebooks**+++，然後單擊“**Create**”。

![A screenshot of a computer Description automatically
generated](./media/image23.png)

5.  創建文件夾後，單擊 **Azuremlnotebooks**
    文件夾的**菜單選項（**文件夾名稱旁邊的三個點），然後單擊 “**Upload
    files**” 。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

6.  選擇 **Click to browse and select files**
    （單擊以瀏覽並選擇文件）。瀏覽到 **C：\Labfiles** 下的
    **explore-data.ipynb，**然後單擊 **Open**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image25.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image26.png)

7.  選中複選框 **Open file after upload** and **I trust the contents of
    this file**（上傳後打開文件，我信任此文件的內容）。然後點擊
    **Upload**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

8.  這將打開上傳的 Notebook。

![A screenshot of a computer Description automatically
generated](./media/image28.png)

9.  如果 Studio 要求您進行身份驗證，請單擊
    **Authenticate**，因為這是您第一次登錄 Studio。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image29.png)

### 任務 2：上傳、訪問和瀏覽數據

#### 任務 2.1：下載數據

1.  在 **Notebooks** 的 **Files** 窗格下，單擊文件夾名稱
    **Azuremlnotebooks** 旁邊的 3 個點，然後單擊 **Create new folder**。

![](./media/image30.png)

2.  將文件夾名稱鍵入 +++**data**+++ ，然後單擊 **Create**。

![](./media/image31.png)

3.  文件夾創建成功後，單擊文件夾**數據**的菜單選項，然後選擇 **Upload
    files**。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

4.  選擇 **Click to browse and select file(s) ，**然後導航到
    **C：\Labfiles** 以選擇 **default_of_credit_card_clients.csv**
    文件，然後單擊 **Open** （打開）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image33.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image34.png)

5.  上傳完成後，通知下會顯示一條消息，指出**File uploaded successfully**
    。

![A close-up of a computer screen Description automatically generated
with low confidence](./media/image35.png)

#### 任務 2.2：創建工作區的句柄

1.  移回筆記本 （**explore-data**）。

2.  在我們深入研究代碼之前，您需要一種方法來引用您的工作區。您將為工作區的句柄創建ml_client。然後，您將使用
    ml_client 來管理資源和作業。

3.  在 **Create handle to workspace** 下的第一個單元格中，替換 **\<
    SUBSCRIPTION_ID \>**、**\< RESOURCE_GROUP \>** 和 **\<
    AML_WORKSPACE_NAME \>** 的占位符。

4.  將 \< RESOURCE_GROUP\> 替換為已分配的資源組的名稱。

5.  \<AML_WORKSPACE_NAME\> 替換為
    [+++**Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  將 \< SUBSCRIPTION_ID \> 替換為
    +++**@lab.CloudSubscription.Id**+++。

7.  單擊單元格左上角的 **Run cell**
    按鈕。執行成功後，在單元格底部查找刻度線。

![A screenshot of a computer Description automatically
generated](./media/image36.png)

#### 任務 2.3：將數據上傳到雲存儲

1.  Azure 機器學習數據資產類似於 Web
    瀏覽器書簽（收藏夾）。您可以創建一個數據資產，然後使用友好名稱訪問該資產，而不是記住指向您最常用數據的長存儲路徑
    （URI）。

2.  下一個筆記本單元格將創建數據資產。該代碼示例將原始數據文件上傳到指定的雲存儲資源。

3.  每次創建數據資產時，都需要為其提供唯一的版本。如果版本已存在，您將收到錯誤。在此代碼中，我們將使用時間在每次運行單元格時生成唯一版本。

4.  通過單擊單元格左上角的 Execute 按鈕來執行下一個單元格。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

5.  **“Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx”** 是顯示在單元格下方的輸出。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image38.png)

6.  單擊左側窗格中的
    **Data**，然後單擊由我們在上一步中執行創建的**credit-card** Data
    資產。瀏覽詳細信息並導航回 **Notebooks** 窗格。

![](./media/image39.png)

#### 任務 2.4：訪問筆記本中的數據

1.  返回筆記本，使用 **%pip** 命令執行單元，以在 **Jupyter** 內核中安裝
    **azureml-fsspec** Python 庫。

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image40.png)

2.  執行下一個單元格以訪問 **Pandas** 中的 CSV 文件。

3.  您將在單元格底部打印 **Data asset URI** ，並且還會顯示數據。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image41.png)

#### 任務 2.5：創建數據資產的新版本

1.  您可能已經注意到，數據需要稍微清理一下，才能適合訓練機器學習模型。它有：

    1.  兩個標頭

    2.  客戶端 ID 列;我們不會在機器學習中使用此功能

    3.  響應變量名稱中的空格

2.  此外，與 CSV 格式相比，**Parquet**
    文件格式成為存儲此數據的更好方式。Parquet
    提供壓縮，並維護架構。因此，要清理數據並將其存儲在 Parquet
    中，請執行下一個單元格。

3.  通過單元格底部的刻度線確保執行成功。

![](./media/image42.png)

4.  此表顯示了原始 **default_of_credit_card_clients.csv file**
    中的數據結構。CSV 文件。上傳的數據包含 23 個解釋變量和 1
    個響應變量，如下所示：

[TABLE]

5.  執行下一個單元格以創建數據資產的新版本（數據會自動上傳到雲存儲）。

6.  成功執行後，輸出顯示 **Data asset created. Name: credit_card,
    version: YYYY.MM.DD.xxxxxx_cleaned** 顯示在單元格之後。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image43.png)

![A screenshot of a computer Description automatically generated with
low confidence](./media/image44.png)

**重要:**

此 Python 代碼單元格為其創建的數據資產設置 **name** 和 **version**
值。因此，如果多次執行，此單元格中的代碼將失敗，而這些值沒有更改。固定
**name** 和 **version**
值提供了一種傳遞適用於特定情況的值的方法，而無需考慮自動生成或隨機生成的值。

7.  清理後的 parquet
    文件是最新版本的數據源。下一個單元格中的代碼首先顯示 CSV
    版本結果集，然後顯示執行時的 Parquet 版本。

8.  執行下一個單元格並檢查下面的結果。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image45.png)

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image46.png)

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image47.png)

![A picture containing text, screenshot, number, display Description
automatically generated](./media/image48.png)

9.  在下面查找已清理的數據 Data。

> ![](./media/image49.png)

**重要提示：**您可以從此處繼續進行下一個練習。但是，如果您要從實驗室執行中休息，請確保**停**止計算實例，並在從中斷中恢復時再次啟動它。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image50.png)

**鍛煉總結**

在本練習中，您學習了如何將數據上傳到雲存儲、創建 Azure
機器學習數據資產、在筆記本中訪問數據以進行交互式開發以及創建新版本的數據資產。

## **練習 3 - 在 Azure 機器學習工作室上訓練和部署圖像分類模型**

**目的**

在本練習中，您將學習

1.  使用Azure Machine Learning Studio Notebook
    UI連接到工作區並設置計算資源

2.  引入數據並準備用於訓練

3.  訓練用於圖像分類的模型

4.  查看和分析用於優化模型的 Metrics

5.  在線部署模型並進行測試

我們正處於**機器學習項目工作流程**的 **Train & validate model** 階段。

### ![A picture containing text, font, number, screenshot Description automatically generated](./media/image51.png)任務 1：上傳筆記本

1.  在 Azure 機器學習工作室的“**Notebooks**”頁中，單擊文件夾
    **AzureMLnotebooks** 的菜單選項，然後單擊“**Upload files** ”。

> ![A screenshot of a computer Description automatically
> generated](./media/image52.png)

2.  選擇，**Click to browse and select file(s)**， 瀏覽到
    **C：\Labfiles** 並選擇文件
    **azureml-getting-started-studio**（Jupyter 源文件）。

> ![A screenshot of a computer screen Description automatically
> generated with medium confidence](./media/image53.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

3.  選中複選框 **Open file after upload** ，然後單擊 **Upload**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image55.png)

4.  文件上傳成功後，它將在 Studio 中打開，並自動連接到處於 Running
    狀態的 Compute （cpu-cluster-fs）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

### 任務 2：連接到 Azure 機器學習工作區

在我們深入研究代碼之前，您需要連接到您的工作區。工作區是 Azure
機器學習的頂級資源，提供了一個集中位置來處理您在使用 Azure
機器學習時創建的所有項目。

我們使用 **DefaultAzureCredential**
來獲取對工作區的訪問權限。**DefaultAzureCredential**
應該能夠處理大多數情況。

*\# 工作區的句柄*

**from** azure.ai.ml **import** MLClient

*\# 鑒權包*

**from** azure.identity **import** DefaultAzureCredential

憑證 **=** DefaultAzureCredential()

*\# 獲取工作區的句柄。您可以在 ml.azure.com 的 workspace （工作區）
選項卡上找到相關信息*

ml_client **=** MLClient (

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\> “, \#
*這將看起來像xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  在上面的代碼（筆記本的第一個單元格）中，將
    **SUBSCRIPTION_ID**、**RESOURCE_GROUP name** 和
    **AML_WORKSPACE_NAME** 占位符替換為我們在前面的練習中保存的值。

2.  Notebook
    中的第一個單元格現在應如下所示。單擊第一個單元格左上角附近的 **Run**
    按鈕。

![A screenshot of a computer Description automatically
generated](./media/image57.png)

3.  通過在單元格底部查看單元格的狀態，確保單元格成功執行。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image58.png)

In \[ \]:

### 任務 3：上傳數據

若要運行 Azure 機器學習訓練作業，需要一個環境。

在本實驗室中，你將使用一個名為
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest
的現成環境，其中包含所有必需的庫（python、MLflow、numpy、pip 等）。

1.  執行下一個單元格中的代碼以上傳數據。

2.  確保一條消息 **Data asset created** 顯示為單元格的輸出。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image59.png)

### 任務 4：構建命令作業進行訓練

現在，你已擁有運行作業所需的所有資產，現在可以使用 Azure ML Python SDK
v2 自行生成作業了。我們將創建一個命令作業。

AzureML
命令作業是一種資源，用於指定在雲中執行訓練代碼所需的所有詳細信息：輸入和輸出、要使用的硬件類型、要安裝的軟件以及如何運行代碼。命令作業包含執行單個命令的信息。

#### 任務 4.1 ： 創建訓練腳本

1.  讓我們從創建訓練腳本 - **main.py** python 文件開始。

2.  執行下一個單元格並確保它成功執行。

![A picture containing text, font, line, screenshot Description
automatically generated](./media/image60.png)

3.  下一個單元格中的腳本處理數據的預處理，將其拆分為測試數據和訓練數據。然後，它使用此數據來訓練基於樹的模型並返回輸出模型。[MLFlow](https://mlflow.org/docs/latest/tracking.html) 將用於在管道運行期間記錄參數和指標。

4.  執行單元格並確保它與輸出一起成功執行，

**Writing ./src/main.py**

> ![A screenshot of a computer program Description automatically
> generated with low confidence](./media/image61.png)
>
> ![A screenshot of a computer program Description automatically
> generated with medium confidence](./media/image62.png)

5.  正如您在此腳本中所看到的，一旦模型被訓練，模型文件就會被保存並註冊到工作區。現在，您可以在推理終端節點中使用已註冊的模型。

#### 任務 4.2：配置命令

現在，您有一個可以執行所需任務的腳本，您將使用可以運行命令行作的通用命令。此命令行作可以直接調用系統命令或運行腳本。

1.  在這裡，您將使用輸入數據、拆分比率、學習率和註冊模型名稱作為輸入變量。

2.  從左窗格中，選擇 **Data** 並選擇 **credit-card-data** 。

![A screenshot of a computer Description automatically
generated](./media/image63.png)

3.  在 **Data sources** 部分下，查找 **Datastore URI**
    值並複製它。保存它以供下一步使用。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image64.png)

4.  在下一個單元格中，將

    1.  **path** 的值，其中包含在上一步中保存的**Datastore URI** 。

    2.  使用 +++**cpu-cluster-fs@lab.LabInstance.Id**+++ 我們在實驗 1
        中保存的集群的名稱）的 **compute** 值

5.  單擊 **Run**（運行）。確保 cell 成功執行。

![A screenshot of a computer program Description automatically
generated](./media/image65.png)

### 任務 6：提交作業

現在，可以提交作業以在 AzureML 中運行。**該作業需要 2 到 3
分鐘才能運行**。如果計算實例已縮減到零個節點，並且自定義環境仍在構建，則可能需要更長的時間（最多
10 分鐘）。

1.  使用以下命令執行單元格以提交作業。

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  單擊 **Run**（運行）。確保執行成功，並且 **Details Page**
    列下有指向結果的鏈接。

**注意：** 這大約需要 2 分鐘才能完成。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image66.png)

3.  在新選項卡中，打開結果的 **Details Page** 列下可用的鏈接。

### 任務 7：查看訓練作業的結果

1.  您可以通過**單擊提交作業後生成的 URL** 來查看訓練作業的結果。

> ![A screenshot of a computer Description automatically
> generated](./media/image67.png)

2.  或者，您也可以單擊左側導航菜單上的
    **Jobs**。作業是來自指定腳本或代碼段的多個運行的分組。運行的信息存儲在該作業下。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image68.png)

3.  **Overview** （概述） 頁面首先將 **Properties** （屬性） 窗格下的
    **Status** （狀態） 顯示為 **Running** （正在運行）。

4.  準備就緒後，狀態將更改為 **Completed** （已完成）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image69.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image70.png)

5.  選擇 **Metrics** （指標） 窗格以查看指標。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image71.png)

6.  選擇 **Images** （圖像） 選項卡以查看 training_confusion
    矩陣、精確率召回曲線和 roc 曲線。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image72.png)

1)  **Overview** 是您可以查看作業狀態的位置。

2)  **Metrics** 將顯示您在腳本中指定的指標的不同可視化效果。

3)  在**“圖像”**中，可以查看使用 MLflow 記錄的任何圖像項目。

4)  **子作業**包含子作業（如果已添加）。

5)  **Outputs + logs** 包含您進行故障排除或其他監控目的所需的日誌文件。

6)  **Code** 包含作業中使用的腳本/代碼。

7)  **Explanations （解釋）** 和 **Fairness （公平性）**
    用於查看您的模型在遵守負責任的 AI
    標準方面的表現。它們目前是預覽功能，需要安裝其他軟件包。

8)  在 **Monitoring** 中，您可以查看計算資源性能的指標。

### 任務 8：將模型部署為連線終端節點

訓練機器學習模型後，您需要部署它，以便其他人可以使用它來進行推理。為此，Azure
機器學習允許您創建終**端節點**並向其添加**部署**。

在此上下文中，終**端節點**是一個 HTTPS
路徑，它為客戶端提供了一個接口，用於向經過訓練的模型發送請求（輸入數據）並接收來自模型的推理（評分）結果。終端節點提供：

- 使用基於“密鑰或令牌”的身份驗證進行身份驗證

- TLS（SSL） 終止

- 穩定的評分 URI （endpoint-name.region.inference.ml.azure.com）

**部署**是託管執行實際推理的模型所需的一組資源。

#### 任務 8.1：創建連線終端節點

1.  現在，將機器學習模型部署為 Azure 雲（連線終結點）中的 Web 服務。

2.  從左側窗格中選擇 **Endpoints**。

![A screenshot of a computer Description automatically
generated](./media/image73.png)

3.  為 Real-time endpoints 選擇 **Create** for Real-time endpoints

![A screenshot of a computer Description automatically
generated](./media/image74.png)

4.  選擇 **credit_defaults_model**然後單擊 **Select**。

![A screenshot of a computer Description automatically
generated](./media/image75.png)

5.  選擇 **Standard_E4s_v3** 在 Virtual machine（虛擬機）下。提供
    Instance count （實例計數） 為 **1**

> 接受唯一 **Endpoint name** （終端節點名稱） 和 **Deployment name**
> （部署名稱） 的其他默認值，然後選擇 **Deploy** （部署）。

![A screenshot of a computer Description automatically
generated](./media/image76.png)

**注意：**終端節點創建大約需要 20 分鐘才能完成。

6.  完成後，Provisioning （預置） 狀態將更改為 **Succeeded** （成功）。

![A screenshot of a computer Description automatically
generated](./media/image77.png)

#### 任務 8.2.. 使用示例查詢進行測試

1.  在終端節點頁面中，選擇 **Test** （測試） 選項卡。

2.  將以下示例請求文件複製並粘貼到**Input data to test real-time
    endpoint**字段中，替換那裡已經存在的代碼。

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

3.  選擇 **Test** （測試） 並在 **Test result** （測試結果）
    下查看結果。

> ![A screenshot of a computer Description automatically
> generated](./media/image78.png)

### **任務 9：刪除終端節點**

1.  從左側窗格中，選擇 **Endpoints**。選擇我們創建的終端節點，然後單擊
    **Delete** （刪除）。

![A screenshot of a computer Description automatically
generated](./media/image79.png)

2.  單擊確認對話框中的 **Delete**。

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image80.png)

3.  查找有關成功刪除的通知。

![A picture containing text, screenshot, font, line Description
automatically generated](./media/image81.png)

**總結**

在本實驗中，你學習了如何在 Azure
機器學習工作室上訓練圖像分類模型，並將其部署為 Web 服務。
