# **실습 09 - GitHub를 사용하여 MLOps를 설정하기**

**목표:**

Azure Machine Learning을 사용하면 **GitHub Actions**와 통합하여 기계
학습 수명 주기를 자동화할 수 있습니다.

이 실습에서는 Azure Machine Learning을 사용하여 선형 회귀를 실행하여
NYC의 택시 요금을 예측하는 end-to-end MLOps 파이프라인을 설정하는 방법을
알아볼 것입니다. 파이프라인은 각각 다른 기능을 제공하는 구성 요소로
구성되며, 작업 공간에 등록하고, 버전을 관리하고, 다양한 입력 및 출력과
함께 재사용할 수 있습니다.

예상 소요 시간: 60분

Azure Machine Learning의 MLOps 단계에 있습니다.

![](./media/image1.png)

## **연습 1: Azure 리소스를 준비하기**

### **작업 1: Azure Machine Learning 작업 공간을 생성하기**

1.  아직 로그인하지 않은 경우 +++<https://portal.azure.com>+++ 에서
    Azure portal에서 로그인하세요.

2.  Azure portal 홈페이지에서 **+ Create a resource**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  **Create a resource** 페이지에서 검색 바를 사용하여 +++Azure Machine
    Learning+++를 찾으세요

4.  **Machine Learning**를 선택하세요.

> ![](./media/image3.png)

5.  **Marketplace**에서 **Create dropdown**를 클릭하고 **Azure Machine
    Learning**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

6.  새 작업 공간을 구성하려면 다음 정보를 제공하세요:

    - **Subscription**: **할당된 Azure subscription**를 선택하세요

    - **Resource group**: **할당된 Resource Group**를 선택하세요

> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstance.Id**+++

- **Region**: 가장 가까운 지역을 선택하세요 **(**여기 **North Central
  US**가 선택됩니다)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstance.Id+++**

![A screenshot of a computer Description automatically
generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  Validation가 통과되면 **Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

8.  새 작업 공간을 보기 위해 **Go to resource**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**에서
    **Work with your model in Azure Machine Learning studio**의 **Launch
    studio**를 선택하세요.

![A screenshot of a software update Description automatically
generated](./media/image9.png)

### **작업 2: 컴퓨팅을 생성하기**

1.  Azure Machine Learning Studio가 열리면 왼쪽 창의 **Manage**에서
    **Compute**을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

2.  **Compute clusters** 탭을 선택하고 **+ New**를 클릭하세요.

![](./media/image11.png)

3.  **Create compute cluster** 화면에서 다음 세부 정보를 입력하세요.

    1.  Location – Azure Machine Learning 작업 공간을 생성한
        **Region**을 선택하세요

    2.  Virtual machine tier – **Dedicated**

    3.  Virtual machine type – **CPU**

    4.  Virtual machine size – **Standard_E4s_v3**를 선택하세요 **(**VM
        크기를 찾으려면 Select from all options을 선택하세요.**)**

> **Next**를 클릭하세요.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

4.  **Advanced Settings** 페이지에서 다음 세부 정보를 입력하세요.

&nbsp;

1.  Compute name – +++**cpu-cluster@lab.LabInstanceId**+++

2.  Minimum number of nodes – 0

3.  Maximum number of nodes – 1

> **Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

**참고:** 컴퓨팅은 Running 상태가 되는 데 약 10분이 걸립니다.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

## **연습 2: Azure 리소스를 검색하기**

1.  Azure Portal(<https://portal.azure.com>)에서 리소스 그룹을 열고 다음
    리소스의 이름을 기록해 두세요,

    1.  **Azure Machine Learning Workspace**

    2.  **Application Insights**

    3.  **Key Vault**

    4.  **Container Registry**

    5.  **Storage account**

> 그리고 메모장에 로컬로 저장하여 구성 파일에서 업데이트하세요.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

## **연습 3: GitHub 계정 및 리소스를 준비하기**

**참고:** GitHub 계정이 아직 없는 경우 여기에서 계정을 생성하세요
+++**https://github.com/**+++ -\> **Signup**.

### **작업 2: repo mlops 데모를 GitHub 계정에 Fork하기**

1.  브라우저를 열고 다음 링크를 입력하세요-
    +++<https://github.com/getazureready/mlops-v2-gha-demo>+++

2.  오른쪽 상단의 **Fork**를 클릭하세요.

![A screenshot of a chat Description automatically generated with medium
confidence](./media/image16.png)

3.  **Create a new fork** 페이지를 열립니다. **Create fork**를
    클릭하세요**.**

![A screenshot of a computer Description automatically
generated](./media/image17.png)

4.  GitHub 프로젝트에서 **Settings**를 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.png)

5.  **Secrets and variables**에서 **Actions**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

6.  **New repository secret**를 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image20.png)

