# **Laboratório 10 - Usando o painel da Responsible AI para melhorar o desempenho dos modelos de machine learning**

**Objetivo**

Este laboratório tem como objetivo o aprendizado prático de como usar o
painel do Responsible AI para depurar os modelos de machine learning a
fim de melhorar o desempenho do modelo para que ele seja mais justo,
inclusivo, seguro, confiável e transparente.

Neste laboratório, veremos como usar a seção **Model overview** do
modelo do painel do Azure Responsible AI (RAI). Usaremos as coortes
criadas no laboratório Error Analysis para investigar por que o
comportamento do modelo é melhor em uma coorte do que em outra.

Duração prevista - 60 minutos

## **Exercício 1: Preparando os recursos**

## **Tarefa 1: Clonar o repositório para este laboratório**

1.  Em um navegador, faça login no portal do Azure em
    <https://portal.azure.com>

2.  Abra o **cloud shell** clicando no ícone do cloud shell no portal do
    Azure.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  No prompt de comando do Azure Cloud Shell, clone o repositório do
    github do projeto **Diabetes Hospital Readmission** executando o
    comando abaixo.

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> Isso clonará o conteúdo do repositório localmente.
>
> ![](./media/image2.png)

4.  Mude para o diretório do projeto executando o comando abaixo.

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### Tarefa 2: Faça login usando o Azure CLI

1.  No cloud shell, execute o comando abaixo.

**login az**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

2.  Abra a url no console e digite o código no navegador.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

3.  Selecione a credencial de **login do Azure**.

> ![A screenshot of a phone Description automatically generated with
> medium confidence](./media/image5.png)

4.  Clique em **Continue**.

> ![A screenshot of a computer error Description automatically generated
> with medium confidence](./media/image6.png)

5.  Feche o navegador e retorne ao portal do Azure.

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  Os detalhes de login são exibidos no cloud shell.

> ![A screenshot of a computer Description automatically
> generated](./media/image8.png)

7.  Defina o padrão do seu ambiente como **assigned Resource group.**

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **Exercício 2: Executar tarefas para treinar o modelo e criar o painel RAI**

1.  Execute o comando abaixo para registrar o **training dataset** no
    workspace do Azure Machine Learning.

> **az ml data create -f cloud/train_data.yml**

O ativo de dados é criado e os detalhes são exibidos no cloud shell.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  Execute o comando abaixo para registrar o **testing dataset** no
    workspace do Azure Machine Learning.

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  Crie uma **compute instance** para executar os trabalhos. Em
    seguida, copie o nome da computação (por exemplo,
    ***compute-xxxxxxxxxxxxxxxx***) no final da execução para usá-lo
    posteriormente.

- Execute o comando abaixo para **criate** a **compute**.

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

4.  No menu do Cloud Shell, clique no painel **Open editor { }** para
    editar alguns dos arquivos.

> ![Open editor](./media/image13.png)

5.  Clique na pasta **RAI-Diabetes-Hospital-Readmission-classification**
    para expandir o diretório.

![Expand directory](./media/image14.png)

6.  Navegue até o arquivo **cloud/training_job.yml**. Em seguida,
    substitua o espaço reservado para o nome da computação pelo
    **compute instance name** que você copiou anteriormente.

![Training job update](./media/image15.png)

7.  Clique com o botão direito do mouse em qualquer lugar do arquivo e
    selecione a opção **Save** para salvar o arquivo.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8.  Em seguida, navegue até o arquivo
    **cloud/rai_dashboard_pipeline.yml**. Depois, atualize o espaço
    reservado para o nome da computação com o **compute instance name**
    que você copiou anteriormente.

![Rai pipeline update](./media/image17.png)

9.  Clique com o botão direito do mouse em qualquer lugar do arquivo e
    selecione a opção **Save** para salvar o arquivo.

10. Clique com o botão direito do mouse em qualquer parte do arquivo e
    selecione a opção **Quit** para fechar a janela do editor.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image18.png)

11. De volta ao prompt de comando do Cloud Shell, envie a tarefa para
    treinar o modelo. Aguarde até que a tarefa atualize seu status de
    execução para **Concluída** durante o treinamento. Copie o bloco de
    código abaixo para fazer isso.

