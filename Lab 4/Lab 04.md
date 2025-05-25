# **실습 04 – Azure Machine Learning Studio에서 코드 없는 AutoML을 사용하여 분류 모델을 학습하기**

**목표**

이 랩에서는 Azure Machine Learning 스튜디오에서 Azure Machine Learning
자동화된 ML을 사용하여 코드 없는 AutoML로 분류 모델을 학습시키는 방법을
알아봅니다. 이 분류 모델은 고객이 금융 기관에 고정 정기 예금에 가입할지
여부를 예측합니다. 자동화된 Machine Learning은 알고리즘과
하이퍼파라미터의 다양한 조합을 빠르게 반복하여 선택한 성공 지표를
기반으로 최상의 모델을 찾는 데 도움을 줍니다.

예상 소요 시간 – 60분

Azure Machine Learning의 **Deploy Model** 단계에 있습니다.

![](./media/image1.png)

## **연습 1: Azure Machine Learning 작업 공간을 생성하기**

1.  **Resources** 탭의 자격 증명을 사용하여 Azure Portal로 **–**
    +++**https://portal.azure.com**+++ 로그인하세요.

2.  Azure portal 홈페이지에서 **+ Create a resource**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  **Create a resource**에서 검색바에서 +++**Azure Machine
    Learning**+++ 찾으세요. **Marketplace**에서 **Azure Machine
    Learning**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

4.  **Marketplace**에서 **Create** 드롭다운을 클릭하고 **Azure Machine
    Learning**를 선택하세요.

> ![A screenshot of a software Description automatically
> generated](./media/image4.png)

5.  새 작업 공간을 구성하려면 다음 정보를 제공합니다:

    - **Subscription**: **할당된 Azure subscription**를 선택하세요

    - **Resource group**: 할당된 Resource Group를 선택하세요

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

**Workspace Details:**

- **Workspace name: +++Azuremlws@lab.LabInstanceId+++**

&nbsp;

- **Region**: 여기에 **North Central US** 지역이 사용됩니다

- **Container registry: Create new**를 선택하세요**.**
  <+++Azuremlcr@lab.LabInstanceId>+++를 입력하세요

![A screenshot of a computer Description automatically
generated](./media/image6.png)

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  작업 공간 구성이 완료되면 **Review + Create**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

7.  유효성 검사가 통과되면 Create를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

8.  새 작업 공간을 보려면 **Go to resource**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**에서
    **Work with your model in Azure Machine Learning studio**의 **Launch
    studio**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

## **연습 2: Automated ML 작업을 생성하기**

1.  Azure Machine Learning Studio 탭으로 이동하세요.

2.  왼쪽 창에서 **Authoring** 섹션의 **Automated ML**를 선택하세요.

3.  **+ New Automated ML job**를 클릭하세요.

![](./media/image12.png)

### **작업 1: 데이터 자산을 생성하기**

1.  **Basic settings** 페이지에서 New experiment name을
    +++MarketingExperiment+++로 지정하고 다른 기본값을 수락한 후
    **Next**을 클릭하세요.

![](./media/image13.png)

2.  Task type & data 페이지에서 **Select task type**에서
    **Classification**를 선택하고**Select data**에서 **+ Create**를
    선택하세요**.**

![A screenshot of a computer Description automatically
generated](./media/image14.png)

3.  Create data asset 페이지에서 다음 세부 정보를 입력하세요.

- **Name** – +++marketingdata+++

- **Type** – **Tabular**

- **Next**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image15.png)

4.  **Data source** 창에서 **From local files**를 선택하고 **Next**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

5.  **Destination storage type**에서 작업 영역을 생성하는 동안 자동으로
    설정된 기본 데이터 저장소인 **workspaceblobstore**를 선택하세요.
    작업 영역에서 사용할 수 있도록 이 위치에 데이터 파일을 업로드합니다.
    **Next**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

6.  **File or folder selection**에서 **Upload files or
    folder** \> **Upload files**를 선택하세요. **C:/Labfiles**에서
    **bankmarketing_train.csv** 파일을 선택하세요. **Next**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

7.  업로드가 완료되면 파일 형식에 따라 **Data preview**영역이
    채워집니다. **Settings **양식에서 데이터 값을 검토하세요. **Next**을
    선택하세요.

