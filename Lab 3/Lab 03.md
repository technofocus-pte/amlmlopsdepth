# 實驗 03 - 使用功能開發和註冊具有託管功能存儲和訓練模型的功能集

本實驗介紹如何使用自定義轉換創建特徵集規範。然後，它使用該特徵集生成訓練數據、啟用具體化並執行回填。具體化計算功能窗口的要素值，然後將這些值存儲在具體化存儲中。然後，所有特徵查詢都可以使用具體化存儲中的這些值。

如果不進行具體化，特徵集查詢會動態地將轉換應用於源，以便在返回值之前計算特徵。此過程適用於原型設計階段。但是，對於生產環境中的訓練和推理作，我們建議您具體化這些功能，以提高可靠性和可用性。

預期持續時間 – 50 分鐘

## 練習 1：分配所需的角色：

1.  在 Azure 門戶主頁中，從 “**Resources**” 選項卡中選擇分配的
    **Resource group**。從左側窗格中，選擇 **Access
    control（IAM）**。單擊 **Add** 旁邊的下拉列表，然後選擇 **Add role
    assignment**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  搜索 +++**AzureML Data Scientist**+++ 並選擇它。單擊 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在 成員 選項卡中，單擊 **+ Select members**，搜索您的**用戶名**
    +++@lab。CloudPortalCredential（User1） 的用戶名 +++。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  選擇您的**用戶名**，然後單擊 **Select** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  在接下來的 2 個屏幕中單擊 **Review + assign** （查看 + 分配）。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  分配完成後，將獲取 Added Role assignment 消息。

7.  重複相同的步驟集，添加角色 +++**Storage Blob Data Reader**+++
    和+++**Storage Blob Data Contributor**+++。

## 練習 2：開發功能集並註冊到託管功能存儲

本教程是託管特徵存儲教程系列的第一部分。在這裡，您將瞭解如何：

- 創建一個新的最小特徵存儲資源。

- 開發並本地測試具有特徵轉換功能的特徵集。

- 向特徵存儲註冊特徵存儲實體。

- 註冊您使用特徵存儲開發的特徵集。

- 使用您創建的特徵生成示例訓練 DataFrame。

- 在特徵集上啟用離線具體化，並回填特徵數據。

### 任務 1：準備好環境

1.  在 Azure 機器學習工作室的左窗格中，選擇 “**Authoring**” 下的
    “**Notebooks**” 。單擊用戶名旁邊的三個點，然後選擇 **Upload
    folder**。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2.  瀏覽並從 **C：\Labfiles** 中選擇 **featurestore** 文件夾，然後單擊
    **Upload**。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  導航到 **featurestore-\> notebooks-\>sdk_and_cli** 並打開筆記本 1.
    Develop-feature-set-and-register. ipynb

![](./media/image8.png)

4.  在 **Compute** （計算） 下選擇 **Serverless Spark Compute**
    （無服務器 Spark 計算）。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5.  選擇 **Configure session** （配置會話） 以使用先決條件配置會話。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6.  選擇 **Python packages -\> Upload Conda file**（Python 包上傳 Conda
    文件）。單擊 **Browse**（瀏覽）並從 **C：\Labfiles** 中選擇
    **conda.yml**，然後選擇 **Apply**（應用）。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

7.  **執行**筆記本的第一個單元格。這將安裝所有**依賴**項並完成其執行。這大約需要
    **10 分鐘**才能完成。

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Spark 會話啟動後，將 **User name**
    替換為您的用戶名並執行下一個單元格

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9.  執行接下來的 3 個單元格以設置 Azure CLI。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10. 在下一個單元格中，按照**輸出**中的步驟登錄到 **Azure**。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### 任務 2：創建最小特徵存儲

1.  **執行第一個**單元格以設置特徵存儲的名稱、位置和其他值。

![A screenshot of a computer program Description automatically
generated](./media/image19.png)

2.  **執行創建特徵存儲**的下一個單元格。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

3.  下一個單元**初始化 AzureML 特徵存儲核心 SDK 客戶端**。**執行**它。

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

### 任務 3：在此筆記本中對事務滾動聚合功能集進行原型設計和開發

1.  **執行**此部分下的第一個單元格以瀏覽**交易**源數據。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  執行第二個單元以在本地**開發事務功能集**。

![A screenshot of a computer code Description automatically
generated](./media/image23.png)

3.  執行下一個單元以從功能集規範**生成 spark 數據幀**。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  為了向特徵存儲註冊特徵集規範，需要以特定格式保存它。請檢查生成的事務
    FeaturesetSpec： 從文件樹中打開此文件以查看規範：
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

執行下一個單元格以導出為特徵集規範。

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

### 任務 4：註冊特徵存儲實體

1.  實體有助於實施最佳實踐，即在使用相同邏輯實體的功能集之間使用相同的聯接鍵定義。執行單元格以註冊特徵存儲實體。

> ![A screen shot of a computer Description automatically
> generated](./media/image26.png)

### 任務 5：向特徵存儲註冊事務特徵集

