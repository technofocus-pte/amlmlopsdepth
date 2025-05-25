# **Laboratório 04 – Treinando um modelo de classificação com AutoML sem código no Azure Machine Learning Studio**

**Objetivo**

Neste laboratório, aprenderemos como treinar um modelo de classificação
com no-code AutoML usando o Azure Machine Learning automated ML no Azure
Machine Learning Studio. Este modelo de classificação prevê se um
cliente irá assinar um depósito a prazo fixo com uma instituição
financeira. O automated machine learning itera rapidamente sobre várias
combinações de algoritmos e hiperparâmetros para ajudá-lo a encontrar o
melhor modelo com base em uma métrica de sucesso de sua escolha.

Duração prevista - 60 minutos

Estamos na fase **Deploy Model** do Azure Machine Learning.

![](./media/image1.png)

## **Exercício 1: Criar um workspace no Azure Machine Learning**

1.  Acesse o portal do Azure – +++**https://portal.azure.com**+++ usando
    as credenciais da guia **Resources**.

2.  Na página inicial do portal do Azure, selecione **+ Create a
    resource**.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  Na página **Create a resource**, use a barra de pesquisa para
    procurar por +++**Azure Machine Learning**+++.

> Selecione **Azure Machine Learning** em **Marketplace**.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  Em **Marketplace**, clique em **Create** no menu suspenso e
    selecione **Azure Machine Learning**.

> ![A screenshot of a software Description automatically
> generated](./media/image4.png)

5.  Forneça as seguintes informações para configurar seu novo workspace:

    - **Subscription**: Selecione sua **assinatura do Azure atribuída**

    - **Resource group**: Selecione o grupo de recursos atribuído a você

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

**Workspace Details:**

- **Workspace name: +++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **Region**: **North Central US** está selecionado aqui

- **Container registry:** Selecione **Create new.** Digite
  **+++Azuremlcr@lab.LabInstanceId**+++

![A screenshot of a computer Description automatically
generated](./media/image6.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  Quando terminar de configurar o workspace, selecione **Review +
    Create**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

7.  Depois que a validação for aprovada, clique em **Create**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

8.  Clique em **Go to resource**, para visualizar o novo workspace.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

9.  **Na página Microsoft.MachineLEarningServices | Overview**,
    selecione **Launch studio** no **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

## **Exercício 2: Criar uma tarefa de ML automatizada**

1.  Navegue até a guia Azure Machine Learning Studio.

2.  No painel esquerdo, selecione **Automated ML** na seção
    **Authoring**.

3.  Clique em **+ New Automated ML job**.

![](./media/image12.png)

### **Tarefa 1: Criar ativo de dados**

1.  Na página **Basic settings**, dê ao novo experimento o nome
    +++MarketingExperiment+++, aceite os outros padrões e clique em
    **Next**.

![](./media/image13.png)

2.  Na página Tipo de tarefa e dados, selecione **Classification** em
    **Select task type** e selecione **+ Create** em **Select data.**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  Na página Create data asset, forneça os detalhes abaixo.

- **Name** – +++marketingdata+++

- **Type** – **Tabular**

- Clique em **Next**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image15.png)

4.  No painel **Data source**, selecione **From local files** e clique
    em **Next**.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

5.  Em **Destination storage type,** selecione o armazenamento de dados
    padrão que foi configurado automaticamente durante a criação da sua
    área de trabalho: **workspaceblobstore**. Você carrega o arquivo de
    dados neste local para disponibilizá-lo na sua área de trabalho.
    Selecione **Next**.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

6.  Em **File or folder selection**, selecione **Upload files or
    folder** \> **Upload files**. Escolha o arquivo
    **bankmarketing_train.csv** em **C:/Labfiles**. Selecione **Next**.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

7.  Quando o carregamento terminar, a área **Data preview** será
    preenchida com base no tipo de arquivo. No formulário **Settings**,
    revise os valores dos seus dados. Em seguida, selecione **Next**.

