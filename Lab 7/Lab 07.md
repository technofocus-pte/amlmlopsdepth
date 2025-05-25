# **实验 07：从 Azure 机器学习工作室开发和测试提示流**

**目的：**

在本实验中，我们将了解在 Azure
机器学习工作室中使用提示流的主要用户旅程。你将了解如何在 Azure
机器学习工作区中启用提示流、创建和开发提示流、测试和评估流，然后将其部署到生产环境。

预计持续时间 – 60 分钟

## 任务 1：准备 Azure 资源

### **任务 1.1：创建 Azure 机器学习工作区**

此任务侧重于创建 Azure
机器学习工作区。您将了解如何设置专用工作区来有效地组织和管理他们的机器学习项目。此工作区充当协作、试验和部署的中心枢纽。

1.  通过 +++<https://portal.azure.com>+++ 登录到 Azure
    门户，然后使用管理员租户凭据登录。

2.  在 Azure 门户主页中，选择“**+ Create a resource**”。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  在“**Create a resource**”页上，使用搜索栏查找 +++Azure Machine
    Learning**+++**，然后选择“**Azure Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  在“**Marketplace**”下，单击“**Create**”**下拉列表**，然后选择“**Azure
    Machine Learning**”。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  提供以下信息以配置您的新工作区：

    - **订阅：**选择已**分配的 Azure 订阅**

    - **资源组：**选择已**分配的资源组。**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **工作区详细信息：**

- **工作区名称：** +++**Azuremlws@lab.LabInstanceId**+++

- **区域：**选择离您最近的区域（此处选择 **North Central US**）

&nbsp;

- **容器注册表：选择 Create new
  （新建）。输入+++azuremlcr@lab.LabInstanceId+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  配置完工作区后，选择“**Review + Create**”。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  验证通过后，单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  单击 **Go to resource**（转到资源）以查看新工作区。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  **在 Microsoft.MachineLEarningServices |Overview 页上，**选择**“Work
    with your model in Azure Machine Learning studio”**下的**“Launch
    studio”。**

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### 任务 1.2：创建计算

此任务演示了如何在 Azure
中创建计算资源。您将探索不同的计算选项，例如虚拟机或托管计算集群，并了解如何配置和预置资源以高效执行机器学习工作负载。

1.  **Azure Machine Learning Studio**
    打开后，单击左侧窗格中“**Manage**”下的“**Compute**”。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  单击 **Compute instances** 屏幕上的 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  在 Create compute instance （创建计算实例）
    屏幕上，输入以下详细信息。

    1.  计算名称 – +++**pfcompute**+++

    2.  虚拟机类型 – **CPU**

    3.  虚拟机大小 – 选择**Standard_E4ds_v4**

> 单击 **Review + Create**。

**注意：**记下此计算名称以供以后使用。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  单击下一个屏幕中的 **Create** 以创建计算。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**注意：**计算大约需要 10 分钟才能达到 Running 状态。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**重要提示：**Compute
启动并运行后，您可以继续执行下一个任务。但是，如果您要从实验室执行中休息，请确保
**stop** 止计算实例，并在休息后启动时重新启动它。

### 任务 1.3：创建 Azure OpenAI 资源

1.  在 Azure 门户 +++https://portal.azure.com+++
    中，搜索并选择+++**AzureOpenAI**+++。

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  单击 **+ Create**。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  填写以下详细信息，然后单击 **Next**。

- 资源组 - 选择已分配的资源组

- Region （区域） – 选择一个区域 （此处使用的是 North Central US）

- 名你 - +++**AOAI-PF@lab.LabInstanceId**+++

- 定价层 - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  在下一页中接受默认值，然后单击 **Review + submit** 页面中的
    **Create**。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  部署完成后，单击 **Go to resource**（转到资源）。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  从左侧窗格中选择 **Keys and Endpoint** （密钥和终端节点）。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  复制 **Key** 和
    **Endpoint**（终端节点），并将其保存在记事本中，以便在实验的后续部分使用。

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  在 **Azure Machine Learning Studio** 中，从左窗格中选择 “**Model
    catalog**” ，然后选择 “**gpt-4o**” 。

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  单击 **Deploy** 以部署模型。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. 接受部署名称，然后选择 **Deploy**
    （部署）。请记下此名称以备将来使用。

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## 任务 2：设置提示流连接

1.  在 Azure 机器学习工作室的左侧导航窗格中，选择 **Prompt
    flow**（提示流）。从菜单栏中选择 **Connections**。选择 **Create**
    旁边的下拉列表，然后选择 **Azure OpenAI**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  在 Add Azure OpenAI connection （添加 Azure OpenAI 连接）
    向导中，提供以下详细信息，然后选择 **Save** （保存）。

- 名称 – +++**AoaiML_pf**+++

- 提供商 – 选择**Azure OpenAI**

- 订阅 ID – 选择您**分配的订阅**

- Azure OpenAI 帐户名称 – 选择 **AOAI-PF@lab.LabInstanceId**

- Auth Mode （身份验证模式） – 选择 选择 **API Key** （API 密钥）

- API 密钥 – 提供我们保存 **Azure OpenAI 资源的密钥**

- API base – 提供我们从 **Azure OpenAI 资源**保存的终**端节点**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  检查连接创建是否成功。

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## 任务 3：创建和开发提示流