> **run_id=$(az ml job create --name my_training_job -f
> cloud/training_job.yml --query name -o tsv)**
>
> **\# wait for job to finish while checking for status**
>
> **if \[\[ -z "$run_id" \]\]**
>
> **then**
>
> **echo "Job creation failed"**
>
> **exit 3**
>
> **fi**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **if \[\[ -z "$status" \]\]**
>
> **then**
>
> **echo "Status query failed"**
>
> **exit 4**
>
> **fi**
>
> **running=("Queued" "Starting" "Preparing" "Running" "Finalizing")**
>
> **while \[\[ ${running\[\*\]} =~ $status \]\]**
>
> **do**
>
> **sleep 8**
>
> **status=$(az ml job show -n $run_id --query status -o tsv)**
>
> **echo $status**
>
> **done**
>
> **Observação:** se esse script não for colado corretamente, copie-o e
> cole-o manualmente
>
> **Observação:** a execução desse script deve levar cerca de 3 a 5
> minutos.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

12. Opcionalmente, você pode verificar o status da tarefa em execução no
    **Azure Machine Learning Studio (**<https://ml.azure.com/>**)** -\>
    **Jobs**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image21.png)

13. Após a conclusão bem-sucedida da tarefa de treinamento, registre o
    modelo no workspace do Azure Machine Learning. Execute o comando
    abaixo para fazer isso.

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> Este comando registra o modelo no worksplace do AML e fornece os
> detalhes no cloud shell, conforme mostrado nas capturas de tela
> abaixo.
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

14. Envie o pipeline de tarefas para criar o **RAI dashboard**. Execute
    o comando abaixo para fazer isso.

az ml job create --file cloud/rai_dashboard_pipeline.yml

Este comando envia a tarefa e o cloud shell é preenchido com o estágio
inicial do pipeline, que é o estado **Preparando**.

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

15. Faça login no **Azure Machine Learning Studio** em
    <https://ml.azure.com/> para monitorar a tarefa do pipeline de
    criação do o painel RAI.

16. Selecione **Pipelines**. Para visualizar o andamento da tarefa do
    pipeline de criação do painel RAI, clique no **Display name** da
    tarefa.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

17. O experimento estará no estado **Running**.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

18. O status muda para **Completed** depois de concluído e o painel RAI
    é criado.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

19. Clique na guia **Models** na navegação à esquerda. Em seguida,
    clique no nome do modelo para abrir a página de detalhes.

> ![](./media/image29.png)

20. Selecione a opção **Responsible AI** no menu superior.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

21. Agora, você está pronto para começar a usar o **RAI dashboard**.

## **Exercício 3: Error Analysis:**

A seção Error Analysis do RAI dashboard ajuda a fornecer uma
distribuição de erros dos grupos de recursos que contribuem para a taxa
de erro do modelo. Os erros geralmente não são distribuídos
uniformemente em diferentes subgrupos de dados e o Error Analysis ajuda
a identificar os recursos com as maiores taxas de erro.

### Tarefa 1: Localizar erros de modelo:

Nesta tarefa, vamos explorar como usar a RAI dashboard para encontrar
erros no modelo treinado e identificar onde estão os erros. Além disso,
aprenderemos a criar coortes de dados para investigar por que um modelo
está tendo um desempenho ruim em alguns coortes e não em outros.

1.  Clique no nome **Diabetes Hospital Readmission**![A screenshot of a
    computer Description automatically generated with medium
    confidence](./media/image31.png)

2.  Selecione **Compute**.

![](./media/image32.png)

#### **Tarefa 1.1: Identificar e criar um coorte para o caminho da árvore com os maiores erros**

Para iniciar a análise, é possível observar que o nó raiz indica que, de
um total de 994 dados de teste, foram encontradas 168 previsões
incorretas durante a avaliação do modelo.

1.  Encontre o caminho da árvore com o maior número de erros. Quanto
    mais escura for a tonalidade vermelha no nó, maior será a taxa de
    erro.

2.  Neste caso, o nó folha com a cor vermelha mais escura está na
    segunda posição a partir da parte inferior direita.

![](./media/image33.png)

3.  **Clique duas vezes** neste **nó** para selecionar **todo o
    caminho** que leva até ele. Isso destaca o caminho e exibe a
    condição do recurso para cada nó no caminho.

4.  Crie um grupo a partir do caminho selecionado clicando no botão
    **Save as a new cohort** no canto superior direito da seção Error
    Analysis.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)

5.  Digite o **Cohort name** como**+++ *Err: Prior_Inpatient \>0;
    Num_meds \>11.50 & \<= 21.50+++***

***Clique em Sav*e*.***

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **Tarefa 1.2: Identificar e criar um coorte para o caminho da árvore com o mínimo de erros**

