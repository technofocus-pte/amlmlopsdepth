# 실습 02 - Azure Machine Learning 데이터 레이블 지정 도구를 사용하여 레이블이 지정된 데이터 세트 생성하기

**목표**

이 실습에서는 Azure Machine Learning 스튜디오에서 Azure Machine Learning
데이터 도구를 사용하여 레이블이 지정되지 않은 데이터 컬렉션을 학습된
개체 검색 모델에서 검색되는 클래스를 수용하는 레이블이 지정된
데이터세트로 관리하는 방법을 알아봅니다.

예상 소요 시간 – 40분

## **연습 1: Azure 리소스를 준비하기**

### **작업 1: Azure Storage Account를 생성하기**

1.  **Azure portal** (+++**https://portal.azure.com**+++) **Home**
    페이지에서 검색 바에 +++**storage account**+++를 입력하고 **Storage
    accounts**를 선택하세요.

![](./media/image1.png)

2.  **+Create**를 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image2.png)

3.  Create a storage account 페이지에서 다음 세부 정보를 입력하세요.

> **프로젝트 세부 정보**

- Subscription – **subscription**를 선택하세요.

- Resource group – 할당된 **Resource group**를 선택하세요.

> **인스턴스 세부 정보**

- Storage account name – +++**imagestoreacc@lab.LabInstance.Id** +++

- Region – **AML Workspace**를 생성한 **Region**을 선택하세요

- Performance – **Standard**를 선택하세요

- Redundancy – **Locally-redundant storage (LRS)** 를 선택하세요

**Next**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  Advanced 탭에서 **Blob Storage** 섹션에서 **Allow cross-tenant
    replication**옵션이 선택 취소되어 있는지 확인하세요. 다른 기본값을
    적용하고 **Review + Create**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  유효성 검사가 통과되면 **Create**를 클릭하세요.

> ![A screenshot of a computer error Description automatically
> generated](./media/image5.png)

6.  배포가 완료되면 **Go to resource**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  랩의 뒷부분에서 사용되므로 Storage 계정 이름을 기록해 두세요. 같은
    페이지를 유지하고 다음 작업을 계속하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

### **작업 2: Azure Storage Container를 생성하기**

1.  스토리지 계정 페이지의 왼쪽 메뉴에서 **Data Storage** 섹션으로
    스크롤하고 **Containers**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  **+ Container**를 선택하세요. 열리는 New container 창에서 컨테이너
    이름을 +++imagedata+++로 입력하고 **Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

3.  컨테이너를 생성하면 왼쪽 창에서 **Security + networking**의 **Access
    keys**를 선택하세요. Access keys 페이지에서 키 값에 대해 **Show**를
    클릭한 후 키를 **copy**하세요. 나중에 참조할 수 있도록 복사된 값을
    메모장에 저장하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

4.  왼쪽 창에서 **Containers**를 선택하여 컨테이너 페이지로 다시
    이동하세요.

![](./media/image11.png)

5.  새로 생성된 컨테이너 **imagedata**를 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image12.png)

6.  **Upload**를 클릭하세요. **Upload blob** 창에서 **Browse for
    files**를 클릭하고 **C:\Labfiles** 아래에서 **train_img** 폴더를
    여세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image13.png)

7.  train_img 폴더의 모든 파일을 선택하고 **Open**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

8.  Upload blob 페이지에서 **Upload**를 클릭하세요.

![](./media/image15.png)

9.  업로드되면 **Successfully uploaded blob(s)** 메시지가 표시되면
    **Upload blob** 창을 닫으세요.

![](./media/image16.png)

10. 완료되면 242개의 이미지가 모두 Azure Storage Container에 추가된 것을
    볼 수 있습니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image17.png)

## **연습 2: Azure Machine Learning 데이터 레이블 지정 프로젝트를 생성하기**

1.  Azure Machine Learning Studio에서 홈페이지에서 왼쪽 창에
    **Manage**에서 **Data Labeling**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

2.  **+ Create**를 선택하세요**.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)

3.  **Project details** 섹션에서 다음 세부 정보를 입력하세요.

    1.  **Project name** - +++**soda**+++

    2.  Media type – **Image**

    3.  **Labeling task type - Object Identification (Bounding Box)** 

**Next**를 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

4.  **Add workforce (optional)** 화면에서 옵션을 비활성화된 상태로 두고
    **Next**를 계속하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image21.png)

5.  **Select or create data page**에서 **+ Create**를 클릭하세요.

> ![](./media/image22.png)

6.  **Create data asset** 페이지의 **Data type**창에서 다음 세부 정보를
    입력하세요.

    1.  **Name** – +++**sodaObjects**+++

    2.  **Description –** +++**Image labelling**+++

    3.  **Type –** 파일

> **Next**를 클릭하세요.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

7.  **Create data asset** 페이지의 **Data source**창에서 **From Azure
    storage** 옵션을 선택하고 **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image24.png)

