# **实验 05 - 在 Azure 机器学习工作室中使用无代码自动化机器学习预测需求**

**目的**

在本实验中，您将学习如何使用 Azure
机器学习工作室中的自动化机器学习创建时间序列预测模型，而无需编写任何代码。该模型将预测自行车共享服务的租赁需求。

您不会在此实验室中编写任何代码;您将使用 Studio 界面执行培训。

预期持续时间 – 60 分钟

## **练习 1：准备环境**

### **任务 1：启动 AML 工作区**

1.  登录到 Azure 门户，如果尚未登录，请登录
    +++**https://portal.azure.com+++**。

2.  从 Azure 门户菜单中，选择 “**All resources**” 。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  选择 Azure 机器学习工作区 (**Azuemlws@lab.LabInstanceId**)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  单击 **Launch studio**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## **练习 2：创建自动化 ML 作业** 

1.  在 Azure 机器学习工作室中，单击左窗格中 “**Author**” 部分下的
    “**Automated** **ML**” 。

2.  选择 **+ New Automated ML job**。

![](./media/image4.png)

### **任务 1：创建数据资产**

1.  将试验名称指定为
    +++**experiment_forecast**+++，接受其他默认值，然后选择 **Next**。

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

2.  选择 **Select task type** （选择任务类型） 作为 **Time series
    forecasting**（时间序列预测），然后单击 **+ Create**（+ 创建）。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  在 Create data asset （创建数据资产） 页面上，提供以下详细信息。

    1.  名字 – +++**bikedata**+++

    2.  类型 – Tabular

> 单击 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)

4.  在 **Data source** （数据源） 窗格中，选择 **From local files**
    （从本地文件） 并单击 **Next** （下一步）。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  在 **Destination storage type** （目标存储类型） 上，选择
    workspaceblob 并选择 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  在文件或文件夹选择中，选择 **Upload files** ，然后从
    **C：\Labfiles** 文件夹中选择**bike-no.csv**，然后单击 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  验证 **Settings and preview** （设置和预览）
    表单是否按如下方式填充，然后选择 **Next**（下一步）。

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **架构**表单允许为此实验进一步配置数据。对于此示例，选择将拨动开关设置为关闭状态

    1.  **casual** 和

    2.  **registered** 列。

> 单击 **Next**。

这些列是 **cnt** 列的细分，因此我们不包括它们。

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

9.  在 **Review** 表单上，验证信息，然后单击 **Create**
    以完成数据资产的创建。

> ![](./media/image13.png)

10. 返回 **Create a new Automated ML job** （创建新的自动化 ML 作业）
    页面，将显示数据资产创建的 **success** 消息。

11. 选择新创建的 **bikedata**，然后单击 **Next**。

> **注意：**如果未显示 bikedata，请 **Refresh** 数据资产窗格。
>
> ![](./media/image14.png)

### **任务 2：配置作业** 

1.  在 **Task settings** （任务设置） 页面上，提供以下详细信息，然后选择
    **View additional configuration settings** （查看其他配置设置）。

> 目标列 – **cnt(Integer)**
>
> 时间列 **– date (Date)**
>
> **取消选择 Autodetect forecast horizon** （自动检测预测范围） 并提供值
> +++**14**+++。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

2.  在 Additional configuration （其他配置）
    窗格中，提供以下详细信息，然后单击 **Save** （保存）。

- 主要指标 - **Normalized root mean squared error**

- 解释最佳模型 – **Enable**

- 受阻算法 - **Extreme Random Trees**

> 展开 Additional forecasting settings （其他预测设置）

- 自动检测预测目标滞后– **未选中**

- 自动检测目标滚动窗口大小 – **未选中**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3.  选择 **Limits** ，然后为 **实验超时（分钟）** 字段输入
    +++**60**+++。

![A screenshot of a test AI-generated content may be
incorrect.](./media/image17.png)

4.  在 **Validate and test** （验证和测试） 下选择以下值，然后选择
    **Next** （下一步）。

> 验证类型 – **k-fold cross-validation**
>
> 交叉验证数 – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

5.  选择 **automl-compute**（我们在上一个实验中创建的那个）。单击
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

6.  查看详细信息，然后选择 **Submit training job** （提交训练作业）。

> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

7.  状态页面将初始状态显示为 **Running**
    （正在运行）。不断刷新页面以了解状态。

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8.  训练完成后，状态将更改为 **Completed** （已完成）。

**注意：**完成培训大约需要 30 到 45 分钟。

## **练习 3：探索模型**

1.  导航到 **Models** （模型） 选项卡以查看测试的算法
    （模型）。默认情况下，模型在完成时按指标分数排序。

2.  在本教程中，根据所选的 **Normalized Mean squared
    误差**指标得分最高的模型位于列表顶部。

3.  在等待所有实验模型完成时，请选择已完成模型的 **Algorithm name**
    （算法名称） 以浏览其性能详细信息。

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

4.  单击 Overview 并查看其详细信息。

> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

5.  单击 **Metrics** 选项卡并浏览详细信息。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image24.png)
>
> **重要提示：**在本培训完成后，请继续执行下一个实验。培训完成后，从此处返回到此实验室。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image25.png)

## **练习 4：确定最佳模型**

Azure 机器学习工作室中的自动化机器学习允许您通过几个步骤将最佳模型部署为
Web 服务。部署是模型的集成，以便它可以预测新数据并确定潜在的机会领域。

1.  作业完成后，通过选择屏幕顶部的**作业名称**，导航回父作业页面。

![](./media/image26.png)

2.  在 Best model summary （最佳模型摘要） 部分中，**根据 Normalized
    root mean squared error** （标准化均方根误差）
    **指标**选择此实验上下文中的最佳模型。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

3.  单击 Algorithm name 将其打开并浏览详细信息。

4.  该模型也可以部署为 Web 服务。

**总结**

在本实验室中，你使用了 Azure 机器学习工作室中的自动化 ML
来创建预测自行车共享租赁需求的时序预测模型。
