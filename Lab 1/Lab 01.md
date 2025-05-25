# Laboratório 01- Preparar conjunto de dados, treinar e implantar um modelo de classificação usando o Azure Machine Learning Studio.

**Objetivo**

O foco deste laboratório é orientá-lo no processo de configuração de um
ambiente do Azure Machine Learning, carregamento, acesso e exploração de
dados, além de treinamento e implementação de um modelo de classificação
de imagens usando o Azure Machine Learning Studio.

Duração prevista - 45 minutos

### Exercício 1: Configurando o workspace do Azure Machine Learning

### Tarefa 1: sincronizar o relógio da VM

1.  Depois de fazer login na VM, clique com o botão direito do mouse no
    relógio no canto inferior direito da tela.

2.  Selecione **Adjust date and time.**

&nbsp;

3.  Na tela Settings que será exibida, clique em **Sync now** em
    Additional settings.

> ![A screenshot of a computer Description automatically
> generated](./media/image1.png)

4.  Essa ação se encarrega de sincronizar a hora, caso a sincronização
    automática não funcione.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image2.png)

### Tarefa 2: Preparar os recursos do Azure

Esta tarefa se concentra na na criação de um workspace do Azure Machine
Learning. Você aprenderá como configurar um workspace dedicado para
organizar e gerenciar seus projetos de aprendizado de máquina de forma
eficaz. Esse workspace serve como um ponto central para colaboração,
experimentação.

#### Tarefa 2.1: Registrar os provedores de recursos necessários 

1.  Navegue até sua **subscription** atribuída na página inicial do
    portal do Azure.

2.  Selecione Resource Providers em **Settings** no painel esquerdo.

3.  Procure por +++Microsoft.StreamAnalytics+++ e selecione os três
    pontos no nome, e depois clique em **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  Repita as etapas para registrar +++Microsoft.Cdn+++ e+++
    Microsoft.PolicyInsights+++

#### Tarefa 2.2: Criar um workspace do Azure Machine Learning 

1.  Acesse o portal do Azure em+++ https://portal.azure.com+++ usando o
    **Username** e **Password** fornecidos na guia **Resources**.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

2.  Na página inicial do portal do Azure, selecione **+ Create a
    resource**.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

3.  Na página **Create a resource**, use a barra de pesquisa para
    encontrar **+++Azure Machine Learning+++** e selecione **Azure
    Machine Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image6.png)

4.  Em **Marketplace**, clique no **Create dropdown** e selecione
    **Azure Machine Learning.**

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

5.  Forneça as seguintes informações para configurar seu novo workspace
    e clique em **Review + create**.

    - **Subscription**: Selecione sua **assinatura do Azure atribuída**

    - **Resource group**: Selecione o **grupo de recursos atribuído** a
      você.

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: Selecione sua região mais próxima **(North Central US**
  está selecionada aqui)

&nbsp;

- **Container registry: Selecione Create new. Enter +++
  azuremlcr@lab.LabInstance.Id+++**

**Observação:** O número que é adicionado aos nomes dos recursos é o seu
ID da Labinstance para garantir a exclusividade. As capturas de tela
terão um número diferente, pois são únicas para cada usuário.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  Depois que a validação for aprovada, clique em **Create**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  Clique em **Go to resource** para visualizar o novo workspace.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **Na página Microsoft.MachineLEarningServices | Overview**,
    selecione **Launch studio** em **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a software update Description automatically
generated](./media/image12.png)

#### Tarefa 2.3: Criar um recurso de computação

Esta tarefa demonstra a criação de um **recurso de computação** no
Azure. Você explorará diferentes opções de computação, como **machine
learning** ou **clusters gerenciados**, e entenderá como configurar e
provisionar recursos para executar cargas de trabalho de **machine
learning** de forma eficiente.

1.  Quando o **Azure Machine Learning Studio** for aberto, clique em
    **Compute** em **Manage** no painel esquerdo.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