[TABLE]

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

8.  **Schema **양식을 사용하면 이 실험에 대한 데이터를 추가로 구성할 수
    있습니다. 이 예에서는 **day_of_week** 대한 토글 스위치를 선택하여
    포함하지 않도록 합니다. **Next**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

9.  **Review** 양식에서 정보를 확인하고 **Create**를 선택하여 **data
    asset** 생성을 완료하세요.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

10. **Create a new Automated ML job** 페이지에서 데이터 자산 생성에 대한
    **success** 메시지가 표시됩니다. 생성된 **marketingdata** 데이터
    자산을 선택하고 **Next**을 클릭하세요.

> **참고:** **marketingdata** 가 표시되지 않으면 Refresh을 클릭하여
> 나열합니다.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

### **작업 2: 작업을 구성하기**

1.  **Task Settings** 페이지에서 **y(String)**를 **Target column**로
    선택하여 예측하려는 항목을 선택하새요. 이 열은 고객이 정기 예금에
    가입했는지 여부를 나타냅니다.

2.  **View additional configuration settings**를 선택하고 다음과 같이
    필드를 채우세요. 이러한 설정은 교육 작업을 더 잘 제어하기 위한
    것입니다. 그렇지 않으면 실험 선택 및 데이터를 기반으로 기본값이
    적용됩니다.

- Primary metric – AUCWeighted

- Explain best model – Enable

- Use all supported models - Enable

- Blocked models – None

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

3.  **Limits**를 선택하고**Experiment timeout(minutes)** 필드에서
    +++**60**+++를 입력하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a test AI-generated content may be
incorrect.](./media/image25.png)

4.  **Validate and test**에서 아래 값을 제공하고 **Next**를 클릭하세요.

- Validation type - **k-fold cross-validation**를 선택하세요

- Number of cross validations – **2**를 선택하세요

![A screenshot of a computer Description automatically
generated](./media/image26.png)

5.  Compute 페이지에서 Select compute type을 **Compute cluster**로
    선택하고 **+ New**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

6.  **Create compute cluster** 창에서 다음 세부 정보를 선택하고
    **Next**를 클릭하세요.

- Location – **North Central US** (Azure Machine Learning 작업 영역의
  위치와 동일)

- Virtual machine tier – **Dedicated**

- Virtual machine type - **CPU**

- Virtual machine size - **Standard_DS12_v2**를 선택하세요

![A screenshot of a computer Description automatically
generated](./media/image28.png)

7.  Advanced settings에서 다음 세부 정보를 입력하고 **Create**를
    선택하세요.

- Compute name - +++automl-compute+++

- Minimum number of nodes - 0

- Maximum number of nodes – 1

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

8.  컴퓨팅 프로비저닝이 성공하면 **Next**을 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

9.  **Review** 페이지에서 **Submit the training job**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image31.png)

10. 실험 준비가 시작되면 **Overview** 화면이 열리고 **Status**가 맨 위에
    표시됩니다. 이 상태는 실험이 진행됨에 따라 업데이트됩니다. 실험
    상태를 알려주는 알림도 스튜디오에 표시됩니다.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

> **참고:** 교육은 완료하는 데 약 40분이 소요됩니다.

## **연습 3: 모델을 탐색하기**

학습이 진행되는 동안 다음과 연결된 모델을 탐색할 수 있습니다..

1.  테스트된 알고리즘(모델)을 확인하려면 **Models + child** jobs탭으로
    이동하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

2.  **StandardScalerWrapper, XGBoostClassifier** 모델을 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

3.  **Metrics**를 클릭하고 Metrics 탭에서 세부 정보를 탐색하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

4.  모든 실험 모델이 완료될 때까지 기다리는 동안 완료된 모델의
    **Algorithm name**을 선택하여 성능 세부 정보를 탐색하세요. 작업에
    대한 정보를 확인하기 위해 **Overview** 및 **Metrics** 탭을
    선택하세요.

> **중요:** 모델 학습은 완료하는 데 약 40분이 걸립니다. 이 작업이
> 진행되는 동안 다음 실습을 진행하세요. 상태가 **Completed**로 변경되면
> 이 실습으로 다시 시작하세요

