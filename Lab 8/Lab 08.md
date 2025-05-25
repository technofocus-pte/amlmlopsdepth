# 실습 08 – 프롬프트 플로우를 사용하여 RAG로 QA 데이터 생성을 구현하기

**목표:**

QA 데이터 생성은 RAG(Retrieval Augmented Generation) 생성 프로세스의
일부로, 자동 생성된 QA 데이터세트를 사용하여 RAG에 대한 최상의
프롬프트를 얻고 RAG에 대한 평가 메트릭을 가져옵니다

이 실습에서는 데이터에서 QA 데이터세트를 생성하는 방법을 배웁니다.

예상 소요 시간 – 60분

## 연습 1: AOAI 배포를 생성하기 

이 연습에서는 이전 실습에서 생성한 Azure OpenAI 리소스를 사용하여
gpt-35-turbo 모델 배포를 생성할 것입니다.

1.  Azure Machine Learning Studio에서 왼쪽 창에서**Model Catalog**를
    선택하세요. +++**gpt-35-turbo**+++를 검색하고 모델
    목록에서**gpt-35-turbo**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  **Azure OpenAI resource**필드에서 AOAI 리소스
    **AOAI-PF@lab.LabInstanceId**가 선택되어 있는지 확인하세요. 모델을
    배포하기 위해 **Deploy**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  **Deployment name**를 수락하고**Deploy**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

1.  배포 이름을 +++**text-embedding-ada-002-2**+++로 지정하여
    **text-embedding-ada-002**에 대한 모델 배포를 반복하세요.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## 연습 2: 환경을 설정하기

1.  Studio의 왼쪽 창에서**Notebooks**를 선택하세요. 사용자 이름 옆에
    있는 세 개의 점을 클릭하고 **Upload files**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

2.  **C:\LabFiles**로 이동하고**qa_data_generation.ipynb** 파일을
    선택하세요. **I trust contents of this file** 확인란을 선택하고
    **Upload**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  Open the notebook and select **Serverless Spark Compute** in the
    **Compute** option.

![A screenshot of a computer program Description automatically
generated](./media/image7.png)

4.  컴퓨팅이 연결되면 **Configure session**을 선택하여 conda.yml 파일을
    업로드하고 이를 사용하여 실행할 환경을 설정하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  **Python packages** -\>**Upload Conda file** -\>를 선택하고
    **Browse**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  **C:\LabFiles**에서 **conda.yml**를 선택하고 **Apply**를 선택하세요.

> ![A screenshot of a computer program Description automatically
> generated](./media/image10.png)

## 연습 3: AzureML Workspace에 대한 클라이언트 가져오기

1.  종속성을 설치하려면notebook의 첫 번째 셀을 실행하세요.

![](./media/image11.png)

![A screenshot of a computer Description automatically
generated](./media/image12.png)

**참고:** 이 작업을 완료하는 데 10분에서 15분 정도 걸립니다

2.  az login을 사용하여 다음 셀을 실행하여 **Azure** **CLI**에
    **login**하세요.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

3.  작업 공간은Azure Machine Learning의 최상위 리소스로, Azure Machine
    Learning을 사용할 때 생성하는 모든 아티팩트를 사용할 수 있는 중앙
    집중식 위치를 제공합니다. 이 섹션에서는 작업이 실행될 작업 영역에
    연결합니다. MLClient는 AzureML과 상호 작용하는 방법입니다.

4.  **Subscription ID**의 자리 표시자를 +++@lab.Subscription()+++로
    바꾸세요. **Resource group name**이 있는 **Resource group** 및 다음
    셀에 +++**Azuremlws@lab.LabInstanceId+++**가 있는 **Azure ML
    Workspace**를 사용하여 MClient를 생성하세요.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer Description automatically
generated](./media/image15.png)

5.  **Connection name**을 설정한 후 셀을 **Execute**하세요. 연결을
    생성하는 동안 다른 이름을 사용한 경우 이 셀에 해당 값을 지정한 후
    실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

6.  **Key**값을 **Azure openAI** **key**로 바꾸고 **target**값을 이전에
    저장한 Azure OpenAI 리소스의 **Endpoint**값으로 바꾸세요.

