# 실습 06 – 하드웨어 데이터셋이 가장 적합한 Regression 모델을 학습하기

**목표**

이 실습에서는 AutoML을 사용하여 Regression 모델을 학습시키는 방법을
살펴봅니다. Hardware Performance 데이터세트를 사용하요 유추 시나리오에서
사용할 모델을 학습하고 배포합니다. Regression의 목표는 하드웨어 부품의
특정 조합의 성능을 예측하는 것입니다.

예상 소요 시간 – 60분

# 연습 0: 환경을 준비하기

### **작업 1: AML Workspace를 시작하기**

1.  아직 로그인하지 않은 경우Azure portal
    +++[**https://portal.azure.com**](https://portal.azure.com)+++의
    로그인하세요.

2.  Azure portal menu에서 **All resources**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Azure Machine Learning Workspace
    ([**Azuemlws@lab.LabInstanceId**](mailto:Azuemlws@lab.LabInstanceId))를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  **Launch studio**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Compute 인스턴스를 생성하기 위해 왼쪽 창에서 **Compute**를
    선택하세요. **+ New**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

6.  다음 세부 정보를 입력하고 **Review + Create**를 클릭하세요.

- Compute name - +++**auto-compute**+++

- Virtual machine type – **CPU**

- Virtual Machine – **Standard E4ds_v4**

![](./media/image5.png)

7.  Compute 인스턴스를 생성하기 위해 **Create**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

### **작업 2: AML Workspace에 notebook를 업로드하기** 

1.  왼쪽 창에서 **Notebooks**를 클릭하세요. **Users** 아래의
    **username**옆에 있는 세 개의 점을 클릭하고 **Upload folder**를
    선택하세요.

![](./media/image7.png)

2.  Click to browse를 선택하고folder(s)를 선택하고
    **automl-regression-task-hardware-performance** 폴더를 선택하려면
    **C:\Labfiles**로 이동하고**Upload**를 클릭하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image8.png)

3.  Upload 3 files to this site?라는 팝업이 나타나면 **Upload**를
    클릭하세요.

![A picture containing text, screenshot, display, font Description
automatically generated](./media/image9.png)

4.  **I trust contents of these files** 확인란을 선택하고 **Upload**를
    선택하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

5.  Notebook (the .ipynb file),
    **automl-regression-task-hardware-performance**를 여세요. Notebook는
    이전에 생성한 컴퓨팅에 자동으로 연결됩니다.

![](./media/image11.png)

## **연습 1: Azure Machine Learning Workspace와 연결하기**

### **작업 1: 필요한 라이브러리 가져오기**

1.  **1.1 필요한 라이브러리 가져오기**에서 셀의 첫 번째 셀을 실행하여
    셀의 왼쪽 상단에 있는 Run cell 버튼을 클릭하여 이 실습 실행에 필요한
    라이브러리를 가져오세요.

2.  셀의 왼쪽 아래에 있는 눈금 기호를 찾아 실행이 성공했는지 확인하세요.

![A screenshot of a computer screen Description automatically generated
with low confidence](./media/image12.png)

### **작업 2: 작업 공건 세부 정보를 구성하고 작업 공간에 대한 핸들을 가져오기**

1.  **1.2. 작업 공건 세부 정보를 구성하고 작업 공간에 대한 핸들을
    가져오기**의 아래 셀에서 다음을 바꾸세요

- SUBSCRIPTION_ID - +++**@lab.CloudSubscription.Id**+++

- RESOURCE_GROUP – **Your assigned Resourcegroup name**

- AML_WORKSPACE_NAME – +++**Azuremlws@lab.LabInstanceId**+++

2.  셀의 왼쪽 상단에 있는 Run cell 옵션을 클릭하고 실행이 성공하면 왼쪽
    하단에 눈금 기호가 표시되는지 확인하세요.

3.  **Found the config file in : /config.json**라는 출력이 셀 아래에
    표시됩니다.

![](./media/image13.png)

### **작업 3: Azure ML Workspace 정보를 표시하기**

1.  다음 셀을 실행하세요 (Azure ML Workspace 정보를 표시하기 아래의 셀).

2.  셀 아래에 출력으로 나열되는 작업 공간, 구독, 위치 및 리소스 그룹의
    세부 정보가 모두 올바른지 확인하세요.

![A screenshot of a computer program Description automatically
generated](./media/image14.png)

## **연습 2: 입력 학습 데이터가 있는 MLTable**

### **작업1:** **MLTable 데이터 입력을 생성하기**

1.  다음 셀을 실행하세요 (**2.1 MLTable 데이터 입력을 생성하기** 아래).

2.  실행이 성공했는지 확인하세요.

![A picture containing text, font, screenshot, software Description
automatically generated](./media/image15.png)

## **연습 3: AutoML Regression 학습 작업을 구성하고 실행하기**

1.  **4.1 AutoML Regression 학습 작업을 구성하고 실행하기**를 하나씩
    실행하고 각 셀이 성공적으로 실행되는지 확인하세요

