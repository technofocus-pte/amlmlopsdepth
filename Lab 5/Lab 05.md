# **實驗 05 - 在 Azure 機器學習工作室中使用無代碼自動化機器學習預測需求**

**目的**

在本實驗中，您將學習如何使用 Azure
機器學習工作室中的自動化機器學習創建時間序列預測模型，而無需編寫任何代碼。該模型將預測自行車共享服務的租賃需求。

您不會在此實驗室中編寫任何代碼;您將使用 Studio 界面執行培訓。

預期持續時間 – 60 分鐘

## **練習 1：準備環境**

### **任務 1：啟動 AML 工作區**

1.  登錄到 Azure 門戶，如果尚未登錄，請登錄
    +++**https://portal.azure.com+++**。

2.  從 Azure 門戶菜單中，選擇 “**All resources**” 。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  選擇 Azure 機器學習工作區 (**Azuemlws@lab.LabInstanceId**)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  單擊 **Launch studio**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## **練習 2：創建自動化 ML 作業** 

1.  在 Azure 機器學習工作室中，單擊左窗格中 “**Author**” 部分下的
    “**Automated** **ML**” 。

2.  選擇 **+ New Automated ML job**。

![](./media/image4.png)

### **任務 1：創建數據資產**

1.  將試驗名稱指定為
    +++**experiment_forecast**+++，接受其他默認值，然後選擇 **Next**。

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

2.  選擇 **Select task type** （選擇任務類型） 作為 **Time series
    forecasting**（時間序列預測），然後單擊 **+ Create**（+ 創建）。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  在 Create data asset （創建數據資產） 頁面上，提供以下詳細信息。

    1.  名字 – +++**bikedata**+++

    2.  類型 – Tabular

> 單擊 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)

4.  在 **Data source** （數據源） 窗格中，選擇 **From local files**
    （從本地文件） 並單擊 **Next** （下一步）。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  在 **Destination storage type** （目標存儲類型） 上，選擇
    workspaceblob 並選擇 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  在文件或文件夾選擇中，選擇 **Upload files** ，然後從
    **C：\Labfiles** 文件夾中選擇**bike-no.csv**，然後單擊 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  驗證 **Settings and preview** （設置和預覽）
    表單是否按如下方式填充，然後選擇 **Next**（下一步）。

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **架構**表單允許為此實驗進一步配置數據。對於此示例，選擇將撥動開關設置為關閉狀態

    1.  **casual** 和

    2.  **registered** 列。

> 單擊 **Next**。

這些列是 **cnt** 列的細分，因此我們不包括它們。

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

9.  在 **Review** 表單上，驗證信息，然後單擊 **Create**
    以完成數據資產的創建。

> ![](./media/image13.png)

10. 返回 **Create a new Automated ML job** （創建新的自動化 ML 作業）
    頁面，將顯示數據資產創建的 **success** 消息。

11. 選擇新創建的 **bikedata**，然後單擊 **Next**。

> **注意：**如果未顯示 bikedata，請 **Refresh** 數據資產窗格。
>
> ![](./media/image14.png)

### **任務 2：配置作業** 

1.  在 **Task settings** （任務設置） 頁面上，提供以下詳細信息，然後選擇
    **View additional configuration settings** （查看其他配置設置）。

> 目標列 – **cnt(Integer)**
>
> 時間列 **– date (Date)**
>
> **取消選擇 Autodetect forecast horizon** （自動檢測預測範圍） 並提供值
> +++**14**+++。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

2.  在 Additional configuration （其他配置）
    窗格中，提供以下詳細信息，然後單擊 **Save** （保存）。

- 主要指標 - **Normalized root mean squared error**

- 解釋最佳模型 – **Enable**

- 受阻算法 - **Extreme Random Trees**

> 展開 Additional forecasting settings （其他預測設置）

- 自動檢測預測目標滯後– **未選中**

- 自動檢測目標滾動窗口大小 – **未選中**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3.  選擇 **Limits** ，然後為 **實驗超時（分鐘）** 字段輸入
    +++**60**+++。

![A screenshot of a test AI-generated content may be
incorrect.](./media/image17.png)

4.  在 **Validate and test** （驗證和測試） 下選擇以下值，然後選擇
    **Next** （下一步）。

> 驗證類型 – **k-fold cross-validation**
>
> 交叉驗證數 – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

5.  選擇 **automl-compute**（我們在上一個實驗中創建的那個）。單擊
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

6.  查看詳細信息，然後選擇 **Submit training job** （提交訓練作業）。

> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

7.  狀態頁面將初始狀態顯示為 **Running**
    （正在運行）。不斷刷新頁面以瞭解狀態。

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8.  訓練完成後，狀態將更改為 **Completed** （已完成）。

**注意：**完成培訓大約需要 30 到 45 分鐘。

## **練習 3：探索模型**

1.  導航到 **Models** （模型） 選項卡以查看測試的算法
    （模型）。默認情況下，模型在完成時按指標分數排序。

2.  在本教程中，根據所選的 **Normalized Mean squared
    誤差**指標得分最高的模型位於列表頂部。

3.  在等待所有實驗模型完成時，請選擇已完成模型的 **Algorithm name**
    （算法名稱） 以瀏覽其性能詳細信息。

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

4.  單擊 Overview 並查看其詳細信息。

> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

5.  單擊 **Metrics** 選項卡並瀏覽詳細信息。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image24.png)
>
> **重要提示：**在本培訓完成後，請繼續執行下一個實驗。培訓完成後，從此處返回到此實驗室。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image25.png)

## **練習 4：確定最佳模型**

Azure 機器學習工作室中的自動化機器學習允許您通過幾個步驟將最佳模型部署為
Web 服務。部署是模型的集成，以便它可以預測新數據並確定潛在的機會領域。

1.  作業完成後，通過選擇屏幕頂部的**作業名稱**，導航回父作業頁面。

![](./media/image26.png)

2.  在 Best model summary （最佳模型摘要） 部分中，**根據 Normalized
    root mean squared error** （標準化均方根誤差）
    **指標**選擇此實驗上下文中的最佳模型。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

3.  單擊 Algorithm name 將其打開並瀏覽詳細信息。

4.  該模型也可以部署為 Web 服務。

**總結**

在本實驗室中，你使用了 Azure 機器學習工作室中的自動化 ML
來創建預測自行車共享租賃需求的時序預測模型。
