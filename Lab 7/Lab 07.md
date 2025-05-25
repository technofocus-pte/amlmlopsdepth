# Laboratório 07: Desenvolver e testar o fluxo de prompt do Azure Machine Learning Studio

**Objetivo:**

Neste laboratório, aprenderemos a principal jornada do usuário ao usar o
fluxo de prompts no Azure Machine Learning Studio. Você aprenderá como
habilitar o fluxo de prompts em sua área de trabalho do Azure Machine
Learning, criar e desenvolver um fluxo de prompts, testar e avaliar o
fluxo e, em seguida, implementá-lo em produção.

Duração prevista – 60 minutos

## Tarefa 1: Preparando os recursos do Azure

### Tarefa 1.1: Criar um workspace do Azure Machine Learning

Esta tarefa se concentra na criação de um workspace do Azure Machine
Learning. Você aprenderá como configurar um workspace dedicado para
organizar e gerenciar seus projetos de machine learning de forma eficaz.
Esse workspace funciona como um centro central de colaboração,
experimentação e implementação.

1.  Acesse o portal do Azure em +++<https://portal.azure.com>+++ e faça
    login com suas credenciais de administrador de locatário.

2.  Na página inicial do portal do Azure, selecione **+ Create a
    resource**.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Na página **Create a resource**, use a barra de pesquisa para
    encontrar +++Azure Machine Learning+++ e selecione **Azure Machine
    Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  Em **Marketplace**, clique no **Create dropdown** e selecione
    **Azure Machine Learning**.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  Forneça as seguintes informações para configurar seu novo workspace:

    - **Subscription**: Selecione sua **assinatura do Azure atribuída**

    - **Resource group**: Selecione o **grupo de recursos atribuído**

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **Detalhes do Workspace:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: Selecione a região mais próxima **(North Central US** está
  selecionado aqui)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstanceId+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  Quando terminar de configurar o workspace, selecione **Review +
    Create**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  Depois que a validação for aprovada, clique em **Create**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  Clique em **Go to resource**, para visualizar o novo workspace

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  Na **página Microsoft.MachineLEarningServices | Overview**,
    selecione **Launch studio** em **Work with your model in Azure
    Machine Learning studio**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### Tarefa 1.2: Criar uma computação

Esta tarefa demonstra a criação de um recurso de computação no Azure.
Você explorará diferentes opções de computação, como virtual machines ou
clusters de computação gerenciados, e entenderá como configurar e
provisionar recursos para executar cargas de trabalho de machine
learning com eficiência.

1.  Quando o **Azure Machine Learning Studio** estiver aberto, clique em
    **Compute** em **Manage** no painel esquerdo.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  Clique em **+ New** na tela **Compute instances**.

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  Na tela Create compute instance, insira os detalhes abaixo.

    1.  Compute name – +++**pfcompute**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – Selecione **Standard_E4ds_v4**

> Clique em **Review + Create**.

**Observação:** Anote esse nome de computação para uso posterior.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  Clique em **Create** na próxima tela para criar a computação.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**Observação:** A computação leva cerca de 10 minutos para chegar ao
estado de execução.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**Importante:** Assim que computação estiver ativa e em execução, você
poderá continuar com as próximas tarefas. No entanto, se for fazer uma
pausa na execução do laboratório, certifique-se de **parar** a instância
de computação e iniciá-la novamente quando retomar após a pausa.

### Tarefa 1.3: Criar o recurso Azure OpenAI

1.  No portal do Azure +++https://portal.azure.com+++, pesquise por e
    selecione +++**AzureOpenAI**+++.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  Clique em **+ Create**.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  Preencha os detalhes abaixo e clique em **Next**.

- Resource group - Selecione seu grupo de recursos atribuído

- Region – Selecione uma região (North Central US está sendo usado aqui)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  Aceite os valores padrão nas próximas páginas e clique em **Create**
    na página **Review + submit**.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  Clique em **Go to resource** assim que a implementação estiver
    concluída.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  Selecione **Keys and Endpoint** no painel à esquerda.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  Copie a **Key** e o **Endpoint** e salve-os em um bloco de notas
    para uso em uma parte posterior do laboratório.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  No **Azure Machine Learning Studio**, selecione **Model catalog** no
    painel esquerdo e selecione **gpt-4º.**

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  Clique em **Deploy** para implementar o modelo.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. Aceite o nome da implementação e selecione **Deploy**. Anote esse
    nome para uso futuro.

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## Tarefa 2: Configurar uma conexão de fluxo de prompt

1.  No painel de navegação à esquerda do Azure Machine Learning Studio,
    selecione **Prompt flow**. Em seguida, selecione **Connections** na
    barra de menu. Clique na seta ao lado de **Create** e selecione
    **Azure OpenAI**.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  No assistente Add Azure OpenAI connection, forneça os detalhes
    abaixo e selecione **Save**.

- Name – +++**AoaiML_pf**+++

- Provider – Selecione **Azure OpenAI**

- Subscription ID – Selecione sua **assinatura atribuída**

- Azure OpenAI Account Name – Selecione **AOAI-PF@lab.LabInstanceId**

- Auth Mode – Selecione **API Key**

- API Key – Forneça a **key** que salvamos anteriormente do **recurso do
  Azure OpenAI**

- API base – Provide the **endpoint** que salvamos do **recurso**
  **Azure OpenAI**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  Verifique se a criação da conexão foi bem-sucedida.

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## Tarefa 3: Criar e desenvolver seu fluxo de prompt

1.  Na guia **Flows** da página inicial do **Prompt flow**, selecione
    **Create** para criar um novo fluxo. A página **Create a new flow**
    exibe os tipos de fluxo que você pode criar, exemplos integrados que
    podem ser clonados para criar um fluxo, e opções para importar um
    fluxo.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  Selecione **Clone** na categoria **WebClassification**.

