# Lab 06 - Treinamento do melhor modelo de regressão para o conjunto de dados de hardware

Objetivo

Neste laboratório, abordaremos como usar o AutoML para treinar um modelo
de regressão. Usaremos o conjunto de dados de desempenho de hardware
para treinar e implantar o modelo para uso em cenários de inferência. O
objetivo da regressão é prever o desempenho de determinadas combinações
de componentes de hardware.

Duração prevista – 60 minutos

# Exercício 0: Prepare o ambiente

### **Tarefa 1: Iniciar o workspace do AML**

1.  Faça login no portal do Azure, +++
    [**https://portal.azure.com**](https://portal.azure.com) +++ caso
    ainda não tenha feito.

2.  No menu do portal do Azure, selecione **All resources.**

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Selecione o workspace do Azure Machine Learning (
    **Azuemlws@lab.LabInstanceId** ).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Clique em **Launch studio**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Selecione **Compute** no painel esquerdo para criar uma instância de
    Compute. Selecione **+ New**.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

6.  Forneça os detalhes abaixo e clique em **Review + Create**.

- Compute name - +++**auto-compute**+++

- Virtual machine type – **CPU**

- Virtual Machine – **Standard E4ds_v4**

![](./media/image5.png)

7.  Selecione **Create** para criar a instância de computação.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

### **Tarefa 2: Carregar o notebook no AML Workspace**

1.  Clique em **Notebooks** no painel esquerdo. Clique nos três pontos
    ao lado de **username** em **Users** e selecione **Upload folder**.

![](./media/image7.png)

2.  Selecione Click to browse and select folder(s) e navegue até
    **C:\Labfiles** para selecionar a pasta
    **automl-regression-task-hardware-performance** e clique em
    **Upload.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image8.png)

3.  Se aparecer uma janela pop-up perguntando Upload 3 files to this
    site?, clique em **Upload**.

![A picture containing text, screenshot, display, font Description
automatically generated](./media/image9.png)

4.  Marque a caixa de seleção **I trust contents of these files** e
    depois selecione **Upload**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

5.  Abra o Notebook(the .ipynb file),
    **automl-regression-task-hardware-performance** . O Notebook será
    conectado automaticamente ao compute que criamos anteriormente.

![](./media/image11.png)

## **Exercício 1: Conectar-se ao workspace do Azure Machine Learning**

### **Tarefa 1: Importar as bibliotecas necessárias**

1.  Execute a primeira célula em **1.1** **Import the required
    libraries** para importar as bibliotecas necessárias para a execução
    deste laboratório clicando no botão Run cell no canto superior
    esquerdo da célula.

2.  Verifique se a execução foi bem-sucedida procurando um símbolo de
    marca de seleção no canto inferior esquerdo da célula.

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image12.png)

### **Tarefa 2: Configurar detalhes do workspace e obter um identificador para o workspace**

1.  Na célula abaixo de **1.2. Configure workspace details and get a
    handle to the workspace,** substitua

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP – **Your assigned Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  Clique na opção Run cell no canto superior esquerdo da célula e
    certifique-se de que haja uma marca de seleção no canto inferior
    esquerdo quando a execução for bem-sucedida.

3.  Uma saída informando "**Found the config file in : /config.json"**
    será exibida abaixo da célula.

![](./media/image13.png)

### **Tarefa 3: Mostrar informações do workspace do Azure ML,**

1.  Execute a próxima célula (a célula abaixo de Mostrar informações do
    Workspace do Azure ML).

2.  Garanta que os detalhes do workspace, assinatura, localização e
    grupo de recursos listados como saída abaixo da célula estejam todos
    corretos.

![A screenshot of a computer program Description automatically
generated](./media/image14.png)

## **Exercício 2: MLTable com dados de treinamento de entrada**

### **Tarefa 1: Criar entrada de dados MLTable**

1.  Execute a próxima célula (aquela em **2.1 Create MLTable data
    input**).

2.  Garanta que a execução seja bem-sucedida.

![A picture containing text, font, screenshot, software Description
automatically generated](./media/image15.png)

## **Exercício 3: Configurar e executar o trabalho de treinamento de Regressão do AutoML**

1.  Execute as células em **4.1** **Configure and run the AutoML
    Regression training job** uma por uma e garanta que cada célula seja
    executada com sucesso.

