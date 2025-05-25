# **实验 01 - 使用 Azure 机器学习工作室准备数据集、训练和部署分类模型**

**目的**

此实验室重点介绍指导你完成设置 Azure
机器学习环境、上传、访问和浏览数据以及使用 Azure
机器学习工作室训练和部署图像分类模型的过程。

预计持续时间 - 45 分钟

## **练习 1：设置 Azure 机器学习工作区**

### **任务 1：同步 VM 时钟**

1.  登录到 VM 后，右键单击屏幕右下角的时钟。

2.  选择 **Adjust date and time**（调整日期和时间）。

&nbsp;

3.  在打开的 个人设置 屏幕上，单击 **Sync now** 下 其他设置。

> ![A screenshot of a computer Description automatically
> generated](./media/image1.png)

4.  这负责同步时间，以防自动同步不起作用。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image2.png)

### 任务 2：准备 Azure 资源

此任务侧重于创建 Azure
机器学习工作区。您将了解如何设置专用工作区来有效地组织和管理他们的机器学习项目。此工作区充当协作、试验和部署的中心枢纽。

#### 任务 2.1：注册所需的资源提供程序 

1.  从 Azure 门户主页导航到分配的**订阅**。

2.  在左侧窗格中的 **Settings** （设置） 下选择 Resource
    Providers（资源提供程序）。

3.  搜索
    +++Microsoft.StreamAnalytics+++，选择名称对应的三个点，然后单击“**Register**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  重复上述步骤以注册 +++Microsoft.Cdn+++ 和
    +++Microsoft.PolicyInsights+++

#### 任务 2.2：创建 Azure 机器学习工作区

1.  使用 **Resources**（资源）选项卡中的 **Username** （用户名） 和
    **Password**（密码）登录到 Azure 门户（地址为
    +++https://portal.azure.com+++）。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

2.  在 Azure 门户主页中，选择“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

3.  在“**Create a resource** ”页上，使用搜索栏查找 +++**Azure Machine
    Learning**+++，然后选择“**Azure Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image6.png)

4.  在“**Marketplace**”下，单击 **Create dropdown** ，然后选择“**Azure
    Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

5.  提供以下信息以配置您的新工作区，然后单击 **Review + create**。

    - **订阅**: 选择已**分配的 Azure 订阅**

    - **资源组**: 选择分**配给您的 Resource Group**。

> **工作区详细信息：**

- **工作区名称：**+++**Azuremlws@lab.LabInstance.Id**+++

- **区域：**选择离您最近的区域（此处选择**美国中北部**）

&nbsp;

- **容器注册表：选择 Create new （新建）。输入
  +++azuremlcr@lab.LabInstance.Id+++**

**注意：**附加到资源名称的数字是您的 Labinstance
ID，以确保唯一性。屏幕截图将具有不同的编号，因为它们是唯一的。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  验证通过后，单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  单击 **Go to resource**（转到资源）以查看新工作区。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **在 Microsoft.MachineLEarningServices |“概述”页上，**选择**“在
    Azure 机器学习工作室中使用模型”**下的**“启动工作室”。**

![A screenshot of a software update Description automatically
generated](./media/image12.png)

#### 任务 2.3：创建计算

此任务演示了如何在 Azure
中创建计算资源。您将探索不同的计算选项，例如虚拟机或托管计算集群，并了解如何配置和预置资源以高效执行机器学习工作负载。

1.  **Azure Machine Learning Studio**
    打开后，单击左侧窗格中“**Manage**”下的“**Compute**”。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

2.  单击 **Compute instances** 屏幕上的 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  在 Create compute instance （创建计算实例）
    屏幕上，输入以下详细信息。

    1.  计算名称 – +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  虚拟机类型 – **CPU**

    3.  虚拟机大小 – 选择 **Standard_E4ds_v4**

> 单击**Review + Create**。

**注意：**记下此计算名称以供以后使用。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