Para fins de comparação, crie outro grupo com o caminho da árvore com o
menor número de erros para ver se conseguimos obter insights sobre por
que o modelo tem um bom desempenho em um grupo em comparação com outro.
O **nó folha** com a condição de recurso ***num_lab_procedures ≤
56,50***, no lado esquerdo da árvore, é o caminho da árvore com o menor
número de erros.

1.  **Clique duas vezes** no nó.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

2.  Clique em **Save as a new cohort**. O **filtro** nesse conjunto de
    dados é: num_lab_procedures \<= 56,50, number_diagnoses \<= 6,50,
    prior_inpatient \<= 0,00.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

3.  **Nomeie** o coorte:**+++ Prior_Inpatient = 0; num_diagnoses \<=
    6,50; lab_procedures \<= 56,50+++** e clique em **Save.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **Tarefa 1.3: Use a lista de características para identificar o principal motivo que contribui para os erros do modelo**

1.  Clique em **Feature list.**

![](./media/image39.png)

2.  A lista é classificada com base na contribuição das características
    para os erros. Quanto mais alta uma característica estiver nessa
    lista, maior será a sua importância para os erros do seu modelo.

3.  Em nosso modelo Diabetes Hospital Readmission model, a **Feature
    List** indica as seguintes características como as principais
    contribuintes para os erros do modelo.

    - Idade

    - núm_medicamentos

    - Medicare

    - tempo_no_hospital

    - núm_procedimentos

    - insulina

    - destino_após_alta

### Tarefa 2: Localizar erros usando o mapa de calor

Na lista de recursos, a **Idade** foi um dos principais contribuintes de
erros. Portanto, usaremos a guia Mapa de calor para explorar qual faixa
etária dos pacientes está levando o modelo a ter um desempenho ruim.

1.  Selecione **Heat map** em **Error Analysis**.

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)

2.  Na guia Heat Map, selecione **Age** no menu suspenso **Rows: Feature
    1** para ver o fator que ela desempenha nos erros do modelo.

3.  Depois de selecionar **Age**, podemos ver como o painel tem uma
    inteligência integrada para dividir as características em diferentes
    células com as condições possíveis.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

2.  **Passe** o mouse sobre cada célula para ver o número de previsões
    corretas e incorretas, a cobertura de erros e a taxa de erros para o
    grupo de dados representado na célula.

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)

3.  *A* célula com **Over 60 years** tem **536** previsões de modelo
    corretas e **126** incorretas. A cobertura de erro é de **73,81%** e
    a taxa de erro é de **18,79%**

4.  A célula com **30–60 years** tem **273** previsões de modelo
    corretas e **25** incorretas. A cobertura de erro é de **25,60%** e
    a taxa de erro é **de 13,61%**.

5.  A célula com **30 years or younger*** *tem **17** previsões de
    modelo corretas e **1** incorreta.

> Como nossa observação mostra que **Age** desempenha um papel
> significativo nas previsões errôneas do modelo, criaremos coortes para
> cada faixa etária para análise adicional no próximo laboratório.

#### ***Tarefa 2.1: Criar coortes com base nas faixas etárias***

1.  Clique na caixa de porcentagem da célula **Over 60 years**. Você
    verá uma borda azul ao redor da célula quadrada.

2.  Clique em **Save as a new cohort.**

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

3.  Na caixa de diálogo Save as a new cohort, digite

    - Cohort name - **+++Age==Over 60 year+++**

Clique em **Save**.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

4.  Repita as etapas 2 e 3 para criar uma coorte para cada uma das
    outras duas células Age.

- **Cohort \#4:** Name - **+++Age == 30–60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### Tarefa 3: Visualizar as listas de coortes

1.  Clique no ícone de engrenagem **Settings** no canto superior direito
    da seção Error Analysis.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)

2.  Isso abrirá uma **janela** **Cohort Settings** com a lista de todas
    as coortes que você criou.

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## Exercício 4: Usando o RAI para realizar a análise de modelos

Neste laboratório, exploraremos como usar a seção **Models Overview** do
painel do Azure Responsible AI (RAI). Usaremos coortes criados no
laboratório Error Analysis para investigar por que o comportamento do
modelo é melhor em uma coorte em comparação com outra.

## **Exercício 4.1: Visão geral do modelo**

### Tarefa 1: Revisar e comparar a tabela de métricas de desempenho do modelo

1.  Role para baixo, abaixo da Error Analysis, para encontrar a seção
    Model Overview.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

2.  Em Model Overview, selecione o painel **Dataset Cohorts**. Isso
    mostrará, em forma de tabela, os diferentes grupos criados junto com
    as métricas do modelo.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

