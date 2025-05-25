# **实验 10 - 使用负责任 AI 仪表板提高机器学习模型的性能**

**目的**

这个实验室是为了让学生实践学习如何使用负责任的AI仪表板来调试机器学习模型，以提高模型的性能，使其更加公平、包容、安全可靠和透明。 

在本实验中，我们将探讨如何使用 Azure 负责任 AI （RAI） 仪表板的“**Model
Overview**”部分。我们将使用从误差分析实验室创建的队列来研究为什么模型在一个队列中的行为优于另一个队列。

预期持续时间 – 60 分钟

## **练习 1：准备资源**

### 任务 1：克隆此实验室的存储库

1.  在浏览器中，通过 <https://portal.azure.com> 登录到 Azure 门户

2.  单击 Azure 门户上的 Cloud Shell 图标打开 **Cloud Shell**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  在 Azure Cloud Shell 命令提示符下，通过执行以下命令克隆 **Diabetes
    Hospital Readmission** 项目 github 存储库。

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> 这将在本地克隆存储库的内容。
>
> ![](./media/image2.png)

4.  通过执行以下命令切换到项目目录。

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### 任务 2：使用 Azure CLI 登录

1.  在 Cloud Shell 中，执行以下命令。

**az login**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

2.  在控制台中打开 URL，然后在浏览器中键入代码。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

3.  选择 **Azure** **登录**凭据。

> ![A screenshot of a phone Description automatically generated with
> medium confidence](./media/image5.png)

4.  单击 **Continue**（继续）。

> ![A screenshot of a computer error Description automatically generated
> with medium confidence](./media/image6.png)

5.  关闭浏览器并返回到 Azure 门户。

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  登录详细信息将显示在 Cloud Shell 中。

> ![A screenshot of a computer Description automatically
> generated](./media/image8.png)

7.  将环境默认值设置为**分配的 Resource group**。

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **练习 2：运行用于训练模型和创建 RAI 控制面板的作业**

1.  执行以下命令，将**训练数据集**注册到 Azure 机器学习工作区。

> **az ml data create -f cloud/train_data.yml**

将创建数据资产，并在 Cloud Shell 上显示详细信息。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  执行以下命令，将**测试数据集**注册到 Azure 机器学习工作区。

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  创建用于运行作业的 **compute instance**
    。然后，复制运行结束时的计算名称（例如
    **compute-xxxxxxxxxxxx**）以供以后使用。

- 执行以下命令以**创建计算**。

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

4.  在 Cloud Shell 菜单上，单击 **Open editor { }** 窗格以编辑某些文件。

> ![Open editor](./media/image13.png)

5.  单击 **RAI-Diabetes-Hospital-Readmission-classification**
    文件夹以展开目录。

![Expand directory](./media/image14.png)

6.  导航到 **cloud/training_job.yml**
    文件。然后将计算名称的占位符替换为您之前复制的**计算实例名称**。

![Training job update](./media/image15.png)

7.  右键单击文件中的任意位置，然后选择 **Save** 选项以保存文件。 

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8.  接下来，导航到 **cloud/rai_dashboard_pipeline.yml**
    文件。然后，使用您之前复制的计算实例名称更新**计算名称**的占位符。

![Rai pipeline update](./media/image17.png)

9.  右键单击文件中的任意位置，然后选择 **Save** 选项以保存文件。

10. 右键单击文件中的任意位置，然后选择 **Quit** 选项以关闭编辑器窗口。

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image18.png)

11. 返回 Cloud Shell
    命令提示符，提交作业以训练模型。在训练期间，等待作业将其运行状态更新为
    **Completed**。复制下面的代码块来执行此作。

> **run_id=$(az ml job create --name my_training_job -f
> cloud/training_job.yml --query name -o tsv)**
>
> **\# wait for job to finish while checking for status**
>
> **if \[\[ -z "$run_id" \]\]**
>
> **then**
>
> **echo "Job creation failed"**
>
> **exit 3**
>
> **fi**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **if \[\[ -z "$status" \]\]**
>
> **then**
>
> **echo "Status query failed"**
>
> **exit 4**
>
> **fi**
>
> **running=("Queued" "Starting" "Preparing" "Running" "Finalizing")**
>
> **while \[\[ ${running\[\*\]} =~ $status \]\]**
>
> **do**
>
> **sleep 8**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **echo $status**
>
> **done**
>
> **注意：**如果此脚本未正确粘贴，请手动复制并粘贴
>
> **注意：**此脚本的执行大约需要 3 到 5 分钟。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

12. （可选）可以从 **Azure Machine Learning Studio**
    **(**<https://ml.azure.com/>**)** -\> **Jobs**
    中检查正在运行的作业的状态

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image21.png)