4.  单击下一个屏幕中的 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

**注意：**计算大约需要 10 分钟才能达到 Running 状态。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

**重要提示：**Compute
启动并运行后，您可以继续执行下一个任务。但是，如果您要从实验室执行中休息，请确保**停**止计算实例，并在休息后启动时重新启动它。

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

**练习总结：**

本练习使参与者熟悉设置 Azure
机器学习环境所涉及的基本步骤。通过这一系列任务，参与者学习了如何创建存储帐户、安装机器学习
SDK、使用 Azure CLI 登录、创建 Azure
机器学习工作区以及设置计算资源。通过完成本练习，您已获得建立功能性 Azure
机器学习环境所需的基础知识和实践技能，使您能够自信地开始机器学习项目。

## **练习 2 - 在 Azure 机器学习中上传、访问和浏览数据**

**目的**

在本练习中，您将学习如何：

- 将数据上传到云存储

- 创建 Azure 机器学习数据资产

- 在 Notebook 中访问数据以进行交互式开发

- 创建数据资产的新版本

机器学习项目的启动通常涉及探索性数据分析
（EDA）、数据预处理（清理、特征工程）以及构建机器学习模型原型以验证假设。此原型设计项目阶段具有高度交互性。它适合在
IDE 或 Jupyter 笔记本中进行开发，并带有 Python
交互式控制台。本实验介绍了这些想法。

我们正处于**机器学习项目工作流程**的**数据：探索和准备**阶段。

![](./media/image19.png)

### 任务 1：准备 Azure 资源

**重要提示：**确保我们在上一个练习中创建的计算已启动并正在运行。如果您要从实验室执行中休息，请确保在休息后启动时**停**止并重新启动它。

#### 任务 1.1：上传笔记本

1.  在 Azure
    机器学习工作室中，计算启动并运行后，从左窗格中选择“**Notebooks**”选项。
    ![](./media/image20.png)

2.  关闭 **What's new in Notebooks** 对话框。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image21.png)

3.  Notebook Files （笔记本文件） 窗格打开，结构为 **Users -\> \<
    UserName \>**。单击用户名旁边的三个点，然后选择 **Create new
    folder**（创建新文件夹）。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

4.  将文件夹名称输入为
    +++**Azuremlnotebooks**+++，然后单击“**Create**”。

![A screenshot of a computer Description automatically
generated](./media/image23.png)

5.  创建文件夹后，单击 **Azuremlnotebooks**
    文件夹的**菜单选项（**文件夹名称旁边的三个点），然后单击 “**Upload
    files**” 。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

6.  选择 **Click to browse and select files**
    （单击以浏览并选择文件）。浏览到 **C：\Labfiles** 下的
    **explore-data.ipynb，**然后单击 **Open**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image25.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image26.png)

7.  选中复选框 **Open file after upload** and **I trust the contents of
    this file**（上传后打开文件，我信任此文件的内容）。然后点击
    **Upload**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

8.  这将打开上传的 Notebook。

![A screenshot of a computer Description automatically
generated](./media/image28.png)

9.  如果 Studio 要求您进行身份验证，请单击
    **Authenticate**，因为这是您第一次登录 Studio。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image29.png)

### 任务 2：上传、访问和浏览数据

#### 任务 2.1：下载数据

1.  在 **Notebooks** 的 **Files** 窗格下，单击文件夹名称
    **Azuremlnotebooks** 旁边的 3 个点，然后单击 **Create new folder**。

![](./media/image30.png)

2.  将文件夹名称键入 +++**data**+++ ，然后单击 **Create**。

![](./media/image31.png)

3.  文件夹创建成功后，单击文件夹**数据**的菜单选项，然后选择 **Upload
    files**。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

4.  选择 **Click to browse and select file(s) ，**然后导航到
    **C：\Labfiles** 以选择 **default_of_credit_card_clients.csv**
    文件，然后单击 **Open** （打开）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image33.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image34.png)

