# **实验 04 - 在 Azure 机器学习工作室中使用无代码 AutoML 训练分类模型**

**目的**

在本实验室中，我们将学习如何在 Azure 机器学习工作室中使用 Azure
机器学习自动化 ML 通过无代码 AutoML
训练分类模型。此分类模型预测客户是否会在金融机构认购定期存款。自动化机器学习可快速迭代算法和超参数的多种组合，以帮助您根据所选的成功指标找到最佳模型。

预期持续时间 – 60 分钟

我们正处于 Azure 机器学习的 **Deploy Model** 阶段。

![](./media/image1.png)

## **练习 1：创建 Azure 机器学习工作区**

1.  登录到 Azure 门户 – +++**https://portal.azure.com**+++
    使用“**Resources**”选项卡中的凭据。

2.  在 Azure 门户主页中，选择“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在“**Create a resource**”中，使用搜索栏查找 +++**Azure Machine
    Learning**+++。在“**Marketplace**”下选择“**Azure Machine
    Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  在“**Marketplace**”下，单击“**Create**”下拉列表，然后选择“**Azure
    Machine Learning**”。

> ![A screenshot of a software Description automatically
> generated](./media/image4.png)

5.  提供以下信息以配置您的新工作区：

    - **订阅：**选择已**分配的 Azure 订阅**

    - **资源组：**选择分配的资源组

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

**工作区详细信息：**

- **工作区名称：+++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **区域：**在此处选择“**North Central US** ”区域

- **容器注册表：**选择**Create new** （新建）。
  输入**+++Azuremlcr@lab.LabInstanceId**+++

![A screenshot of a computer Description automatically
generated](./media/image6.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  配置完工作区后，选择“**Review + Create**”。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

7.  验证通过后，单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

8.  单击 **Go to resource**（转到资源）以查看新工作区。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

9.  **在 Microsoft.MachineLEarningServices | Overview
    页上，**选择**“Work with your model in Azure Machine Learning
    studio”**下的**“Launch studio”。**

![A screenshot of a computer Description automatically
generated](./media/image11.png)

## **练习 2：创建自动化 ML 作业**

1.  导航到 Azure 机器学习工作室选项卡。

2.  在左侧窗格中，选择 **Authoring** （创作） 部分下的 **Automated ML**
    （自动化 ML）。

3.  单击 **+ New Automated ML job**（新建自动化 ML 作业）。

![](./media/image12.png)

### **任务 1：创建数据资产**

1.  在 **Basic settings** （基本设置） 页面上，将 New experiment name
    （新实验名称） 指定为
    +++MarketingExperiment+++，接受其他默认值，然后单击 **Next**
    （下一步）。

![](./media/image13.png)

2.  在任务类型和数据页面中，**Select task
    type**下的**Classification**，然后在 **Select data**下选择 **+
    Create。**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  在 Create data asset （创建数据资产） 页面中，提供以下详细信息。

- **名你** – +++marketingdata+++

- **类型** – **Tabular**

- 单击 **Next**（下一步）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image15.png)

4.  在 **Data source** （数据源） 窗格中，选择 **From local files**
    （从本地文件） 并单击 **Next** （下一步）。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

5.  在 **Destination storage type** （目标存储类型）
    中，选择在创建工作区期间自动设置的默认数据存储：**workspaceblobstore**。您可以将数据文件上传到此位置，以使其可用于您的工作区。选择
    **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

6.  在 **File or folder selection** （文件或文件夹选择） 中，选择
    **Upload files or folder** （上传文件或文件夹） **\> Upload
    files。**从 **C：/Labfiles** 中选择 **bankmarketing_train.csv**
    文件。选择 **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

7.  上传完成后，将根据文件类型填充 **Data preview** 区域。在
    **Settings** 窗体中，查看数据的值。然后选择 **Next**（下一步）。

[TABLE]

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

8.  **Schema** 表单允许为此实验进一步配置数据。对于此示例，请选择
    **day_of_week** 的切换开关，以便不包含它。选择 **Next**（下一步）。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

9.  在 **Review** 表单中，验证信息并选择 **Create**
    以完成**数据资产**的创建。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. 返回 **Create a new Automated ML job** （创建新的自动化 ML 作业）
    页面，将显示数据资产创建的 **success** 消息。选择创建的
    **marketingdata** 数据资产，然后单击 **Next**。

> **注意：**如果未显示 **marketingdata**，请单击 Refresh 将其列出。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

### **任务 2：配置作业**

1.  在 **Task Settings** 页面中，选择 **y （String）** 作为 **Target
    列**，这是您要预测的内容。此栏显示客户是否申购定期存款。

2.  选择 **View additional configuration settings**
    并填充字段，如下所示。这些设置是为了更好地控制训练作业。否则，将根据试验选择和数据应用默认值。

- 主要指标 – AUCWeighted

- 解释最佳模型 – 启用

- 使用所有支持的模型 - 启用

- 阻止的模型 – 无

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3.  选择 **Limits**，然后为 **实验超时（分钟）** 字段输入 +++**60**+++。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a test AI-generated content may be
incorrect.](./media/image25.png)

4.  在 **Validate and test** 下，提供以下值，然后单击 **Next**。

- 验证类型 - 选择**k-fold cross-validation**

- 交叉验证数量 – 选择 **2**

![A screenshot of a computer Description automatically
generated](./media/image26.png)

5.  在 Compute 页面中，选择 Select compute type （选择计算类型） 作为
    **Compute cluster**（计算集群），然后单击 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