1.  在 Azure 門戶 （+++https：//portal.azure.com+++）
    中，導航到已分配的資源組下以 **featureset** 開頭的**存儲帳戶**。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  從左側窗格中，選擇 Access Control （IAM）。選擇 **Add -\> Add role
    assignment**（添加角色分配）。

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  搜索並選擇 +++**Storage Blob Data Reader**+++。

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

4.  完成類似於我們在練習 1 中所做的角色分配。

5.  同樣，添加 +++**Storage Blob Data Contributor**+++ 角色。

6.  導航回 Azure 機器學習工作室。

7.  您可以在特徵存儲中註冊特徵集資產，以便與他人共享和重複使用。您還可以獲得版本控制和具體化等託管功能。功能集資產引用了您之前創建的功能集規範以及其他屬性，例如版本和具體化設置。

8.  **執行** next cell 以向 Feature Store **註冊事務特徵集**。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### 任務 6：瀏覽特徵存儲 UI

1.  在瀏覽器中打開一個新選項卡，然後導航到 Azure ML 全域登陸頁（地址為
    +++https://ml.azure.com/home+++）。

2.  單擊左側導航欄中的 **Feature stores**。

![A screenshot of a computer Description automatically
generated](./media/image31.png)

3.  單擊 **featurestore**。

**注意：**只能通過 SDK 和 CLI
創建和更新特徵存儲資產（特徵集和實體）。您可以使用 UI
搜索/瀏覽特徵存儲。

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

### 任務 7：使用註冊的特徵生成訓練數據數據

1.  我們首先探索觀測數據。觀察數據通常是訓練和推理數據中使用的核心數據。然後將其與特徵數據聯接以創建完整的訓練數據。觀察數據是在事件發生期間捕獲的數據：在這種情況下，它具有核心交易數據，包括交易
    ID、賬戶
    ID、交易金額。在這種情況下，由於它是用於訓練的，因此它還附加了
    target 變量 （is_fraud）。

2.  **執行** cel land，觀察輸出數據。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  **執行**下一個單元格以獲取**已註冊的功能集**並**列出其功能**。

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

4.  執行下一個單元格以**打印示例值。**

![A screenshot of a computer Description automatically
generated](./media/image35.png)

5.  **執行**下一個單元格。在此步驟中，我們將**選擇希**望成為訓練數據一部分的**特徵**，並使用特徵存儲
    SDK 生成**訓練數據。**

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

6.  執行下一個單元格，以使用特徵數據和觀察數據生成訓練數據幀。

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

### 任務 8：在事務功能集上啟用離線具體化

在功能集上啟用具體化後，您可以執行回填或計劃定期具體化作業。

1.  執行下一個 cell，根據特徵數據大小在 yaml 文件中設置
    spark.sql.shuffle.partitions

2.  Spark 配置 spark.sql.shuffle.partitions 是一個 OPTIONAL
    參數，當功能集具體化到離線存儲中時，該參數可能會影響生成的 parquet
    文件數（每天）。該參數的默認值為 200。最佳做法是避免生成許多小的
    parquet 文件。如果 Feature Set 物化後，離線 Feature Retrieval
    變得很慢，請前往 offline store 中對應的文件夾查看是否是 parquet
    小文件過多（每天）的問題，並相應地調整該參數的值。

**注意：**此筆記本中使用的示例數據很小。所以這個參數在
featureset_asset_offline_enabled.yaml 文件中設置為 1。

> ![A screenshot of a computer Description automatically
> generated](./media/image38.png)

3.  具體化是計算給定特徵窗口的特徵值並將其存儲在具體化存儲中的過程。實現這些功能將提高其可靠性和可用性。所有特徵查詢都將使用具體化存儲中的具體化值。在此步驟中，您將對
    18 個月的功能窗口執行一次性回填。

4.  以下代碼單元將按已定義功能窗口的當前狀態 None 或 Incomplete
    **具體化數據**。**執行**它。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

5.  讓我們**打印**來自下一個單元格中的特徵集的**樣本數據**。**執行**它。您可以從輸出信息中注意到，數據是從材料存儲中檢索的。get_offline_features（）
    方法也將默認使用具體化存儲。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 練習 3：使用特徵試驗和訓練模型

在此筆記本中，您將瞭解如何：

- 通過使用現有的預計算值作為特徵，對新的 accounts
  特徵集規範進行原型設計。然後，將本地特徵集規範註冊為特徵存儲中的特徵集。此過程與第一個教程不同，在第一個教程中，您創建了一個具有自定義轉換的功能集。

- 從 transactions 和 accounts
  特徵集中為模型選擇特徵，並將其另存為特徵檢索規範。

- 運行使用特徵檢索規範訓練新模型的訓練管道。此管道使用內置的特徵檢索組件來生成訓練數據。

### 任務 1：設置環境

1.  在 Notebooks 窗格中，打開筆記本 **Experiment and train models using
    features**（使用特徵試驗和訓練模型）。

2.  單擊 **Configure session** 並上傳
    **conda.yaml**，類似於我們對早期筆記本所做的方法。

3.  **執行第一個**單元格以啟動會話。這大約需要 10 分鐘。

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4.  在下一個單元格中，將占位符替換為 **\< your_user_alias \>**
    替換為您在文件夾結構中的**用戶名**，然後**執行**該單元格。