7.  이 비밀의 이름을 **+++AZURE_CREDENTIALS+++로** 지정하고 아래
    **Service Principal**출력을 비밀의 내용으로 붙여넣습니다. 이 Service
    Principal는 사용자를 위해 미리 생성했습니다. **Add secret**를
    선택하세요.

> {
>
> "clientId": "+++@lab .Variable(spAppId)+++",
>
>   "clientSecret": "+++@lab .Variable(spClientSecret)+++",
>
>   "subscriptionId": "+++@lab.CloudSubscription.Id+++",
>
>   "tenantId": "+++@lab.CloudSubscription.TenantId+++",
>
>   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
>
>   "resourceManagerEndpointUrl": "https://management.azure.com/",
>
>   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
>
>   "sqlManagementEndpointUrl":
> "https://management.core.windows.net:8443/",
>
>   "galleryEndpointUrl": "https://gallery.azure.com/",
>
>   "managementEndpointUrl": "https://management.core.windows.net/"
>
> }
>
> ![A screen shot of a computer Description automatically generated with
> low confidence](./media/image21.png)

8.  추가된 시크릿 **AZURE_CREDENTIALS**는 **Repository secrets** 아래에
    표시됩니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

9.  **New repository secret**를 클릭하세요.

![](./media/image23.png)

10. 다음 세부 정보를 입력하세요.

    1.  Name – +++ARM_CLIENT_ID+++

    2.  Secret – +++@lab .Variable(spAppId)+++

> ![A screenshot of a computer secret Description automatically
> generated with low confidence](./media/image24.png)

11. 다음 값에 대해 9단계와 10단계를 반복하여 추가 GitHub 비밀을
    생성하세요.

    - +++ARM_CLIENT_SECRET+++ - +++@lab .Variable(spClientSecret)+++

    - +++ARM_SUBSCRIPTION_ID+++ - +++@lab.CloudSubscription.Id+++

    - +++ARM_TENANT_ID+++ - +++@lab.CloudSubscription.TenantId+++

## **연습 4: Machine Learning 환경 매개변수 구성하기**

1.  비밀 페이지에서 왼쪽 상단의 GitHub ID 옆에 있는
    **mlops-v2-gha-demo**를 클릭하여 리포지토리 페이지로 이동하세요.

![](./media/image25.png)

2.  루트에서 **config-infra-prod.yml** 파일을 선택하세요. **Edit** (연필
    아이콘)를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

3.  값을 변경하세요,

    1.  **Namespace** – **mlopsliteXX** (XX를 임의의 숫자로 바꾸기)

    2.  **Postfix** – **c**

    3.  **location** – **Same as your workspace region**

> **Commit changes**를 클릭하세요.
>
> **For pipeline reference** 섹션에서 Azure Resources의 **values**을
> 연습 2에서 가져와서 저장한 값으로 바꾸세요.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image27.png)

4.  Commit changes 창에서 **Commit changes**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

5.  **.github/workflows**에서
    **deploy-model-training-pipeline-classical.yml**를 여세요. **Edit**
    (연필 아이콘)를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