2.  Clique em **+ New** na tela **Compute instances.**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  Na tela Create compute instance, insira os detalhes abaixo.

    1.  Compute name – +++**cpu-cluster-fs@lab.labInstance.Id**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Selecione **Standard_E4ds_v4**

> Clique em **Review + Create.**

**Observação:** Anote esse compute name para uso posterior.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

4.  Clique em **Create** na próxima tela.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

**Observação:** A computação leva cerca de 10 minutos para chegar ao
estado de execução.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

**Importante:** Quando a computação estiver em funcionamento, você
poderá continuar com as próximas tarefas. Porém, se estiver fazendo uma
pausa na execução do laboratório, certifique-se de **interromper** a
instância de computação e iniciá-la novamente quando começar após o
intervalo.

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

**Resumo do exercício:**

Este exercício familiariza os participantes com as etapas essenciais
para configurar um ambiente de Azure Machine Learning. Ao longo das
tarefas, os participantes aprenderam a criar uma conta de armazenamento,
instalar o SDK de Machine Learning, fazer login usando o Azure CLI,
criar um workspace do Azure Machine Learning e configurar um recurso de
computação. Ao concluir este exercício, você adquiriu o conhecimento
fundamental e as habilidades práticas necessárias para estabelecer um
ambiente funcional no Azure Machine Learning, permitindo que você inicie
seus projetos de machine learning com confiança.

.

## Exercício 2 - Carregando, acessando e explorando seus dados no Azure Machine Learning

**Objetivo**

Neste exercício, você aprenderá a:

- Carregar seus dados para o armazenamento em nuvem

- Criar um ativo de dados do Azure Machine Learning

- Acessar seus dados em um notebook para desenvolvimento interativo

- Criar novas versões de ativos de dados

O início de um projeto de machine learning normalmente envolve a
exploratory data analysis (EDA), pré-processamento dos dados (limpeza,
engenharia de atributos) e a construção de protótipos de modelos de
machine learning para validar hipóteses. Essa fase de prototipagem do
projeto é altamente interativa e se adapta bem ao desenvolvimento em um
IDE ou em um notebook Jupyter, com um console interativo em Python. Este
laboratório descreve esses conceitos.

Estamos na etapa **Data: Explore & prepare** do **fluxo de trabalho de
um projeto de Machine Learning.**

![](./media/image19.png)

### Tarefa 1: Preparar os recursos do Azure

**Importante:** certifique-se de que a computação que criamos no último
exercício esteja funcionando. Se você estiver fazendo uma pausa na
execução do laboratório, certifique-se de **interromper** e iniciar
novamente quando começar após o intervalo.

#### Tarefa 1.1: Fazer upload do notebook 

1.  No Azure Machine Learning Studio, assim que o recurso de computação
    estiver ativo e em execução, selecione a opção **Notebooks** no
    painel à esquerda ![](./media/image20.png)

2.  Feche a caixa de diálogo **What’s new in Notebooks**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image21.png)

3.  O painel **Notebook Files** será aberto com a estrutura **Users -\>
    \< UserName \>**. Clique nos três pontos ao lado do nome de usuário
    e selecione **Create new folder**.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

4.  Digite o nome da pasta como+++ **Azuremlnotebooks**+++ e clique em
    **Create.**

![A screenshot of a computer Description automatically
generated](./media/image23.png)

5.  Depois que a pasta for criada, clique no **menu options** (os três
    pontos ao lado do nome da pasta) da pasta **Azuremlnotebooks** e
    clique em **Upload files**.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

6.  Selecione **Click to browse and select file(s).** Navegue até o
    arquivo **explore-data.ipynb** em **C:\Labfiles** e clique em
    **Open**.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image25.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image26.png)

7.  Marque a caixa de seleção **Open file after upload** e **I trust the
    contents of this file.** Em seguida, clique em **Upload.**

![A screenshot of a computer Description automatically
generated](./media/image27.png)

8.  Isso abrirá o Notebook carregado.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

9.  Clique em **Authenticate** se o Studio solicitar a autenticação, já
    que esta é a primeira vez que você faz login no Studio.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image29.png)