3.  Compare o coorte com o maior número de erros **Err: Prior_Inpatient
    \> 0; Num_Meds \> 11 e ≤ 21,50** com o coorte com o menor número de
    erros **Prior_inpatient = 0; num_diagnose ≤ 6,50; lab_procedures \<
    56,50.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4.  Passe o mouse sobre a linha do gráfico de caixa para ver os detalhes
    da medição.

![A screenshot of a computer Description automatically
generated](./media/image50.png)

5.  Observe que a pontuação de precisão para o **coorte errônea** é
    0,806, o que é ruim. A taxa de **False Positive** é **very low** e o
    valor de **False Negative** é **high**. Ou seja, a maioria dos
    pacientes que o modelo está prevendo tem uma alta taxa de previsão
    de pacientes que não serão readmitidos como readmitidos em 30 dias
    de volta ao hospital.

> ![A red line in a white sheet Description automatically
> generated](./media/image51.png)

6.  Em seguida, observe as métricas da **coorte** com o **menor número
    de erros**, que tem uma pontuação de precisão de 0,94, muito melhor
    do que a pontuação de precisão geral do modelo com todos os dados.
    No entanto, essa coorte também tem uma baixa taxa de **False
    positive** de **0**.

![A picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### Tarefa 2: Examinar o gráfico Probability distribution

1.  Role a tela para baixo para ver a **Probability distribution**.

2.  O gráfico Probability distribution mostra a probabilidade do modelo
    que prevê se os pacientes dos coortes serão readmitidos ou não serão
    readmitidos de volta ao hospital em 30 dias.

3.  Compare a probabilidade de os pacientes não serem readmitidos entre
    os 3 coortes.

4.  Você verá que o coorte **All data** com todos os pacientes do
    conjunto de teste, indica que a maioria dos pacientes não será
    readmitida no hospital dentro de 30 dias, com uma probabilidade
    média de pacientes não readmitidos de 0,854 e um quartil superior de
    0,986, o que é bom.

5.  Em seguida, o coorte com a maior taxa de erro: ***Err:
    Prior_Inpatient \>0; Num_meds \>11.50 & \<= 21.50***, mostra uma
    probabilidade um pouco menor, com mediana de 0,719 e um valor em
    torno de 0,89.

6.  Por fim, o coorte com a menor taxa de erro: ***Prior_Inpatient = 0*;
    *num_diagnoses \<= 6,50*; *lab_procedures \<= 56,50***, mostra que a
    probabilidade de os pacientes não serem readmitidos tem uma mediana
    de 0,90 e um quartil superior de 0,986.

> ![A screenshot of a computer Description automatically
> generated](./media/image53.png)

7.  Para alterar o gráfico e exibir a probabilidade de os pacientes
    serem readmitidos nos 3 coortes, clique no botão **Choose Label** no
    eixo x.

8.  Na janela pop-up, selecione a opção **Probability: Readmitted**.

9.  Em seguida, clique no botão **Apply**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

10. Compare a probabilidade de os pacientes serem readmitidos nos 3
    coortes

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image55.png)

9.  Você verá que os 3 coortes apresentam uma probabilidade de
    readmissão inferior a 0,55. O coorte com o menor número de erros do
    modelo tem a menor probabilidade, de 0,179. Já o coorte com o maior
    número de erros apresenta a maior probabilidade, de 0,543.

### Tarefa 3: Revisar o gráfico de visualização de métricas

Agora, vamos entender melhor o desempenho do modelo, alternando para o
painel de visualizações Metric.

1.  Clique na guia Metric visualizations.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

2.  Para escolher outra métrica, clique em **Choose metric** no eixo x
    para escolher a **Precision score** na lista de outras métricas
    disponíveis. Em seguida, clique no botão **Apply**.

> **Observação**: Como o modelo treinado é um problema de classificação,
> o painel do RAI exibirá apenas métricas de classificação.
>
> ![](./media/image57.png)

3.  Ao analisar o gráfico, você verá que o desempenho do modelo para o
    coorte com todos os dados de teste e o coorte com mais erros está
    correto em aproximadamente 70% do tempo.

4.  A taxa de **Precision score** para o **coorte com menos erros** é de
    **0,94** para pacientes sem hospitalizações anteriores e com menos
    de 7 diagnósticos. Esse valor é consistente com a acurácia do
    modelo.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image58.png)