13. 训练作业成功完成后，将模型注册到 Azure
    机器学习工作区。执行以下命令来执行此作。

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> 此命令将模型注册到 AML 工作区，并在 Cloud Shell
> 中提供详细信息，如下面的屏幕截图所示。
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

14. 提交作业管道以创建 **RAI 控制面板**。执行以下命令来执行此作。

az ml job create --file cloud/rai_dashboard_pipeline.yml

此命令将提交作业，并且 **Cloud Shell** 将填充管道的初始阶段，即
Preparing 状态。

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

15. [*https://ml.azure.com/*](https://ml.azure.com/) 登录到 **Azure
    Machine Learning studio**，以监视用于创建 RAI 仪表板的管道作业。

16. 选择 **Pipelines**。要查看创建 RAI
    控制面板的管道作业的进度，请单击作业 **Display name**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

17. 实验将处于 **Running** 状态。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

18. 完成后，状态将更改为 **Completed** （已完成） 并创建 RAI 控制面板。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

19. 单击左侧导航栏中的 **Models**
    选项卡。然后单击模型的名称以打开详细信息页面。

> ![](./media/image29.png)

20. 在顶部菜单中选择 **Responsible AI** 选项。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

21. 现在，您可以开始使用 RAI 控制面板了。

## **练习 3：错误分析：**

RAI 控制面板的 Error Analysis （错误分析）
部分有助于提供影响模型错误率的特征组的错误分布。错误通常不会均匀分布在不同的数据子组中，错误分析可帮助您识别错误率最高的特征。

### 任务 1：查找模型错误：

在本任务中，我们将探索如何使用错误分析在经过训练的模型中查找错误，以确定错误的位置。此外，我们还将学习如何创建数据队列，以调查为什么模型在某些队列中表现不佳，而在其他队列中表现不佳。

1.  点击名称 **Diabetes Hospital Readmission**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

2.  选择 **Compute** （计算）。

![](./media/image32.png)

#### **任务 1.1：为错误最高的树路径识别并创建同类群组**

要开始分析，您可以观察到根节点显示，在评估模型时，在总共 994
个测试数据中发现了 168 个错误的预测。

1.  查找错误数最多的树路径。节点中的红色阴影越深，错误率越高。

2.  在我们的例子中，红色最深的树路径是倒数第二个叶节点。

![](./media/image33.png)

3.  **双击**此**节点**以选择通向该节点的**entire
    path**。这将突出显示路径并显示路径中每个节点的特征条件。

4.  通过单击“错误分析”部分右上角的 **Save as a new cohort**
    按钮，从所选路径创建同期群。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)

5.  将 **Cohort name 输入**为 **+++Err: Prior_Inpatient \>0; Num_meds
    \>11.50 & \<= 21.50+++**

**点击 Save （保存）。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **任务 1.2：为错误最少的树路径识别并创建同类群组**

出于对比目的，使用错误数最少的树路径创建另一个队列，看看我们是否可以深入了解为什么模型在一个队列中表现良好，而不是在另一个队列中表现良好。特征条件为
**num_lab_procedures ≤ 56.50**
的**叶节点**位于树的最左侧，是错误最少的树路径。

1.  **双击**该节点**。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

2.  单击 **Save as a new
    cohort**（另存为新同期群）。此数据集中的**筛选器**为：num_lab_procedures
    \<= 56.50、number_diagnoses \<= 6.50、prior_inpatient \<= 0.00。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

3.  将同类群组**命名**为**：+++Prior_Inpatient = 0; num_diagnoses \<=
    6.50; lab_procedures \<= 56.50+++** ，然后单击**Save**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **任务 1.3：使用特征列表确定导致模型误差的主要特征**

1.  单击 **Feature list**（功能列表）。

![](./media/image39.png)

2.  该列表根据特征对错误的贡献进行排序。特征在此列表中的位置越高，它对模型误差的贡献重要性就越高。

3.  在我们的糖尿病医院再入院模型中，**特征列表**表明以下特征是模型误差的主要贡献者之一。

    - 年龄

    - num_medications

    - 医疗

    - time_in_hospital

    - num_procedures

    - 胰岛素

    - discharge_destination

### 任务 2：使用热图查找错误

从 Feature List 中，**Age** 是导致错误最多的因素之一。因此，我们将使用
Heat map （热图） 选项卡来探索哪个年龄组的患者导致模型表现不佳。

1.  在 **Error Analysis** （错误分析） 下选择 **Heat map** （热图）。

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)

2.  在 Heat Map 选项卡下，选择 **Rows ： Feature 1** 下拉菜单中的
    **Age** 以查看它在模型误差中的作用因素。

3.  选择 **Age**
    后，我们可以看到仪表板如何具有内置智能，将功能划分为具有可能条件的不同单元格。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