[TABLE]

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

8.  O formulário **Schema** permite uma configuração mais detalhada dos
    seus dados para este experimento. Para este exemplo, selecione o
    botão de alternância para **day_of_week**, para não incluí-lo.
    Selecione **Next**.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

9.  No formulário **Review**, verifique as informações e selecione
    **Create** para concluir a criação do seu **data asset**.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. De volta à página **Create a new Automated ML job**, é exibida uma
    mensagem de **sucess** para a criação do ativo de dados. Selecione o
    ativo de dados **marketingdata** criado e clique em **Next**.

> **Observação:** Se os **marketingdata** não forem exibidos, clique em
> Refresh para que sejam listados.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

### **Tarefa 2: Configurar trabalho**

1.  Na página **Task Settings** , selecione **y (String)** como a
    **Target column**, que é o que você deseja prever. Essa coluna
    indica se o cliente assinou um depósito a prazo ou não.

2.  Selecione **View additional configuration settings** e preencha os
    campos conforme mostrado a seguir. Essas configurações servem para
    controlar melhor o trabalho de treinamento. Caso contrário, os
    padrões serão aplicados com base na seleção do experimento e nos
    dados.

- Primary metric – AUCWeighted

- Explain best model – Ativar

- Use all supported models - Ativar

- Blocked models – Nenhum

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3.  Selecione **Limits** e insira +++**60**+++ no campo **Experiment
    timeout(minutes)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a test AI-generated content may be
incorrect.](./media/image25.png)

4.  Em **Validate and test**, forneça os valores abaixo e clique em
    **Next**.

- Validation type - Selecione **k-fold cross-validation**

- Number of cross validations – Selecione **2**

![A screenshot of a computer Description automatically
generated](./media/image26.png)

5.  Na página Compute, selecione o compute type como **Compute cluster**
    e clique em **+ New**.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

6.  No painel **Create compute cluster**, selecione os detalhes abaixo e
    clique em **Next**.

- Location – **North Central US** (O mesmo que a localização do seu
  workspace do Azure Machine Learning)

- Virtual machine tier – **Dedicated**

- Virtual machine type - **CPU**

- Virtual machine size -Selecione **Standard_DS12_v2**

![A screenshot of a computer Description automatically
generated](./media/image28.png)

7.  Nas Advanced settings, forneça os detalhes abaixo e selecione
    **Create**.

- Compute name - +++automl-compute+++

- Minimum number of nodes - 0

- Maximum number of nodes – 1

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

8.  Selecione **Next** quando o provisionamento de computação for
    bem-sucedido.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

9.  Na página **Review**, selecione **Submit the training job**.

> ![A screenshot of a computer Description automatically
> generated](./media/image31.png)

10. A tela **Overview** é aberta com o **Status** na parte superior,
    quando a preparação do experimento começa. Esse status é atualizado
    à medida que o experimento avança. Notificações também aparecem no
    Studio para informar o status do seu experimento.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

> **Observação:** O treinamento leva cerca de 40 minutos para ser
> concluído.

## **Exercício 3: Explorar modelos**

Enquanto aguarda a conclusão do treinamento, explore os modelos
associados.

1.  Navegue até a guia **Modelos + child** jobs para ver os algoritmos
    (modelos) testados.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

2.  Selecione o modelo **StandardScalerWrapper, XGBoostClassifier**.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

3.  Clique em **Metrics** e explore os detalhes na guia Métricas.

.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

4.  Enquanto espera que todos os modelos de experimento sejam
    concluídos, selecione o **Algorithm name** de um modelo concluído
    para explorar os detalhes de desempenho. Selecione as guias
    **Overview** e **Metrics** para obter informações sobre as tarefas.

> **Importante:** O treinamento do modelo leva cerca de 40 minutos para
> ser concluído. Continue com o próximo laboratório enquanto isso
> estiver em andamento. Retorne a este laboratório assim que o status
> mudar para **Completed**.