Na **Explore gallery**, você pode navegar pelos exemplos integrados e
selecionar **View detail** em qualquer bloco para verificar se ele é
adequado ao seu cenário.

Este laboratório usa a amostra **Web Classification** para guiar você
pela principal jornada do usuário.

A classificação da Web é um fluxo que demonstra a classificação
multiclasse com um LLM. Dada uma URL, o fluxo classifica a URL em uma
categoria da Web com apenas algumas imagens, resumos simples e prompts
de classificação. Por exemplo, dada uma URL https://www.imdb.com, ela
classifica a URL como Filme.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  Aceite o nome preenchido como **Folder name** e, em seguida,
    selecione **Clone.**

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  Uma sessão de computação é necessária para a execução do fluxo. A
    sessão de computação gerencia os recursos de computação necessários
    para a execução do aplicativo, incluindo uma imagem do Docker que
    contém todos os pacotes de dependência necessários.

5.  Na página de criação de fluxo, inicie uma sessão de computação
    selecionando **Start compute session**.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**Observação:** Levará cerca de **10 minutos** para que a sessão de
computação entre em estado de execução.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## Tarefa 4: Inspecionar a página de criação de fluxo

A sessão de computação pode levar alguns minutos para iniciar. Enquanto
a sessão de computação está sendo iniciada, explore as partes da página
de criação do fluxo.

- A visualização de **Flow** ou *nivelamento*, localizada no lado
  esquerdo da página, é a área principal de trabalho, onde você pode
  criar o fluxo adicionando ou removendo nós, editando e executando nós
  diretamente, ou editando *prompts*. Nas seções **Inputs** e
  **Outputs**, é possível visualizar, adicionar, remover e editar
  entradas e saídas.

Quando você clonou o exemplo atual de Web Classification, os inputs e
outputs já estavam definidos. O esquema de entrada para o fluxo é: name:
url; type: string, ou seja, uma URL do tipo string. Você pode alterar o
valor de entrada pré-definido manualmente para outro, como
https://www.imdb.com.

- A seção **Files**, no canto superior direito, exibe a estrutura de
  pastas e arquivos do fluxo. Cada pasta de fluxo contém um arquivo
  *flow.dag.yaml*, arquivos de código-fonte e pastas de sistema. Você
  pode criar, carregar ou baixar arquivos para testes, implementação ou
  colaboração.

- A exibição de **Graph**, no canto inferior direito, é usada para
  visualizar a estrutura do fluxo. Você pode aplicar zoom, ou usar a
  opção de **auto layout**.

- Você pode editar arquivos diretamente na **Flow** view ou na flatten
  view, ou pode ativar o modo **Raw file mode** e selecionar um arquivo
  na seção **Files** para abri-lo em uma aba para edição.

Você pode editar arquivos diretamente na visualização de **Flow** view
ou nivelamento, ou pode ativar o modo **Raw file mode** e selecionar um
arquivo na seção **Files** para abri-lo em uma guia para edição.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## Tarefa 5: Configurar os nós do LLM

Para cada nó LLM, você precisa selecionar uma **Connection** para
definir as chaves de API do LLM. Selecione sua conexão do Azure OpenAI.

Dependendo do tipo de conexão, você deverá selecionar um
**deployment_name** ou um modelo no menu suspenso. Para uma conexão com
o Azure OpenAI, selecione uma implementação. 

1.  Para summarize_text_content, preencha os detalhes abaixo.

Connection – Selecione **AoaiML_pf**

Api – Selecione **chat**

deployment name – Selecione **gpt-4o-2024-11-20**

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  Configure a conexão da mesma forma para os nós LLM chamados
    **classify_with_llm**![A screenshot of a computer Description
    automatically generated](./media/image38.png)

3.  Para testar e depurar um único nó, selecione o ícone **Run** no topo
    de um nó na visualização do **Flow**. Você pode expandir **Inputs**
    e alterar a URL de entrada do fluxo para testar o comportamento do
    nó com diferentes URLs.

4.  O status da execução é exibido na parte superior do nó. Após a
    conclusão da execução, a saída da execução é exibida na seção
    **Output** do nó.

5.  Mova para o início do fluxo e execute o
    **fetch_text_content_from_url** e, em seguida, execute o bloco.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

A visualização **Graph** também exibe o status do nó de execução único.

6.  Na seção **Inputs**, forneça o valor para o campo **Value** como
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++

Selecione **Run** no canto superior direito para testar e depurar todo o
fluxo.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## Tarefa 5: Exibir saídas de fluxo

Também é possível definir saídas de fluxo para verificar as saídas de
vários nós em um único local. As saídas de fluxo ajudam você a:

- Verificar resultados de testes em massa em uma única tabela.

- Definir o mapeamento da interface de avaliação.

- Definir o esquema de resposta da implementação.

1.  Selecione **View outputs** na barra superior ou no menu superior
    para visualizar informações detalhadas sobre entrada, saída,
    execução de fluxo e orquestração.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  Na guia Outputs da tela de Outputs, observe que o fluxo prevê a URL
    de entrada com uma **category** e **evidence**. ![A screenshot of a
    computer Description automatically generated](./media/image42.png)

3.  Selecione a guia **Trace** na tela de **Outputs** e, em seguida,
    selecione **flow** sob o **node name** para ver informações
    detalhadas sobre o fluxo no painel à direita. Expanda **flow** e
    selecione qualquer etapa para ver informações detalhadas dessa
    etapa.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**Resumo:**

> Neste laboratório, aprendemos a classificar a URL em uma categoria de
> site com uma simples resumo e prompt de classificação usando o Prompt
> Flow no Azure Machine Learning Studio.
