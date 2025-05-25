# **實驗 07：從 Azure 機器學習工作室開發和測試提示流**

**目的：**

在本實驗中，我們將瞭解在 Azure
機器學習工作室中使用提示流的主要用戶旅程。你將瞭解如何在 Azure
機器學習工作區中啟用提示流、創建和開發提示流、測試和評估流，然後將其部署到生產環境。

預計持續時間 – 60 分鐘

## 任務 1：準備 Azure 資源

### **任務 1.1：創建 Azure 機器學習工作區**

此任務側重於創建 Azure
機器學習工作區。您將瞭解如何設置專用工作區來有效地組織和管理他們的機器學習項目。此工作區充當協作、試驗和部署的中心樞紐。

1.  通過 +++<https://portal.azure.com>+++ 登錄到 Azure
    門戶，然後使用管理員租戶憑據登錄。

2.  在 Azure 門戶主頁中，選擇“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  在“**Create a resource**”頁上，使用搜索欄查找 +++Azure Machine
    Learning**+++**，然後選擇“**Azure Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  在“**Marketplace**”下，單擊“**Create**”**下拉列表**，然後選擇“**Azure
    Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  提供以下信息以配置您的新工作區：

    - **訂閱：**選擇已**分配的 Azure 訂閱**

    - **資源組：**選擇已**分配的資源組。**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **工作區詳細信息：**

- **工作區名稱：** +++**Azuremlws@lab.LabInstanceId**+++

- **區域：**選擇離您最近的區域（此處選擇 **North Central US**）

&nbsp;

- **容器註冊表：選擇 Create new
  （新建）。輸入+++azuremlcr@lab.LabInstanceId+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  配置完工作區後，選擇“**Review + Create**”。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  驗證通過後，單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  單擊 **Go to resource**（轉到資源）以查看新工作區。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  **在 Microsoft.MachineLEarningServices |Overview 頁上，**選擇**“Work
    with your model in Azure Machine Learning studio”**下的**“Launch
    studio”。**

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### 任務 1.2：創建計算

此任務演示了如何在 Azure
中創建計算資源。您將探索不同的計算選項，例如虛擬機或託管計算集群，並瞭解如何配置和預置資源以高效執行機器學習工作負載。

1.  **Azure Machine Learning Studio**
    打開後，單擊左側窗格中“**Manage**”下的“**Compute**”。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  單擊 **Compute instances** 屏幕上的 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  在 Create compute instance （創建計算實例）
    屏幕上，輸入以下詳細信息。

    1.  計算名稱 – +++**pfcompute**+++

    2.  虛擬機類型 – **CPU**

    3.  虛擬機大小 – 選擇**Standard_E4ds_v4**

> 單擊 **Review + Create**。

**注意：**記下此計算名稱以供以後使用。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  單擊下一個屏幕中的 **Create** 以創建計算。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**注意：**計算大約需要 10 分鐘才能達到 Running 狀態。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**重要提示：**Compute
啟動並運行後，您可以繼續執行下一個任務。但是，如果您要從實驗室執行中休息，請確保
**stop** 止計算實例，並在休息後啟動時重新啟動它。

### 任務 1.3：創建 Azure OpenAI 資源

1.  在 Azure 門戶 +++https://portal.azure.com+++
    中，搜索並選擇+++**AzureOpenAI**+++。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  單擊 **+ Create**。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  填寫以下詳細信息，然後單擊 **Next**。

- 資源組 - 選擇已分配的資源組

- Region （區域） – 選擇一個區域 （此處使用的是 North Central US）

- 名你 - +++**AOAI-PF@lab.LabInstanceId**+++

- 定價層 - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  在下一頁中接受默認值，然後單擊 **Review + submit** 頁面中的
    **Create**。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  部署完成後，單擊 **Go to resource**（轉到資源）。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  從左側窗格中選擇 **Keys and Endpoint** （密鑰和終端節點）。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  複製 **Key** 和
    **Endpoint**（終端節點），並將其保存在記事本中，以便在實驗的後續部分使用。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  在 **Azure Machine Learning Studio** 中，從左窗格中選擇 “**Model
    catalog**” ，然後選擇 “**gpt-4o**” 。

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  單擊 **Deploy** 以部署模型。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. 接受部署名稱，然後選擇 **Deploy**
    （部署）。請記下此名稱以備將來使用。

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## 任務 2：設置提示流連接

1.  在 Azure 機器學習工作室的左側導航窗格中，選擇 **Prompt
    flow**（提示流）。從菜單欄中選擇 **Connections**。選擇 **Create**
    旁邊的下拉列表，然後選擇 **Azure OpenAI**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  在 Add Azure OpenAI connection （添加 Azure OpenAI 連接）
    嚮導中，提供以下詳細信息，然後選擇 **Save** （保存）。

- 名稱 – +++**AoaiML_pf**+++

- 提供商 – 選擇**Azure OpenAI**

- 訂閱 ID – 選擇您**分配的訂閱**

- Azure OpenAI 帳戶名稱 – 選擇 **AOAI-PF@lab.LabInstanceId**

- Auth Mode （身份驗證模式） – 選擇 選擇 **API Key** （API 密鑰）

- API 密鑰 – 提供我們保存 **Azure OpenAI 資源的密鑰**

- API base – 提供我們從 **Azure OpenAI 資源**保存的終**端節點**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  檢查連接創建是否成功。

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## 任務 3：創建和開發提示流