## **Exercício 4: Explicações do Modelo**

As explicações do modelo podem ser geradas sob demanda. O painel de
explicações do modelo, que faz parte da guia **Explanations (preview),**
resume essas explicações.

1.  Na guia Models + child jobs tab(a partir da tarefa principal),
    selecione **MaxAbsScaler, LightGBM.**

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  Selecione a guia **Explain model**.

![A screenshot of a computer Description automatically
generated](./media/image37.png)

3.  No painel Explain model que será aberto, selecione

    1.  Select compute type - **Compute cluster**

    2.  Select AzureML compute instance - Selecione **automl-compute**

Selecione **Create**.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  Uma mensagem de sucesso é exibida. Selecione a guia
    **Explanations(preview)**. Essa guia é preenchida após a conclusão
    da execução de explicabilidade.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

5.  Expanda o painel esquerdo. Em **Features**, selecione a linha que
    diz **raw**. Depois, selecione a guia **Aggregate feature
    importance**. Este gráfico mostra quais características dos dados
    influenciaram as previsões do modelo selecionado.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

Neste exemplo, a variável **duration** parece ter a maior influência nas
previsões deste modelo.

## **Exercício 5: Implementar o melhor modelo**

A interface de **Machine Learning automatizado** permite que você
implante o melhor modelo como um serviço da web. A ***Implementação*** é
a integração do modelo para que ele possa fazer previsões sobre novos
dados e identificar áreas potenciais de oportunidade. Para este
experimento, a implantação como serviço web significa que a instituição
financeira agora possui uma solução web iterativa e escalável para
identificar potenciais clientes para depósitos a prazo fixo*.*

Após a execução do experimento ser concluída, a página **Details** é
preenchida com uma seção de **Best model summary**. No contexto deste
experimento, o, **VotingEnsemble** é considerado o melhor modelo, com
base na métrica **AUCWeighted**.

1.  Selecione **Jobs** no painel esquerdo e selecione o experimento que
    você criou.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image41.png)

2.  Clique no display name do experimento.

![](./media/image42.png)

3.  Verifique se o status está **Completed**.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

4.  Uma vez que a execução do experimento esteja completa, a página de
    **Details** é preenchida com uma seção de **Best model summary**. No
    contexto deste experimento, **VotingEnsemble** é considerado o
    melhor modelo, com base na métrica **AUC_weighted**.

> ![A screenshot of a computer Description automatically
> generated](./media/image44.png)

Implementamos este modelo, mas esteja ciente de que a implementação leva
cerca de 20 minutos para ser concluída. O processo de implantação
envolve várias etapas, incluindo registrar o modelo, gerar recursos e
configurá-los para o serviço da web.

5.  **Selecione VotingEnsemble** para abrir a página específica do
    modelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

6.  **Selecione o menu Deploy** no canto superior esquerdo e selecione
    **Deploy to web service**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

7.  Preencha o painel **Deploy a model** da seguinte forma:

[TABLE]

> Clique em **Deploy**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image47.png)

8.  Uma mensagem de sucesso informando que o **Model deployment is
    successfully triggered** é exibida na tela Model e o status passa
    para **Running**.

![](./media/image48.png)

9.  Quando a implementação estiver concluída, o status será alterado
    para **Completed**.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

> Agora você tem um serviço web operacional para gerar previsões.

## **Exercício 6: Excluir os recursos**

### **Tarefa 1: Excluir Endpoint**

1.  No painel esquerdo do AML Studio, clique em **Endpoints**.

2.  Selecione o endpoint, **my-automl-deploy**, e clique em **Delete**.

![](./media/image50.png)

3.  Selecione **Delete** na caixa de diálogo Delete real-time endpoint.

4.  Você deve receber uma mensagem de sucesso assim que o endpoint for
    excluído.

**Resumo**

Neste laboratório, aprendemos a treinar um modelo de classificação com
AutoML no-code no Azure Machine Learning Studio e a implementar o melhor
modelo como um serviço da web.