값을 바꾼 후 셀을 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

7.  이제 작업 영역이 Azure OpenAI에 연결되었으므로 gpt-35-turbo 모델이
    유추할 준비가 되었는지 확인합니다.

8.  모델 및 **deployment** 이름을 설정하려면 다음 셀을
    **Execute**하세요. 모델 및 배포를 생성하는 동안 다른 이름을 지정한
    경우 모델 이름과 배포 이름의 값을 바꾸세요.

![A screenshot of a computer code Description automatically
generated](./media/image18.png)

9.  마지막으로, 배포 및 모델 정보를 AzureML 포함 구성 요소가 입력으로
    예상하는 uri 형식으로 결합합니다. 이 작업을 수행하려면 다음 셀을
    **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

## 연습 4: Pipeline을 설정하기

AzureML Pipelines은 여러 구성 요소를 함께 연결합니다. 각 구성 요소는
입력, 코드에서 생성된 입력 및 출력을 사용하는 코드를 정의합니다.
파이프라인 자체에는 개별 하위 구성 요소를 함께 연결하여 생성된 입력과
출력이 있을 수 있습니다. 임베딩 및 인덱싱을 위해 데이터를 처리하기 위해
여러 구성 요소를 함께 연결하며, 각 구성 요소는 워크플로의 자체 단계를
수행합니다.

구성 요소는 기본적으로 액세스 권한이 있어야 하는 레지스트리 azureml에
게시되며, 모든 작업 영역에서 액세스할 수 있습니다. 아래 셀에서 azureml
레지스트리에서 구성 요소 정의를 가져옵니다.

1.  문제 없이 실행되는지 확인하려면 다음 셀을 실행하세요.

![A screenshot of a computer code Description automatically
generated](./media/image20.png)

2.  각 구성 요소에는 구성 요소의 목적과 각 입력/출력에 대한 전반적인
    설명을 제공하는 설명서가 있습니다. 예를 들어, Component 정의를
    검사하여 **data_generation_component** 수행하는 작업을 이해할 수
    있습니다. 이에 대해 출력을 관찰하고 다음 셀을 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

3.  Below a Pipeline 는 위의 구성 요소, 입력 및 출력을 함께 연결하는
    python 함수를 정의하여 구축됩니다. 함수에 대한 인수는 파이프라인
    자체에 대한 입력이고 반환 값은 파이프라인의 출력을 정의하는
    사전입니다. **Next cell**이 성공적으로 **executed**되는지
    확인하세요.

![A screenshot of a computer code Description automatically
generated](./media/image22.png)

![A screenshot of a computer program Description automatically
generated](./media/image23.png)

4.  아래 설정은 더 큰 AzureDocs git 리포지토리의 AzureML 설명서만
    처리하도록 다양한 git 및 data_source 매개 변수를 설정하고, 각 문서의
    원본 URL이 git url 대신 공개적으로 호스팅된 URL에 연결되도록
    처리하는 방법을 보여 줍니다.

5.  성공적으로 실행되는지 확인하려면 다음 두 셀을 실행하세요.

![](./media/image24.png)

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

## 연습 5: Pipeline을 제출하기

1.  파이프라인의 각 단계 출력은 Workspace UI를 통해 검사할 수 있으며,
    아래 셀을 실행한 후 ' Details Page' 아래의 링크를 클릭하세요.

2.  다음 셀을 실행하고 출력에서 링크를 클릭하여 플로우 상태를
    확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  실행은 프롬프트 플로우에서 열립니다. 플로우의 각 단계 탐색하세요.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

![A screenshot of a computer Description automatically
generated](./media/image28.png)

6.  플로우이 성공하면 다음 단계로 이동하세요.

## 연습 6: 생성한 QA 데이터를 검토하기

1.  QA 데이터에 대한 출력을 검토하려면 다음 2개의 셀을 실행하세요.

![A screenshot of a computer code Description automatically
generated](./media/image29.png)

> ![A screenshot of a computer code Description automatically
> generated](./media/image30.png)

요약:

이 실습에서는 데이터에서 QA 데이터세트를 생성하는 방법을 배웠습니다.