2.  **4.2 명령을 실행하기** 아래의 셀은AutoML 작업을 제출합니다.

![A screenshot of a computer program Description automatically
generated](./media/image16.png)

3.  왼쪽 창에서 **Jobs**을 클릭하고 **Running** 상태인 실험을 선택하여
    작업 상태를 확인할 수 있습니다.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

**참고:** 완료하는 데 약 10-15분이 걸립니다.

4.  Notebook의 다음 셀은 AutoML 작업이 완료될 때까지 기다리세요.

5.  그것을 실행하고 실행이 완료 될 때까지 기다렸다가 다음 셀로
    이동하세요.

![](./media/image19.png)

6.  실행이 완료된 후에만 다음 단계를 진행하세요.

![](./media/image20.png)

7.  URL과 작업 이름을 검색하는 다음 2 개의 셀을 하나씩 실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

## **연습 4: 최상의 평가판 검색하기 (Best Model의 평가판/실행)**

1.  이 연습에서 첫 번째 셀 위에 셀을 추가하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image22.png)

2.  아래 코드를 복사하세요. **Run cell**을 클릭하세요.

> **%pip install azureml-mlflow**
>
> **%pip install mlflow**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image23.png)

3.  다음 3개의 셀을 하나씩 계속 실행하여 각 코드와 해당 출력을
    분석합니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image24.png)

4.  **Get the parent run**를 위해 다음 셀을 실행하세요.

![A screenshot of a computer program Description automatically generated
with low confidence](./media/image25.png)

5.  **Print the parent tags**를 위해 다음 셀을 실행하세요.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image26.png)

6.  **Get the AutoML best child run**를 위해 다음 셀을 실행하세요.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image27.png)

7.  **Get the best model run’s metrics**를 위해 다음 셀을 실행하세요.

![A screenshot of a computer error Description automatically generated
with low confidence](./media/image28.png)

8.  **Download the best model locally**를 위해 다음 3개의 셀을
    실행하세요.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image29.png)

## **연습 5: 최상 모델을 등록하고 배포하기**

### **작업 1: 관리형 온라인 엔드포인트를 생성하기**

1.  이 작업에서 처음 2개의 셀을 실행하세요.

![](./media/image30.png)

2.  코드로 다음 셀을 실행하세요,

**ml_client.begin_create_or_update(endpoint).result()**

이렇게 하면 **regression-\<Currentdate&time\>**이라는 온라인
엔드포인트가 생성됩니다.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image31.png)

3.  **Endpoint "regression-\<Currentdate&time\>" update completed**다는
    알림을 확인하세요

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

### **작업 2: 최상 모델을 등록하고 배포하기**

1.  Register best model 아래의 첫 번째 셀을 실행하고 -\> **Register
    model**을 배포하여 **hardware-performance-model**이라는 모델을
    등록하세요.

2.  실행이 성공하면등록된 모델 ID를 검색하려면 다음 셀을 실행하세요.

> ![](./media/image33.png)

### **작업 3: 배포하기**

1.  Deploy 아래의 첫 번째 셀에서 값 **instance_type**를
    **Standard_E4s_v3**로 바꾸세요.

2.  셀을 실행하여 최상의 모델을 배포하세요.

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

3.  배포를 생성하려면 다음 셀을 실행하세요.

![A picture containing text, screenshot, line, font Description
automatically generated](./media/image35.png)

4.  **This will take around 40 minutes to complete**. **Endpoints**
    아래에서 상태를 확인할 수도 있습니다 (왼쪽 창에서 **Endpoints**를
    선택한 후 이전에 배포한 **regression-XXXXXXX** 엔드포인트를 클릭).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

5.  실행이 완료되고 배포가 성공하면 셀은 배포 세부 정보를 출력합니다.

![](./media/image37.png)

6.  Endpoints 세부 정보 페이지에서 배포 상태가 **Succeeded**가 됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

7.  배포를 위해 100% 트래픽을 사용하려면 노트북에서 다음 셀을
    실행하세요.

![A screenshot of a computer Description automatically generated with
low confidence](./media/image39.png)

8.  Endpoints 세부 정보 페이지에서 실시간 트래픽 할당이 100%인지
    확인하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

## **연습 6: 배포를 테스트하기**

1.  Test the deployment 아래의 셀을 실행하세요.

2.  출력을 확인하세요.

![](./media/image41.png)

3.  나머지 셀을 따라 실행하고 endpoint를 삭제하세요.

![](./media/image42.png)

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image43.png)

4.  Endpoints 실습 아래에서 엔드포인트의 상태를 확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

**요약**

이 실습에서 다음을 배웠습니다

- Python SDK에서 AML 작업 영역에 연결하기

- 'regression()' 팩토리 함수를 사용하여 AutoML 회귀 작업을 생성하기

- AutoML 회귀 학습 작업을 제출/실행하여 AmlCompute를 사용하여 모델
  학습하기

- 모델을 얻고 이를 사용하여 예측을 점수화하기
