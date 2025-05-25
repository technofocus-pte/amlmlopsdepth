# **실습 05 - Azure Machine Learning 스튜디오에서 코드 없는 Automated Machine Learning을 사용하여 수요 예측하기**

**목표**

이 실습에서는 Azure Machine Learning 스튜디오에서 자동화된 Machine
Learning을 사용하여 코드를 한 줄도 작성하지 않고 시계열 예측 모델을
만드는 방법을 알아봅니다. 이 모델은 자전거 공유 서비스에 대한 임대
수요를 예측합니다.

이 실습에서는 코드를 작성하지 않습니다. Studio 인터페이스를 사용하여
교육을 수행합니다..

예상 소요 시간 – 60분

## **연습 1: 환경을 준비하기**

### **작업 1: AML Workspace를 시작하기**

1.  아직 로그인하지 않은 경우Azure Portal에
    +++**https://portal.azure.com**+++로 로그인하세요.

2.  Azure portal 메뉴에서**All resources**를 선택하세요.

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

## **연습 2: Automated ML job을 생성하기**

1.  Azure Machine Learning Studio에서 왼쪽 창에서 **Author** 섹션의
    **Automated ML**를 클릭하세요.

2.  **+ New Automated ML job**를 선택하세요.

![](./media/image4.png)

### **작업 1: 데이터 자산을 생성하기**

1.  실험 이름을 +++**experiment_forecast**+++로 입력하고 다른 기본값을
    수락하고 **Next**을 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

2.  **Select task type**를 **Time series forecasting**로 선택하고 **+
    Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  Create data asset 페이지에서 다음 세부 정보를 입력하세요.

    1.  Name – +++**bikedata**+++

    2.  Type – Tabular

> **Next**를 클릭하세요.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image7.png)

4.  **Data source** 창에서 **From local files**를 선택하고 **Next**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

5.  **Destination storage type**에서 workspaceblob를 선택하고 **Next**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

6.  On the File or folder selection에서 **Upload files**를 선택하고
    **C:\Labfiles** 폴더에서 **bike-no.csv**를 선택하고 **Next**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

7.  **Settings and preview** 양식이 다음과 같이 채워져 있는지 확인하고
    **Next**을 선택하세요.

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image11.png)

8.  **Schema **양식을 사용하면 이 실험에 대한 데이터를 추가로 구성할 수
    있습니다. 이 예에서는 **toggle switch**를 꺼짐 상태로 선택하세요. 

    1.  **casual** 및

    2.  **registered** 열

> **Next**를 클릭하세요.

이 열은 **cnt** 열의 분석이므로 포함하지 않습니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

9.  On the **Review** 양식에서 정보를 확인하고 데이터 자산을 생성을
    완료하려면 **Create**를 클릭하세요.

> ![](./media/image13.png)

10. **Create a new Automated ML job page**로 돌아가고 데이터 자산 생성에
    대한 **success** 메시지가 표시됩니다.

11. 새로 생성된 **bikedata**를 선택하고 **Next**를 클릭하세요.

> **참고:** bikedata가 표시되지 않는 경우 데이터 자산 창을 **Refresh**.
>
> ![](./media/image14.png)

### **작업 2: 작업을 구성하기**

1.  **Task settings** 페이지에서 다음 세부 정보를 제공하고 **View
    additional configuration settings**를 선택하세요.

> Target column – **cnt(Integer)**
>
> Time column **– date (Date)**
>
> **Deselect** **Autodetect forecast horizon** 하고 값을 +++**14**+++로
> 제공하세요.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image15.png)

2.  Additional configuration 창에서 다음 세부 정보를 입력하고**Save**를
    클릭하세요.

- Primary metric - **Normalized root mean squared error**

- Explain best model – **Enable**

- Blocked algorithms - **Extreme Random Trees**

> Additional forecasting 설정을 확장하세요

- Autodetect Forecast target lags – **UnSelected**

- Autodetect Target rolling window size – **UnSelected**

> ![A screenshot of a computer Description automatically
> generated](./media/image16.png)

3.  **Limits**를 선택하고 **Experiment timeout(minutes)** 필드에 대한
    +++**60**+++를 입력하세요.

![A screenshot of a test AI-generated content may be
incorrect.](./media/image17.png)

4.  **Validate and test**에서 아래 값을 선택한 후**Next**를 선택하세요.

> Validation type – **k-fold cross-validation**
>
> Number of cross validations – **5**
>
> ![A screenshot of a computer Description automatically
> generated](./media/image18.png)

5.  **automl-compute** (이전 실습에서 생성한)를 선택하세요. **Next**를
    클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image19.png)

6.  세부 정보를 검토하고 **Submit training job**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

7.  상태 페이지에는 초기 상태가 **Running**으로 표시됩니다. 상태를 알기
    위해 페이지를 계속 새로고침하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

8.  교육이 완료되면 상태가 **Completed**으로 변경됩니다.

**참고:** 교육은 완료하는 데 약 30-45분이 소요됩니다.

## **연습 3: 모델을 살펴보기**

1.  **Models **탭으로 이동하여 테스트된 알고리즘(모델)을 확인하세요.
    기본적으로 모델은 완료될 때 메트릭 점수에 따라 정렬됩니다.

2.  이 자습서에서는 선택한 **Normalized root mean squared
    error** 메트릭을 기반으로 가장 높은 점수를 받은 모델이 목록의 맨
    위에 있습니다.

3.  모든 실험 모델이 완료될 때까지 기다리는 동안 완료된 모델의
    **Algorithm name**을 선택하여 성능 세부 정보를 탐색하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image22.png)

4.  **Overview**를 클릭하고 세부 정보를 보세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image23.png)

5.  **Metrics** 탭을 클릭하고 세부 정보를 탐색하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image24.png)
>
> **중요:** 이 교육이 완료되는 동안 다음 실습을 계속 실행하세요. 교육이
> 완료되면 여기에서 이 실습으로 다시 시작하세요.
>
> ![A screenshot of a computer Description automatically
> generated](./media/image25.png)

## **연습 4: 최상의 모델을 식별하기**

Azure Machine Learning 스튜디오의 Automated Machine Learning을 사용하면
몇 가지 단계를 통해 최상의 모델을 웹 서비스로 배포할 수 있습니다. 배포는
새로운 데이터를 예측하고 잠재적인 기회 영역을 식별할 수 있도록 모델을
통합하는 것입니다.

1.  작업이 완료되면 화면 맨 위에 있는 **job name**을 선택하여 상위 작업
    페이지로 다시 이동하세요.

![](./media/image26.png)

2.  **Best model summary** 섹션에서 이 실험의 컨텍스트에서 가장 적합한
    모델은 **Normalized root mean squared error metric**을 기반으로
    선택됩니다.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

3.  세부 정보를 탐색하려면 알고리즘 이름을 클릭하여 여세요.

4.  모델을 웹 서비스로 배포할 수도 있습니다.

**요약**

이 실습에서는 Azure Machine Learning 스튜디오에서 자동화된 ML을 사용하여
자전거 공유 대여 수요를 예측하는 시계열 예측 모델을 생성했습니다.