2.  将鼠标**悬停**在每个单元格上，您可以看到单元格中表示的数据组的正确与错误预测的数量、错误覆盖率和错误率。

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)

3.  **超过 60 年**的单元格有 **536** 个正确的模型预测和 **126**
    个错误的模型预测。错误覆盖率为 **73.81%**，错误率为 **18.79%**

4.  **30-60 年**的单元格有 **273 个**正确的模型预测和 **25
    个**错误的模型预测。错误覆盖率为 **25.60%**，错误率为 **13.61%**。

5.  **30 岁或更短**的单元格有 **17 个**正确和 **1 个**错误模型预测。

> 由于我们的观察表明 **Age**
> 在模型的错误预测中起着重要作用，因此我们将为每个年龄组创建队列，以便在下一个实验中进一步分析。

#### ***任务 2.1：根据年龄组创建同类群组***

1.  单击 **Over 60 years**
    单元格的百分比框。您将在方形单元格周围看到一个蓝色边框。

2.  单击 **Save as a new cohort**（另存为新同期群）。

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

3.  在 Save as a new cohort 对话框中，输入

    - 群组名称 - **+++Age==Over 60 year+++**

点击 **Save** （保存）。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

4.  重复步骤 2 和 3，为其他两个 Age 单元格中的每个单元格创建一个队列。

- **队列 \#4：**姓名 **- +++之前 == 30–60 岁+++**

- **队列 \#5：**姓名 **- +++\<之前 = 30 岁+++**

### 任务 3：查看同期群列表

1.  单击 Error Analysis 部分右上角的 **Settings** 齿轮图标。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)

2.  这将打开 **Cohort Settings
    窗口窗格**，其中包含您创建的所有同类群组的列表。

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## 练习 4：使用 RAI 执行模型分析

在本实验中，我们将探讨如何使用 Azure 负责任 AI （RAI） 仪表板的“**Model
Overview**”部分。我们将使用从误差分析实验室创建的队列来研究为什么模型在一个队列中的行为优于另一个队列。

## **练习 4.1：模型概述**

### 任务 1：查看和比较模型性能指标表

1.  向下滚动到 Error Analysis 下方，找到 Model Overview 部分。

![A screenshot of a computer Description automatically
generated](./media/image47.png)

2.  在 Model Overview （模型概述） 下，选择 **Dataset Cohorts**
    （数据集队列） 窗格。这将显示使用模型指标在表中创建的不同队列。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

3.  比较错误最多的同期群 **Err： Prior_Inpatient \> 0;Num_Meds \> 11 and
    ≤ 21.50** 的误差最小 **Prior_inpatient = 0;num_diagnose ≤
    6.50;lab_procedures \< 56.50**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4.  将鼠标悬停在图表上的箱形图线上可查看测量详细信息。

![A screenshot of a computer Description automatically
generated](./media/image50.png)

5.  请注意，错误队列的准确率分数为 0.806，这很糟糕。**False Positive**
    （假阳性率） **非常低**，而 **False Negative** （假阴性）
    值很**高**。这意味着，该模型预测的大多数患者在 30
    天内不会再次入院的预测率很高。

> ![A red line in a white sheet Description automatically
> generated](./media/image51.png)

6.  接下来，查看**误差最小**的**队列**的指标，准确率得分为
    0.94，这远优于包含所有数据的模型的总体准确率得分。但是，此队列的**假阳性**率也很低，为
    **0**。

![A picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### 任务 2：检查概率分布图

1.  向下滚动以查看 **Probability** （概率） **分布**。

2.  Probability distribution 图表显示模型的概率，用于预测队列中的患者在
    30 天内是否会再次入院。

3.  比较所有 3 个队列中患者未再次入院的概率。

4.  您将看到 **All data** cohort with all the patients test
    数据集显示，大多数患者在 30
    天内不会再次入院，患者未再次入院的概率中位数为 0.854，上四分位数为
    0.986，这很好。

5.  接下来，错误率最高的同期群：**Err： Prior_Inpatient \>0;Num_meds
    \>11.50 & \<= 21.50**，显示概率略低，为0.89，中位数为0.719。

6.  最后，错误率最低的队列：**Prior_Inpatient = 0;num_diagnoses \<=
    6.50;lab_procedures \<= 56.50**，显示患者未再次入院的概率的中位数为
    0.90，上四分位数为 0.986。

> ![A screenshot of a computer Description automatically
> generated](./media/image53.png)

7.  要更改图表以显示 3 个队列中患者再次入院的概率，请单击 x 轴上的
    **Choose Label**（选择标签）按钮。

8.  选择 **Probability： Readmitted** 单选按钮。在弹出窗口窗格中。

9.  然后点击 **Apply** 按钮。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

10. 比较 3 个队列中患者再次入院的概率

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image55.png)

