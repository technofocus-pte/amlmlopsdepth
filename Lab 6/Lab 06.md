# 实验 06 - 为硬件数据集训练最佳回归模型

目的

在本实验中，我们将介绍如何使用 AutoML 训练回归模型。我们将使用 Hardware
Performance
数据集来训练和部署模型以用于推理场景。回归的目标是预测硬件部件的某些组合的性能。

预期持续时间 – 60 分钟

# 练习 0：准备好环境

### **任务 1：启动 AML 工作区**

1.  登录到 Azure 门户，如果尚未登录，请登录
    +++[**https://portal.azure.com**](https://portal.azure.com)+++。

2.  从 Azure 门户菜单中，选择“**All resources**”。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  选择 Azure 机器学习工作区 (**Azuemlws@lab.LabInstanceId**)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  单击 **Launch studio**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  从左侧窗格中选择 **Compute** 以创建 Compute 实例。选择 **+ New**。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

6.  提供以下详细信息，然后单击 Review + Create。

- 计算名称 - +++**auto-compute**+++

- 虚拟机类型 – **CPU**

- 虚拟机 – **Standard E4ds_v4**

![](./media/image5.png)

7.  选择 **Create** （创建） 以创建计算实例。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

### **任务 2：将笔记本上传到 AML 工作区**

1.  单击左侧窗格中的 **Notebooks**。单击 **Users**
    下**用户名**旁边的三个点，然后选择 **Upload folder**。

![](./media/image7.png)

2.  选择单击以浏览并选择文件夹，然后浏览 **C：\Labfiles** 以选择文件夹
    **automl-regression-task-hardware-performance**，然后单击
    **Upload**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image8.png)

3.  如果您看到一个弹出窗口，询问 Upload 3 files to this site？，请单击
    **Upload**。

![A picture containing text, screenshot, display, font Description
automatically generated](./media/image9.png)

4.  选中复选框 **I trust contents of these
    files**（我信任这些文件的内容），然后选择 **Upload**（上传）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

5.  打开 Notebook（. ipynb
    文件），**automl-regression-task-hardware-performance**。Notebook
    会自动连接到我们之前创建的计算。

![](./media/image11.png)

## **练习 1：连接到 Azure 机器学习工作区**

### **任务 1：导入所需的库**

1.  执行 **1.1 Import the required libraries**
    下的单元格第一个单元格，通过单击单元格左上角的 Run cell
    按钮来导入此实验室执行所需的库。

2.  通过在单元格的左下角查找刻度线符号，确保执行成功。

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image12.png)

### **任务 2：配置工作区详细信息并获取工作区的句柄**

1.  在 1.2. 1.2. Configure workspace details and get a handle to the
    workspace 下的单元格中，替换

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP – **您分配的 Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  单击单元格左上角的 Run cell
    选项，并确保执行成功后在左下角获得一个刻度线符号。

3.  单元格下方显示一个输出，指出 **Found the config file in ：
    /config.json** 。

![](./media/image13.png)

### **任务 3：显示 Azure ML 工作区信息**

1.  执行下一个单元格（显示 Azure ML 工作区信息下方的单元格）。

2.  确保在单元格下方列为输出的工作区、订阅、位置和资源组的详细信息都正确无误。

![A screenshot of a computer program Description automatically
generated](./media/image14.png)

## **练习 2：使用输入训练数据的 MLTable** 

### **任务 1：创建 MLTable 数据输入**

1.  执行下一个单元格（**2.1 Create MLTable data input**（2.1 创建
    MLTable 数据输入）下的单元格）。

2.  确保执行成功。

![A picture containing text, font, screenshot, software Description
automatically generated](./media/image15.png)

## **练习 3：配置并运行 AutoML 回归训练作业**

1.  执行 **4.1 Configure 下的单元格并逐个运行 AutoML
    回归训练作业**，并确保每个单元格都成功执行。

