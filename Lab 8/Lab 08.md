# Lab 08 – Implementando a geração de dados de QA com RAG usando um fluxo de prompt

**Objetivo:**

A Geração de Dados de QA faz parte do processo de criação RAG (Retrieval
Augmented Generation), onde o conjunto de dados de QA gerado
automaticamente é utilizado na obtenção do melhor prompt para RAG, e
também para obter métricas de avaliação para RAG.

Neste laboratório, você aprenderá a criar um conjunto de dados de QA a
partir de seus dados.

Duração esperada – 60 minutos

## Exercício 1: Criar implementações AOAI 

Neste exercício, criaremos as implantações de modelos gpt-35-turbo
usando o recurso OpenAI do Azure que criamos no laboratório anterior.

1.  No Azure Machine Learning Studio, selecione **Model Catalog** no
    painel esquerdo. Procure por +++**gpt-35-turbo+++** e selecione
    **gpt-35-turbo** na lista de modelos.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image1.png)

2.  Certifique-se de que o recurso **AOAI-PF@lab.LabInstanceId** está
    selecionado no campo **Azure OpenAI resource**. Selecione **Deploy**
    para implementar o modelo.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image2.png)

3.  Aceite o **Deployment name** e selecione **Deploy**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image3.png)

4.  Repita a implementação de modelo para **text-embedding-ada-002** com
    o nome da implementação como +++**text-embedding-ada-002-2**+++

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image4.png)

## Exercício 2: Configurar o ambiente

1.  No painel esquerdo do Studio, selecione **Notebooks**. Clique nos
    três pontos ao lado do nome do usuário e selecione **Upload Files**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image5.png)

2.  Navegue até **C:\LabFiles** e selecione o arquivo
    **qa_data_generation.ipynb**. Marque **I trust contents of this
    file** e clique em **Upload**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image6.png)

3.  Abra o notebook e selecione **Serverless Spark Compute** na opção
    **Compute.**

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image7.png)

4.  Depois que a computação estiver anexada, selecione **Configure
    session** para carregar o arquivo conda.yml e configurar o ambiente
    para execução usando-o.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image8.png)

5.  Selecione **Python packages** -\> **Upload Conda file** -\> clique
    em **Browse**.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image9.png)

6.  Selecione o **conda.yml** em **C:\LabFiles** e selecione
    **Aplicar**.

> ![Uma captura de tela de um programa de computador Descrição gerada
> automaticamente](./media/image10.png)

## Exercício 3: Obter cliente para o AzureML Workspace

1.  Execute a primeira célula do notebook para instalar as dependências

![](./media/image11.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image12.png)

**Observação:** isso levará de 10 a 15 minutos para ser concluído

2.  Execute a próxima célula com az login para fazer **login** no
    **Azure** CLI.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image13.png)

3.  O Workspace é o recurso de nível superior do Azure Machine Learning,
    fornecendo um local centralizado para trabalhar com todos os
    artefatos que você criar ao usar o Azure Machine Learning. Nesta
    seção, nos conectaremos ao workspace no qual o trabalho será
    executado. O MLClient é a ferramente que você usa para interagir com
    o AzureML.

4.  Substitua os espaços reservados para **Subscription ID** por
    +++@lab. Subscription()+++, **Resource group** com o nome do seu
    **Resource group name** e **Azure ML Workspace** com
    +++**Azuremlws@lab.LabInstanceId+++** na próxima célula para criar o
    MClient.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image14.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image15.png)

5.  **Execute** a próxima célula, que define o **connection name**. Se
    você tiver usado qualquer outro nome ao criar a conexão, forneça
    esse valor nesta célula e, em seguida, execute.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image16.png)

6.  Substitua o valor da **key** pela **Azure OpenAI key** e o valor do
    **target** pelo valor **Endpoint** do recurso Azure OpenAI que
    salvamos anteriormente.

**Execute** a célula após substituir os valores.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image17.png)

