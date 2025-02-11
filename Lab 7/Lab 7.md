# Lab 07: Develop and test prompt flow from Azure Machine Learning Studio

**Objective:**

In this lab, we will learn the main user journey of using prompt flow in
Azure Machine Learning studio. You learn how to enable prompt flow in
your Azure Machine Learning workspace, create and develop a prompt flow,
test and evaluate the flow, and then deploy it to production.

Expected Duration – 60 minutes

## Task 1: Getting the Azure resources ready

### Task 1.1: Create an Azure Machine Learning workspace

This task focuses on creating an Azure Machine Learning workspace. You
will discover how to set up a dedicated workspace to organize and manage
their machine learning projects effectively. This workspace serves as a
central hub for collaboration, experimentation, and deployment.

1.  Sign in to the Azure portal at +++<https://portal.azure.com>+++ and
    login with your admin tenant credentials.

2.  From the Azure portal home page, select **+ Create a resource**.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  On the **Create a resource** page, use the search bar to find
    +++Azure Machine Learning**+++** and select **Azure** **Machine
    Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  Under **Marketplace**, click on **Create dropdown** and select
    **Azure Machine Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  Provide the following information to configure your new workspace:

    - **Subscription**: Select your **assigned Azure subscription**

    - **Resource group**: Select your **assigned Resource Group**.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: Select your nearest region **(North Central US** is
  selected here)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstanceId+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  Once you are done configuring the workspace, select **Review +
    Create**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  Once the Validation is passed, click on **Create**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  Click on **Go to resource**, to view the new workspace.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**,
    select **Launch studio** under **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### Task 1.2: Create a compute

This task demonstrates the creation of a compute resource in Azure. You
will explore different compute options, such as virtual machines or
managed compute clusters, and understand how to configure and provision
resources to execute machine learning workloads efficiently.

1.  Once the **Azure Machine Learning Studio** opens, click on
    **Compute** under **Manage** from the left pane.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  Click on **+ New** on the **Compute instances** screen.

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  On the Create compute instance screen, enter the below details.

    1.  Compute name – +++**pfcompute**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Select **Standard_E4ds_v4**

> Click on **Review + Create**.

**Note:** Make a note of this compute name for later use.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  Click on **Create** in the next screen to create the compute.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**Note:** The compute takes around 10 minutes to come up to the Running
state.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**Important:** Once the Compute is up and running you can continue with
the next tasks. But, if you are taking a break from the lab execution,
please ensure to **stop** the compute instance and start it again when
you start after the break.

### Task 1.3: Create Azure OpenAI resource

1.  From the Azure portal +++https://portal.azure.com+++, search for and
    select +++**AzureOpenAI**+++.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  Click on **+ Create**.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  Fill in the below details and click on **Next**.

- Resource group - Select your assigned Resource group

- Region – Select a region (North Central US is being used here)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  Accept the defaults in the next pages and click on **Create** in the
    **Review + submit** page.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  Click on **Go to resource** once the deployment is complete.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  Select **Keys and Endpoint** from the left pane.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  Copy the **Key** and the **Endpoint** and save it in a notepad for
    use in a later part of the lab.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  From the **Azure Machine Learning Studio**, select **Model catalog**
    from the left pane and select **gpt-4o**.

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  Click on **Deploy** to deploy the model.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. Accept the deployment name and select **Deploy**. Keep a note of
    this name for future usage.

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## Task 2: Set up a Prompt flow connection

1.  From the left navigation pane of the Azure Machine Learning Studio,
    select **Prompt flow**. Select **Connections** from the menu bar.
    Select the drop down next to **Create** and select **Azure OpenAI**.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  In the Add Azure OpenAI connection wizard, provide the below details
    and select **Save**.

- Name – +++**AoaiML_pf**+++

- Provider – Select **Azure OpenAI**

- Subscription ID – Select your **assigned subscription**

- Azure OpenAI Account Name – Select **AOAI-PF@lab.LabInstanceId**

- Auth Mode – Select **API Key**

- API Key – Provide the **key** that we saved the **Azure OpenAI
  resource**

- API base – Provide the **endpoint** that we saved from the **Azure
  OpenAI resource**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  Check that the connection creation is successful.

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## Task 3: Create and develop your prompt flow