5.  上传完成后，通知下会显示一条消息，指出**File uploaded successfully**
    。

![A close-up of a computer screen Description automatically generated
with low confidence](./media/image35.png)

#### 任务 2.2：创建工作区的句柄

1.  移回笔记本 （**explore-data**）。

2.  在我们深入研究代码之前，您需要一种方法来引用您的工作区。您将为工作区的句柄创建ml_client。然后，您将使用
    ml_client 来管理资源和作业。

3.  在 **Create handle to workspace** 下的第一个单元格中，替换 **\<
    SUBSCRIPTION_ID \>**、**\< RESOURCE_GROUP \>** 和 **\<
    AML_WORKSPACE_NAME \>** 的占位符。

4.  将 \< RESOURCE_GROUP\> 替换为已分配的资源组的名称。

5.  \<AML_WORKSPACE_NAME\> 替换为
    [+++**Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  将 \< SUBSCRIPTION_ID \> 替换为
    +++**@lab.CloudSubscription.Id**+++。

7.  单击单元格左上角的 **Run cell**
    按钮。执行成功后，在单元格底部查找刻度线。

![A screenshot of a computer Description automatically
generated](./media/image36.png)

#### 任务 2.3：将数据上传到云存储

1.  Azure 机器学习数据资产类似于 Web
    浏览器书签（收藏夹）。您可以创建一个数据资产，然后使用友好名称访问该资产，而不是记住指向您最常用数据的长存储路径
    （URI）。

2.  下一个笔记本单元格将创建数据资产。该代码示例将原始数据文件上传到指定的云存储资源。

3.  每次创建数据资产时，都需要为其提供唯一的版本。如果版本已存在，您将收到错误。在此代码中，我们将使用时间在每次运行单元格时生成唯一版本。

4.  通过单击单元格左上角的 Execute 按钮来执行下一个单元格。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

5.  **“Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx”** 是显示在单元格下方的输出。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image38.png)

6.  单击左侧窗格中的
    **Data**，然后单击由我们在上一步中执行创建的**credit-card** Data
    资产。浏览详细信息并导航回 **Notebooks** 窗格。

![](./media/image39.png)

#### 任务 2.4：访问笔记本中的数据

1.  返回笔记本，使用 **%pip** 命令执行单元，以在 **Jupyter** 内核中安装
    **azureml-fsspec** Python 库。

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image40.png)

2.  执行下一个单元格以访问 **Pandas** 中的 CSV 文件。

3.  您将在单元格底部打印 **Data asset URI** ，并且还会显示数据。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image41.png)

#### 任务 2.5：创建数据资产的新版本

1.  您可能已经注意到，数据需要稍微清理一下，才能适合训练机器学习模型。它有：

    1.  两个标头

    2.  客户端 ID 列;我们不会在机器学习中使用此功能

    3.  响应变量名称中的空格

2.  此外，与 CSV 格式相比，**Parquet**
    文件格式成为存储此数据的更好方式。Parquet
    提供压缩，并维护架构。因此，要清理数据并将其存储在 Parquet
    中，请执行下一个单元格。

3.  通过单元格底部的刻度线确保执行成功。

![](./media/image42.png)

4.  此表显示了原始 **default_of_credit_card_clients.csv file**
    中的数据结构。CSV 文件。上传的数据包含 23 个解释变量和 1
    个响应变量，如下所示：

[TABLE]

5.  执行下一个单元格以创建数据资产的新版本（数据会自动上传到云存储）。

6.  成功执行后，输出显示 **Data asset created. Name: credit_card,
    version: YYYY.MM.DD.xxxxxx_cleaned** 显示在单元格之后。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image43.png)

![A screenshot of a computer Description automatically generated with
low confidence](./media/image44.png)

**重要:**

