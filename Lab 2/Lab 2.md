# Lab 02 - Creating a labeled dataset using Azure Machine Learning data labeling tools

Expected execution time: **40 min**

Lab type: **Instructor led**

**Objective**

In this lab you will learn how to use the Azure Machine Learning Data
Tools in Azure Machine Learning studio to manage their collections of
unlabeled data into labeled datasets that accommodate the classes that
would be detected by the trained object detection model.

## **Exercise 1: Getting the Azure resources ready**

### **Task 1: Create an Azure Storage Account**

1.  Login to Azure portal at +++**https://portal.azure.com**+++ if not
    already logged in.

2.  Type in +++**storage account**+++ in the search bar and select the
    **Storage account**.

    ![](./media/image1.png)

3.  Select **+Create**.

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image2.png)

4.  On the Create a storage account page, enter the below details.

    **Project details**

    - Subscription – Select your **subscription**.
    
    - Resource group – Select the **Resource group** created in the previous
      lab.

    **Instance details**

    - Storage account name – +++**imagestoreaccXX**+++ (Replace XX with a
      unique number)
    
    - Region – Select the Region in which you had created your AML Workspace
      (**East US/East US2/North Central US**)
    
    - Performance – Select **Standard**
    
    - Redundancy – Select **Locally-redundant storage(LRS)**
    
    Select **Next.**

    ![A screenshot of a computer Description automatically generated](./media/image3.png)

5.  On the Advanced tab, ensure that the option **Allow cross-tenant
    replication** under **Blob storage** section is unchecked. Accept
    the other defaults and select **Review + create**.

    ![A screenshot of a computer Description automatically
generated](./media/image4.png)

6.  Once the validation passes, click on **Create**.

    ![A screenshot of a computer error Description automatically generated](./media/image5.png)

7.  Once the deployment is completed, click on **Go to resource**.

    ![](./media/image6.png)

8.  Keep a note of the Storage account name(**imagestoreaccXX**) as this
    will be used in the later part of the lab. Stay on the same page and
    continue with the next task.

    ![A screenshot of a computer Description automatically
generated](./media/image7.png)

### **Task 2: Create an Azure Storage Container**

1.  From the left menu of the storage account page, scroll to the **Data
    Storage** section, then select **Containers**.

    ![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  Select the **+ Container**. In the New container pane that opens,
    type in the name of the container as +++imagedata+++ and then click
    on **Create**.

    ![A screenshot of a computer Description automatically
generated](./media/image9.png)

3.  Once the container is created, select the **Access keys** under
    **Security + networking** from the left pane. On the Access keys
    page, click on **Show** against the key value and then **copy** the
    key. Store the copied value in a notepad for future reference.

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4.  Navigate back to the containers page by selecting **Containers**
    from the left pane.

    ![](./media/image11.png)

5.  Select the newly created container, **imagedata**.

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6.  Click on **Upload**. On the **Upload blob** pane, click on **Browse
    for files** and open the **train_img** folder from under
    **C:\Labfiles**

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image13.png)

7.  Select all the files in the train_img folder and click on **Open**.

    ![A screenshot of a computer Description automatically
generated](./media/image14.png)

8.  Click on **Upload** on the Upload blob page.

    ![](./media/image15.png)

9.  Once uploaded, **Successfully uploaded blob(s)** message is
    displayed, close the **Upload blob** pane.

    ![](./media/image16.png)

10. Once completed, you should see that all the 242 images have been
    added to the Azure Storage Container.

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **Exercise 2: Create an Azure Machine Learning data labeling project**

1.  From the Home page of the Azure Machine Learning Studio, select
    **Data Labeling** from under **Manage** in the left pane.

    ![A screenshot of a computer Description automatically
generated](./media/image18.png)

2.  Select **+ Create.**

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image19.png)

3.  Under the **Project details** section, give the following details.

    -  **Project name** - +++**soda**+++

    -  Media type – **Image**

    -  **Labeling task type - Object Identification (Bounding Box)** 

    Select **Next**.

    ![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

4.  In the **Add workforce (optional)** screen, leave the option
    disabled and select **Next** to continue.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image21.png)

5.  On the **Select or create data page**, click on **+ Create**.

    ![](./media/image22.png)

6.  On the **Data type** pane of **Create data asset** page, provide the
    below details.

    -  **Name** – +++**sodaObjects**+++

    -  **Description –** +++**Image labelling**+++

    -  **Type –** File

    Click on **Next**.

    ![A screenshot of a computer Description automatically generated](./media/image23.png)

7.  On the **Data source** pane of **Create data asset** page, select
    **From Azure storage** option and then click on **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

