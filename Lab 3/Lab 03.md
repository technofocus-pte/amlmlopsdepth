# Laboratório 03 - Desenvolva e registre um conjunto de recursos com o armazenamento de recursos gerenciado e treine modelos usando recursos

Este laboratório descreve como criar uma especificação de conjunto de
recursos com transformações personalizadas. Em seguida, ele usa esse
conjunto de recursos para gerar dados de treinamento, habilitar a
materialização e realizar um preenchimento. A materialização calcula os
valores dos recursos para uma janela de recursos e, em seguida, armazena
esses valores em um armazenamento de materialização. Todas as consultas
de recursos podem então usar esses valores do armazenamento de
materialização.

Sem materialização, uma consulta de conjunto de recursos aplica as
transformações à fonte dinamicamente, para calcular os recursos antes de
retornar os valores. Esse processo funciona bem para a fase de
prototipagem. No entanto, para operações de treinamento e inferência em
um ambiente de produção, recomendamos que você materialize os recursos,
para obter maior confiabilidade e disponibilidade.

Duração prevista – 50 minutos

## Exercício 1: Atribuir funções necessárias:

**Exercício 1: Atribuir funções necessárias:**

1.  Na página inicial do portal do Azure, selecione o **Resource Group**
    atribuído na guia **Resources**. No painel esquerdo, selecione
    **Access control (IAM)**. Clique no menu suspenso ao lado de **Add**
    e selecione **Add role assignment.**

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  Procure por +++**AzureML Data Scientist**+++ e selecione-o. Clique
    em **Next**.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  Na guia Members, clique em **+ Select members**, procure seu **User
    name,** +++@lab.CloudPortalCredential(User1).Username+++.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  Selecione seu **username** e clique no botão **Select**.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  Clique em **Review + assign** nas duas telas seguintes.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  A mensagem de atribuição de função adicionada é obtida assim que a
    atribuição é concluída.

7.  Repita o mesmo conjunto de etapas para adicionar as funções
    +++**Storage Blob Data Reader**+++ e +++**Storage Blob Data
    Contributor**+++.

## Exercício 2: Desenvolva um conjunto de recursos e registre-se no armazenamento de recursos gerenciado

Este tutorial é a primeira parte da série de tutoriais sobre o
armazenamento de recursos gerenciado. Aqui, você aprenderá como:

- Criar um novo recurso de armazenamento de recursos mínimo.

- Desenvolver e testar localmente um conjunto de recursos com capacidade
  de transformação de recursos.

- Registrar uma entidade de armazenamento de recursos no armazenamento
  de recursos.

- Registrar o conjunto de recursos que você desenvolveu no armazenamento
  de recursos.

- Gerar um DataFrame de treinamento de amostra usando os recursos que
  você criou.

- Habilitar a materialização offline nos conjuntos de recursos e
  preencha os dados dos recursos.

### Tarefa 1: Preparar o ambiente

1.  No painel esquerdo do Azure Machine Learning Studio, selecione
    **Notebooks** em **Authoring**. Clique nos três pontos ao lado do
    nome de usuário e selecione **Upload folder**.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2.  Navegue e selecione a pasta **featurestore** em **C:\Labfiles** e
    clique em **Upload**.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  Navegue até **featurestore-\> notebooks-\>sdk_and_cli** e abra o
    notebook 1.Develop-feature-set-and-register.ipynb

![](./media/image8.png)

4.  Selecione **Serverless Spark Compute** em **Compute**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5.  Selecione **Configure session** para configurar a sessão com os
    pré-requisitos.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6.  Selecione **Python packages -\> Upload Conda file**. Clique em
    **Browse** e selecione **conda.yml** em **C:\Labfiles** e, em
    seguida, selecione **Apply**.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

7.  **Execute** a primeira célula do notebook. Isso instalará todas as
    **dependências** e concluirá sua execução. Isso levará cerca de **10
    minutos** para ser concluído.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Assim que a sessão Spark começar, substitua o **User name** pelo seu
    nome de usuário e execute a próxima célula.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9.  Execute as próximas 3 células para configurar o Azure CLI.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10. Na célula seguinte, siga as etapas exibidas no **output** para fazer
    login no **Azure**.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### Tarefa 2: Criar um armazenamento de recursos mínimo

1.  **Execute** a **primeira** célula para definir o nome, a localização
    e outros valores para o armazenamento de recursos.

![A screenshot of a computer program Description automatically
generated](./media/image19.png)

2.  **Execute** a primeira célula que **creates the feature store**.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

3.  A próxima célula é **initializes AzureML feature store core SDK
    client**. **Execute**-a.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