此 Python 代码单元格为其创建的数据资产设置 **name** 和 **version**
值。因此，如果多次执行，此单元格中的代码将失败，而这些值没有更改。固定
**name** 和 **version**
值提供了一种传递适用于特定情况的值的方法，而无需考虑自动生成或随机生成的值。

7.  清理后的 parquet
    文件是最新版本的数据源。下一个单元格中的代码首先显示 CSV
    版本结果集，然后显示执行时的 Parquet 版本。

8.  执行下一个单元格并检查下面的结果。

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image45.png)

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image46.png)

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image47.png)

![A picture containing text, screenshot, number, display Description
automatically generated](./media/image48.png)

9.  在下面查找已清理的数据 Data。

> ![](./media/image49.png)

**重要提示：**您可以从此处继续进行下一个练习。但是，如果您要从实验室执行中休息，请确保**停**止计算实例，并在从中断中恢复时再次启动它。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image50.png)

**锻炼总结**

在本练习中，您学习了如何将数据上传到云存储、创建 Azure
机器学习数据资产、在笔记本中访问数据以进行交互式开发以及创建新版本的数据资产。

## **练习 3 - 在 Azure 机器学习工作室上训练和部署图像分类模型**

**目的**

在本练习中，您将学习

1.  使用Azure Machine Learning Studio Notebook
    UI连接到工作区并设置计算资源

2.  引入数据并准备用于训练

3.  训练用于图像分类的模型

4.  查看和分析用于优化模型的 Metrics

5.  在线部署模型并进行测试

我们正处于**机器学习项目工作流程**的 **Train & validate model** 阶段。

### ![A picture containing text, font, number, screenshot Description automatically generated](./media/image51.png)任务 1：上传笔记本

1.  在 Azure 机器学习工作室的“**Notebooks**”页中，单击文件夹
    **AzureMLnotebooks** 的菜单选项，然后单击“**Upload files** ”。

> ![A screenshot of a computer Description automatically
> generated](./media/image52.png)

2.  选择，**Click to browse and select file(s)**， 浏览到
    **C：\Labfiles** 并选择文件
    **azureml-getting-started-studio**（Jupyter 源文件）。

> ![A screenshot of a computer screen Description automatically
> generated with medium confidence](./media/image53.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

3.  选中复选框 **Open file after upload** ，然后单击 **Upload**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image55.png)

4.  文件上传成功后，它将在 Studio 中打开，并自动连接到处于 Running
    状态的 Compute （cpu-cluster-fs）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

### 任务 2：连接到 Azure 机器学习工作区

在我们深入研究代码之前，您需要连接到您的工作区。工作区是 Azure
机器学习的顶级资源，提供了一个集中位置来处理您在使用 Azure
机器学习时创建的所有项目。

我们使用 **DefaultAzureCredential**
来获取对工作区的访问权限。**DefaultAzureCredential**
应该能够处理大多数情况。

*\# 工作区的句柄*

**from** azure.ai.ml **import** MLClient

*\# 鉴权包*

**from** azure.identity **import** DefaultAzureCredential

凭证 **=** DefaultAzureCredential()

*\# 获取工作区的句柄。您可以在 ml.azure.com 的 workspace （工作区）
选项卡上找到相关信息*

ml_client **=** MLClient (

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\> “, \#
*这将看起来像xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  在上面的代码（笔记本的第一个单元格）中，将
    **SUBSCRIPTION_ID**、**RESOURCE_GROUP name** 和
    **AML_WORKSPACE_NAME** 占位符替换为我们在前面的练习中保存的值。

2.  Notebook
    中的第一个单元格现在应如下所示。单击第一个单元格左上角附近的 **Run**
    按钮。

![A screenshot of a computer Description automatically
generated](./media/image57.png)

3.  通过在单元格底部查看单元格的状态，确保单元格成功执行。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image58.png)

In \[ \]:

### 任务 3：上传数据

若要运行 Azure 机器学习训练作业，需要一个环境。

