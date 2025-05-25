# 实验 08 – 使用提示流通过 RAG 实现 QA 数据生成

**目的：**

QA 数据生成是 RAG（检索增强生成）创建过程的一部分，其中自动生成的 QA
数据集用于获取 RAG 的最佳提示并获取 RAG 的评估指标

在本实验中，您将学习如何根据数据创建 QA 数据集。

预计持续时间 – 60 分钟

## 练习 1：创建 AOAI 部署

在本练习中，我们将使用在上一个实验室中创建的 Azure OpenAI 资源创建
gpt-35-turbo 模型部署。

1.  在 Azure 机器学习工作室中，从左窗格中选择 “**Model Catalog**” 。搜索
    +++**gpt-35-turbo**+++ 并从模型列表中选择 **gpt-35-turbo**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  确保在 **Azure OpenAI 资源**字段中选中 AOAI 资源
    **AOAI-PF@lab.LabInstanceId**。选择 **Deploy** 以部署模型。

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  接受 **Deployment name** 并选择 **Deploy**。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  对 **text-embedding-ada-002** 重复模型部署，部署名称为
    +++**text-embedding-ada-002-2**+++

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## 练习 2：设置环境

1.  从 Studio 的左侧窗格中，选择
    **Notebooks**。单击用户名旁边的三个点，然后选择 **Upload
    files**（上传文件）。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

2.  浏览到 **C：\LabFiles** 并选择 **qa_data_generation.ipynb**
    文件。选中 **I trust contents of this file** 复选框，然后单击
    **Upload** 。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  打开笔记本，然后在 **Compute** （计算） 选项中选择 **Serverless
    Spark Compute** （无服务器 Spark 计算）。

![A screenshot of a computer program Description automatically
generated](./media/image7.png)

4.  附加计算后，选择 **Configure session** （配置会话） 以上传 conda.yml
    文件并设置环境以使用它执行。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  选择 **Python packages** -\>**Upload Conda file** -\>然后单击
    **Browse**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  从 **C：\LabFiles** 中选择 **conda.yml**，然后选择 **Apply**。

> ![A screenshot of a computer program Description automatically
> generated](./media/image10.png)

## 练习 3：获取 AzureML 工作区的客户端

1.  执行 notebook 的第一个单元格以安装依赖项

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

**注意：**这将需要 10 到 15 分钟才能完成

2.  使用 az login 执行下一个单元格以**登录**到 **Azure** CLI。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

3.  工作区是 Azure 机器学习的顶级资源，提供了一个集中位置来处理您在使用
    Azure 机器学习时创建的所有项目。在本节中，我们将连接到将在其中运行
    Job 的工作区。 MLClient 是您与 AzureML 交互的方式

4.  将 **Subscription ID** 的占位符替换为 +++@lab.Subscription
    ()+++、具有**Resource group name** 的 **Resource group**
    和下一个单元格中具有 +++**Azuremlws@lab.LabInstanceId+++** 的
    **Azure ML Workspace**，用于创建 MClient。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer Description automatically
generated](./media/image15.png)

5.  **执行**设置**连接名称**的下一个单元格。如果您在创建连接时使用了任何其他名称，请在此单元格中指定该值，然后执行。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

6.  将 **key** 值替换为 **Azure openAI key**，将 **target**
    值替换为我们之前保存的 Azure OpenAI 资源的 **Endpoint** 值。

替换值后**执行**单元格。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

7.  现在，您的工作区已连接到 Azure OpenAI，我们将确保 gpt-35-turbo
    模型已部署，可供推理使用。

8.  **执行**下一个单元格以设置模型和**部署**名称。如果您在创建模型和部署时指定了不同的名称，请替换
    model name （模型名称） 和 deployment name （部署名称） 的值。

![A screenshot of a computer code Description automatically
generated](./media/image18.png)

9.  最后，我们将部署和模型信息合并到 Azure ML 嵌入组件期望作为输入的 uri
    形式中。**执行** next cell 以执行此作。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

## 练习 4：设置管道

AzureML
管道将多个组件连接在一起。每个组件定义输入，即使用代码生成的输入和输出的代码。管道本身可以具有通过将各个子组件连接在一起而生成的输入和输出。为了处理您的数据以进行嵌入和索引，我们将多个组件链接在一起，每个组件都执行自己的工作流程步骤。

组件将发布到注册表
azureml，默认情况下，该注册表应具有访问权限，可以从任何工作区访问它。在下面的单元格中，我们从
azureml 注册表获取组件定义。

1.  执行下一个单元格并确保它被执行而没有任何问题。

![A screenshot of a computer code Description automatically
generated](./media/image20.png)

2.  每个组件都有文档，这些文档提供了组件用途和每个输入/输出的总体描述。例如，我们可以通过检查
    Component 定义来了解 **data_generation_component**
    的作用。为此**执行**下一个单元格并观察输出。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

3.  下面是一个 Pipeline 是通过定义一个 python
    函数来构建的，该函数将上述组件、输入和输出链接在一起。该函数的参数是
    Pipeline 本身的输入，返回值是定义 Pipeline 输出的字典。确保 **next
    cell** 成功**执行**。

![A screenshot of a computer code Description automatically
generated](./media/image22.png)

![A screenshot of a computer program Description automatically
generated](./media/image23.png)

4.  下面的设置显示了如何将不同的 git 和 data_source
    参数设置为仅处理来自较大的 AzureDocs git 存储库的 AzureML
    文档，并确保处理每个文档的源 url 以链接到公共托管的 URL 而不是 git
    url。

5.  执行接下来的两个单元格并确保它们成功执行。

![](./media/image24.png)

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

## 练习 5：提交管道

1.  管道中每个步骤的输出都可以通过 Workspace UI
    检查，运行以下单元格后，单击“详细信息页面”下的链接。

2.  执行下一个单元格并单击输出中的链接以查看流状态

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  执行将在提示流中打开。探索流程的每个阶段。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

6.  流程成功后，进入下一步。

## 练习 6：查看生成的 QA 数据

1.  执行接下来的 2 个单元格并查看 QA 数据的输出。

![A screenshot of a computer code Description automatically
generated](./media/image29.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image30.png)

总结：

在本实验中，我们学习了如何根据您的数据创建 QA 数据集。
