# **Laboratório 09 – Configurando MLOps com o GitHub**

**Objetivo:**

O Azure Machine Learning permite a integração com o **GitHub Actions**
para automatizar o ciclo de vida do machine learning.

Neste laboratório, você aprenderá a usar o Azure Machine Learning para
configurar um pipeline MLOps completo que executa uma regressão linear
para prever tarifas de táxi em Nova York. O pipeline é composto por
componentes, cada um com funções diferentes, que podem ser registrados
no workspace, versionados e reutilizados com várias entradas e saídas.

Duração prevista: 60 minutos

Estamos na fase MLOps do Azure Machine Learning

![](./media/image1.png)

## **Exercício 1: Preparando os recursos do Azure**

### **Tarefa 1: Criar um workspace do Azure Machine Learning**

1.  Acesse o portal do Azure em +++<https://portal.azure.com>+++ e faça
    login, caso ainda não esteja conectado.

2.  Na página inicial do portal do Azure, selecione **+ Create a
    resource**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image2.png)

3.  Na página **Create a resource**, use a barra de pesquisa para
    procurar por +++Azure Machine Learning+++

4.  Selecione **Machine Learning**.

> ![](./media/image3.png)

5.  Em **Marketplace**, clique no **Create dropdown** e selecione
    **Azure Machine Learning**.

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image4.png)

6.  Forneça as seguintes informações para configurar seu novo workspace:

    - **Subscription**: selecione sua **assinatura do Azure atribuída**

    - **Resource group**: selecione o **Grupos de Recursos atribuído** a
      você.

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: Selecione a região mais próxima **(North Central US** está
  selecionado aqui)

&nbsp;

- **Container registry: selecione Create new. Digite
  +++azuremlcr@lab.LabInstance.Id+++**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image5.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.png)

7.  Depois que a validação for aprovada, clique em **Create**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image7.png)

8.  Clique em **Go to resource** para exibir o novo workspace.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image8.png)

9.  **No Microsoft.MachineLEarningServices | Overview page**, selecione
    **Launch studio** em **Work with your model in Azure Machine
    Learning studio**.

![Uma captura de tela de uma atualização de software Descrição gerada
automaticamente](./media/image9.png)

### **Tarefa 2: Criar uma computação**

1.  Depois que o Azure Machine Learning Studio estiver aberto, clique em
    **Compute** na seção **Manage** no painel à esquerda.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image10.png)

2.  Selecione a guia **Compute clusters** e clique em **+ New**.

![](./media/image11.png)

3.  Na tela **Create compute cluster**, insira os detalhes abaixo.

    1.  Location – selecione a **Region** na qual você criou seu
        workspace do Azure Machine Learning

    2.  Virtual machine tier – **Dedicated**

    3.  Virtual machine type – **CPU**

    4.  Virtual machine size – Selecione **Standard_E4s_v3 (**marque
        Select from all options para encontrar o tamanho da VM**)**

> Clique em **Next**.
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image12.png)

4.  Na página **Advanced Settings**, insira os detalhes abaixo.

&nbsp;

1.  Compute name – +++**cpu-cluster@lab. LabInstanceId**+++

2.  Minimum number of nodes – 0

3.  Maximum number of nodes – 1

> Clique em **Create**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image13.png)

**Observação:** a computação leva cerca de 10 minutos para chegar ao
estado Em execução.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image14.png)

## **Exercício 2: Recuperar os recursos do Azure**

1.  No portal do Azure (<https://portal.azure.com>), abra o Resource
    group e anote os nomes dos seguintes recursos:

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage account**

> E salve-os localmente em um bloco de notas para serem atualizados no
> arquivo de configuração.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image15.png)

## **Exercício 3: Preparar a conta e os recursos do GitHub**

**Observação:** se você ainda não tem uma conta no GitHub, crie uma
acessando +++https://github.com/+++ -\> **Signup**.

### **Tarefa 2: Faça um fork do repositório mlops-demo** **para a sua conta do GitHub**

1.  Abra um navegador e digite este link -
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  Clique em **Fork** no canto superior direito.

![Uma captura de tela de um bate-papo Descrição gerada automaticamente
com confiança média](./media/image16.png)

3.  Isso abrirá a página **Create a new fork**. Clique em **Create
    fork.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image17.png)

4.  No seu projeto GitHub, selecione **Settings**.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image18.png)

5.  Selecione **Actions** em **Secrets and variables.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image19.png)

6.  Selecione **New repository secret**.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image20.png)

7.  Dê o nome **+++AZURE_CREDENTIALS+++** a este segredo e cole a saída
    do **Service Principal** abaixo como o conteúdo do segredo. Este
    **Service Principal já foi previamente criado para você**. Clique em
    **Add secret**.

> {
>
> "clientId": "+++@lab . Variável(spAppId)+++",
>
> "clientSecret": "+++@lab . Variável(spClientSecret)+++",
>
> "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
> "tenantId": "+++@lab. CloudSubscription.TenantId+++",
>
> "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
> "resourceManagerEndpointUrl": "https://management.azure.com/",
>
> "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
> "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
> "galleryEndpointUrl": "https://gallery.azure.com/",
>
> "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com baixa confiança](./media/image21.png)

8.  O **AZURE_CREDENTIALS** secreto que foi adicionado será exibido em
    **Repository secrets**.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image22.png)

9.  Clique em **New repository secret**.

![](./media/image23.png)

10. Forneça os detalhes abaixo.

    1.  Name – +++ARM_CLIENT_ID+++

    2.  Secret – +++@lab .Variable(spAppId)+++