在本实验室中，你将使用一个名为
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest
的现成环境，其中包含所有必需的库（python、MLflow、numpy、pip 等）。

1.  执行下一个单元格中的代码以上传数据。

2.  确保一条消息 **Data asset created** 显示为单元格的输出。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image59.png)

### 任务 4：构建命令作业进行训练

现在，你已拥有运行作业所需的所有资产，现在可以使用 Azure ML Python SDK
v2 自行生成作业了。我们将创建一个命令作业。

AzureML
命令作业是一种资源，用于指定在云中执行训练代码所需的所有详细信息：输入和输出、要使用的硬件类型、要安装的软件以及如何运行代码。命令作业包含执行单个命令的信息。

#### 任务 4.1 ： 创建训练脚本

1.  让我们从创建训练脚本 - **main.py** python 文件开始。

2.  执行下一个单元格并确保它成功执行。

![A picture containing text, font, line, screenshot Description
automatically generated](./media/image60.png)

3.  下一个单元格中的脚本处理数据的预处理，将其拆分为测试数据和训练数据。然后，它使用此数据来训练基于树的模型并返回输出模型。[MLFlow](https://mlflow.org/docs/latest/tracking.html) 将用于在管道运行期间记录参数和指标。

4.  执行单元格并确保它与输出一起成功执行，

**Writing ./src/main.py**

> ![A screenshot of a computer program Description automatically
> generated with low confidence](./media/image61.png)
>
> ![A screenshot of a computer program Description automatically
> generated with medium confidence](./media/image62.png)

5.  正如您在此脚本中所看到的，一旦模型被训练，模型文件就会被保存并注册到工作区。现在，您可以在推理终端节点中使用已注册的模型。

#### 任务 4.2：配置命令

现在，您有一个可以执行所需任务的脚本，您将使用可以运行命令行作的通用命令。此命令行作可以直接调用系统命令或运行脚本。

1.  在这里，您将使用输入数据、拆分比率、学习率和注册模型名称作为输入变量。

2.  从左窗格中，选择 **Data** 并选择 **credit-card-data** 。

![A screenshot of a computer Description automatically
generated](./media/image63.png)

3.  在 **Data sources** 部分下，查找 **Datastore URI**
    值并复制它。保存它以供下一步使用。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image64.png)

4.  在下一个单元格中，将

    1.  **path** 的值，其中包含在上一步中保存的**Datastore URI** 。

    2.  使用 +++**cpu-cluster-fs@lab.LabInstance.Id**+++ 我们在实验 1
        中保存的集群的名称）的 **compute** 值

5.  单击 **Run**（运行）。确保 cell 成功执行。

![A screenshot of a computer program Description automatically
generated](./media/image65.png)

### 任务 6：提交作业

现在，可以提交作业以在 AzureML 中运行。**该作业需要 2 到 3
分钟才能运行**。如果计算实例已缩减到零个节点，并且自定义环境仍在构建，则可能需要更长的时间（最多
10 分钟）。

1.  使用以下命令执行单元格以提交作业。

> ***\# submit the command job***
>
> **ml_client.create_or_update(job)**

2.  单击 **Run**（运行）。确保执行成功，并且 **Details Page**
    列下有指向结果的链接。

**注意：** 这大约需要 2 分钟才能完成。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image66.png)

3.  在新选项卡中，打开结果的 **Details Page** 列下可用的链接。

### 任务 7：查看训练作业的结果

1.  您可以通过**单击提交作业后生成的 URL** 来查看训练作业的结果。

> ![A screenshot of a computer Description automatically
> generated](./media/image67.png)

2.  或者，您也可以单击左侧导航菜单上的
    **Jobs**。作业是来自指定脚本或代码段的多个运行的分组。运行的信息存储在该作业下。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image68.png)

3.  **Overview** （概述） 页面首先将 **Properties** （属性） 窗格下的
    **Status** （状态） 显示为 **Running** （正在运行）。