### Tarefa 2: Carregue, acesse e explore seus dados 

#### Tarefa 2.1: Download de dados

1.  No painel **Files** do **Notebooks**, clique nos três pontos ao lado
    do nome da pasta **Azuremlnotebooks** e clique em **Create new
    folder.**

![](./media/image30.png)

2.  Digite o nome da pasta como **+++data**+++ e clique em **Create**.

![](./media/image31.png)

3.  Após a criação da pasta com sucesso, clique no menu de opções da
    pasta **data** e selecione **Upload files**.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

4.  Selecione **Click to browse and select file(s)** e navegue até
    **C:\Labfiles** para selecionar o arquivo
    **default_of_credit_card_clients.csv** e clique em **Open**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image33.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image34.png)

5.  Uma mensagem informando **File uploaded successfully** será exibida
    em Notifications após a conclusão do carregamento.

![A close-up of a computer screen Description automatically generated
with low confidence](./media/image35.png)

#### Tarefa 2.2: Criar referência ao workspace

1.  Volte para o notebook (**explore-data**).

2.  Antes de começarmos com o código, é necessário uma forma de
    **referenciar seu workspace**. Você criará um ml_client para obter
    um identificador para o workspace. Em seguida, você usará o
    ml_client para gerenciar recursos e tarefas.

3.  Na primeira célula, em **Create handle to workspace**, substitua os
    espaços reservados de**\< SUBSCRIPTION_ID ,\>\< RESOURCE_GROUP\>**
    e**\< AML_WORKSPACE_NAME \>.**

4.  Substitua \< RESOURCE_GROUP\> pelo nome do grupo de recursos
    atribuído a você.

5.  Substitua \<AML_WORKSPACE_NAME\> por[+++
    **Azuremlws@lab.LabInstance.Id**](mailto:+++Azuremlws@lab.LabInstance.Id)**+++**

6.  Substitua \< SUBSCRIPTION_ID \> pelo **+++@lab.CloudSubscription.Id
    .**+++

7.  Clique no botão **Run cell** localizado no canto superior esquerdo
    da célula. Uma marca de seleção aparecerá na parte inferior da
    célula quando a execução for bem-sucedida.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

#### Tarefa 2.3: Carregar dados para o armazenamento em nuvem

1.  Um ativo de dados do Azure Machine Learning é semelhante aos
    favoritos de um navegador da web. Em vez de lembrar longos caminhos
    de armazenamento (URIs) que apontam para os dados usados com mais
    frequência, você pode criar um ativo de dados e acessá-lo com um
    nome amigável.

2.  A próxima célula do notebook cria o ativo de dados. O código de
    exemplo envia o arquivo de dados brutos para o recurso de
    armazenamento em nuvem designado

3.  Toda vez que você cria um ativo de dados, precisa de uma versão
    exclusiva para ele. Se a versão já existir, ocorrerá um erro. Neste
    código, usamos a hora atual para gerar uma versão única sempre que a
    célula for executada.

4.  Execute a próxima célula clicando no botão Execute no canto superior
    esquerdo da célula.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

5.  **“Data asset created. Name: credit-card, version:
    YYYY:MM:DD.xxxxxx”** é a saída exibida abaixo da célula.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image38.png)

6.  Clique em **Data** no painel à esquerda e, em seguida, clique no
    data asset **credit-card** que foi criado na execução do passo
    anterior. Explore os detalhes do ativo de dados e depois retorne ao
    painel **Notebooks**.

![](./media/image39.png)

#### Tarefa 2.4: Acesse seus dados em um notebook

1.  De volta ao notebook, execute a célula com o comando **%pip** para
    instalar a biblioteca Python **azureml-fsspec** em seu kernel do
    **Jupyter**.

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image40.png)

2.  Execute a próxima célula para acessar o arquivo CSV usando o
    **Pandas**.

3.  O **Data asset URI** será exibido ao final da célula, e os dados
    também serão exibidos.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image41.png)

#### Tarefa 2.5: Criar uma nova versão do ativo de dados