7.  Agora que seu workspace está conectado com o Azure OpenAI, vamos
    garantir que o modelo gpt-35-turbo tenha sido implementado e esteja
    pronto para inferência.

8.  **Execute** a próxima célula para definir os nomes do modelo e do
    **deployment**. Substitua os valores do nome do modelo e da
    implementação caso tenha usado nomes diferentes ao criar o modelo e
    a implantação.

![Uma captura de tela de um código de computador Descrição gerada
automaticamente](./media/image18.png)

9.  Por fim, combinaremos as informações de implementação e modelo em um
    formulário uri, que é o esperado pelos componentes de embeddings do
    AzureML. **Execute** a próxima célula para realizar essa operação.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image19.png)

## Exercício 4: Configurar Pipeline

Os pipelines do AzureML conectam vários componentes entre si. Cada
componente define entradas, o código que consome essas entradas e as
saídas geradas por esse código. Os próprios pipelines podem ter entradas
e saídas produzidas ao conectar os diferentes subcomponentes
individuais. Para processar seus dados para embedding e indexação,
encadearemos vários componentes, cada um executando sua própria etapa no
fluxo de trabalho.

Os Componentes são publicados em um Registro, chamado azureml, que deve
ter acesso por padrão, ele pode ser acessado de qualquer Workspace. Na
célula abaixo, obtemos as Definições de Componente a partir do registro
azureml.

1.  Execute a próxima célula e certifique-se de que ela seja executada
    sem nenhum erro.

![Uma captura de tela de um código de computador Descrição gerada
automaticamente](./media/image20.png)

2.  Cada componente possui uma documentação que fornece uma descrição
    geral de sua finalidade, além de detalhes sobre cada uma das
    entradas/saídas. Por exemplo, podemos entender o que o
    **data_generation_component** faz inspecionando sua definição.
    **Execute** a próxima célula para isso e observe o resultado
    exibido.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image21.png)

3.  Abaixo, um Pipeline é construído definindo uma função python que
    encadeia as entradas e saídas dos componentes mencionados
    anteriormente. Os argumentos da função são as entradas do próprio
    Pipeline, e o valor de retorno é um dicionário que define as saídas
    do Pipeline. Certifique-se de que a **próxima célula** seja
    **executada** com sucesso.

![Uma captura de tela de um código de computador Descrição gerada
automaticamente](./media/image22.png)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image23.png)

4.  As configurações abaixo mostram como os diferentes parâmetros git e
    data_source podem ser definidos para processar apenas a documentação
    do AzureML dentro do repositório maior AzureDocs, e garantir que a
    URL de origem de cada documento seja convertida para apontar para a
    URL pública em vez da URL do Git.

5.  Execute as próximas duas células e certifique-se de que elas estão
    sendo executadas com sucesso.

![](./media/image24.png)

![Uma captura de tela de um programa de computador Descrição gerada
automaticamente](./media/image25.png)

## Exercício 5: Enviar pipeline

1.  A saída de cada etapa do pipeline pode ser inspecionada por meio da
    interface do usuário do Workspace, clique no link em "Details Page"
    depois de executar a célula abaixo.

2.  Execute a próxima célula e clique no link exibido na saída para
    visualizar o status do fluxo.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image26.png)

3.  A execução será aberta no fluxo de prompt. Explore cada etapa do
    fluxo.

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image27.png)

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image28.png)

6.  Assim que o fluxo for concluído com sucesso, avance para a próxima
    etapa.

## Exercício 6: Revisar os dados de QA gerados

1.  Execute as próximas 2 células e revise a saída dos dados de QA.

![Uma captura de tela de um código de computador Descrição gerada
automaticamente](./media/image29.png)

> ![Uma captura de tela de um código de computador Descrição gerada
> automaticamente](./media/image30.png)

Resumo:

Neste laboratório, aprendemos a criar um conjunto de dados de QA a
partir de seus próprios dados.
