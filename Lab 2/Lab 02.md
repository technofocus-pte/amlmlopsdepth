# 实验 02 - 使用 Azure 机器学习数据标记工具创建标记数据集

**目的**

在本实验中，你将了解如何使用 Azure 机器学习工作室中的 Azure
机器学习数据工具将其未标记数据集合管理到已标记的数据集中，这些数据集可容纳经过训练的对象检测模型将检测到的类。

预计持续时间 - 40 分钟

## **练习 1：准备 Azure 资源**

### **任务 1：创建 Azure 存储帐户**

1.  在 **Azure 门户** （+++**https://portal.azure.com**+++）
    **主**页中，在搜索栏中键入 +++**storage account**+++，然后选择
    “**Storage accounts**”。

![](./media/image1.png)

2.  选择 **+Create**。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image2.png)

3.  在 Create a storage account （创建存储帐户）
    页面上，输入以下详细信息。

> **项目详情**

- Subscription （订阅） – 选择您的**subscription** （订阅）。

- Resource group （资源组） – 选择您分配的 **Resource
  group**（资源组）。

> **实例详细信息**

- 存储帐户名称 – +++**imagestoreacc@lab.LabInstance.Id** +++

- Region （区域） – 选择您在其中创建 **AML 工作区**的**区域**

- 性能 – 选择**Standard**

- Redundancy （冗余） – 选择 **Locally-redundant storage （LRS）**
  （本地冗余存储 （LRS））

选择 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  在 Advanced 选项卡上，确保取消选中 **Allow cross-tenant
    replication** under **Blob storage** 部分。接受其他默认值，然后选择
    **Review + create**。

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  验证通过后，单击 **Create**。

> ![A screenshot of a computer error Description automatically
> generated](./media/image5.png)

6.  部署完成后，单击 **Go to resource**。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  记下 Storage account
    name（存储帐户名称），因为这将在实验室的后面部分使用。保持在同一页面上并继续执行下一个任务。

![A screenshot of a computer Description automatically
generated](./media/image7.png)

### **任务 2：创建 Azure 存储容器**

1.  在存储帐户页面的左侧菜单中，滚动到 **Data Storage** 部分，然后选择
    **Containers**。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  选择 **+ Container**。在打开的 New container 窗格中，将容器名称键入
    +++imagedata+++，然后单击 **Create**。

![A screenshot of a computer Description automatically
generated](./media/image9.png)

3.  创建容器后，从左窗格中的 **Security + networking** 下选择 **Access
    keys**。在 Access keys （访问密钥） 页面上，单击键值对应的 **Show**
    （显示），然后**复制**密钥。将复制的值存储在记事本中以备将来参考。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4.  通过从左侧窗格中选择 **Containers** （容器） 导航回容器页面。

![](./media/image11.png)

5.  选择新创建的容器 imagedata。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6.  点击 **Upload**（上传）。在 **Upload blob** （上传 blob）
    窗格中，单击 **Browse for files** （浏览文件），然后从
    **C：\Labfiles** 下打开 **train_img** 文件夹

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image13.png)

7.  选择 train_img 文件夹中的所有文件，然后单击 **Open**.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

8.  单击 Upload blob （上传 blob） 页面上的 **Upload** （上传）。

![](./media/image15.png)

9.  上传后，将显示 **Successfully uploaded blob（s）** 消息，关闭
    **Upload blob** 窗格。

![](./media/image16.png)

10. 完成后，您应该会看到所有 242 个映像都已添加到 Azure 存储容器中。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **练习 2：创建 Azure 机器学习数据标记项目**

1.  在 Azure 机器学习工作室的主页中，从左窗格中的 “**Manage**” 下选择
    “**Data Labeling**” 。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

2.  选择 **+ Create。**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)

3.  在 **Project details** （项目详细信息） 部分下，提供以下详细信息。

    1.  **项目名称** - +++**soda**+++

    2.  媒体类型 – **Image**

    3.  **标记任务类型 - Object Identification (Bounding Box)** 

选择 **Next**（下一步）。

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

4.  在 **Add workforce （optional）** （添加劳动力（可选））
    屏幕中，将选项保持禁用状态，然后选择 **Next** （下一步） 以继续。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image21.png)

5.  在 **Select or create data** （选择或创建数据） **页面**上，单击 **+
    Create** （+ 创建）。

> ![](./media/image22.png)

6.  在 **Create data asset** （创建数据资产） 页面的 **Data type**
    （数据类型） 窗格中，提供以下详细信息。

    1.  **名你** – +++**sodaObjects**+++

    2.  **描述–** +++**Image labelling**+++

    3.  **类型 –** File

