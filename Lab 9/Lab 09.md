# **實驗 09 - 使用 GitHub 設置 MLOps** 

**目的:**

Azure 機器學習允許與 **GitHub Actions**
集成，以自動執行機器學習生命週期。

在本實驗中，你將瞭解如何使用 Azure 機器學習來設置端到端 MLOps
管道，該管道運行線性回歸來預測紐約市的出租車費用。管道由組件組成，每個組件提供不同的功能，這些組件可以註冊到工作區、進行版本控制，並與各種輸入和輸出一起重複使用。.

預計持續時間：60 分鐘

我們正處於 Azure 機器學習的 MLOps 階段

![](./media/image1.png)

## **練習 1：準備 Azure 資源**

### **任務 1：創建 Azure 機器學習工作區**

1.  如果尚未登錄，請通過 +++<https://portal.azure.com>+++ 登錄到 Azure
    門戶。

2.  在 Azure 門戶主頁中，選擇“**+ Create a resource” 。**

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在“**Create a resource**”頁上，使用搜索欄查找 +++Azure Machine
    Learning+++

4.  選擇 **Machine Learning**。

> ![](./media/image3.png)

5.  在“**Marketplace**”下，單擊“**Create**”**下拉列表**，然後選擇“**Azure
    Machine Learning”。**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

6.  提供以下信息以配置您的新工作區：

    - **訂閱：**選擇已**分配的 Azure 訂閱**

    - **資源組：**選擇**分配給你的資源組。**

> **工作區詳細信息：**

- **工作區名稱：**+++**Azuremlws@lab.LabInstance.Id**+++

- **區域：**選擇離您最近的區域（此處選擇 **North Central US** ）

&nbsp;

- **容器註冊表：選擇 Create new
  （新建）。輸入+++azuremlcr@lab.LabInstance.Id+++**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  驗證通過後，單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

8.  單擊 **Go to resource**（轉到資源）以查看新工作區。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9.  **在 Microsoft.MachineLEarningServices |Overview 頁上，**選擇**“Work
    with your model in Azure Machine Learning studio”**下的**“Launch
    studio”。**

![A screenshot of a software update Description automatically
generated](./media/image9.png)

### **任務 2：創建計算**

1.  Azure 機器學習工作室打開後，單擊左側窗格中 “**Manage**” 下的
    “**Compute**” 。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

2.  選擇 **Compute clusters** 選項卡，然後單擊 **+ New**。

![](./media/image11.png)

3.  在 **Create compute cluster** （創建計算集群）
    屏幕上，輸入以下詳細信息。

    1.  位置 – 選擇您在其中創建 Azure 機器學習工作區的**區域**

    2.  虛擬機層 – **Dedicated**

    3.  虛擬機類型 – **CPU**

    4.  虛擬機大小 – 選擇**Standard_E4s_v3 (**選中從所有選項中選擇以查找
        VM 大小**)**

> 單擊 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

4.  在 **Advanced Settings** （高級設置） 頁面上，輸入以下詳細信息。

&nbsp;

1.  計算名稱 – +++**cpu-cluster@lab.LabInstanceId**+++

2.  最小節點數 – 0

3.  最大節點數 – 1

> 單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

**注意：**計算大約需要 10 分鐘才能達到 Running 狀態。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

## **練習 2：檢索 Azure 資源**

1.  在 Azure 門戶
    (<https://portal.azure.com>中，打開資源組並記下以下資源的名稱：

    1.  **Azure 機器學習工作區**

    2.  **應用程序洞察**

    3.  **密鑰保管庫**

    4.  **容器註冊表**

    5.  **存儲帳戶**

> 並將它們保存在本地記事本中，以便在配置文件中更新。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

## **練習 3：準備好 GitHub 賬戶和資源**

**注意：**如果您還沒有 GitHub 帳戶，請從此處創建一個
+++**https://github.com/**+++ -\> **Signup**。

### **任務 2：將存儲庫 mlops 演示分叉到您的 GitHub 帳戶中**

1.  打開瀏覽器並輸入此鏈接 -
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  點擊右上角的 **Fork**。

![A screenshot of a chat Description automatically generated with medium
confidence](./media/image16.png)

3.  這將打開 **Create a new fork** （創建新複刻） 頁面。單擊 **Create
    fork**（創建分叉）。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

4.  從 GitHub 項目中，選擇 **Settings** （設置）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.png)

5.  在 **Secrets and variables**（密鑰和變量）下選擇 **Actions**（作）。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

6.  選擇 **New repository secret** （新建存儲庫密鑰）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

7.  將此密鑰命名為 +++**AZURE_CREDENTIALS**+++，並將以下 **Service
    Principal** 輸出粘貼為密鑰的內容。此服務主體是預先創建的。選擇 **Add
    secret** （添加密鑰）。

> {
>
> "clientId": "+++@lab .Variable(spAppId)+++",
>
>   "clientSecret": "+++@lab .Variable(spClientSecret)+++",
>
>   "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
>   "tenantId": "+++@lab.CloudSubscription.TenantId+++",
>
>   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
>   "resourceManagerEndpointUrl": "https://management.azure.com/",
>
>   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
>   "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
>   "galleryEndpointUrl": "https://gallery.azure.com/",
>
>   "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![A screen shot of a computer Description automatically generated with
> low confidence](./media/image21.png)