9.  您会看到 3 个队列再次入院的概率小于
    0.55。模型误差数最少的队列的概率最低，为
    0.179。错误最多的队列的概率最高，为 0.543。

### 任务 3：查看指标可视化图表

现在，让我们通过切换到 Metric visualizations （指标可视化）
窗格来更深入地了解模型的性能。 

1.  单击 Metric visualizations 选项卡。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

2.  要选择其他指标，请单击 x 轴上的 **Choose metric** （选择指标）
    ，从其他可用指标列表中选择 **Precision score**
    （精度分数）。然后点击 **Apply** 按钮。 

> **注意：**由于经过训练的模型是一个分类问题，因此 RAI
> 控制面板将仅显示分类指标。
>
> ![](./media/image57.png)

3.  通过查看图表，您会发现所有测试数据同期群和错误同期群的模型性能在
    ~70% 的时间内都是正确的。 

4.  对于既往无住院史且诊断数量少于 7 的患者，**错误最少队列**的
    **Precision 评分**率为 **0.94**。这与准确率分数一致。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image58.png)

5.  最后，将指标更改为 **Recall**
    （召回率），以查看模型正确预测队列中的患者将在 30
    天内再次入院的程度。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6.  召回表明，对于再次入院的患者的所有队列，**模型的预测正确率不到
    25%**。这表明，在尝试预测将在 30
    天内再次入院的患者时，模型的预测在大多数情况下都是不正确的。

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### 任务 4：查看混淆矩阵

混淆矩阵有助于检查模型正确做出正确预测的速率。这将揭示模型对患者在 30
天内再次入院与未再次入院的情况的学习效果。

1.  单击 **Confusion matrix** 选项卡。

&nbsp;

2.  您将观察到，与 **Readmitted** 相比，该**模型**对 **Not Readmitted**
    的患者表现**更好**。

3.  False Negative 的数量应小于 True
    Negative。这意味着在所有患者数据中，该模型只能正确预测 24 名患者在
    30 天内\<再次入院。

- 真阳性 （TP） 的数量为：**802**

- 假阴性 （FN） 的数量为：**159**

- 误报 （FP） 数为：**9**

- 真阴性 （TN） 的数量为：**24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **练习 2：特征同类群组**

由于错误最高的队列中有 Prior_Inpatient \> 0 天的患者，并且药物数量在 11
到 22
之间是模型错误率较高的地方，因此仔细研究Prior_Inpatient和Num_medications将有助于隔离存在问题的地方。在本实验中，我们将仅分析
Prior_Inpatient。

1.  单击 **Feature Cohorts** 选项卡。

2.  在 **Feature（s）** 下拉菜单下，向下滚动列表并选中
    **prior_inpatient** 复选框。这将显示 3
    个不同的特征队列和模型性能指标。

> ![A screenshot of a computer Description automatically
> generated](./media/image62.png)

3.  **prior_inpatient \< 3** 队列的样本量为
    **943**。这意味着测试数据中的大多数患者过去住院时间少于 3
    次。该**模型**对该队列的**准确率**为 **0.838**，这很好。

4.  测试数据中只有 39 名患者属于 **prior_inpatient ≥ 3 和 \< 6**
    队列。该模型的准确率为 **0.692**，这并不好。

5.  最后，测试数据中只有 12 名患者的既往住院时间大于或等于 6
    天。此队列的**模型准确率**为 **0.75** 是可以的。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

### 任务 1：特征概率分布

与 Dataset 同类群组类似，您可以查看 “Probability Distribution” 。

1.  您可以看到，糖尿病患者的prior_inpatient住院次数越少，患者在 30
    天内不再次入院的可能性就越大。 

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### 任务 2：功能量度可视化

1.  选择 **Metrics visualization**（指标可视化）。在 x 轴上，单击
    **Choose metric** 按钮。然后选择 **Precision score** （精度分数）
    指标。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image65.png)

2.  您看到**prior_inpatient \< 3** 患者的精确率评分为
    0.40，这非常糟糕。这意味着，在模型所做的所有预测中，只有 40%
    的预测对这个队列是正确的。

> ![A blue and white bar graph Description automatically
> generated](./media/image66.png)

3.  其他 2 个队列的精确率分数很好。

4.  接下来，为 x 轴选择 **Recall score** metric （召回率指标）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image67.png)

5.  相反，您将看到**prior_inpatient \< 3** 的患者的召回率得分为
    0.013。这意味着，对于测试数据中的大多数患者，该模型难以正确预测患者是否会在
    30 天内再次入院。

> ![A picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **总结**
>
> 本实验展示了传统模型性能指标（例如准确率、召回率、混淆矩阵等）仍然非常重要。通过将
> RAI
> 洞察和传统性能指标相结合，该控制面板为我们提供了一个全面的工具，可以在更精细的级别上分析和调试模型。