1.  In the **Flows** tab of the **Prompt flow** home page,
    select **Create** to create the prompt flow. The **Create a new
    flow** page shows flow types you can create, built-in samples you
    can clone to create a flow, and ways to import a flow.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  Select **Clone** under the **WebClassification** category.

In the **Explore gallery**, you can browse the built-in samples and
select **View detail** on any tile to preview whether it's suitable for
your scenario.

This lab uses the **Web Classification** sample to walk through the main
user journey.

Web Classification is a flow demonstrating multiclass classification
with a LLM. Given a URL, the flow classifies the URL into a web category
with just a few shots, simple summarization, and classification prompts.
For example, given a URL https://www.imdb.com, it classifies the URL
into Movie.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  Accept the name populated for **Folder name** and then select
    **Clone**.

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  A compute session is necessary for flow execution. The compute
    session manages the computing resources required for the application
    to run, including a Docker image that contains all necessary
    dependency packages.

5.  On the flow authoring page, start a compute session by
    selecting **Start compute session**.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**Note:** It will take around **10 minutes** to get the compute session
in the Running state.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## Task 4: Inspect the flow authoring page

The compute session can take a few minutes to start. While the compute
session is starting, view the parts of the flow authoring page.

- The **Flow** or *flatten* view on the left side of the page is the
  main working area, where you can author the flow by adding or removing
  nodes, editing and running nodes inline, or editing prompts. In
  the **Inputs** and **Outputs** sections, you can view, add or remove,
  and edit inputs and outputs.

When you cloned the current Web Classification sample, the inputs and
outputs were already set. The input schema for the flow is name: url;
type: string, a URL of string type. You can change the preset input
value to another value like https://www.imdb.com manually.

- **Files** at top right shows the folder and file structure of the
  flow. Each flow folder contains a *flow.dag.yaml* file, source code
  files, and system folders. You can create, upload, or download files
  for testing, deployment, or collaboration.

- The **Graph** view at lower right is for visualizing what the flow
  looks like. You can zoom in or out, or use auto layout.

You can edit files inline in the **Flow** or flatten view, or you can
turn on the **Raw file mode** toggle and select a file from **Files** to
open the file in a tab for editing.

You can edit files inline in the **Flow** or flatten view, or you can
turn on the **Raw file mode** toggle and select a file from **Files** to
open the file in a tab for editing.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## Task 5: Set up LLM nodes

For each LLM node, you need to select a **Connection** to set the LLM
API keys. Select your Azure OpenAI connection.

Depending on the connection type, you must select
a **deployment_name** or a model from the dropdown list. For an Azure
OpenAI connection, select a deployment. 

1.  For the summarize_text_content, fill in the below details.

Connection – Select **AoaiML_pf**

Api – Select **chat**

deployment name – Select **gpt-4o-2024-11-20**

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  Set up connection similarly for the LLM nodes **classify_with_llm**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

3.  To test and debug a single node, select the **Run** icon at the top
    of a node in the **Flow** view. You can expand **Inputs** and change
    the flow input URL to test the node behavior for different URLs.

4.  The run status appears at the top of the node. After the run
    completes, run output appears in the node **Output** section.

5.  Move to the starting of the flow and execute the
    **fetch_text_content_from url** and execute the block.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

The **Graph** view also shows the single run node status.

6.  Under **Inputs** section, provide the value for the **Value** field
    as
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++

Select **Run** from the top right, to test and debug the whole flow.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## Task 5: View flow outputs

You can also set flow outputs to check outputs of multiple nodes in one
place. Flow outputs help you:

- Check bulk test results in a single table.

- Define evaluation interface mapping.

- Set deployment response schema.

1.  Select **View outputs** in the top banner or the top menu bar to
    view detailed input, output, flow execution, and orchestration
    information.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  On the Outputs tab of the Outputs screen, note that the flow
    predicts the input URL with a **category** and **evidence**. ![A
    screenshot of a computer Description automatically
    generated](./media/image42.png)

3.  Select the **Trace** tab on the **Outputs** screen and then
    select **flow** under **node name** to see detailed flow overview
    information in the right pane. Expand **flow** and select any step
    to see detailed information for that step.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**Summary:**

In this lab, we have learnt to classify the URL into a web category with
simple summarization, and classification prompts using prompt flow in
Azure Machine Learning Studio.