### Tarefa 3: Prototipar e desenvolver um conjunto de recursos de agregação contínua de transações neste notebook

1.  **Execute** a primeira célula nesta seção para explorar os dados de
    origem das **transações.**

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  Execute a segunda célula para **Desenvolver um conjunto de recursos
    de transações** localmente.

![A screenshot of a computer code Description automatically
generated](./media/image23.png)

3.  Execute a próxima célula **generate a spark dataframe** from the
    feature set specification.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  Para registrar a especificação do conjunto de recursos no
    repositório de recursos, ela precisa ser salva em um formato
    específico. Verifique as transações geradas FeaturesetSpec: abra
    este arquivo na árvore de arquivos para ver a especificação:
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

> Execute a próxima célula para exportar como especificação do conjunto
> de recursos.

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

### Tarefa 4: Registrar uma entidade do repositório de recursos

1.  Entidade ajuda a aplicar práticas recomendadas para que as mesmas
    definições de chave de junção sejam usadas em diferentes conjuntos
    de características que utilizam as mesmas entidades lógicas. Execute
    a célula para registrar uma entidade do repositório de
    características.

> ![A screen shot of a computer Description automatically
> generated](./media/image26.png)

### Tarefa 5: Registrar o conjunto de recursos da transação no repositório de recursos