8.  **Create data asset** 페이제의 **Storage type** 창에서 **Create new
    datastore**를 선택하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image25.png)

9.  **New** **datastore** 창에서 다음 세부 정보를 입력하세요.

    1.  **Datastore name** – +++**sodadatastore**+++

    2.  **Datastore type** – **Azure Blob Storage**를 선택하세요

    3.  **Account selection method – From Azure subscription**를
        선택하세요

    4.  **Subscription ID –** subscription을 선택하세요

    5.  **Storage account –** **imagestoreacc**를 선택하세요

    6.  **Blob container – imagedata**를 선택하세요

    7.  **Authentication type – Account Key**를 선택하세요

    8.  **Account key –** 연습 1에서 이전에 저장한 account key를
        입력하세요

> **Create**를 클릭하세요.
>
> ![](./media/image26.png)
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image27.png)

10. **Select a datastore** 페이지에 **Create success** 메시지가
    표시됩니다. 생성한 **sodadatastore**를 선택하세요. **Next**를
    클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

11. Under Choose a storage path에서 **Enter storage path manually**를
    선택하고 Storage path에 대한 **/**를 입력하고 **Skip data
    validation**를 활성화하세요. **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

12. 세부 정보를 검토하고 **Create**를 클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

13. **Select or create data** 창으로 돌아가서 **sodaObjects**를
    선택하세요. **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

14. **Incremental refresh** 페이지에서 **Enable incremental refresh at
    regular intervals**를 선택하세요. **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

15. **Label categories** 페이지에서 **Add label category**를 두 번
    클릭하여 기존 범주 이름 자리 표시자 외에 두 개의 범주 이름 자리
    표시자를 추가하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

16. 추가 후 각 레이블 범주 자리 표시자에 +++**coke**+++,
    +++**diet_coke**+++ 및 +++**sprite**+++를 입력하세요. **Next**를
    클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

17. Labelling instructions를 비워 두고 **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

18. **Quality control(preview)** 페이지에서 **Next**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

19. **Enable** **ML assisted labelling** 옵션을 비활성화하고 **Create**
    **project**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

20. **Success: soda data labelling project created successfully. Project
    is initializing** 메시지가 Data Labelling 화면에 표시됩니다.
    **Soda** 프로젝트를 클릭하세요**.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

21. **Label data**를 클릭하세요.

> ![](./media/image39.png)

22. 오른쪽 상단의 **Shortcut keys**에는 사용 가능한 다양한 바로 가기가
    표시됩니다.

> ![A group of soda cans on a table Description automatically generated
> with medium confidence](./media/image40.png)

23. 상단 메뉴 모음은 사용 가능한 다양한 옵션을 제공합니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

24. 첫 번째 이미지가 화면에 열립니다. 왼쪽의 **Tags**창에서 적절한
    태그를 선택하세요.

> 이미지에 레이블이 첨부되는 것을 확인하려면 이미지를 클릭하고 약간
> 드래그하세요. **Submit** 를 클릭하세요.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image42.png)

25. 현재 이미지를 제출할 때 나타나는 다음 이미지에 대해 동일한
    프로세스를 반복하세요.

> 최소 10개의 이미지에 레이블 지정하세요
>
> ![](./media/image43.png)

26. 다음 이미지는 이미지의 끝에 도달할 때까지 업로드됩니다. 10개 이상의
    이미지를 초과하는 지점에서 중지하거나 모든 이미지에 대한 라벨링을
    진행하고 완료하세요.

27. 상단 탐색 경로에서 soda를 클릭하여 **Dashboard**로 돌아가세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image44.png)

28. **Dashboard**는 **labeled assets** 및 **label distribution**에 대한
    세부 정보를 제공합니다.

> ![](./media/image45.png)
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image46.png)

29. **Export**를 클릭하세요**.**

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image47.png)

30. **Export data** 창에서 다음을 선택하세요

    - **Asset type - Labeled**

    - **Export format -** **Azure ML dataset**

> **Submit**를 클릭하세요.
>
> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image48.png)

31. 내보내기가 완료되면 **Labels successfully exported** 메시지가
    Dashboard 페이지에 표시됩니다. 내보낸 파일의 세부 정보를 여려면 성공
    메시지에서 **file link**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> low confidence](./media/image49.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image50.png)

32. **Datasources** -\> **Actions** 섹션의 **View in** **datastores**
    또는 **View in Azure portal**링크를 클릭하세요.

> ![](./media/image51.png)

33. 데이터스토어에서 보세요.

> ![A picture containing text, number, software, font Description
> automatically generated](./media/image52.png)

**요약**

이 실습에서는 Azure Storage에서 데이터 자산을 생성하는 방법과 이미지에
레이블을 지정하고 레이블이 지정된 데이터 세트를 생성하는 방법을
알아보았습니다.

이 전체 작업 집합은 **Machine Learning project workflow**의 **Data:
Explore & Prepare** 단계 에도 있습니다.