1.  在 **Prompt flow** （提示流） 主页的 **Flows** （流） 选项卡中，选择
    **Create** （创建） 以创建提示流。**Create a new flow**
    （创建新流程）
    页面显示您可以创建的流程类型、您可以克隆以创建流程的内置示例以及导入流程的方法。

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  选择 **WebClassification** 类别下的 **Clone**。

在 **Explore gallery** 中，可以浏览内置示例，并在任何磁贴上选择 “**View
detail**” 以预览它是否适合你的方案。

此实验室使用 **Web 分类**示例来演练主要用户旅程。

Web 分类是一个流程，演示了使用 LLM 进行多类分类。给定一个
URL，该流只需几个镜头、简单的摘要和分类提示，即可将 URL 分类为 Web
类别。例如，给定一个 URL https://www.imdb.com，它将 URL 分类为 Movie。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  接受为 **Folder name** （文件夹名称） 填充的名称，然后选择 **Clone**
    （克隆）。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  计算会话对于流执行是必需的。计算会话管理应用程序运行所需的计算资源，包括包含所有必要依赖项包的
    Docker 映像。

5.  在流程创作页面上，通过选择 **Start compute session**
    （启动计算会话） 来启动计算会话。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**注意：**大约需要 **10 分钟**才能使计算会话处于 Running 状态。

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## 任务 4：检查流程创作页面

计算会话可能需要几分钟才能启动。在计算会话启动时，查看流程创作页面的各个部分。

页面左侧的 **Flow** 或 flatten
视图是主要工作区，您可以在其中通过添加或删除节点、内联编辑和运行节点或编辑提示来创作流。在
**Inputs** （输入） 和 **Outputs** （输出）
部分中，您可以查看、添加或删除以及编辑输入和输出。

当您克隆当前 Web 分类示例时，输入和输出已经设置好了。流的输入架构为
name： url;type： string，字符串类型的
URL。您可以将预设输入值更改为其他值，例如 https://www.imdb.com 手动。

- 右上角的 **Files** 显示流程的文件夹和文件结构。每个 flow
  文件夹都包含一个 flow.dag.yaml
  文件、源代码文件和系统文件夹。您可以创建、上传或下载用于测试、部署或协作的文件。

- 右下角的 **Graph** （图形）
  视图用于可视化流的外观。您可以放大或缩小，或使用自动布局。

您可以在 **Flow** （流） 或 flatten （展平）
视图中内联编辑文件，也可以打开 **Raw file** （原始文件）
**模式**切换并从 **Files** （文件）
中选择一个文件，以在选项卡中打开该文件进行编辑。

您可以在 **Flow** （流） 或 flatten （展平）
视图中内联编辑文件，也可以打开 **Raw file** （原始文件）
**模式**切换并从 **Files** （文件）
中选择一个文件，以在选项卡中打开该文件进行编辑。

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## 任务 5：设置 LLM 节点

对于每个 LLM 节点，您需要选择一个 **Connection** 来设置 LLM API
密钥。选择您的 Azure OpenAI 连接。

根据连接类型，您必须从下拉列表中选择 **deployment_name** 或 型号。对于
Azure OpenAI 连接，请选择部署。 

1.  对于summarize_text_content，请填写以下详细信息。

Connection （连接） – 选择 **AoaiML_pf**

Api – 选择**chat**

部署名称 — 选择 **gpt-4o-2024-11-20**

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  以类似的方式为 LLM 节点设置连接**classify_with_llm**。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

3.  要测试和调试单个节点，请在 **Flow** （流） 视图中选择节点顶部的
    **Run** （运行） 图标。您可以展开 **Inputs** 并更改流输入
    URL，以测试不同 URL 的节点行为。

4.  运行状态显示在节点顶部。运行完成后，运行输出将显示在节点 **Output**
    部分中。

5.  移动到流的开头，执行 **fetch_text_content_from url**并执行块。

![A screenshot of a computer Description automatically
generated](./media/image39.png)

**Graph** （图形） 视图还显示单个运行节点状态。

在 **Inputs** 部分下，将 **Value** 字段的值指定为
+++https://play.google.com/store/apps/details?id=com.spotify.music+++

从右上角选择 **Run** 以测试和调试整个流程。

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 任务 5：查看流输出

您还可以设置流输出，以便在一个位置检查多个节点的输出。流输出可帮助您：

- 在单个表中检查批量测试结果。

- 定义评估接口映射。

- 设置部署响应架构。

1.  在顶部横幅或顶部菜单栏中选择 **View
    outputs**（查看输出）以查看详细的输入、输出、流程执行和编排信息。

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  在 Outputs 屏幕的 Outputs
    选项卡上，请注意，该流使用**类别**和**证据**预测输入 URL。 ![A
    screenshot of a computer Description automatically
    generated](./media/image42.png)

3.  选择 **Outputs** （输出） 屏幕上的 **Trace** （跟踪）
    选项卡，然后选择 **Node name** （节点名称） 下的 **flow**
    （流），以在右侧窗格中查看详细的流概述信息。展开 **flow**
    并选择任何步骤以查看该步骤的详细信息。

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**总结:**

在本实验中，我们学习了如何通过简单的摘要将 URL 分类为 Web 类别，并使用
Azure 机器学习工作室中的提示流对 URL 进行分类。
