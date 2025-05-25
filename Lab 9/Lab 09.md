# **实验 09 - 使用 GitHub 设置 MLOps** 

**目的:**

Azure 机器学习允许与 **GitHub Actions**
集成，以自动执行机器学习生命周期。

在本实验中，你将了解如何使用 Azure 机器学习来设置端到端 MLOps
管道，该管道运行线性回归来预测纽约市的出租车费用。管道由组件组成，每个组件提供不同的功能，这些组件可以注册到工作区、进行版本控制，并与各种输入和输出一起重复使用。.

预计持续时间：60 分钟

我们正处于 Azure 机器学习的 MLOps 阶段

![](./media/image1.png)

## **练习 1：准备 Azure 资源**

### **任务 1：创建 Azure 机器学习工作区**

1.  如果尚未登录，请通过 +++<https://portal.azure.com>+++ 登录到 Azure
    门户。

2.  在 Azure 门户主页中，选择“**+ Create a resource” 。**

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  在“**Create a resource**”页上，使用搜索栏查找 +++Azure Machine
    Learning+++

4.  选择 **Machine Learning**。

> ![](./media/image3.png)

5.  在“**Marketplace**”下，单击“**Create**”**下拉列表**，然后选择“**Azure
    Machine Learning”。**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

6.  提供以下信息以配置您的新工作区：

    - **订阅：**选择已**分配的 Azure 订阅**

    - **资源组：**选择**分配给你的资源组。**

> **工作区详细信息：**

- **工作区名称：**+++**Azuremlws@lab.LabInstance.Id**+++

- **区域：**选择离您最近的区域（此处选择 **North Central US** ）

&nbsp;

- **容器注册表：选择 Create new
  （新建）。输入+++azuremlcr@lab.LabInstance.Id+++**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  验证通过后，单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

8.  单击 **Go to resource**（转到资源）以查看新工作区。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9.  **在 Microsoft.MachineLEarningServices |Overview 页上，**选择**“Work
    with your model in Azure Machine Learning studio”**下的**“Launch
    studio”。**

![A screenshot of a software update Description automatically
generated](./media/image9.png)

### **任务 2：创建计算**

1.  Azure 机器学习工作室打开后，单击左侧窗格中 “**Manage**” 下的
    “**Compute**” 。

![A screenshot of a computer Description automatically
generated](./media/image10.png)

2.  选择 **Compute clusters** 选项卡，然后单击 **+ New**。

![](./media/image11.png)

3.  在 **Create compute cluster** （创建计算集群）
    屏幕上，输入以下详细信息。

    1.  位置 – 选择您在其中创建 Azure 机器学习工作区的**区域**

    2.  虚拟机层 – **Dedicated**

    3.  虚拟机类型 – **CPU**

    4.  虚拟机大小 – 选择**Standard_E4s_v3 (**选中从所有选项中选择以查找
        VM 大小**)**

> 单击 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

4.  在 **Advanced Settings** （高级设置） 页面上，输入以下详细信息。

&nbsp;

1.  计算名称 – +++**cpu-cluster@lab.LabInstanceId**+++

2.  最小节点数 – 0

3.  最大节点数 – 1

> 单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

**注意：**计算大约需要 10 分钟才能达到 Running 状态。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

## **练习 2：检索 Azure 资源**

1.  在 Azure 门户
    (<https://portal.azure.com>中，打开资源组并记下以下资源的名称：

    1.  **Azure 机器学习工作区**

    2.  **应用程序洞察**

    3.  **密钥保管库**

    4.  **容器注册表**

    5.  **存储帐户**

> 并将它们保存在本地记事本中，以便在配置文件中更新。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

## **练习 3：准备好 GitHub 账户和资源**

**注意：**如果您还没有 GitHub 帐户，请从此处创建一个
+++**https://github.com/**+++ -\> **Signup**。

### **任务 2：将存储库 mlops 演示分叉到您的 GitHub 帐户中**

1.  打开浏览器并输入此链接 -
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  点击右上角的 **Fork**。

![A screenshot of a chat Description automatically generated with medium
confidence](./media/image16.png)

3.  这将打开 **Create a new fork** （创建新复刻） 页面。单击 **Create
    fork**（创建分叉）。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

4.  从 GitHub 项目中，选择 **Settings** （设置）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.png)

5.  在 **Secrets and variables**（密钥和变量）下选择 **Actions**（作）。

![A screenshot of a computer Description automatically
generated](./media/image19.png)

6.  选择 **New repository secret** （新建存储库密钥）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

7.  将此密钥命名为 +++**AZURE_CREDENTIALS**+++，并将以下 **Service
    Principal** 输出粘贴为密钥的内容。此服务主体是预先创建的。选择 **Add
    secret** （添加密钥）。

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

8.  添加的密钥**AZURE_CREDENTIALS**显示在 **Repository
    secrets**（存储库密钥）下。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

