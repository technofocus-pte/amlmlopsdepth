# 實驗 02 - 使用 Azure 機器學習數據標記工具創建標記數據集

**目的**

在本實驗中，你將瞭解如何使用 Azure 機器學習工作室中的 Azure
機器學習數據工具將其未標記數據集合管理到已標記的數據集中，這些數據集可容納經過訓練的對象檢測模型將檢測到的類。

預計持續時間 - 40 分鐘

## **練習 1：準備 Azure 資源**

### **任務 1：創建 Azure 存儲帳戶**

1.  在 **Azure 門戶** （+++**https://portal.azure.com**+++）
    **主**頁中，在搜索欄中鍵入 +++**storage account**+++，然後選擇
    “**Storage accounts**”。

![](./media/image1.png)

2.  選擇 **+Create**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image2.png)

3.  在 Create a storage account （創建存儲帳戶）
    頁面上，輸入以下詳細信息。

> **項目詳情**

- Subscription （訂閱） – 選擇您的**subscription** （訂閱）。

- Resource group （資源組） – 選擇您分配的 **Resource
  group**（資源組）。

> **實例詳細信息**

- 存儲帳戶名稱 – +++**imagestoreacc@lab.LabInstance.Id** +++

- Region （區域） – 選擇您在其中創建 **AML 工作區**的**區域**

- 性能 – 選擇**Standard**

- Redundancy （冗餘） – 選擇 **Locally-redundant storage （LRS）**
  （本地冗餘存儲 （LRS））

選擇 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  在 Advanced 選項卡上，確保取消選中 **Allow cross-tenant
    replication** under **Blob storage** 部分。接受其他默認值，然後選擇
    **Review + create**。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  驗證通過後，單擊 **Create**。

> ![A screenshot of a computer error Description automatically
> generated](./media/image5.png)

6.  部署完成後，單擊 **Go to resource**。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  記下 Storage account
    name（存儲帳戶名稱），因為這將在實驗室的後面部分使用。保持在同一頁面上並繼續執行下一個任務。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

### **任務 2：創建 Azure 存儲容器**

1.  在存儲帳戶頁面的左側菜單中，滾動到 **Data Storage** 部分，然後選擇
    **Containers**。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  選擇 **+ Container**。在打開的 New container 窗格中，將容器名稱鍵入
    +++imagedata+++，然後單擊 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

3.  創建容器後，從左窗格中的 **Security + networking** 下選擇 **Access
    keys**。在 Access keys （訪問密鑰） 頁面上，單擊鍵值對應的 **Show**
    （顯示），然後**複製**密鑰。將複製的值存儲在記事本中以備將來參考。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4.  通過從左側窗格中選擇 **Containers** （容器） 導航回容器頁面。

![](./media/image11.png)

5.  選擇新創建的容器 imagedata。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6.  點擊 **Upload**（上傳）。在 **Upload blob** （上傳 blob）
    窗格中，單擊 **Browse for files** （瀏覽文件），然後從
    **C：\Labfiles** 下打開 **train_img** 文件夾

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image13.png)

7.  選擇 train_img 文件夾中的所有文件，然後單擊 **Open**.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

8.  單擊 Upload blob （上傳 blob） 頁面上的 **Upload** （上傳）。

![](./media/image15.png)

9.  上傳後，將顯示 **Successfully uploaded blob（s）** 消息，關閉
    **Upload blob** 窗格。

![](./media/image16.png)

10. 完成後，您應該會看到所有 242 個映像都已添加到 Azure 存儲容器中。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **練習 2：創建 Azure 機器學習數據標記項目**

1.  在 Azure 機器學習工作室的主頁中，從左窗格中的 “**Manage**” 下選擇
    “**Data Labeling**” 。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

2.  選擇 **+ Create。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)

3.  在 **Project details** （項目詳細信息） 部分下，提供以下詳細信息。

    1.  **項目名稱** - +++**soda**+++

    2.  媒體類型 – **Image**

    3.  **標記任務類型 - Object Identification (Bounding Box)** 

選擇 **Next**（下一步）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

4.  在 **Add workforce （optional）** （添加勞動力（可選））
    屏幕中，將選項保持禁用狀態，然後選擇 **Next** （下一步） 以繼續。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image21.png)

5.  在 **Select or create data** （選擇或創建數據） **頁面**上，單擊 **+
    Create** （+ 創建）。

> ![](./media/image22.png)

6.  在 **Create data asset** （創建數據資產） 頁面的 **Data type**
    （數據類型） 窗格中，提供以下詳細信息。

    1.  **名你** – +++**sodaObjects**+++

    2.  **描述–** +++**Image labelling**+++

    3.  **類型 –** File

