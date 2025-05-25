# 实验 03 - 使用功能开发和注册具有托管功能存储和训练模型的功能集

本实验介绍如何使用自定义转换创建特征集规范。然后，它使用该特征集生成训练数据、启用具体化并执行回填。具体化计算功能窗口的要素值，然后将这些值存储在具体化存储中。然后，所有特征查询都可以使用具体化存储中的这些值。

如果不进行具体化，特征集查询会动态地将转换应用于源，以便在返回值之前计算特征。此过程适用于原型设计阶段。但是，对于生产环境中的训练和推理作，我们建议您具体化这些功能，以提高可靠性和可用性。

预期持续时间 – 50 分钟

## 练习 1：分配所需的角色：

1.  在 Azure 门户主页中，从 “**Resources**” 选项卡中选择分配的
    **Resource group**。从左侧窗格中，选择 **Access
    control（IAM）**。单击 **Add** 旁边的下拉列表，然后选择 **Add role
    assignment**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  搜索 +++**AzureML Data Scientist**+++ 并选择它。单击 **Next**。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在 成员 选项卡中，单击 **+ Select members**，搜索您的**用户名**
    +++@lab。CloudPortalCredential（User1） 的用户名 +++。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  选择您的**用户名**，然后单击 **Select** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  在接下来的 2 个屏幕中单击 **Review + assign** （查看 + 分配）。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  分配完成后，将获取 Added Role assignment 消息。

7.  重复相同的步骤集，添加角色 +++**Storage Blob Data Reader**+++
    和+++**Storage Blob Data Contributor**+++。

## 练习 2：开发功能集并注册到托管功能存储

本教程是托管特征存储教程系列的第一部分。在这里，您将了解如何：

- 创建一个新的最小特征存储资源。

- 开发并本地测试具有特征转换功能的特征集。

- 向特征存储注册特征存储实体。

- 注册您使用特征存储开发的特征集。

- 使用您创建的特征生成示例训练 DataFrame。

- 在特征集上启用脱机具体化，并回填特征数据。

### 任务 1：准备好环境

1.  在 Azure 机器学习工作室的左窗格中，选择 “**Authoring**” 下的
    “**Notebooks**” 。单击用户名旁边的三个点，然后选择 **Upload
    folder**。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2.  浏览并从 **C：\Labfiles** 中选择 **featurestore** 文件夹，然后单击
    **Upload**。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  导航到 **featurestore-\> notebooks-\>sdk_and_cli** 并打开笔记本 1.
    Develop-feature-set-and-register. ipynb

![](./media/image8.png)

4.  在 **Compute** （计算） 下选择 **Serverless Spark Compute**
    （无服务器 Spark 计算）。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5.  选择 **Configure session** （配置会话） 以使用先决条件配置会话。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6.  选择 **Python packages -\> Upload Conda file**（Python 包上传 Conda
    文件）。单击 **Browse**（浏览）并从 **C：\Labfiles** 中选择
    **conda.yml**，然后选择 **Apply**（应用）。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

7.  **执行**笔记本的第一个单元格。这将安装所有**依赖**项并完成其执行。这大约需要
    **10 分钟**才能完成。

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Spark 会话启动后，将 **User name**
    替换为您的用户名并执行下一个单元格

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9.  执行接下来的 3 个单元格以设置 Azure CLI。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10. 在下一个单元格中，按照**输出**中的步骤登录到 **Azure**。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### 任务 2：创建最小特征存储

1.  **执行第一个**单元格以设置特征存储的名称、位置和其他值。

![A screenshot of a computer program Description automatically
generated](./media/image19.png)

2.  **执行创建特征存储**的下一个单元格。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

3.  下一个单元**初始化 AzureML 特征存储核心 SDK 客户端**。**执行**它。

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

### 任务 3：在此笔记本中对事务滚动聚合功能集进行原型设计和开发

1.  **执行**此部分下的第一个单元格以浏览**交易**源数据。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  执行第二个单元以在本地**开发事务功能集**。

![A screenshot of a computer code Description automatically
generated](./media/image23.png)

3.  执行下一个单元以从功能集规范**生成 spark 数据帧**。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  为了向特征存储注册特征集规范，需要以特定格式保存它。请检查生成的事务
    FeaturesetSpec： 从文件树中打开此文件以查看规范：
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

执行下一个单元格以导出为特征集规范。

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

### 任务 4：注册特征存储实体

1.  实体有助于实施最佳实践，即在使用相同逻辑实体的功能集之间使用相同的联接键定义。执行单元格以注册特征存储实体。

> ![A screen shot of a computer Description automatically
> generated](./media/image26.png)

### 任务 5：向特征存储注册事务特征集