5.  Por fim, altere a métrica para **Recall** para ver até que ponto o
    modelo foi capaz de prever corretamente que os pacientes dos coortes
    serão readmitidos no hospital em até 30 dias.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6.  O **recall** mostra que a previsão do modelo foi correta em **menos
    de 25% das vezes** para todos os coortes, no caso de pacientes que
    foram readmitidos. Isso revela que as previsões do modelo não são
    corretas na maioria das vezes ao tentar prever quais pacientes serão
    readmitidos em até 30 dias.

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### Tarefa 4: Examinar a matriz de confusão

A Matriz de Confusão é útil para verificar a taxa de acerto do modelo em
suas previsões. Ela mostra o quão bem o modelo está aprendendo a
identificar os casos em que o paciente é readmitido no hospital em até
30 dias, em comparação com os casos em que não é readmitido.

1.  Clique na guia **Confusion matrix**.

&nbsp;

2.  Você observará que o **modelo** tem um desempenho **melhor** com
    pacientes que **Não são Readmitidos**, em comparação com os que são
    **Readmitidos**.

3.  O número de Falsos Negativos deve ser menor que o de Verdadeiros
    Negativos. Isso significa que, de todos os dados dos pacientes, o
    modelo conseguiu prever corretamente que apenas 24 pacientes seriam
    readmitidos no hospital em menos de 30 dias..

- O número de True Positive (TP) é: **802**

- O número de False Negative (FN) é: **159**

- O número de False Positive (FP) é: **9**

- O número de True Negative (TN) é: **24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **Exercício 2: Coorte por Atributo**

Como o coorte com maior taxa de erro inclui pacientes com número de dias
de *Prior_Inpatient \> 0* dias e com número de medicamentos entre 11 e
22, analisar mais de perto as variáveis *Prior_Inpatient* e
*Num_medications* pode ajudar a identificar onde estão os problemas.
Neste laboratório, vamos focar apenas na análise de *Prior_Inpatient*.

1.  Clique na guia **Feature Cohorts.**

2.  No menu suspenso **Feature(s)**, role a lista para baixo e marque a
    caixa de seleção **prior_inpatient**. Isso exibirá 3 diferentes
    coortes de atributos, junto com as métricas de desempenho do modelo.

> ![A screenshot of a computer Description automatically
> generated](./media/image62.png)

3.  O coorte com **prior_inpatient \< 3** possui um tamanho de amostra
    de **943**. Isso significa que a maioria dos pacientes nos dados de
    teste foi hospitalizada **menos** de 3 vezes no passado. A **taxa de
    acurácia** **do modelo** para esse coorte é de **0,838**, o que é
    **bom**.

4.  Apenas 39 pacientes dos dados de teste se enquadram no coorte
    **prior_inpatient ≥ 3 e \< 6**. A acurácia do modelo é de **0,692**,
    o que não é bom.

5.  Por fim, apenas 12 pacientes nos dados de teste tiveram
    hospitalizações anteriores maiores ou iguais a 6 vezes. A **acurácia
    do modelo** para esse coorte é de **0,75**, o que é razoável.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

**Tarefa 1: Distribuição de Probabilidade por Atributo**

Assim como no coorte do conjunto de dados, você pode visualizar a
"Distribuição de Probabilidade".

1.  É possível observar que, quanto menor o número de hospitalizações
    anteriores do paciente diabético, maior a probabilidade de que ele
    não seja readmitido no hospital em até 30 dias.

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### Tarefa 2: Visualizações de Métricas por Atributo

1.  Selecione **Metrics visualization**. No eixo x, clique no botão
    **Choose metric**. Em seguida, selecione a métrica **Precision
    score**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image65.png)

2.  Você verá que o precision score para pacientes com **prior_inpatient
    \< 3** é **0,40**, o que é muito ruim. Isso significa que, de todas
    as previsões feitas pelo modelo para esse coorte, apenas 40% estavam
    corretas.

> ![A blue and white bar graph Description automatically
> generated](./media/image66.png)

3.  O precision score para os outros 2 coortes é bom.

4.  Em seguida, selecione a métrica **Recall score** para o **eixo x**.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image67.png)

5.  Por outro lado, você verá que o recall score para pacientes com
    **prior_inpatient \< 3** é **0,013**. Isso significa que, para a
    maioria dos pacientes nos dados de teste, o modelo está tendo
    dificuldade em prever corretamente se o paciente será readmitido no
    hospital em até 30 dias ou não.

> ![A picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **Resumo**

Este laboratório mostra como as métricas tradicionais de desempenho do
modelo (por exemplo, acurácia, recall, matriz de confusão, etc.) ainda
são muito importantes. Ao combinar os insights de RAI e as métricas
tradicionais de desempenho, o painel nos fornece uma ferramenta
holística para analisar e depurar o modelo de uma forma mais detalhada.