> 單擊 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

7.  在 “**Create data asset**” 頁面的 “**Data source**” 窗格中，選擇
    “**From Azure storage**” 選項，然後單擊 “**Next**” 。

> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

8.  在 **Create data asset** （創建數據資產） 頁面的 **Storage type**
    （存儲類型） 窗格中，選擇 **Create new datastore**
    （創建新數據存儲）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image25.png)

9.  在 **New datastore** （新建數據存儲） 窗格中，提供以下詳細信息。

    1.  **數據存儲名稱** – +++**sodadatastore**+++

    2.  **數據存儲類型** – 選擇**Azure Blob Storage**

    3.  **帳戶選擇方法 – From Azure subscription** 中選擇

    4.  **訂閱 ID –** 選擇您的訂閱

    5.  **存儲帳戶 –** 選擇**imagestoreacc**

    6.  **Blob 容器 –** 選擇**imagedata**

    7.  **Authentication type –** 選擇**Account Key**

    8.  **Account key （帳戶密鑰） –** 輸入之前在練習 1 中保存的帳戶密鑰

> 單擊 **Create**。
>
> ![](./media/image26.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image27.png)

10. **Create success** 消息將顯示在 **Select a datastore**
    頁面上。選擇已創建的 **sodadatastore**。單擊 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

11. 在 Choose a storage path 下，選擇 **Enter storage path manually**
    並鍵入 / 作為 Storage path ，啟用 **Skip data validation**。單擊
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

12. 查看詳細信息，然後單擊 **Create**。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

13. 返回 **Select or create data** （選擇或創建數據） 窗格中，選擇
    **sodaObjects**。單擊 **Next**（下一步）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

14. 在 **Incremental refresh** （增量刷新） 頁面上，選擇 **Enable
    incremental refresh at regular intervals**
    （啟用定期增量刷新）。單擊 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

15. 在 **Label categories** 頁面上，單擊 **Add label category**
    兩次，以在現有的類別名稱占位符之外再添加兩個類別名稱占位符。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

16. 添加後，輸入+++**coke**+++，+++**diet_coke**+++
    和+++**sprite**+++，每個標簽類別占位符中各輸入一個。單擊
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

17. 將貼標說明留空，然後點擊 **Next**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

18. 單擊 **Quality control（preview）** （質量控制（預覽）） 頁面中的
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

19. 禁用 **Enable ML assisted labelling** 選項，然後單擊 **Create
    project**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing** 消息顯示在 Data Labelling 屏幕上。單擊 **soda**
    項目。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

21. 單擊 **Label data** （標記數據）。

> ![](./media/image39.png)

22. 右上角的 **Shortcuts keys** 顯示了可用的不同快捷鍵。

> ![A group of soda cans on a table Description automatically generated
> with medium confidence](./media/image40.png)

23. 頂部菜單欄提供了不同的可用選項。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

24. 第一個圖像將在屏幕上打開。從左側的 **Tags** 窗格中選擇相應的標簽。

> 然後，單擊圖像並稍微拖動以查看附加到圖像的標簽。點擊
> **Submit**（提交）。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image42.png)

25. 對提交當前圖像時出現的下一個圖像重複相同的過程。

> 標記至少 10 張圖像。
>
> ![](./media/image43.png)

26. 將上傳下一個圖像，直到到達圖像的末尾。請在超過 10
    張圖片的任何位置停下來，或者繼續並完成所有圖片的標記。

27. 單擊頂部導航路徑上的 soda 以返回 **Dashboard**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image44.png)

28. Dashboard 提供有關**標記資產**和**標簽分配**的詳細信息。

> ![](./media/image45.png)
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image46.png)

29. 點擊 **Export**。

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image47.png)

30. 在 **Export data** （導出數據） 窗格中，選擇

    - **資產類型 - Labeled**

    - **導出格式 -** **Azure ML dataset**

> 單擊 **Submit**。
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image48.png)

31. 導出完成後，**Labels successfully exported** 消息將顯示在 Dashboard
    頁面上。單擊成功消息中的 **file 鏈接**以打開導出文件的詳細信息。

> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image49.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image50.png)

32. 單擊 **Datasources -\> Actions** 部分下的 **View in datastores** 或
    **View in Azure portal** 鏈接。

> ![](./media/image51.png)

33. 在數據存儲中查看。

> ![A picture containing text, number, software, font Description
> automatically generated](./media/image52.png)

**總結**

在本實驗中，你學習了如何從 Azure
存儲創建數據資產，以及如何標記圖像和創建標記的數據集。

這整套任務也屬**機器學習項目工作流程**的 **Data: Explore & prepare**
階段。