6.  파일 내용에서 **Size** 값을 다음으로 바꾸세요.
    **+++Standard_E4s_v3+++**

**Commit changes**를 선택하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image30.png)

7.  **mlops/azureml/deploy/online**에서 **online-deployment.yml** 파일을
    여세요. **Edit** (연필 아이콘)를 클릭하세요.

![](./media/image31.png)

8.  **instance_type**의 값을 **+++Standard_E4s_v3+++**로 바꾸세요.
    **Commit changes**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image32.png)

9.  **.github/workflows**에서 **tf-gha-deploy-infra.yml** 파일을 여세요.
    **Edit**을 클릭하고 9줄과 14줄에서 Azure를 +++CoursesTF+++로
    바꾸세요.

**Commit changes**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image33.png)

10. 상단 메뉴 바에서 **Actions**를 선택하세요. **I understand my
    workflows, go ahead and enable them**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

11. 그러면 프로젝트와 연결된 사전 정의된 GitHub 워크플로가 표시됩니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image35.png)

## **연습 5: Machine Learning 인프라를 배포하기**

1.  **tf-gha-deploy-infra.yml**를 선택하세요. **Runworkflow**를
    클릭하세요.

다음을 선택

- Branch – **main**

**Run workflow**를 선택하세요

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image36.png)

2.  이렇게 하면 GitHub Actions 및 Terraform을 사용하여 Machine Learning
    인프라가 배포됩니다.

3.  작업 상태를 추적하고 실행이 성공했는지 확인하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image37.png)

**참고:** 이 워크플로를 완료하는 데 약 5분이 걸립니다.

## **연습 6: 모델 학습 파이프라인을 배포하기**

다음으로, 모델 학습 파이프라인을 새 Machine Learning 작업 영역에
배포합니다.

이 파이프라인은 컴퓨팅 클러스터 인스턴스를 만들고, 필요한 Docker 이미지
및 python 패키지를 정의하는 학습 환경을 등록하고, 학습 데이터 세트를
등록한 다음, 마지막 섹션에 설명된 학습 파이프라인을 시작합니다. 

1.  **tf-gha-deploy-infra.yml** w워크플로우 페이지에서 **Actions**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

2.  그러면 프로젝트와 연결된 사전 정의된 GitHub 워크플로가 표시됩니다.
    목록에서 **deploy-model-training-pipeline**을 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image39.png)

3.  **Run workflow** -\> **Run workflow**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image40.png)

4.  진행 상황을 추적하려면 방금 시작된 파이프라인을 클릭하세요.

![A picture containing text, software, web page, font Description
automatically generated](./media/image41.png)

5.  이 파이프라인을 완료하는 데 약 15분에서 45분 정도 걸립니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image42.png)

6.  성공적인 파이프라인 실행의 스크린샷은 다음과 같습니다.

![](./media/image43.png)

7.  이 실행은 Machine Learning 작업 영역에 모델을 등록합니다.

8.  <https://ml.azure.com/> 에서 AzureMachineLearning 스튜디오에
    로그인하고 왼쪽 창에서 **Data**를 클릭하여 **taxi-data**가
    추가되었는지 확인하세요합니다. 이 작업은 워크플로의
    **register-dataset** 작업의 일부로 수행됩니다.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

9.  왼쪽 창에서 **Jobs**을 클릭하고 **taxi-fare-training**을 선택하세요.
    이는 워크플로의 **run-pipeline** 작업에서 실행됩니다.

![](./media/image45.png)

10. 최신 실행의 표시 이름을 선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image46.png)

11. 교육과 관련된 단계 및 세부 정보를 살펴보세요.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

Machine Learning 작업 영역에 학습된 모델이 등록되면 채점을 위해 모델을
배포할 준비가 된 것입니다.

**요약**

이 실습에서는 Azure Machine Learning을 사용하여 데이터를 준비하고 모델
학습 파이프라인을 새 Machine Learning 작업 공간에 배포한 end-to-end
MLOps 파이프라인을 설정하는 방법을 알아보았습니다.