1.  在 Azure 门户 （+++https：//portal.azure.com+++）
    中，导航到已分配的资源组下以 **featureset** 开头的**存储帐户**。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  从左侧窗格中，选择 Access Control （IAM）。选择 **Add -\> Add role
    assignment**（添加角色分配）。

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  搜索并选择 +++**Storage Blob Data Reader**+++。

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

4.  完成类似于我们在练习 1 中所做的角色分配。

5.  同样，添加 +++**Storage Blob Data Contributor**+++ 角色。

6.  导航回 Azure 机器学习工作室。

7.  您可以在特征存储中注册特征集资产，以便与他人共享和重复使用。您还可以获得版本控制和具体化等托管功能。功能集资产引用了您之前创建的功能集规范以及其他属性，例如版本和具体化设置。

8.  **执行** next cell 以向 Feature Store **注册事务特征集**。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### 任务 6：浏览特征存储 UI

1.  在浏览器中打开一个新选项卡，然后导航到 Azure ML 全局登陆页（地址为
    +++https://ml.azure.com/home+++）。

2.  单击左侧导航栏中的 **Feature stores**。

![A screenshot of a computer Description automatically
generated](./media/image31.png)

3.  单击 **featurestore**。

**注意：**只能通过 SDK 和 CLI
创建和更新特征存储资产（特征集和实体）。您可以使用 UI
搜索/浏览特征存储。

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

### 任务 7：使用注册的特征生成训练数据数据

1.  我们首先探索观测数据。观察数据通常是训练和推理数据中使用的核心数据。然后将其与特征数据联接以创建完整的训练数据。观察数据是在事件发生期间捕获的数据：在这种情况下，它具有核心交易数据，包括交易
    ID、账户
    ID、交易金额。在这种情况下，由于它是用于训练的，因此它还附加了
    target 变量 （is_fraud）。

2.  **执行** cel land，观察输出数据。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  **执行**下一个单元格以获取**已注册的功能集**并**列出其功能**。

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

4.  执行下一个单元格以**打印示例值。**

![A screenshot of a computer Description automatically
generated](./media/image35.png)

5.  **执行**下一个单元格。在此步骤中，我们将**选择希**望成为训练数据一部分的**特征**，并使用特征存储
    SDK 生成**训练数据。**

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

6.  执行下一个单元格，以使用特征数据和观察数据生成训练数据帧。

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

### 任务 8：在事务功能集上启用脱机具体化

在功能集上启用具体化后，您可以执行回填或计划定期具体化作业。

1.  执行下一个 cell，根据特征数据大小在 yaml 文件中设置
    spark.sql.shuffle.partitions

2.  Spark 配置 spark.sql.shuffle.partitions 是一个 OPTIONAL
    参数，当功能集具体化到离线存储中时，该参数可能会影响生成的 parquet
    文件数（每天）。该参数的默认值为 200。最佳做法是避免生成许多小的
    parquet 文件。如果 Feature Set 物化后，离线 Feature Retrieval
    变得很慢，请前往 offline store 中对应的文件夹查看是否是 parquet
    小文件过多（每天）的问题，并相应地调整该参数的值。

**注意：**此笔记本中使用的示例数据很小。所以这个参数在
featureset_asset_offline_enabled.yaml 文件中设置为 1。

> ![A screenshot of a computer Description automatically
> generated](./media/image38.png)

3.  具体化是计算给定特征窗口的特征值并将其存储在具体化存储中的过程。实现这些功能将提高其可靠性和可用性。所有特征查询都将使用具体化存储中的具体化值。在此步骤中，您将对
    18 个月的功能窗口执行一次性回填。

4.  以下代码单元将按已定义功能窗口的当前状态 None 或 Incomplete
    **具体化数据**。**执行**它。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

5.  让我们**打印**来自下一个单元格中的特征集的**样本数据**。**执行**它。您可以从输出信息中注意到，数据是从材料存储中检索的。get_offline_features（）
    方法也将默认使用具体化存储。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 练习 3：使用特征试验和训练模型

在此笔记本中，您将了解如何：

- 通过使用现有的预计算值作为特征，对新的 accounts
  特征集规范进行原型设计。然后，将本地特征集规范注册为特征存储中的特征集。此过程与第一个教程不同，在第一个教程中，您创建了一个具有自定义转换的功能集。

- 从 transactions 和 accounts
  特征集中为模型选择特征，并将其另存为特征检索规范。

- 运行使用特征检索规范训练新模型的训练管道。此管道使用内置的特征检索组件来生成训练数据。

### 任务 1：设置环境

1.  在 Notebooks 窗格中，打开笔记本 **Experiment and train models using
    features**（使用特征试验和训练模型）。

2.  单击 **Configure session** 并上传
    **conda.yaml**，类似于我们对早期笔记本所做的方法。

3.  **执行第一个**单元格以启动会话。这大约需要 10 分钟。

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4.  在下一个单元格中，将占位符替换为 **\< your_user_alias \>**
    替换为您在文件夹结构中的**用户名**，然后**执行**该单元格。