2.  A célula em **4.2 Run the Command** envia a tarefa do AutoML.

![A screenshot of a computer program Description automatically
generated](./media/image16.png)

3.  Você pode verificar o status do trabalho clicando em **Jobs** no
    painel à esquerda e selecionando o experimento que está no estado Em
    execução.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**Observação:** isso leva de 10 a 15 minutos para ser concluído.

4.  A próxima célula no Notebook aguarda até que o AutoMLjob seja
    concluído.

5.  Execute-o e aguarde até que a execução seja concluída para passar
    para a próxima célula.

![](./media/image19.png)

6.  Prossiga para a próxima etapa somente quando a execução estiver
    concluída.

![](./media/image20.png)

7.  Execute as próximas 2 células, uma por uma, que recuperam a URL e o
    nome da tarefa.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

## **Exercício 4: Recuperar o melhor teste (teste/execução do melhor modelo)**

1.  Adicione uma célula acima da primeira célula deste exercício.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

2.  Copie o código abaixo. Clique em **Run cell.**

> **%pip instalar azureml-mlflow**
>
> **%pip instalar mlflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image23.png)

3.  Continue executando as próximas 3 células, uma por uma, analisando
    cada código e sua saída.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image24.png)

4.  Execute a próxima célula para **Get the parent run**.

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image25.png)

5.  Execute a próxima célula para **print the parent tags**.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image26.png)

6.  Execute a próxima célula para **Get the AutoML best child run**.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image27.png)

7.  Execute a próxima célula para **Get the best model run’s metrics**.

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image28.png)

8.  Execute as próximas 3 células para **Download the best model
    locally**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

## **Exercício 5: Registrar o Melhor Modelo e Implementar**

### **Tarefa 1: Criar endpoint on-line gerenciado**

1.  Execute as 2 primeiras células desta tarefa.

![](./media/image30.png)

2.  Execute a próxima célula com o código,

**ml_client.begin_create_or_update(endpoint).result()**

Isso cria um endpoint online chamado
**regression-\<Currentdate&time\>.**

![A screenshot of a computer Description automatically generated with
low confidence](./media/image31.png)

3.  Verifique a notificação informando que que a **atualização do
    Endpoint "regression-\<Currentdate&time\>" foi concluída.**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

### **Tarefa 2: Registrar o melhor modelo e implementar**

1.  Execute a primeira célula em Register best model and deploy -\>
    **Register model**, para registrar o modelo chamado
    **hardware-performance-model** .

2.  Após a execução ser bem-sucedida, execute a próxima célula para
    recuperar a ID do modelo registrado.

> ![](./media/image33.png)

### **Tarefa 3: Implementar**

1.  Na primeira célula em Deployment, substitua o valor
    **instance_type** por **Standard_E4s_v3 .**

2.  Em seguida, execute a célula para implementar o melhor modelo.

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

3.  Execute a próxima célula para criar a implementação.

![A picture containing text, screenshot, line, font Description
automatically generated](./media/image35.png)

4.  **Isso levará cerca de 40 minutos para ser concluído**. Você também
    pode verificar o status em **Endpoints** (selecione **Endpoints** no
    painel esquerdo e clique no endpoint **regression-XXXXXXX** que você
    implementou anteriormente).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  Quando a execução for concluída e a implementação for bem-sucedida,
    a célula exibirá os detalhes da implementação.

![](./media/image37.png)

6.  Além disso, na página Endpoints details, o status da implementação
    se torna **Succeeded**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

7.  Execute a próxima célula no notebook para que a implementação ocupe
    100% do tráfego.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image39.png)

8.  Verifique se a Live traffic allocation é de 100% na página Endpoints
    details.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

## **Exercício 6: Teste a implementação**

1.  Execute a célula em Test the deployment.

2.  Verifique a saída.

![](./media/image41.png)

3.  Siga e execute as células restantes para excluir o endpoint.

![](./media/image42.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image43.png)

4.  Verifique o status do endpoint na guia Endpoints.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

**Resumo**

Neste laboratório, aprendemos como

- Conectar-se ao seu workspace do AML a partir do SDK do Python

- Criar uma tarefa de regressão do AutoML com a função de fábrica
  “regression()”.

- Treinar o modelo usando o AmlCompute , enviando/executando a tarefa de
  treinamento de regressão do AutoML

- Obter o modelo e as previsões de pontuação com ele