1.  在 **Prompt flow** （提示流） 主頁的 **Flows** （流） 選項卡中，選擇
    **Create** （創建） 以創建提示流。**Create a new flow**
    （創建新流程）
    頁面顯示您可以創建的流程類型、您可以克隆以創建流程的內置示例以及導入流程的方法。

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  選擇 **WebClassification** 類別下的 **Clone**。

在 **Explore gallery** 中，可以瀏覽內置示例，並在任何磁貼上選擇 “**View
detail**” 以預覽它是否適合你的方案。

此實驗室使用 **Web 分類**示例來演練主要用戶旅程。

Web 分類是一個流程，演示了使用 LLM 進行多類分類。給定一個
URL，該流只需幾個鏡頭、簡單的摘要和分類提示，即可將 URL 分類為 Web
類別。例如，給定一個 URL https://www.imdb.com，它將 URL 分類為 Movie。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  接受為 **Folder name** （文件夾名稱） 填充的名稱，然後選擇 **Clone**
    （克隆）。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  計算會話對於流執行是必需的。計算會話管理應用程序運行所需的計算資源，包括包含所有必要依賴項包的
    Docker 映像。

5.  在流程創作頁面上，通過選擇 **Start compute session**
    （啟動計算會話） 來啟動計算會話。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**注意：**大約需要 **10 分鐘**才能使計算會話處於 Running 狀態。

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## 任務 4：檢查流程創作頁面

計算會話可能需要幾分鐘才能啟動。在計算會話啟動時，查看流程創作頁面的各個部分。

頁面左側的 **Flow** 或 flatten
視圖是主要工作區，您可以在其中通過添加或刪除節點、內聯編輯和運行節點或編輯提示來創作流。在
**Inputs** （輸入） 和 **Outputs** （輸出）
部分中，您可以查看、添加或刪除以及編輯輸入和輸出。

當您克隆當前 Web 分類示例時，輸入和輸出已經設置好了。流的輸入架構為
name： url;type： string，字符串類型的
URL。您可以將預設輸入值更改為其他值，例如 https://www.imdb.com 手動。

- 右上角的 **Files** 顯示流程的文件夾和文件結構。每個 flow
  文件夾都包含一個 flow.dag.yaml
  文件、源代碼文件和系統文件夾。您可以創建、上傳或下載用於測試、部署或協作的文件。

- 右下角的 **Graph** （圖形）
  視圖用於可視化流的外觀。您可以放大或縮小，或使用自動佈局。

您可以在 **Flow** （流） 或 flatten （展平）
視圖中內聯編輯文件，也可以打開 **Raw file** （原始文件）
**模式**切換並從 **Files** （文件）
中選擇一個文件，以在選項卡中打開該文件進行編輯。

您可以在 **Flow** （流） 或 flatten （展平）
視圖中內聯編輯文件，也可以打開 **Raw file** （原始文件）
**模式**切換並從 **Files** （文件）
中選擇一個文件，以在選項卡中打開該文件進行編輯。

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## 任務 5：設置 LLM 節點

對於每個 LLM 節點，您需要選擇一個 **Connection** 來設置 LLM API
密鑰。選擇您的 Azure OpenAI 連接。

根據連接類型，您必須從下拉列表中選擇 **deployment_name** 或 型號。對於
Azure OpenAI 連接，請選擇部署。 

1.  對於summarize_text_content，請填寫以下詳細信息。

Connection （連接） – 選擇 **AoaiML_pf**

Api – 選擇**chat**

部署名稱 — 選擇 **gpt-4o-2024-11-20**

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  以類似的方式為 LLM 節點設置連接**classify_with_llm**。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

3.  要測試和調試單個節點，請在 **Flow** （流） 視圖中選擇節點頂部的
    **Run** （運行） 圖標。您可以展開 **Inputs** 並更改流輸入
    URL，以測試不同 URL 的節點行為。

4.  運行狀態顯示在節點頂部。運行完成後，運行輸出將顯示在節點 **Output**
    部分中。

5.  移動到流的開頭，執行 **fetch_text_content_from url**並執行塊。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

**Graph** （圖形） 視圖還顯示單個運行節點狀態。

在 **Inputs** 部分下，將 **Value** 字段的值指定為
+++https://play.google.com/store/apps/details?id=com.spotify.music+++

從右上角選擇 **Run** 以測試和調試整個流程。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 任務 5：查看流輸出

您還可以設置流輸出，以便在一個位置檢查多個節點的輸出。流輸出可幫助您：

- 在單個表中檢查批量測試結果。

- 定義評估接口映射。

- 設置部署響應架構。

1.  在頂部橫幅或頂部菜單欄中選擇 **View
    outputs**（查看輸出）以查看詳細的輸入、輸出、流程執行和編排信息。

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  在 Outputs 屏幕的 Outputs
    選項卡上，請注意，該流使用**類別**和**證據**預測輸入 URL。 ![A
    screenshot of a computer Description automatically
    generated](./media/image42.png)

3.  選擇 **Outputs** （輸出） 屏幕上的 **Trace** （跟蹤）
    選項卡，然後選擇 **Node name** （節點名稱） 下的 **flow**
    （流），以在右側窗格中查看詳細的流概述信息。展開 **flow**
    並選擇任何步驟以查看該步驟的詳細信息。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**總結:**

在本實驗中，我們學習了如何通過簡單的摘要將 URL 分類為 Web 類別，並使用
Azure 機器學習工作室中的提示流對 URL 進行分類。