1.  No portal do Azure (https://portal.azure.com), navegue até a
    **Storage account** que começa com **featureset** no grupo de
    recursos atribuído a você.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  No painel esquerdo, selecione Access Control (IAM). Selecione
    **Add** -\> **Add role assignment**

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  Procure e selecione +++**Storage Blob Data Reader**+++.

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

4.  Complete a atribuição de funções semelhante à que fizemos no
    Exercício 1.

5.  Da mesma forma adicione a função +++**Storage Blob Data
    Contributor**+++.

6.  Volte para o Azure Machine Learning Studio.

7.  Você registra um ativo de conjunto de recursos no armazenamento de
    recursos para poder compartilhá-lo e reutilizá-lo com outras
    pessoas. Você também obtém recursos gerenciados, como controle de
    versão e materialização. O ativo de conjunto de recursos tem
    referência à especificação do conjunto de recursos que você criou
    anteriormente e propriedades adicionais, como configurações de
    versão e materialização.

8.  **Execute** a próxima célula para **registrar o conjunto de recursos
    de transação** no repositório de recursos.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### Tarefa 6: Explore as funcionalidades de armazenamento da interface do usuário 

1.  Abra uma nova guia no navegador e acesse a página inicial global do
    Azure ML em +++https://ml.azure.com/home+++.

2.  Clique em **Feature stores** na navegação à esquerda.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

3.  Clique em **featurestore**.

**Observação:** a criação e a atualização de ativos do repositório de
recursos (conjuntos de recursos e entidades) só são possíveis por meio
do SDK e da CLI. Você pode usar a interface do usuário para
pesquisar/navegar no repositório de recursos.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

### Tarefa 7: Gerar uma estrutura de dados de treinamento usando os recursos registrados

1.  Começamos explorando os dados de observação. Os dados de observação
    são normalmente os dados principais usados nos dados de treinamento
    e inferência. Em seguida, eles são combinados com os dados de
    recursos para criar os dados de treinamento completos. Os dados de
    observação são os dados capturados durante o evento: neste caso,
    eles contêm os dados principais da transação, incluindo o ID da
    transação, o ID da conta e o valor da transação. Neste caso, como se
    trata de treinamento, eles também têm a variável-alvo anexada
    (is_fraud).

2.  **Execute** a célula e observe os dados de saída

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  **Execute** a próxima célula para get the **registered feature set**
    and list its features.

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

4.  **Execute** a próxima célula para **imprimir** os **valores de
    amostra**.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

5.  **Execute** a próxima célula. Nesta etapa, vamos **selecionar
    recursos** que gostaríamos que fizessem parte dos **dados de
    treinamento** e usar o SDK do armazenamento de recursos para gerar
    os dados de treinamento.

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

6.  Execute a próxima célula para gerar uma estrutura de dados de
    treinamento usando dados de recursos e dados de observação.

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

Tarefa 8: Ativar a materialização offline no conjunto de recursos de
transações

Depois que a materialização estiver habilitada em um conjunto de
recursos, você poderá executar preenchimento ou agendar tarefas de
materialização recorrentes.

1.  Execute a próxima célula para definir spark.sql.shuffle.partitions
    no arquivo yaml de acordo com o tamanho dos dados do recurso

2.  A configuração spark spark.sql.shuffle.partitions é um parâmetro
    OPCIONAL que pode afetar o número de arquivos parquet gerados (por
    dia) quando o conjunto de recursos é materializado no armazenamento
    offline. O valor padrão desse parâmetro é 200. A prática recomendada
    é evitar a geração de muitos arquivos parquet pequenos. Se a
    recuperação de recursos offline ficar lenta após a materialização do
    conjunto de recursos, acesse a pasta correspondente no armazenamento
    offline para verificar se o problema é o excesso de arquivos parquet
    pequenos (por dia) e ajuste o valor desse parâmetro de acordo.

**Observação:** os dados de amostra usados neste notebook são pequenos.
Portanto, esse parâmetro está definido como 1 no arquivo
featureset_asset_offline_enabled.yaml.![A screenshot of a computer
Description automatically generated](./media/image38.png)

3.  A materialização é o processo de calcular os valores dos recursos
    para uma janela de recursos determinada e armazená-los em um
    armazenamento de materialização. A materialização dos recursos
    aumentará sua confiabilidade e disponibilidade. Todas as consultas
    de recursos usarão os valores materializados do armazenamento de
    materialização. Nesta etapa, você executa um preenchimento único
    para uma janela de recursos de 18 meses.

4.  A célula de código a seguir **materializará os dados** pelo status
    atual Nenhum ou Incompleto para a janela de recursos definida.
    **Execute**-a.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

5.  Vamos **imprimir dados de amostra** do conjunto de recursos na
    próxima célula. **Execute**. Você pode observar nas informações de
    saída que os dados foram recuperados do armazenamento de
    materialização. O método get_offline_features(), usado para
    recuperar dados de treinamento/inferência, também usará o
    armazenamento de materialização por padrão.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## Exercício 3: Testar e treinar modelos usando recursos

Neste notebook, você aprenderá como:

- Criar um protótipo de uma especificação de conjunto de recursos de
  novas contas, usando valores pré-calculados existentes como recursos.
  Em seguida, registrar a especificação do conjunto de recursos local
  como um conjunto de recursos no armazenamento de recursos. Esse
  processo difere do primeiro tutorial, no qual você criou um conjunto
  de recursos com transformações personalizadas.

- Selecionar recursos para o modelo nos conjuntos de recursos de
  transações e contas e salve-os como uma especificação de recuperação
  de recursos.

- Executar um pipeline de treinamento que usa a especificação de
  recuperação de recursos para treinar um novo modelo. Esse pipeline usa
  o componente de recuperação de recursos integrado para gerar os dados
  de treinamento.

### Tarefa 1: Configurar o ambiente

1.  No painel Notebooks, abra o notebook **Experiment and train models
    using features**.

2.  Clique em **Configure session** e carregue o arquivo **conda.yaml**
    da mesma forma que fizemos no notebook anterior.

3.  **Execute** a **primeira célula** para iniciar a sessão. Isso levará
    cerca de 10 minutos.

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4.  Na próxima célula, substitua o espaço reservado para **\<
    your_user_alias \>** pelo seu **user name** na estrutura da pasta e
    **execute** a célula.

![A screenshot of a computer program Description automatically
generated](./media/image42.png)

5.  **Execute** as próximas **3** células para **configurar a CLI**.

6.  A próxima célula inicializa as variáveis do workspace do projeto.
    **Execute**-a para **inicializar as variáveis**.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

7.  A próxima célula inicializa as variáveis do armazenamento de
    recursos. Execute-a.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

8.  Execute a próxima célula para **Initialize the feature store
    consumption client.**

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

Tarefa 2: Criar um conjunto de recursos de contas localmente a partir de
dados pré-calculados

Para incorporar recursos pré-calculados, você pode criar uma
especificação de conjunto de recursos sem escrever nenhum código de
transformação. A especificação de conjunto de recursos é uma
especificação para desenvolver e testar um conjunto de recursos em um
ambiente totalmente local/de desenvolvimento, sem se conectar a nenhum
armazenamento de recursos. Nesta etapa, você criará a especificação do
conjunto de recursos localmente e obterá amostras dos valores a partir
dela.

1.  Execute a célula abaixo para **explorar os dados de origem das
    contas.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  Execute a próxima célula para **criar o conjunto de especificações
    do recurso contas** no local a partir desses recursos
    pré-calculados.

![A screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3.  **Execute** a próxima célula para **gerar uma estrutura de dados
    Spark** a partir da especificação do conjunto de recursos.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  Para registrar a especificação do conjunto de recursos no
    armazenamento de recursos, ela precisa ser salva em um formato
    específico. Ação: após executar a célula abaixo, verifique as contas
    FeatureSetSpec geradas: abra este arquivo na árvore de arquivos para
    ver a especificação:
    featurestore/featuresets/accounts/spec/FeatureSetSpec. **Execute** a
    próxima célula.![A screenshot of a computer program Description
    automatically generated](./media/image50.png)

### Tarefa 3: Experimente recursos não registrados localmente e registre-os no armazenamento de recursos quando estiver pronto

Ao desenvolver recursos, você pode querer testar/validar localmente
antes de registrar no armazenamento de recursos ou executar pipelines de
treinamento na nuvem. Nesta etapa, você irá gerar dados de treinamento
para o modelo de ML a partir da combinação de recursos de um conjunto de
recursos locais não registrados (contas) e do conjunto de recursos
registrado no armazenamento de recursos (transações).

1.  **Execute** a próxima célula para **select features** for **model.**

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

2.  **Execute** as próximas 2 células para **gerar dados de
    treinamento** localmente.

![A close-up of a computer code Description automatically
generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

3.  **Execute** a próxima célula para **registrar o conjunto de recursos
    de contas** com o featurestore. Depois de experimentar diferentes
    definições de recursos localmente e testar sua integridade, você
    pode registrá-las no featureset. Para isso, você registrará uma
    definição de ativo de featureset com o armazenamento de recursos.

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  **Execute** as próximas 2 células para obter o conjunto de recursos
    registrados e o teste de integridade.

![A screenshot of a computer Description automatically
generated](./media/image55.png)

### Tarefa 4: Executar experimento de treinamento

1.  Execute a próxima célula para descobrir recursos do SDK.

![A screenshot of a computer Description automatically
generated](./media/image56.png)

2.  Nas etapas anteriores, você selecionou recursos de uma combinação de
    conjuntos de recursos não registrados e registrados para
    experimentação e teste locais. Agora você está pronto para
    experimentar na nuvem. Salvar os recursos selecionados como uma
    especificação de recuperação de recursos e usá-la no fluxo
    mlops/cicd para treinamento/inferência aumenta sua agilidade no
    envio de modelos.

3.  **Execute** a próxima célula para **selecionar recursos do modelo**.

![A screenshot of a computer program Description automatically
generated](./media/image57.png)

4.  **Execute** a próxima célula e exporte os recursos selecionados como
    uma **feature-retrieval spec.**

![A screenshot of a computer program Description automatically
generated](./media/image58.png)

Tarefa 5: Treinar na nuvem usando pipelines e registrar o modelo se
satisfatório

Nesta etapa, você acionará manualmente o pipeline de treinamento. Em um
cenário de produção, isso poderia ser acionado por um pipeline de ci/cd
com base nas alterações na especificação de recuperação de recursos no
repositório de origem.

1.  **Execute** a próxima célula para **executar o pipeline de
    treinamento.**

![A screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2.  No painel esquerdo do Studio, clique com o botão direito do mouse em
    **Jobs** e abra em uma nova guia. Selecione o experimento,
    **training_on_fraud_model**.

![A screenshot of a computer Description automatically
generated](./media/image61.png)

3.  Clique no **training job** e explore os detalhes. O experimento deve
    levar cerca de 5 a 15 minutos para ser concluído.

![A screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4.  Aguarde até que o processo seja concluído. Quando terminar,
    selecione **Model** no painel esquerdo. Selecione **fraud_model** na
    lista. Este é o modelo que foi criado agora.

![A screenshot of a computer Description automatically
generated](./media/image64.png)

5.  Selecione a guia **Feature sets**. Aqui você pode ver os conjuntos
    de recursos **transactions** e **accounts** dos quais este modelo
    depende.

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6.  Abra **o feature store** **na interface do usuário** em
    +++https://ml.azure.com/home+++. Selecione **Feature stores** -\>
    **featurestore**.

![A screenshot of a computer Description automatically
generated](./media/image66.png)

7.  Selecione **Feature sets** no painel esquerdo e, em seguida,
    selecione qualquer um dos **feature sets**.

![A screenshot of a computer Description automatically
generated](./media/image67.png)

8.  Clique na guia **Model**. Você pode ver a lista de modelos que estão
    usando os conjuntos de recursos (determinados a partir da
    especificação de recuperação de recursos quando o modelo foi
    registrado).

![A screenshot of a computer Description automatically
generated](./media/image68.png)

Resumo:

Neste laboratório, aprendemos a desenvolver e registrar um conjunto de
recursos com armazenamento gerenciado de recursos e treinar modelos
usando recursos.