1.  Você pode ter notado que os dados precisam de uma leve limpeza para
    que estejam prontos para treinar um modelo de machine learning. Eles
    contêm:

    1.  dois cabeçalhos

    2.  uma coluna de ID do cliente; não usaremos esse recurso no
        machine learning

    3.  espaços no nome da variável de resposta

2.  Além disso, em comparação com o formato CSV, o formato Parquet é uma
    maneira melhor de armazenar esses dados. O Parquet oferece
    compressão e mantém o esquema dos dados. Portanto, para limpar os
    dados e armazená-los em formato Parquet, execute a próxima célula.

3.  Certifique-se de que a execução foi bem-sucedida, verificando marca
    de seleção na parte inferior da célula.

![](./media/image42.png)

4.  Essa tabela mostra a estrutura dos dados no arquivo original
    **default_of_credit_card_clients.csv,** o arquivo .CSV que foi
    baixado em uma etapa anterior. Os dados enviados contêm 23 variáveis
    explicativas e 1 variável de resposta, conforme mostrado abaixo:

[TABLE]

5.  Execute a próxima célula para criar uma nova *versão* do ativo de
    dados (os dados são carregados automaticamente no armazenamento em
    nuvem).

6.  Quando a execução for bem-sucedida, será exibida uma saída
    informando: **Data asset created. Nome: credit_card, versão:
    YYYY.MM.DD.xxxxxx_cleaned**.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image43.png)

![A screenshot of a computer Description automatically generated with
low confidence](./media/image44.png)

**Importante:**

Essa célula de código Python define os valores **nome** e **versão**
para o ativo de dados que ela cria. Como resultado, o código nessa
célula falhará se for executado mais de uma vez, a menos que esses
valores sejam alterados. Usar valores fixos de nome e versão é útil para
situações específicas, onde não se deseja depender de valores gerados
automaticamente ou aleatoriamente.

7.  O arquivo Parquet limpo passa a ser a fonte de dados da versão mais
    recente.  
    O código na próxima célula mostra primeiro o resultado da versão CSV
    e, em seguida, exibe o resultado da versão em Parquet ao ser
    executado.

8.  Execute a próxima célula e verifique o resultado abaixo.

![A screenshot of a computer code Description automatically generated
with low confidence](./media/image45.png)

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image46.png)

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image47.png)

![A picture containing text, screenshot, number, display Description
automatically generated](./media/image48.png)

9.  Procure os dados limpos em **Data**.

> ![](./media/image49.png)

**Importante:** você pode continuar com o próximo exercício a partir
daqui. No entanto, se estiver fazendo uma pausa na execução do
laboratório, certifique-se de **interromper** a instância de computação
e iniciá-la novamente quando voltar do intervalo.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image50.png)

**Resumo do exercício**

Neste exercício, você aprendeu a carregar seus dados no armazenamento em
nuvem, criar um ativo de dados do Azure Machine Learning, acessar seus
dados em um notebook para desenvolvimento interativo e criar novas
versões de ativos de dados.

## Exercício 3 - Treinar e implementar um modelo de classificação de imagem no Azure Machine Learning Studio

**Objetivo**

Neste exercício, você aprenderá a

1.  Conectar-se ao workspace e configurar um recurso de computação
    usando a interface de notebook do Azure Machine Learning Studio

2.  Importar os dados e prepará-los para serem usados no treinamento

3.  Treinar um modelo para classificação de imagens

4.  Visualizar e analisar as métricas para otimizar seu modelo

5.  Implementar o modelo online e testá-lo

Estamos na etapa **Train & validate model** do **fluxo de trabalho do
projeto do Machine Learning.**

### ![A picture containing text, font, number, screenshot Description automatically generated](./media/image51.png)Tarefa 1: Carregar notebook

1.  Na página **Notebooks** do Azure Machine Learning Studio, clique nas
    opções de menu da pasta **AzureMLnotebooks** e selecione **Upload
    files**.

> ![A screenshot of a computer Description automatically
> generated](./media/image52.png)