4.  准备就绪后，状态将更改为 **Completed** （已完成）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image69.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image70.png)

5.  选择 **Metrics** （指标） 窗格以查看指标。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image71.png)

6.  选择 **Images** （图像） 选项卡以查看 training_confusion
    矩阵、精确率召回曲线和 roc 曲线。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image72.png)

1)  **Overview** 是您可以查看作业状态的位置。

2)  **Metrics** 将显示您在脚本中指定的指标的不同可视化效果。

3)  在**“图像”**中，可以查看使用 MLflow 记录的任何图像项目。

4)  **子作业**包含子作业（如果已添加）。

5)  **Outputs + logs** 包含您进行故障排除或其他监控目的所需的日志文件。

6)  **Code** 包含作业中使用的脚本/代码。

7)  **Explanations （解释）** 和 **Fairness （公平性）**
    用于查看您的模型在遵守负责任的 AI
    标准方面的表现。它们目前是预览功能，需要安装其他软件包。

8)  在 **Monitoring** 中，您可以查看计算资源性能的指标。

### 任务 8：将模型部署为联机终端节点

训练机器学习模型后，您需要部署它，以便其他人可以使用它来进行推理。为此，Azure
机器学习允许您创建终**端节点**并向其添加**部署**。

在此上下文中，终**端节点**是一个 HTTPS
路径，它为客户端提供了一个接口，用于向经过训练的模型发送请求（输入数据）并接收来自模型的推理（评分）结果。终端节点提供：

- 使用基于“密钥或令牌”的身份验证进行身份验证

- TLS（SSL） 终止

- 稳定的评分 URI （endpoint-name.region.inference.ml.azure.com）

**部署**是托管执行实际推理的模型所需的一组资源。

#### 任务 8.1：创建联机终端节点

1.  现在，将机器学习模型部署为 Azure 云（联机终结点）中的 Web 服务。

2.  从左侧窗格中选择 **Endpoints**。

![A screenshot of a computer Description automatically
generated](./media/image73.png)

3.  为 Real-time endpoints 选择 **Create** for Real-time endpoints

![A screenshot of a computer Description automatically
generated](./media/image74.png)

4.  选择 **credit_defaults_model**然后单击 **Select**。

![A screenshot of a computer Description automatically
generated](./media/image75.png)

5.  选择 **Standard_E4s_v3** 在 Virtual machine（虚拟机）下。提供
    Instance count （实例计数） 为 **1**

> 接受唯一 **Endpoint name** （终端节点名称） 和 **Deployment name**
> （部署名称） 的其他默认值，然后选择 **Deploy** （部署）。

![A screenshot of a computer Description automatically
generated](./media/image76.png)

**注意：**终端节点创建大约需要 20 分钟才能完成。

6.  完成后，Provisioning （预置） 状态将更改为 **Succeeded** （成功）。

![A screenshot of a computer Description automatically
generated](./media/image77.png)

#### 任务 8.2.. 使用示例查询进行测试

1.  在终端节点页面中，选择 **Test** （测试） 选项卡。

2.  将以下示例请求文件复制并粘贴到**Input data to test real-time
    endpoint**字段中，替换那里已经存在的代码。

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

3.  选择 **Test** （测试） 并在 **Test result** （测试结果）
    下查看结果。

> ![A screenshot of a computer Description automatically
> generated](./media/image78.png)

### **任务 9：删除终端节点**

1.  从左侧窗格中，选择 **Endpoints**。选择我们创建的终端节点，然后单击
    **Delete** （删除）。

![A screenshot of a computer Description automatically
generated](./media/image79.png)

2.  单击确认对话框中的 **Delete**。

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image80.png)

3.  查找有关成功删除的通知。

![A picture containing text, screenshot, font, line Description
automatically generated](./media/image81.png)

**总结**

在本实验中，你学习了如何在 Azure
机器学习工作室上训练图像分类模型，并将其部署为 Web 服务。