6.  在 **Create compute cluster** 窗格中，选择以下详细信息，然后单击
    **Next**。

- 位置 – **美国中北部**（与 Azure 机器学习工作区的位置相同）

- 虚拟机层 – **Dedicated**

- 虚拟机类型 - **CPU**

- 虚拟机大小 - 选择 **Standard_DS12_v2**

![A screenshot of a computer Description automatically
generated](./media/image28.png)

7.  在 Advanced settings （高级设置） 中，提供以下详细信息并选择
    **Create** （创建）。

- 计算名称 - +++automl-compute+++

- 最小节点数 - 0

- 最大节点数 – 1

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

8.  计算预置成功后，选择 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

9.  在 **Review** （查看） 页面中，选择 **Submit the training job**
    （提交训练作业）。

> ![A screenshot of a computer Description automatically
> generated](./media/image31.png)

10. 当实验准备开始时，**Overview** （概述） 屏幕将打开，其中 **Status**
    （状态）
    位于顶部。此状态会随着实验的进行而更新。工作室中还会显示通知，以告知您实验的状态。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

> **注意：**完成培训大约需要 40 分钟。

## **练习 3：探索模型**

在训练过程中，您可以浏览与之关联的模型。

1.  导航到 **Models + child** jobs 选项卡，查看测试的算法（模型）。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

2.  选择 **StandardScalerWrapper, XGBoostClassifier** 模型。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

3.  单击 **Metrics** 并浏览 Metrics 选项卡下的详细信息。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

4.  在等待所有实验模型完成时，请选择已完成模型的 **Algorithm name**
    （算法名称） 以浏览其性能详细信息。选择 **Overview** 和 **Metrics**
    选项卡以获取有关作业的信息。

> **重要提示：**模型训练大约需要 40
> 分钟才能完成。在进行中，请继续进行下一个实验。在状态更改为
> **Completed**（已完成）后，继续执行此实验室。

## **练习 4：模型解释**

模型解释可以按需生成。模型解释仪表板是 **Explanations （preview）**
（解释（预览）） 选项卡的一部分，它总结了这些解释。

1.  在 Models + child jobs （模型 + 子作业）
    选项卡（从父作业中）下，选择 **MaxAbsScaler, LightGBM。**

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  选择 **Explain model** （解释模型） 选项卡。

![A screenshot of a computer Description automatically
generated](./media/image37.png)

3.  在打开的 Explain model （解释模型） 窗格中，选择

    1.  选择计算类型 - **Compute cluster**

    2.  选择 AzureML 计算实例 - 选择 **automl-compute**

选择 **Create**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  此时将显示 Success 消息。选择 **Explanations（preview）**
    选项卡。此选项卡在可解释性运行完成后填充。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

5.  展开左窗格。在 **Features** （功能） 下，选择显示 **raw** 的行。选择
    **Aggregate** **feature importance**
    选项卡。此图表显示哪些数据特征影响了所选模型的预测。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

在此示例中，持续**时间**似乎对此模型的预测影响最大。

## **练习 5：部署最佳模型**

自动化机器学习界面允许您将最佳模型部署为 Web
服务。部署是模型的集成，以便它可以预测新数据并确定潜在的机会领域。对于此实验，部署到
Web 服务意味着金融机构现在拥有用于识别潜在定期存款客户的迭代和可缩放 Web
解决方案。

试验运行完成后，**Details** （详细信息） 页面将填充 **Best model
summary** （最佳模型摘要） 部分。在此实验上下文中，**VotingEnsemble**
被认为是基于 **AUCWeighted** 指标的最佳模型。

1.  从左侧窗格中选择 **Jobs**，然后选择您创建的实验。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image41.png)

2.  单击实验的显示名称。

![](./media/image42.png)

3.  检查状态是否为 **Completed**。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

4.  试验运行完成后，**Details** （详细信息） 页面将填充 **Best model
    summary** （最佳模型摘要） 部分。在此实验上下文中，根据
    **AUC_weighted** 指标，**VotingEnsemble** 被认为是最佳模型。

> ![A screenshot of a computer Description automatically
> generated](./media/image44.png)

我们部署了此模型，但请注意，部署大约需要 20
分钟才能完成。部署过程包括几个步骤，包括注册模型、生成资源以及为 Web
服务配置资源。

5.  选择 **VotingEnsemble** 以打开特定于模型的页面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

6.  选择左上角的 **Deploy** 菜单，然后选择 **Deploy to web service**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

7.  填充 **Deploy a model** （部署模型） 窗格，如下所示：

[TABLE]

> 单击 **Deploy**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image47.png)

8.  模型屏幕上会显示一条成功消息，指出已 **Model deployment is
    successfully triggered**，状态为正在**Running**。

![](./media/image48.png)

9.  部署完成后，状态将更改为 **Completed** （已完成）。

![A screenshot of a computer Description automatically
generated](./media/image49.png)

> 现在，您有一个可作的 Web 服务来生成预测。

## **练习 6：删除资源**

### **任务 1：删除端点**

1.  在 AML Studio 的左侧窗格中，单击 **Endpoints**（终端节点）。

2.  选择终端节点 **my-automl-deploy**，然后单击 **Delete**。

![](./media/image50.png)

3.  在 Delete real-time endpoint 对话框中选择 **Delete**。

4.  删除终端节点后，您应该会收到一条成功消息。

**总结**

在本实验室中，我们学习了如何在 Azure 机器学习工作室中训练没有代码的
AutoML 分类模型，并将最佳模型部署为 Web 服务。