2.  Selecione, **Click to browse and select file(s)**, procure
    **C:\Labfiles** e selecione o arquivo
    **azureml-getting-started-studio** (um arquivo de origem Jupyter).

> ![A screenshot of a computer screen Description automatically
> generated with medium confidence](./media/image53.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

3.  Marque a caixa de seleção **Open file after upload** e clique em
    **Upload**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image55.png)

4.  Quando o carregamento do arquivo for concluído com sucesso, ele será
    aberto automaticamente no Studio, já conectado ao Compute
    (cpu-cluster-fs) que está em estado **De execução**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

### Tarefa 2: Conectar-se ao workspace do Azure Machine Learning

Antes de começarmos com o código, você precisará se conectar ao seu
workspace. O workspace é o recurso de nível superior no Azure Machine
Learning, fornecendo um local centralizado para trabalhar com todos os
artefatos que você cria ao usar o Azure Machine Learning.

Estamos utilizando o **DefaultAzureCredential** para obter acesso ao
workspace.  
O **DefaultAzureCredential** é capaz de lidar com a maioria dos cenários
de autenticação automaticamente.

*\# Handle to the workspace*

**from** azure.ai.ml **import** MLClient

*\# Authentication package*

**from** azure.identity **import** DefaultAzureCredential

credential **=** DefaultAzureCredential()

*\# Get a handle to the workspace. You can find the info on the
workspace tab on ml.azure.com*

ml_client **=** MLClient(

credential**=**credential,

subscription_id**=**"\<SUBSCRIPTION_ID\>", *\# this will look like
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*

resource_group_name**=**"\<RESOURCE_GROUP\>",

workspace_name**=**"\<AML_WORKSPACE_NAME\>",

)

1.  No código acima (primeira célula do notebook), substitua os campos
    **SUBSCRIPTION_ID**, **nome do RESOURCE_GROUP** e
    **AML_WORKSPACE_NAME** pelos valores que você salvou no exercício
    anterior.

2.  Sua primeira célula no notebook agora deve se parecer com isso.
    Clique no botão **Run**, localizado no canto superior esquerdo da
    primeira célula.

![A screenshot of a computer Description automatically
generated](./media/image57.png)

3.  Certifique-se de que a célula foi executada com sucesso verificando
    o status exibido na parte inferior da célula.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image58.png)

Em \[ \]:

### Tarefa 3: carregar dados

Para executar um trabalho de treinamento do Azure Machine Learning, você
precisará de um ambiente.

Neste laboratório, você usará um ambiente pronto chamado
AzureML-sklearn-1.0-ubuntu20.04-py38-cpu@latest que contém todas as
bibliotecas necessárias (python, MLflow, numpy, pip etc.).

1.  Execute o código na próxima célula para enviar carregar os dados.

2.  Certifique-se de que uma mensagem dizendo **Data asset created**
    seja exibida como saída da célula.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image59.png)

###  Tarefa 4: Criar a tarefa de comando para treinar

Agora que você tem todos os recursos necessários para executar sua
tarefa, é hora de criar a tarefa em si, usando o SDK do Azure ML Python
v2. Criaremos uma tarefa de comando.

Uma tarefa de comando do AzureML é um recurso que especifica todos os
detalhes necessários para executar seu código de treinamento na nuvem:
entradas e saídas, o tipo de hardware a ser usado, o software a ser
instalado e como executar seu código. A tarefa de comando contém
informações para executar um único comando.

#### Tarefa 4.1 : Criar script de treinamento

1.  Vamos começar criando o script de treinamento - o arquivo python
    **main.py**.

2.  Execute a próxima célula e certifique-se de que ela seja executada
    com sucesso.

![A picture containing text, font, line, screenshot Description
automatically generated](./media/image60.png)