8.  On the **Storage type** pane of **Create data asset** page, select
    **Create new datastore**.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image25.png)

9.  On the **New** **datastore** pane, provide the below details.

    -  **Datastore name** – +++**sodadatastore**+++

    -  **Datastore type** – Select **Azure Blob Storage**

    -  **Account selection method –** Select **From Azure
        subscription**

    -  **Subscription ID –** Select your subscription

    -  **Storage account –** Select **imagestoreacc**

    -  **Blob container –** Select **imagedata**

    -  **Authentication type –** Select **Account Key**

    -  **Account key –** Enter the account key saved earlier in
        Exercise 1

    Click on **Create**.

    ![](./media/image26.png)

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image27.png)

10. **Create success** message gets displayed on the **Select a
    datastore** page**.** Select the **sodadatastore** that got created.
    Click on **Next**.

    ![A screenshot of a computer Description automatically generated](./media/image28.png)

11. Under Choose a storage path, select **Enter storage path manually**
    and type in **/** for the Storage path, enable **Skip data
    validation**. Click on **Next**.

    ![A screenshot of a computer Description automatically generated](./media/image29.png)

12. Review the details and click on **Create**.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

13. Back in the **Select or create data** pane, select **sodaObjects.**
    Click on **Next**.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image31.png)

14. On the **Incremental refresh** page, select **Enable incremental
    refresh at regular intervals.** Click on **Next**.

    ![A screenshot of a computer Description automatically generated](./media/image32.png)

15. On the **Label categories** page, click on **Add label category**
    twice to add two more category name place holders in addition to the
    already existing one.

    ![A screenshot of a computer Description automatically generated](./media/image33.png)

16. After adding, type in +++**coke**+++, +++**diet_coke**+++ and
    +++**sprite**+++, one in each label category place holder. Click on
    **Next**.

    ![A screenshot of a computer Description automatically generated](./media/image34.png)

17. Leave the Labelling instructions blank and click on **Next**.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image35.png)

18. Click on **Next** from the **Quality control(preview)** page.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image36.png)

19. Disable **Enable** **ML assisted labelling** option and click on
    **Create** **project**.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing** message gets displayed on the Data Labelling
    screen. Click on the **soda** project**.**

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image38.png)

21. Click on **Label data**.

    ![](./media/image39.png)

22. The **Shortcut keys** on the top right shows the different shortcuts
    available.

    ![A group of soda cans on a table Description automatically generated with medium confidence](./media/image40.png)

23. The top menu bar provides the different options available.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image41.png)

24. The first image opens on the screen. Select the appropriate tag from
    the **Tags** pane on the left.

    Then, click on the image and drag a little to see the label being attached to the image. Click on **submit**.

     ![A screenshot of a computer Description automatically generated with medium confidence](./media/image42.png)

25. Repeat the same process for the next images that come up on
    submitting the current one.

    Label at least 10 images.

    ![](./media/image43.png)

26. The next image gets uploaded till the end of the images is reached.
    Please stop at any point beyond 10 images or proceed and complete
    labelling for all the images.

27. Click on soda on the top navigation path to go back to the
    **Dashboard**.

    ![A screenshot of a computer Description automatically generated with medium confidence](./media/image44.png)

28. The **Dashboard** gives the details on the **labeled assets** and
    the **label distribution**.

    ![](./media/image45.png)

    ![A screenshot of a computer Description automatically generated with low confidence](./media/image46.png)

29. Click on **Export.**

    ![A screenshot of a graph Description automatically generated with low confidence](./media/image47.png)

30. On the **Export data** pane, select the

    - **Asset type - Labeled**

    - **Export format -** **Azure ML dataset**

    Click on **Submit**.

    ![A screenshot of a computer Description automatically generated with low confidence](./media/image48.png)

31. **Labels successfully exported** message gets displayed on the
    Dashboard page once the export is completed. Click on the **file
    link** in the success message to open the details of the exported
    file.

    ![A screenshot of a computer Description automatically generated with low confidence](./media/image49.png)

    ![A screenshot of a computer Description automatically generated](./media/image50.png)

32. Click on the **View in** **datastores** or **View in Azure portal**
    link under the **Datasources** -\> **Actions** section.

    ![](./media/image51.png)

33. View in datastores.

    ![A picture containing text, number, software, font Description automatically generated](./media/image52.png)

**Summary**

In this lab, you have learnt how to create a data asset from Azure
storage and how to label the images and create a labeled dataset.

This entire set of tasks also belong to the **Data: Explore & prepare**
stage of the **Machine Learning project workflow.**