> ![Uma captura de tela de um segredo de computador Descrição gerada
> automaticamente com baixa confiança](./media/image24.png)

11. Repita as etapas 9 e 10 para os valores a seguir, criando segredos
    adicionais do GitHub.

    - +++ARM_CLIENT_SECRET+++ - +++@lab . Variável(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab. CloudSubscription.TenantId+++

## **Exercício 4: Configurar parâmetros de ambiente do Machine Learning**

1.  Na página de segredos, navegue até a página do repositório clicando
    em **mlops-v2-gha-demo** ao lado do seu ID do GitHub no canto
    superior esquerdo.

![](./media/image25.png)

2.  Selecione o arquivo **config-infra-prod.yml** na raiz. Clique em
    **Edit** (ícone de lápis).

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image26.png)

3.  Altere os valores,

    1.  **Namespace** – **mlopsliteXX(**Substitua XX por um número
        aleatório)

    2.  **Postifx** – **c**

    3.  **location** – **Igual à sua região do workspace**

> Clique em **Commit changes**.
>
> Na **seção For pipeline reference**, substitua os **valores** dos
> Recursos do Azure pelos valores que buscamos e salvamos no Exercício
> 2.
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image27.png)

4.  Clique em **Commit changes** no painel de alterações de confirmação.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image28.png)

5.  Abra **deploy-model-training-pipeline-classical.yml** em
    **.github/workflows**. Clique em **Edit** (ícone de lápis).

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image29.png)

6.  No conteúdo do arquivo, substitua o valor de **Size** por
    **+++Standard_E4s_v3+++**

Selecione **Commit changes**.

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image30.png)

7.  Abra o aquivo **online-deployment.yml localizado** em
    **mlops/azureml/deploy/online.** Clique em **Edit** (ícone de
    lápis).

![](./media/image31.png)

8.  Substitua o valor de **instance_type** por
    **+++Standard_E4s_v3+++**. Clique em **Commit changes**.

![Uma captura de tela de um computador Descrição gerada automaticamente
com baixa confiança](./media/image32.png)

9.  Abra o arquivo **tf-gha-deploy-infra.yml** em **.github/workflows**.
    Clique em **Edit** e substitua Azure por +++CoursesTF+++ nas linhas
    9 e 14.

Selecione **Commit changes**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image33.png)

10. Na barra de menu superior, selecione **Actions**. Clique em **I
    understand my workflows, go ahead and enable them.**

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image34.png)

11. Essa ação exibirá os fluxos de trabalho predefinidos do GitHub
    associados ao seu projeto.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image35.png)

## **Exercício 5: Implementar a infraestrutura do Machine Learning**

1.  Selecione **tf-gha-deploy-infra.yml**. Clique em **Runworkflow**.

Selecionar

- Branch – **main**

Selecione **Run workflow**

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image36.png)

2.  Isso implementaria a infraestrutura do Machine Learning usando o
    GitHub Actions e o Terraform.

3.  Acompanhe o status do trabalho e confirme se a execução foi
    bem-sucedida.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image37.png)

**Observação:** esse fluxo de trabalho leva cerca de 5 minutos para ser
concluído.

## **Exercício 6: Implementando o pipeline de treinamento do modelo**

Em seguida, você implementará o pipeline de treinamento de modelo em seu
novo workspace do Machine Learning.

Esse pipeline criará uma instância de cluster de computação, registrará
um ambiente de treinamento definindo a imagem do Docker e os pacotes
python necessários, registrará um conjunto de dados de treinamento e, em
seguida, iniciará o pipeline de treinamento descrito na última seção.

1.  Na página do fluxo de trabalho **tf-gha-deploy-infra.yml**, clique
    em **Actions**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image38.png)

2.  Isso exibirá os fluxos de trabalho do GitHub predefinidos associados
    ao seu projeto. Selecione **deploy-model-training-pipeline** na
    lista.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image39.png)

3.  Clique em **Run workflow** -\> **Run workflow**.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image40.png)

4.  Clique no pipeline que acabou de ser iniciado para acompanhar o
    progresso.

![Uma imagem contendo texto, software, página da web, fonte Descrição
gerada automaticamente](./media/image41.png)

5.  Esse pipeline leva cerca de 15 a 45 minutos para ser concluído.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image42.png)

6.  Abaixo está uma captura de tela da execução bem-sucedida do
    pipeline.

![](./media/image43.png)

7.  Essa execução registrará o modelo no workspace do Machine Learning.

8.  Faça login no estúdio do AzureMachineLearning em
    <https://ml.azure.com/> e clique em **Data** no painel à esquerda
    para verificar se os dados de **taxi-data** foram adicionados lá.
    Isso é feito como parte da tarefa **register-dataset** do fluxo de
    trabalho.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image44.png)

9.  Clique em **Jobs** no painel à esquerda e selecione
    **taxi-fare-training**. Isso é executado na tarefa **run-pipeline**
    do fluxo de trabalho.

![](./media/image45.png)

10. Selecione o Display name da execução mais recente.

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image46.png)

11. Explore as etapas e os detalhes envolvidos no treinamento.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image47.png)

Com o modelo treinado registrado no workspace do Machine Learning, você
está pronto para implementar o modelo para pontuação.

**Resumo**

Neste laboratório, aprendemos a usar o Azure Machine Learning para
configurar um pipeline de MLOps de ponta a ponta, que preparou os dados
e implementou o pipeline de treinamento de modelo em seu novo workspace
do Machine Learning.