![A screenshot of a computer program Description automatically
generated](./media/image42.png)

5.  **执行**接下来的 **3 个**单元格以**设置 CLI。**

6.  下一个单元格初始化项目工作区变量。**执行**它以**初始化变量**。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

7.  下一个单元格初始化特征存储变量。执行它。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

8.  执行下一个单元格以**初始化特征存储使用客户端**。

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

### 任务 2：从预计算数据本地创建账户功能集

对于载入预计算功能，您可以创建功能集规范，而无需编写任何转换代码。Featureset
规范是一种在完全本地/开发环境中开发和测试功能集的规范，无需连接到任何功能存储。在此步骤中，您将在本地创建特征集
spec 并从中采样值。

1.  执行以下单元格以**浏览帐户的源数据**。

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  执行下一个单元格，从这些预先计算的特征中**创建** local
    中的**账户特征集规范**。

![A screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3.  **执行**下一个单元以从功能集规范**生成 spark 数据帧。**

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  为了向特征存储注册特征集规范，需要以特定格式保存它。作：运行以下单元格后，请检查生成的帐户
    FeatureSetSpec：从文件树中打开此文件以查看规范：featurestore/featuresets/accounts/spec/FeatureSetSpec。**执行**下一个单元格。![A
    screenshot of a computer program Description automatically
    generated](./media/image50.png)

### 任务 3：在本地试验未注册的特征，并在准备就绪时向特征存储注册

在开发功能时，您可能希望先在本地测试/验证，然后再注册到功能存储或在云中运行训练管道。在此步骤中，您将根据本地未注册特征集
（账户） 和特征存储中注册的特征集 （交易） 的特征组合生成 ML
模型的训练数据。

1.  **执行**下一个单元格以**选择模型的特征。**

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

2.  **执行**接下来的 2 个单元格以在本地**生成训练数据。**

![A close-up of a computer code Description automatically
generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

3.  **执行**下一个单元格以将 **accounts
    功能集注册**到特征库。在本地试验不同的功能定义并对其进行健全性测试后，您可以将其注册到功能存储中。为此，您将向特征存储注册特征集资产定义。

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  **执行**接下来的 2 个单元格以获取注册的 featureset 和健全性测试。

![A screenshot of a computer Description automatically
generated](./media/image55.png)

### 任务 4：运行训练实验

1.  执行下一个单元格以从 SDK 中发现功能。

![A screenshot of a computer Description automatically
generated](./media/image56.png)

2.  在前面的步骤中，您从未注册和已注册的功能集组合中选择了功能，用于本地实验和测试。现在，您已准备好在云中进行实验。将所选特征保存为特征检索规范，并在
    mlops/cicd 流程中将其用于训练/推理，可以提高交付模型的敏捷性。

3.  **执行**下一个单元格以**选择模型的特征。**

![A screenshot of a computer program Description automatically
generated](./media/image57.png)

4.  **执行**下一个单元，并将所选特征导出为**特征检索规范。**

![A screenshot of a computer program Description automatically
generated](./media/image58.png)

### 任务 5：使用管道在云中训练，并在满意时注册模型

在此步骤中，您将手动触发训练管道。在生产场景中，这可能是由 ci/cd
管道根据源存储库中功能检索规范的更改触发的。

1.  **执行** next cell 以**运行训练管道。**

![A screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2.  在工作室的左侧窗格中，右键单击 **Jobs**
    并在新选项卡中打开。选择实验，**training_on_fraud_model**。

![A screenshot of a computer Description automatically
generated](./media/image61.png)

3.  单击 **training job** 并浏览详细信息。实验大约需要 5 到 15
    分钟才能完成。

![A screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4.  等待它完成。完成后，从左侧窗格中选择 **Models**。从列表中选择
    **fraud_model**。这是现在创建的模型。

![A screenshot of a computer Description automatically
generated](./media/image64.png)

5.  选择 **Feature sets** （功能集）
    选项卡。在这里，您可以看到此模型所依赖的 **transactions** 和
    **accounts** 特征集。

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6.  以 +++https://ml.azure.com/home+++ 打开**feature store UI**。选择
    **Feature stores -\> featurestore**。

![A screenshot of a computer Description automatically
generated](./media/image66.png)

7.  从左侧窗格中选择 **Feature
    sets**（功能集），然后选择任意一个**功能集**。

![A screenshot of a computer Description automatically
generated](./media/image67.png)

8.  单击 **Models** 选项卡。
    您可以查看正在使用特征集的模型列表（根据注册模型时的特征检索规范确定）。

![A screenshot of a computer Description automatically
generated](./media/image68.png)

总结：

在本实验中，我们学习了如何使用托管特征存储和训练模型来开发和注册特征集。