3.  O script na próxima célula lida com o pré-processamento dos dados,
    dividindo-os em dados de teste e de treinamento. Em seguida, ele
    consome esses dados para treinar um modelo baseado em árvore e
    retornar o modelo de saída. [O
    MLFlow](https://mlflow.org/docs/latest/tracking.html) será usado
    para registrar os parâmetros e as métricas durante a execução do
    pipeline.

4.  Execute a célula e certifique-se de que ela seja executada com
    sucesso com a saída,

**Writing ./src/main.py**

> ![A screenshot of a computer program Description automatically
> generated with low confidence](./media/image61.png)
>
> ![A screenshot of a computer program Description automatically
> generated with medium confidence](./media/image62.png)

5.  Como você pode ver neste script, depois que o modelo é treinado, o
    arquivo do modelo é salvo e registrado no workspace. Agora você pode
    usar o modelo registrado nos endpoints de inferência.

#### Tarefa 4.2: Configurar o comando

Agora que você tem um script que pode executar as tarefas desejadas,
você usará o comando de propósito geral que pode executar ações de linha
de comando. Essa ação de linha de comando pode chamar diretamente os
comandos do sistema ou executar um script.

1.  Aqui, você usará dados de entrada, proporção de divisão, taxa de
    aprendizado e nome do modelo registrado como variáveis de entrada.

2.  No painel esquerdo, selecione **Data** e selecione
    **Credit-card-data**.

![A screenshot of a computer Description automatically
generated](./media/image63.png)

3.  Na seção **Data source**, procure o valor **Datastore URI** e
    copie-o. Salve-o para usá-lo na próxima etapa.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image64.png)

4.  Na próxima célula, substitua o

    1.  Valor do path com o **Datastore URI** salvo na etapa anterior.

    2.  Valor da **compute** com
        **+++cpu-cluster-fs@lab.LabInstance.Id**+++ (o nome do cluster
        que salvamos no Laboratório 1)

5.  Clique em **Run**. Certifique-se de que a célula seja executada com
    sucesso.

![A screenshot of a computer program Description automatically
generated](./media/image65.png)

### Tarefa 6: Enviar a tarefa

Agora é hora de enviar a tarefa para ser executada no AzureML. **A
tarefa levará de 2 a 3 minutos para ser executada**. Pode demorar mais
(até 10 minutos) se a instância de computação tiver sido reduzida para
zero nós e o ambiente personalizado ainda estiver sendo criado.

1.  Execute a célula com o comando abaixo para enviar a tarefa.

> ***\# enviar o comando job***
>
> **ml_client.create_or_update(job)**

2.  Clique em **Run**. Verifique se a execução foi bem-sucedida e se há
    um link para o resultado na coluna **Details Page**.

**Observação**: isso levará cerca de 2 minutos para ser concluído.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image66.png)

3.  Abra o link disponível na coluna **Details Page** do resultado, em
    uma nova guia.

### Tarefa 7: Visualizar o resultado de uma tarefa de treinamento

1.  Você pode visualizar o resultado de uma tarefa de treinamento
    **clicando no URL gerado após enviar uma tarefa**.

> ![A screenshot of a computer Description automatically
> generated](./media/image67.png)

2.  Como alternativa, você também pode clicar em **Jobs** no menu de
    navegação à esquerda. Uma tarefa é um agrupamento de várias
    execuções de um script ou trecho de código especificado. As
    informações da execução são armazenadas nessa tarefa.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image68.png)

3.  A página **Overview** mostra primeiro o painel **Status** em
    **Properties** para ser **Executado**.

4.  O status muda para **Completed** quando estiver pronto.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image69.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image70.png)

5.  Selecione o painel **Metrics** para visualizar as métricas.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image71.png)

6.  Selecione a guia **Images** para visualizar a matriz de confusão de
    treinamento, a curva de precisão e recall e a curva ROC.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image72.png)

1)  **Overview** é onde você pode ver o status do trabalho.

2)  **Metrics** pode exibir diferentes visualizações das métricas que
    você especificou no script.

3)  **Images** é onde você pode visualizar todos os artefatos de imagem
    registrados com o MLflow.

4)  **Child jobs** contém tarefas secundárias, se você as adicionou.