## **연습 4: 모델 설명**

모델 설명은 요청 시 생성할 수 있습니다. **Explanations (preview)** 탭의
일부인 모델 설명 대시보드에는 이러한 설명이 요약되어 있습니다.

1.  Models + child jobs 탭 (parent job)에서 **MaxAbsScaler, LightGBM**를
    선택하세요**.**

![A screenshot of a computer Description automatically
generated](./media/image36.png)

2.  **Explain model** 탭을 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image37.png)

3.  열리는 Explain model 창에서 다음을 선택하세요

    1.  Select compute type - **Compute cluster**

    2.  Select AzureML compute instance - **automl-compute**를
        선택하세요

**Create**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

4.  성공 메시지가 표시됩니다. **Explanations(preview)** 탭을 선택하세요.
    이 탭은 설명 가능성 실행이 완료된 후 채워집니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

5.  왼쪽 창을 확장하세요. **Features**에서 **raw**라고 표시된 행을
    선택하세요. **Aggregate feature importance** 탭을 선택하세요. 이
    차트는 선택한 모델의 예측에 영향을 준 데이터 기능을 보여줍니다.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

이 예에서 **duration**은 이 모델의 예측에 가장 큰 영향을 미치는 것으로
보입니다.

## **연습 5: 최상의 모델을 배포하기**

자동화된 Machine Learning 인터페이스를 사용하면 최상의 모델을 웹
서비스로 배포할 수 있습니다. *배포*는 새로운 데이터를 예측하고 잠재적인
기회 영역을 식별할 수 있도록 모델을 통합하는 것입니다. 이 실험에서 웹
서비스에 배포한다는 것은 금융 기관이 이제 잠재적인 정기 예금 고객을
식별하기 위한 반복적이고 확장 가능한 웹 솔루션을 갖게 되었음을
의미합니다.

실험 실행이 완료되면 **Details** 페이지가 **Best model
summary** 섹션으로 채워집니다. 이 실험 컨텍스트에서 **VotingEnsemble**은
**AUCWeighted** 메트릭을 기반으로 최상의 모델로 간주됩니다.

1.  왼쪽 창에서**Jobs**를 선택하고 생성한 실험을 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image41.png)

2.  실험의 표시 이름을 클릭하세요.

![](./media/image42.png)

3.  상태가 **Completed**되는지 확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

4.  실험 실행이 완료되면 **Details** 페이지가 **Best model
    summary** 섹션으로 채워집니다. 이 실험 컨텍스트에서
    **VotingEnsemble**은 **AUCWeighted** 메트릭을 기반으로 최상의 모델로
    간주됩니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image44.png)

이 모델을 배포하지만 배포를 완료하는 데 약 20분이 걸립니다. 배포
프로세스에는 모델 등록, 리소스 생성 및 웹 서비스에 대한 구성을 포함한
여러 단계가 수반됩니다.

5.  model-specific 페이지를 열기 위해 **VotingEnsemble**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

6.  왼쪽 상단의 **Deploy** 메뉴를 선택하고 **Deploy to web service**를
    선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

7.  **Deploy a model** 창을 다음으로 채우세요:

[TABLE]

> **Deploy**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image47.png)

8.  **Model deployment is successfully triggered**다는 성공 메시지가
    모델 화면에 표시되고 상태가 **Running**입니다.

![](./media/image48.png)

9.  배포가 완료되면 상태가 **Completed**으로 변경됩니다.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

> 이제 예측을 생성하는 운영 웹 서비스가 있습니다.

## **연습 6: 리소스를 삭제하기**

### **작업 1: Endpoint를 삭제하기**

1.  AML Studio의 왼쪽 창에서 **Endpoints**를 클릭하세요.

2.  Endpoint **my-automl-deploy**를 선택하고 **Delete**를 클릭하세요.

![](./media/image50.png)

3.  Delete real-time endpoint 대화상자에서 **Delete**를 선택하세요.

4.  엔드포인트가 삭제되면 성공 메시지를 받아야 합니다.

**요약**

이 실습에서는 Azure Machine Learning 스튜디오에서 코드 AutoML을 사용하여
분류 모델을 학습하고 최상의 모델을 웹 서비스로 배포하는 방법을
알아보았습니다.
