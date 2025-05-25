> **Laboratório 02 – Criação de um conjunto rotulado de dados usando
> ferramentas de rotulagem de dados do Azure Machine Learning **
>
>  
>
> **Objetivo** 
>
> Neste laboratório, você aprenderá a usar as Ferramentas de Dados do
> Azure Machine Learning no Azure Machine Learning Studio, para
> organizar grupos de dados não rotulados em conjuntos de dados
> rotulados, que acomodam as classes que seriam detectadas pelo modelo
> de detecção de objeto treinado. 
>
> Duração prevista - 40 min 
>
> **Exercício 1: Preparando os recursos do Azure** 
>
> **Tarefa 1: Criar uma conta de armazenamento do Azure** 

1.  Na página inicial do **Portal do Azure,**
    (+++**https://portal.azure.com**+++), digite +++**storage
    account**+++ na barra de pesquisa e selecione **Storage accounts**. 

>  
>
> ![](./media/image1.png) 

2.  Selecione **+Create**. 

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image2.png) 

3.  Na página Create a storage account page, insira os detalhes abaixo. 

**Detalhes do projeto** 

- Subscription – Selecione sua **assinatura**. 

&nbsp;

- Resource group – selecione o **Grupo de recursos** atribuído a você. 

**Instance details** 

- Storage account name – +++**imagestoreacc@lab.LabInstance.Id** +++  

- Region – Selecione a **região** na qual você criou seu **AML
  Workspace** 

- Performance – Selecione **Standard** 

- Redundancy – Selecione **Locally-redundant Storage (LRS)** 

> Selecione **Next.** 
>
>  
>
>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image3.png) 

4.  Na guia Advanced, verifique se a opção **Allow cross-tenant
    replication **na seção **Blob Storage** está desmarcada. Aceite os
    outros padrões e selecione **Review + Create**. 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image4.png) 
>
>  

5.  Assim quando a validação for aprovada, clique em **Create**. 

> ![Uma captura de tela de um erro do computador Descrição gerada
> automaticamente](./media/image5.png) 

6.  Quando a implementação estiver concluída, clique em **Go to
    resource**. 

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image6.png) 

7.  Anote o nome da conta de armazenamento, este será usado
    posteriormente no laboratório. Permaneça na mesma página e continue
    para a próxima tarefa. 

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image7.png) 
>
> **Tarefa 2: Criar um contêiner de armazenamento do Azure** 
>
>  
>
>  

1.  No menu esquerdo da página da conta de armazenamento, role até
    a seção **Data Storage** e selecione **Containers**. 

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image8.png) 

2.  Selecione +**Container**. No painel New Container que será aberto,
    digite o nome do container como +++imagedata+++ e clique em
    **Create**. 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image9.png) 

3.  Após a criação do container, selecione as **Access Keys**
    em **Security + networking **no painel esquerdo. Na página Access
    Keys, clique em **Show** ao lado do valor da chave e, em seguida,
    **copie** a chave. Armazene o valor copiado em um bloco de notas
    para referência futura. 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image10.png) 
>
>  

4.  Navegue de volta para a página de contêineres selecionando
    **Containers** no painel esquerdo. 

> ![](./media/image11.png) 
>
>  

5.  Selecione o contêiner recém-criado, **imagedata**. 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image12.png) 
>
>  

6.  Clique em **Upload**. No painel **Upload blob**, clique em **Browse
    for files** e abra a pasta **train_img** localizada em
    **C:\Labfiles** 

> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image13.png) 
>
>  

7.  Selecione todos os arquivos na pasta train_img e clique em
    **Open.** 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image14.png) 
>
>  

8.  Na página Upload blob, clique em **Upload**. 

>  
>
> ![](./media/image15.png) 
>
>  

9.  Depois de carregado, a mensagem **Successfully uploaded blob(s)**
    será exibida. Feche o painel **Carregar blob**. 

>  
>
> ![](./media/image16.png) 
>
>  

10. Depois de concluído, você deverá ver que todas as 242 imagens foram
    adicionadas ao Contêiner de Armazenamento do Azure. 

>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image17.png) 
>
>  
>
> **Exercício 2: Criar um projeto de rotulagem de dados do Azure Machine
> Learning** 

1.  Na página inicial do Azure Machine Learning Studio, selecione **Data
    Labeling** em **Manage** no painel esquerdo. 

>  
>
>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente](./media/image18.png) 
>
>  

2.  Selecione **+Create.** 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image19.png) 

 

3.  Na seção **Project Details**, forneça os seguintes detalhes. 

&nbsp;

1.  **Project Name** - +++**soda**+++ 

&nbsp;

2.  Media type – **Image** 

&nbsp;

3.  **Labeling task type - Object Identification (Bounding Box)**  

> Selecione **Next**. 
>
>  
>
> ![Uma captura de tela de um computador Descrição gerada
> automaticamente com confiança média](./media/image20.png) 

4.  Na tela **Add workforce (optional),** deixe a opção desativada e
    selecione **Next** para continuar. 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image21.png) 

5.  Na **página Select or create data**, clique em **+Create**. 

 

![](./media/image22.png) 

 

6.  No painel **Data Type** da página **Create Data Asset**, forneça os
    detalhes abaixo. 

&nbsp;

1.  **Name** – +++**sodaObjects**+++ 

&nbsp;

2.  **Description –** +++**Image labelling**+++ 

&nbsp;

3.  **Type –** File 

Clique em **Avançar**. 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image23.png) 

 

7.  No painel **Data Source** da página **Create Data Asset**, selecione
    a opção **From Azure Storage** e clique em **Next.** 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image24.png) 

 