![A screenshot of a computer program Description automatically
generated](./media/image42.png)

5.  **執行**接下來的 **3 個**單元格以**設置 CLI。**

6.  下一個單元格初始化項目工作區變量。**執行**它以**初始化變量**。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

7.  下一個單元格初始化特徵存儲變量。執行它。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

8.  執行下一個單元格以**初始化特徵存儲使用客戶端**。

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

### 任務 2：從預計算數據本地創建賬戶功能集

對於載入預計算功能，您可以創建功能集規範，而無需編寫任何轉換代碼。Featureset
規範是一種在完全本地/開發環境中開發和測試功能集的規範，無需連接到任何功能存儲。在此步驟中，您將在本地創建特徵集
spec 並從中採樣值。

1.  執行以下單元格以**瀏覽帳戶的源數據**。

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  執行下一個單元格，從這些預先計算的特徵中**創建** local
    中的**賬戶特徵集規範**。

![A screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3.  **執行**下一個單元以從功能集規範**生成 spark 數據幀。**

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  為了向特徵存儲註冊特徵集規範，需要以特定格式保存它。作：運行以下單元格後，請檢查生成的帳戶
    FeatureSetSpec：從文件樹中打開此文件以查看規範：featurestore/featuresets/accounts/spec/FeatureSetSpec。**執行**下一個單元格。![A
    screenshot of a computer program Description automatically
    generated](./media/image50.png)

### 任務 3：在本地試驗未註冊的特徵，並在準備就緒時向特徵存儲註冊

在開發功能時，您可能希望先在本地測試/驗證，然後再註冊到功能存儲或在雲中運行訓練管道。在此步驟中，您將根據本地未註冊特徵集
（賬戶） 和特徵存儲中註冊的特徵集 （交易） 的特徵組合生成 ML
模型的訓練數據。

1.  **執行**下一個單元格以**選擇模型的特徵。**

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

2.  **執行**接下來的 2 個單元格以在本地**生成訓練數據。**

![A close-up of a computer code Description automatically
generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

3.  **執行**下一個單元格以將 **accounts
    功能集註冊**到特徵庫。在本地試驗不同的功能定義並對其進行健全性測試後，您可以將其註冊到功能存儲中。為此，您將向特徵存儲註冊特徵集資產定義。

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  **執行**接下來的 2 個單元格以獲取註冊的 featureset 和健全性測試。

![A screenshot of a computer Description automatically
generated](./media/image55.png)

### 任務 4：運行訓練實驗

1.  執行下一個單元格以從 SDK 中發現功能。

![A screenshot of a computer Description automatically
generated](./media/image56.png)

2.  在前面的步驟中，您從未註冊和已註冊的功能集組合中選擇了功能，用於本地實驗和測試。現在，您已準備好在雲中進行實驗。將所選特徵保存為特徵檢索規範，並在
    mlops/cicd 流程中將其用於訓練/推理，可以提高交付模型的敏捷性。

3.  **執行**下一個單元格以**選擇模型的特徵。**

![A screenshot of a computer program Description automatically
generated](./media/image57.png)

4.  **執行**下一個單元，並將所選特徵導出為**特徵檢索規範。**

![A screenshot of a computer program Description automatically
generated](./media/image58.png)

### 任務 5：使用管道在雲中訓練，並在滿意時註冊模型

在此步驟中，您將手動觸發訓練管道。在生產場景中，這可能是由 ci/cd
管道根據源存儲庫中功能檢索規範的更改觸發的。

1.  **執行** next cell 以**運行訓練管道。**

![A screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2.  在工作室的左側窗格中，右鍵單擊 **Jobs**
    並在新選項卡中打開。選擇實驗，**training_on_fraud_model**。

![A screenshot of a computer Description automatically
generated](./media/image61.png)

3.  單擊 **training job** 並瀏覽詳細信息。實驗大約需要 5 到 15
    分鐘才能完成。

![A screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4.  等待它完成。完成後，從左側窗格中選擇 **Models**。從列表中選擇
    **fraud_model**。這是現在創建的模型。

![A screenshot of a computer Description automatically
generated](./media/image64.png)

5.  選擇 **Feature sets** （功能集）
    選項卡。在這裡，您可以看到此模型所依賴的 **transactions** 和
    **accounts** 特徵集。

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6.  以 +++https://ml.azure.com/home+++ 打開**feature store UI**。選擇
    **Feature stores -\> featurestore**。

![A screenshot of a computer Description automatically
generated](./media/image66.png)

7.  從左側窗格中選擇 **Feature
    sets**（功能集），然後選擇任意一個**功能集**。

![A screenshot of a computer Description automatically
generated](./media/image67.png)

8.  單擊 **Models** 選項卡。
    您可以查看正在使用特徵集的模型列表（根據註冊模型時的特徵檢索規範確定）。

![A screenshot of a computer Description automatically
generated](./media/image68.png)

總結：

在本實驗中，我們學習了如何使用託管特徵存儲和訓練模型來開發和註冊特徵集。
