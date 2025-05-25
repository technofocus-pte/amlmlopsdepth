# **Laboratório 05 – Previsão de demanda com Machine Learning automatizado no-code no Azure Machine Learning Studio.**

**Objetivo**

Neste laboratório, você aprenderá a criar um modelo de previsão de série
temporal sem escrever uma única linha de código usando o aprendizado de
máquina automatizado no Azure Machine Learning Studio. Este modelo irá
prever a demanda de aluguel para um serviço de compartilhamento de
bicicletas.

Você não escreverá nenhum código neste laboratório; Você usará a
interface do Studio para realizar o treinamento.

Duração prevista – 60 minutos

## **Exercício 1: Preparando o ambiente de trabalho**

### **Tarefa 1: Iniciar o AML Workspace**

1.  Acesse o **portal do Azure**, +++**https://portal.azure.com+++** se
    ainda não estiver logado.

2.  No menu do portal do Azure, selecione **All resources.**

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Selecione o Azure Machine Learning Workspace
    (**Azuemlws@lab.LabInstanceId**).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Clique em **Launch studio**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

## **Exercício 2: Criar uma tarefa de ML automatizado**

1.  No Azure Machine Learning Studio, clique em **Automated ML** na
    seção **Author**, no painel à esquerda.

2.  Selecione **+ New Automated ML job.**

![](./media/image4.png)

### **Tarefa 1: Criar um ativo de dados**

1.  Dê o nome do experimento como +++**experiment_forecast**+++, aceite
    os outros padrões e selecione **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

2.  Selecione **task type** como **Time series forecasting** e clique em
    **+ Create.**

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  Na página Create data asset, forneça os detalhes a seguir:

    1.  Name – +++**bikedata**+++

    2.  Type – Tabular

> Clique em **Next**.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)

4.  No painel **Data source**, selecione **From local files** e clique
    em **Next**.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  No **Destination storage type**, selecione workspaceblob e clique em
    **Next**.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  Na seleção File or folder, selecione **Upload files** e selecione
    **bike-no.csv** da pasta **C:\Labfiles** e clique em **Next**.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  Verifique se o formulário **Settings and preview** está preenchido
    conforme abaixo e clique em **Next**.

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  O formulário **Schema** permite uma configuração adicional de seus
    dados para este experimento. Para este exemplo, selecione a **toggle
    switch** to be in the off state for th para que esteja no estado
    desligado em relação às colunas

    1.  **casual** e

    2.  **registered**.

> Clique **Next**.

Essas colunas são um detalhamento da coluna **cnt**, portanto, não as
incluímos.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

9.  No formulário **Review**, verifique as informações e clique em
    **Create** para concluir a criação do ativo de dados.

> ![](./media/image13.png)

10. De volta à página **Create a new Automated ML job**, uma mensagem de
    **sucesso** referente à criação do ativo de dados será exibida..

11. Selecione o **bikedata** recém-criado e clique em **Next**.

> **Observação: Refresh** o painel de ativos de dados se os dados de
> bicicleta não estiverem sendo exibidos.
>
> ![](./media/image14.png)

### **Tarefa 2: Configurar a tarefa**

1.  Na página **Task settings**, forneça os detalhes abaixo e selecione
    **View additional configuration settings**.

> Target column – **cnt(Integer)**
>
> Time column **– date (Date)**
>
> **Desmarque** a opção **Autodetect forecast horizon** e forneça o
> valor como +++**14**+++.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

2.  No painel Additional configuration, forneça os detalhes abaixo e
    clique em **Save**.

- Primary metric - **Normalized root mean squared error**

- Explain best model – **Ativar**

- Blocked algorithms - **Extreme Random Trees**

> Expanda as configurações de previsão adicionais

- Autodetect Forecast target lags – **Não selecione**

- Autodetect Target rolling window size – **Não selecione**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3.  Selecione **Limits** e digite +++**60**+++ no campo **Experiment
    timeout(minutes)**.

![A screenshot of a test AI-generated content may be
incorrect.](./media/image17.png)

4.  Selecione os valores abaixo em **Validate and test** e, em seguida,
    clique em **Next**.

> Validation type – **k-fold cross-validation**
>
> Number of cross validations – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

5.  Selecione **automl-compute** (o recurso que criamos no laboratório
    anterior). Clique em **Next.**

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

6.  Revise os detalhes e selecione **Submit training job**.

> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

7.  A página de status mostra o status inicial como **Running**.
    Continue atualizando a página para acompanhar o progresso.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8.  Quando o treinamento for concluído, o status será alterado para
    **Completed**.

**Observação:** O treinamento leva aproximadamente de 30 a 45 minutos
para ser concluído.

## **Exercício 3: Explorar modelos**

1.  Navegue até a guia **Models** para ver os algoritmos (modelos)
    testados. Por padrão, os modelos são ordenados pela pontuação da
    métrica à medida que são concluídos.

2.  Neste tutorial, o modelo com a maior pontuação com base na métrica
    escolhida **Normalized root mean squared error** estará no topo da
    lista.

3.  Enquanto você aguarda a conclusão de todos os modelos de
    experimento, selecione o **Algorithm name** de um modelo concluído
    para explorar seus detalhes de desempenho.

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

4.  Clique em **Overview** e veja seus detalhes.

> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

5.  Clique na guia **Metrics** e explore os detalhes.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image24.png)
>
> **Importante**: Continue executando o próximo laboratório enquanto
> este treinamento é concluído. Retorne a este laboratório a partir
> deste ponto assim que o treinamento for finalizado![A screenshot of a
> computer Description automatically generated](./media/image25.png)

## **Exercício 4: Identifique o melhor modelo**

O Automated Machine Learning no Azure Machine Learning Studio permite
que você implemente o melhor modelo como um serviço da web em algumas
etapas. A implementação é a integração do modelo para que ele possa
prever novos dados e identificar possíveis áreas de oportunidade.

1.  Quando o trabalho estiver concluído, navegue de volta para a página
    do trabalho pai selecionando o **job name** na parte superior da
    tela.

![](./media/image26.png)

2.  Na seção **Best model summary**, o melhor modelo (neste contexto
    experimental) é escolhido com base na métrica **Normalized root mean
    squared error.**

![A screenshot of a computer Description automatically
generated](./media/image27.png)

3.  Clique no Algorithm name para abri-lo e explorar os detalhes.

4.  O modelo também pode ser implementado como um serviço da web**.**

**Resumo**

Neste laboratório, você usou o automated ML no Azure Machine Learning
studio para criar um modelo de previsão de séries temporais que prevê a
demanda de aluguel de bicicletas em um serviço de compartilhamento.