8.  No painel **Storage Type** da página **Create data asset**,
    selecione **Create new datastore**. 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image25.png) 

 

9.  No painel **New datastore**, forneça os detalhes abaixo. 

&nbsp;

1.  **Datastore name** – +++**sodadatastore**+++ 

&nbsp;

2.  **Datastore type** – Selecione **Azure Blob Storage** 

&nbsp;

3.  **Account selection method – **Selecione **From Azure
    subscription** 

&nbsp;

4.  **Subscription ID – **Selecione sua assinatura 

&nbsp;

5.  **Storage account –** Selecione **imagestoreacc** 

&nbsp;

6.  **Blob container – **Selecione** imagedata** 

&nbsp;

7.  **Authentication type – **Selecione **Account Key** 

&nbsp;

8.  **Account key –** Insira a chave da conta salva anteriormente no
    Exercício 1 

>  
>
> Clique em **Create.** 

![](./media/image26.png) 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image27.png) 

 

10. A mensagem **Create Success** é exibida na página **Select a
    datastore.** Selecione o **sodadatastore** que acabou de ser Criado.
    Clique em **Next**. 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image28.png) 

 

11. Em Choose a storage path selecione **Enter storage path manually** e
    digite **/** para o Storage path. Habilite **Skip data validation**.
    Clique em **Next**. 

 

 

 

 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image29.png) 

 

12. Revise os detalhes e clique em **Create**. 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image30.png) 

 

13. De volta ao painel **Select or create data**, selecione
    **sodaObjects.** Clique em **Next**. 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image31.png) 

 

14. Na página **Incremental refresh**, selecione **Enable incremental
    refresh at regular intervals.** Clique em **Next**. 

>  

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image32.png) 

 

15. Na página **Label Categories**, clique em **Add label category**
    duas vezes para adicionar dois novos espaços reservados para nomes
    de categorias, além do já existente. 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image33.png) 

 

16. Depois de adicionar, digite +++**coke**+++, +++**diet_coke**+++
    e +++**sprite**+++, um em cada espaço reservado para label category.
    Clique em **Next**. 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image34.png) 

 

17. Deixe as instruções de rotulagem em branco e clique em **Next**. 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image35.png) 

 

18. Clique em **Next** na página **Quality control(preview)**. 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image36.png) 

 

19. Desative a opção **Enable** **ML assisted labelling **e clique em
    **Create project**. 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image37.png) 

20. Uma mensagem **Success: soda data labelling project created
    successfully. Project is initializing** será exibida na tela Data
    Labelling. Clique no projeto **soda.** 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image38.png) 

 

21. Clique em **Label data**. 

 

![](./media/image39.png) 

 

22. As **Shortcut keys **no canto superior direito mostram os diferentes
    atalhos disponíveis. 

 

![Um grupo de latas de soda em uma tabela Descrição gerada
automaticamente com confiança média](./media/image40.png) 

 

23. A barra de menu superior fornece as diferentes opções disponíveis. 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image41.png) 

 

24. A primeira imagem é aberta na tela. Selecione a marca apropriada no
    painel **Tags** à esquerda. 

> Em seguida, clique na imagem e arraste um pouco para ver o rótulo
> sendo anexado à imagem.
>
> Clique em **submit**. 

 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image42.png) 

 

25. Repita o mesmo processo para as próximas imagens que surgirem após
    enviar a primeira. 

Rotule pelo menos 10 imagens. 

 

 

![](./media/image43.png) 

 

26. A próxima imagem será carregada até que se chegue ao final das
    imagens. Pare em qualquer ponto após a décima imagem, ou prossiga e
    complete a rotulagem de todas as imagens. 

&nbsp;

27. Clique em soda no menu de navegação superior para voltar ao
    **Dashboard**. 

 

 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com confiança média](./media/image44.png) 

 

28. O **Dashboard** fornece os detalhes sobre os **labeled assets **e a
    **label distribution**. 

 

![](./media/image45.png) 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com baixa confiança](./media/image46.png) 

 

 

29. Clique em **Export.** 

 

 

![Uma captura de tela de um gráfico Descrição gerada automaticamente com
baixa confiança](./media/image47.png) 

 

 

30. No painel **Export data**, selecione 

- **Asset type - Labeled ** 

&nbsp;

- **Export format -** **Azure ML dataset** 

> Clique em **Submit**. 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com baixa confiança](./media/image48.png) 

 

31. A mensagem **Labels successfully exported** será exibida na página
    Dashboard quando a exportação é concluída. Clique no **file link**
    na mensagem de sucesso, para abrir os detalhes do arquivo
    exportado. 

 

 

 

![Uma captura de tela de um computador Descrição gerada automaticamente
com baixa confiança](./media/image49.png) 

 

![Uma captura de tela de um computador Descrição gerada
automaticamente](./media/image50.png) 

 

32. Clique no link **View in** **datastores** ou **View in Azure
    portal** na seção **Datasources** -\> **Actions**. 

 

![](./media/image51.png) 

 

33. Exibir em datastores. 

 

![Uma imagem contendo texto, número, software, fonte Descrição gerada
automaticamente](./media/image52.png) 

>  
>
> **Resumo** 
>
> Neste laboratório, você aprendeu a criar um data asset pelo
> armazenamento Azure, e a rotular as imagens, criando um conjunto de
> dados rotulado.  
>
> Todo esse conjunto de tarefas também pertence ao estágio **Data:
> Explore & prepare** do **Machine Learning project workflow.** 
>
>   