9.  单击 **New repository secret**（新建仓库密钥）。

![](./media/image23.png)

10. 请提供以下详细信息。

    1.  名你 – +++ARM_CLIENT_ID+++

    2.  秘密 – +++@lab .Variable(spAppId)+++

> ![A screenshot of a computer secret Description automatically
> generated with low confidence](./media/image24.png)

11. 对以下值重复步骤 9 和 10，创建其他 GitHub 机密。

    - +++ARM_CLIENT_SECRET+++ - +++@lab .Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab.CloudSubscription.TenantId+++

## **练习 4：配置机器学习环境参数**

1.  在密钥页面中，通过单击左上角 GitHub ID 旁边的 **mlops-v2-gha-demo**
    导航到存储库页面。

![](./media/image25.png)

2.  在根目录中选择 **config-infra-prod.yml** 文件。单击 **Edit**
    （编辑）（铅笔图标）。

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  更改值，

    1.  **命名空间 – mlopsliteXX**（将 XX 替换为随机数）

    2.  **后缀 – c**

    3.  **location （位置） – 与您的工作区区域相同**

> 单击 **Commit changes**（提交更改）。
>
> 在 **For pipeline reference 部分下**，将 Azure Resources
> 的值替换为我们在练习 2 中获取和保存的**值**。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

4.  单击 **Commit changes** （提交更改） 窗格中的 Commit changes
    （提交更改）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

5.  从 **.github/workflows** 打开
    **deploy-model-training-pipeline-classical.yml**。单击
    **Edit**（铅笔图标）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

6.  在文件内容中，将 **Size** 的值替换为 **+++Standard_E4s_v3+++**

选择 **Commit changes**（提交更改）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image30.png)

7.  从 **mlops/azureml/deploy/online**
    打开文件**online-deployment.yml**。点击 **Edit**（铅笔图标）。

![](./media/image31.png)

8.  将 **instance_type** 的值替换为 **+++Standard_E4s_v3+++**。单击
    **Commit changes**（提交更改）。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image32.png)

9.  在 **.github/workflows**
    下打开**tf-gha-deploy-infra.yml**文件。单击“**Edit**”，将第 9 行和第
    14 行中的 Azure 替换为 +++CoursesTF+++。

选择 **Commit changes**（提交更改）。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

10. 从顶部菜单栏中，选择 **Actions** （作）。单击**I understand my
    workflows, go ahead and enable them**。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

11. 这将显示与您的项目关联的预定义 GitHub 工作流。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

## **练习 5：部署 Machine Learning 基础设施**

选择 **tf-gha-deploy-infra.yml**。单击 **Runworkflow**。

选择

- Branch – **main** （分支 – 主）

选择**Run workflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image36.png)

1.  这将使用 GitHub Actions 和 Terraform 部署机器学习基础设施。

2.  跟踪任务的状态并确认执行成功。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

**注意：**此工作流程大约需要 5 分钟才能完成。

## **练习 6：部署模型训练管道**

接下来，您需要将模型训练管道部署到新的 Machine Learning 工作区。

此管道将创建一个计算集群实例，注册一个定义必要 Docker 映像和 python
包的训练环境，注册一个训练数据集，然后启动上一节中描述的训练管道。 

1.  在 **tf-gha-deploy-infra.yml** 工作流页面中，单击 **Actions**
    （作）。

![A screenshot of a computer Description automatically
generated](./media/image38.png)

2.  这将显示与您的项目关联的预定义 GitHub 工作流。从列表中选择
    **deploy-model-training-pipeline**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

3.  单击 **Run workflow** -\> **Run workflow**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image40.png)

4.  单击刚刚启动的管道以跟踪进度。

![A picture containing text, software, web page, font Description
automatically generated](./media/image41.png)

5.  此管道大约需要 15 到 45 分钟才能完成。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image42.png)

6.  下面是成功执行管道的屏幕截图。

![](./media/image43.png)

7.  此执行将在 Machine Learning 工作区中注册模型。

8.  在<https://ml.azure.com/> 登录到 AzureMachineLearning
    工作室，然后单击左侧窗格中的 **Data**（数据），检查是否已在此处添加
    **taxi-data**。这是作为工作流的 **register-dataset**
    作业的一部分完成的。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

9.  单击左侧窗格中的 **Jobs**（作业），然后选择
    **taxi-fare-training**。这是在工作流的 **run-pipeline**
    作业中执行的。

![](./media/image45.png)

10. 选择最新执行的 Display name （显示名称）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

11. 探索培训中涉及的阶段和细节。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

在 Machine learning
工作区中注册经过训练的模型后，您就可以部署模型进行评分了。

**总结**

在此实验室中，我们学习了如何使用 Azure 机器学习设置端到端 MLOps
管道，该管道准备了数据并将模型训练管道部署到新的机器学习工作区。