8.  添加的密鑰**AZURE_CREDENTIALS**顯示在 **Repository
    secrets**（存儲庫密鑰）下。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

9.  單擊 **New repository secret**（新建倉庫密鑰）。

![](./media/image23.png)

10. 請提供以下詳細信息。

    1.  名你 – +++ARM_CLIENT_ID+++

    2.  秘密 – +++@lab .Variable(spAppId)+++

> ![A screenshot of a computer secret Description automatically
> generated with low confidence](./media/image24.png)

11. 對以下值重複步驟 9 和 10，創建其他 GitHub 機密。

    - +++ARM_CLIENT_SECRET+++ - +++@lab .Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab.CloudSubscription.TenantId+++

## **練習 4：配置機器學習環境參數**

1.  在密鑰頁面中，通過單擊左上角 GitHub ID 旁邊的 **mlops-v2-gha-demo**
    導航到存儲庫頁面。

![](./media/image25.png)

2.  在根目錄中選擇 **config-infra-prod.yml** 文件。單擊 **Edit**
    （編輯）（鉛筆圖標）。

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  更改值，

    1.  **命名空間 – mlopsliteXX**（將 XX 替換為隨機數）

    2.  **後綴 – c**

    3.  **location （位置） – 與您的工作區區域相同**

> 單擊 **Commit changes**（提交更改）。
>
> 在 **For pipeline reference 部分下**，將 Azure Resources
> 的值替換為我們在練習 2 中獲取和保存的**值**。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

4.  單擊 **Commit changes** （提交更改） 窗格中的 Commit changes
    （提交更改）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

5.  從 **.github/workflows** 打開
    **deploy-model-training-pipeline-classical.yml**。單擊
    **Edit**（鉛筆圖標）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

6.  在文件內容中，將 **Size** 的值替換為 **+++Standard_E4s_v3+++**

選擇 **Commit changes**（提交更改）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image30.png)

7.  從 **mlops/azureml/deploy/online**
    打開文件**online-deployment.yml**。點擊 **Edit**（鉛筆圖標）。

![](./media/image31.png)

8.  將 **instance_type** 的值替換為 **+++Standard_E4s_v3+++**。單擊
    **Commit changes**（提交更改）。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image32.png)

9.  在 **.github/workflows**
    下打開**tf-gha-deploy-infra.yml**文件。單擊“**Edit**”，將第 9 行和第
    14 行中的 Azure 替換為 +++CoursesTF+++。

選擇 **Commit changes**（提交更改）。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

10. 從頂部菜單欄中，選擇 **Actions** （作）。單擊**I understand my
    workflows, go ahead and enable them**。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

11. 這將顯示與您的項目關聯的預定義 GitHub 工作流。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

## **練習 5：部署 Machine Learning 基礎設施**

選擇 **tf-gha-deploy-infra.yml**。單擊 **Runworkflow**。

選擇

- Branch – **main** （分支 – 主）

選擇**Run workflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image36.png)

1.  這將使用 GitHub Actions 和 Terraform 部署機器學習基礎設施。

2.  跟蹤任務的狀態並確認執行成功。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

**注意：**此工作流程大約需要 5 分鐘才能完成。

## **練習 6：部署模型訓練管道**

接下來，您需要將模型訓練管道部署到新的 Machine Learning 工作區。

此管道將創建一個計算集群實例，註冊一個定義必要 Docker 映像和 python
包的訓練環境，註冊一個訓練數據集，然後啟動上一節中描述的訓練管道。 

1.  在 **tf-gha-deploy-infra.yml** 工作流頁面中，單擊 **Actions**
    （作）。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

2.  這將顯示與您的項目關聯的預定義 GitHub 工作流。從列表中選擇
    **deploy-model-training-pipeline**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

3.  單擊 **Run workflow** -\> **Run workflow**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image40.png)

4.  單擊剛剛啟動的管道以跟蹤進度。

![A picture containing text, software, web page, font Description
automatically generated](./media/image41.png)

5.  此管道大約需要 15 到 45 分鐘才能完成。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image42.png)

6.  下面是成功執行管道的屏幕截圖。

![](./media/image43.png)

7.  此執行將在 Machine Learning 工作區中註冊模型。

8.  在<https://ml.azure.com/> 登錄到 AzureMachineLearning
    工作室，然後單擊左側窗格中的 **Data**（數據），檢查是否已在此處添加
    **taxi-data**。這是作為工作流的 **register-dataset**
    作業的一部分完成的。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

9.  單擊左側窗格中的 **Jobs**（作業），然後選擇
    **taxi-fare-training**。這是在工作流的 **run-pipeline**
    作業中執行的。

![](./media/image45.png)

10. 選擇最新執行的 Display name （顯示名稱）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

11. 探索培訓中涉及的階段和細節。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

在 Machine learning
工作區中註冊經過訓練的模型後，您就可以部署模型進行評分了。

**總結**

在此實驗室中，我們學習了如何使用 Azure 機器學習設置端到端 MLOps
管道，該管道準備了數據並將模型訓練管道部署到新的機器學習工作區。