5)  **Outputs + logs** contém arquivos de registro necessários para a
    solução de problemas ou para outros fins de monitoramento.

6)  **Code** contém o script/código usado no trabalho.

7)  **Explanations** e **Fairness** são usados para ver o desempenho do
    seu modelo em relação aos padrões de resposible AI. Atualmente,
    esses recursos estão em pré-visualização e exigem a instalação de
    pacotes adicionais.

8)  **Monitoring** é onde você pode visualizar métricas para o
    desempenho dos recursos de computação.

### Tarefa 8: Implementar o modelo como um endpint online

Após treinar um modelo de machine learning, é necessário implantá-lo
para que outras pessoas possam usá-lo para inferência. Para esse
propósito, o Azure Machine Learning permite criar **endpoints** e
adicionar **deployments** a eles.  
Um **endpoint**, nesse contexto, é um caminho HTTPS que fornece uma
interface para que os clientes enviem solicitações (dados de entrada)
para um modelo treinado e recebam os resultados de inferência
(pontuação) do modelo. Um endpoint oferece:

- Autenticação usando autenticação baseada em "chave ou token"

- Encerramento TLS(SSL)

- Um URI de pontuação estável
  (endpoint-name.region.inference.ml.azure.com)

Um **deployment** é um conjunto de recursos necessários para hospedar o
modelo e realizar a inferência.

#### Tarefa 8.1: Criar um endpoint online

1.  Agora implemente seu modelo de machine learning como um serviço web
    na nuvem Azure, um endpoint online.

2.  Selecione **Endpoints** no painel esquerdo.

![A screenshot of a computer Description automatically
generated](./media/image73.png)

3.  Selecione **Create** para endpoints em tempo real

![A screenshot of a computer Description automatically
generated](./media/image74.png)

4.  Selecione **credit_defaults_model** e, em seguida, clique em
    **Select.**

![A screenshot of a computer Description automatically
generated](./media/image75.png)

5.  Selecione **Standard_E4s_v3** em Virtual machine. Defina a contagem
    de instâncias como **1**

> Aceite os outros padrões, como o **Endpoint name** e o **Deployment
> name**, e então selecione **Deploy**.

![A screenshot of a computer Description automatically
generated](./media/image76.png)

**Observação:** a criação do endpoint leva cerca de 20 minutos para ser
concluída.

6.  Depois de concluído, o estado do provisionamento muda para
    **Succeeded**.

![A screenshot of a computer Description automatically
generated](./media/image77.png)

#### Tarefa 8.2: Teste com uma consulta de amostra

1.  Na página do endpoint, selecione a guia **Test.**

2.  Copie e cole o seguinte arquivo de solicitação de exemplo no campo
    **Input data to test real-time endpoint,** substituindo o código que
    já estiver presente lá.

> **{**
>
> **"input_data": {**
>
> **"columns":
> \[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22\],**
>
> **"index": \[0, 1\],**
>
> **"data": \[**
>
> **\[20000,2,2,1,24,2,2,-1,-1,-2,-2,3913,3102,689,0,0,0,0,689,0,0,0,0\],**
>
> **\[10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 10,
> 9, 8\]**
>
> **\]**
>
> **}**
>
> **}**

3.  Selecione **Test** e visualize o resultado em **Test result**.

> ![A screenshot of a computer Description automatically
> generated](./media/image78.png)

### Tarefa 9: Excluir o endpoint

1.  No painel esquerdo, selecione **Endpoints**. Selecione o endpoint
    que criamos e clique em **Delete**.

![A screenshot of a computer Description automatically
generated](./media/image79.png)

2.  Clique **Delete** na caixa de diálogo de confirmação.

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image80.png)

3.  Procure uma notificação sobre a exclusão bem-sucedida.

![A picture containing text, screenshot, font, line Description
automatically generated](./media/image81.png)

**Resumo**

Neste laboratório, você aprendeu a treinar um modelo de classificação de
imagens no Azure Machine Learning Studio e a implementá-lo como um
serviço da Web.