2.  **4.2 Run the Command** 下的单元格提交 AutoML 作业。

![A screenshot of a computer program Description automatically
generated](./media/image16.png)

3.  您可以通过单击左侧窗格中的 **Jobs** 并选择处于 Running
    状态的实验来检查作业的状态。

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**注意：** 这大约需要 10 到 15 分钟才能完成。

4.  Notebook 中的下一个单元格将等待 AutoMLjob 完成。

5.  执行它并等待执行完成，然后移动到下一个单元格。

![](./media/image19.png)

6.  只有在执行完成后，才能继续执行下一步。

![](./media/image20.png)

7.  逐个执行接下来的 2 个单元格，用于检索 url 和作业名称。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

## **练习 4：检索最佳试用 （最佳模型的试用/运行）**

1.  在此练习下的第一个单元格上方添加一个单元格。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

2.  复制以下代码。单击 **Run cell**。

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image23.png)

3.  继续逐个执行接下来的 3 个单元格，分析每个代码及其输出。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image24.png)

4.  执行下一个单元格以**获取父级运行**。

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image25.png)

5.  执行下一个单元格以**打印父标签**。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image26.png)

6.  执行下一个单元格以**获取 AutoML 最佳子运行**。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image27.png)

7.  执行下一个单元格以**获取最佳模型运行的指标**。

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image28.png)

8.  执行接下来的 3 个单元格以**在本地下载最佳模型**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

## **练习 5：注册最佳模型并部署**

### **任务 1：创建托管联机终端节点**

1.  执行此任务下的前 2 个单元格。

![](./media/image30.png)

2.  使用代码执行下一个单元格

**ml_client.begin_create_or_update(endpoint).result()**

这将创建一个名为 **regression-\<Currentdate&time\>** 的在线终端节点。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image31.png)

3.  检查是否有通知指出**端点 “regression-\<Currentdate&time\>”
    更新已完成**。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

### **任务 2：注册最佳模型并部署**

1.  执行 Register best model 下的第一个单元并部署 -\> **Register
    model**，以注册名为 **hardware-performance-model** 的模型。

2.  执行成功后，执行下一个单元格以检索已注册的模型 ID。

> ![](./media/image33.png)

### **任务 3：部署**

1.  在 Deploy （部署） 下的第一个单元格中，将值 **instance_type** 替换为
    **Standard_E4s_v3。**

2.  然后，执行 cell 以部署最佳模型。

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

3.  执行下一个单元以创建部署。

![A picture containing text, screenshot, line, font Description
automatically generated](./media/image35.png)

4.  **这大约需要 40 分钟才能完成。**您还可以从 **Endpoints**
    （从左侧窗格中选择 **Endpoints ，**然后单击您之前部署的
    **regression-XXXXXXX** 终端节点） 下检查状态。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  执行完成且部署成功后，该单元将输出部署详细信息。

![](./media/image37.png)

6.  此外，在 Endpoints details （终端节点）
    详细信息页面中，部署状态将变为 **Succeeded**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

7.  执行笔记本中的下一个单元，以便部署获取 100% 的流量。

![A screenshot of a computer Description automatically generated with
low confidence](./media/image39.png)

8.  在 Endpoints details （终端节点详细信息） 页面中检查 Live traffic
    allocation （实时流量分配） 为 100%。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

## **练习 6：测试部署**

1.  执行 Test the deployment（测试部署）下的单元。

2.  验证输出。

![](./media/image41.png)

3.  关注并执行其余单元格以删除终端节点。

![](./media/image42.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image43.png)

4.  从 Endpoints （终端节点） 选项卡下检查终端节点的状态。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

**总结**

在本实验中，我们学习了如何

- 从 Python SDK 连接到 AML 工作区

- 使用 'regression（）' 工厂函数创建 AutoML 回归作业。

- 通过提交/运行 AutoML 回归训练作业，使用 AmlCompute 训练模型

- 获取模型并使用它对预测进行评分