> 单击 **Next**（下一步）。
>
> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

7.  在 “**Create data asset**” 页面的 “**Data source**” 窗格中，选择
    “**From Azure storage**” 选项，然后单击 “**Next**” 。

> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

8.  在 **Create data asset** （创建数据资产） 页面的 **Storage type**
    （存储类型） 窗格中，选择 **Create new datastore**
    （创建新数据存储）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image25.png)

9.  在 **New datastore** （新建数据存储） 窗格中，提供以下详细信息。

    1.  **数据存储名称** – +++**sodadatastore**+++

    2.  **数据存储类型** – 选择**Azure Blob Storage**

    3.  **帐户选择方法 – From Azure subscription** 中选择

    4.  **订阅 ID –** 选择您的订阅

    5.  **存储帐户 –** 选择**imagestoreacc**

    6.  **Blob 容器 –** 选择**imagedata**

    7.  **Authentication type –** 选择**Account Key**

    8.  **Account key （帐户密钥） –** 输入之前在练习 1 中保存的帐户密钥

> 单击 **Create**。
>
> ![](./media/image26.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image27.png)

10. **Create success** 消息将显示在 **Select a datastore**
    页面上。选择已创建的 **sodadatastore**。单击 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

11. 在 Choose a storage path 下，选择 **Enter storage path manually**
    并键入 / 作为 Storage path ，启用 **Skip data validation**。单击
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

12. 查看详细信息，然后单击 **Create**。

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

13. 返回 **Select or create data** （选择或创建数据） 窗格中，选择
    **sodaObjects**。单击 **Next**（下一步）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

14. 在 **Incremental refresh** （增量刷新） 页面上，选择 **Enable
    incremental refresh at regular intervals**
    （启用定期增量刷新）。单击 **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

15. 在 **Label categories** 页面上，单击 **Add label category**
    两次，以在现有的类别名称占位符之外再添加两个类别名称占位符。

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

16. 添加后，输入+++**coke**+++，+++**diet_coke**+++
    和+++**sprite**+++，每个标签类别占位符中各输入一个。单击
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

17. 将贴标说明留空，然后点击 **Next**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

18. 单击 **Quality control（preview）** （质量控制（预览）） 页面中的
    **Next**（下一步）。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

19. 禁用 **Enable ML assisted labelling** 选项，然后单击 **Create
    project**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing** 消息显示在 Data Labelling 屏幕上。单击 **soda**
    项目。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

21. 单击 **Label data** （标记数据）。

> ![](./media/image39.png)

22. 右上角的 **Shortcuts keys** 显示了可用的不同快捷键。

> ![A group of soda cans on a table Description automatically generated
> with medium confidence](./media/image40.png)

23. 顶部菜单栏提供了不同的可用选项。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

24. 第一个图像将在屏幕上打开。从左侧的 **Tags** 窗格中选择相应的标签。

> 然后，单击图像并稍微拖动以查看附加到图像的标签。点击
> **Submit**（提交）。
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image42.png)

25. 对提交当前图像时出现的下一个图像重复相同的过程。

> 标记至少 10 张图像。
>
> ![](./media/image43.png)

26. 将上传下一个图像，直到到达图像的末尾。请在超过 10
    张图片的任何位置停下来，或者继续并完成所有图片的标记。

27. 单击顶部导航路径上的 soda 以返回 **Dashboard**。

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image44.png)

28. Dashboard 提供有关**标记资产**和**标签分配**的详细信息。

> ![](./media/image45.png)
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image46.png)

29. 点击 **Export**。

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image47.png)

30. 在 **Export data** （导出数据） 窗格中，选择

    - **资产类型 - Labeled**

    - **导出格式 -** **Azure ML dataset**

> 单击 **Submit**。
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image48.png)

31. 导出完成后，**Labels successfully exported** 消息将显示在 Dashboard
    页面上。单击成功消息中的 **file 链接**以打开导出文件的详细信息。

> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image49.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image50.png)

32. 单击 **Datasources -\> Actions** 部分下的 **View in datastores** 或
    **View in Azure portal** 链接。

> ![](./media/image51.png)

33. 在数据存储中查看。

> ![A picture containing text, number, software, font Description
> automatically generated](./media/image52.png)

**总结**

在本实验中，你学习了如何从 Azure
存储创建数据资产，以及如何标记图像和创建标记的数据集。

这整套任务也属于**机器学习项目工作流程**的 **Data: Explore & prepare**
阶段。
