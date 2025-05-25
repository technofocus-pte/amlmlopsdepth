# **실습 10 - Responsible AI 대시보드를 사용하여 기계 학습 모델의 성능을 개선하기**

**목표**

이 실습에서는 Responsible AI 대시보드를 사용하여 머신 러닝 모델을
디버그하여 모델의 성능을 보다 공정하고 포괄적이며 안전하고 신뢰할 수
있고 투명하게 개선하는 방법에 대한 실습 학습을 제공합니다. 

이 실습에서는 Azure Responsible AI (RAI) 대시보드의 **Model Overview**
섹션을 사용하는 방법을 살펴봅니다. 오류 분석 랩에서 생성된 코호트를
사용하여 한 코호트와 다른 코호트에서 모델의 동작이 더 나은 이유를
조사합니다.

예상 소요 시간 – 60 분

## **연습 1: 리소스를 준비하기**

### 작업 1: 이 실습에 대한 리포지토리를 복제하기

1.  브라우저에서 Azure Portal https://portal.azure.com에 로그인하세요

2.  Azure Portal에서 클라우드 셸 아이콘을 클릭하여 **cloud shell**을
    여세요.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  Azure Cloud Shell 명령 프롬프트에서 아래 명령을 실행하여 **Diabetes
    Hospital Readmission** 프로젝트 github 리포지토리를 복제하세요.

> **+++git clone
> <https://github.com/getazureready/RAI-Diabetes-Hospital-Readmission-classification>**+++
>
> 이렇게 하면 저장소의 내용이 로컬로 복제됩니다.
>
> ![](./media/image2.png)

4.  아래 명령을 실행하여 프로젝트 디렉토리로 변경하세요.

**+++cd RAI-Diabetes-Hospital-Readmission-classification+++**

### 작업 2: Azure CLI를 사용하여 로그인하기

1.  Cloud shell에서 아래 명령을 실행하세요.

**az login**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image3.png)

2.  콘솔에서 URL을 열고 브라우저에 코드를 입력하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)

3.  **Azure login** 자격 증명을 선택하세요.

> ![A screenshot of a phone Description automatically generated with
> medium confidence](./media/image5.png)

4.  **Continue**를 클릭하세요.

> ![A screenshot of a computer error Description automatically generated
> with medium confidence](./media/image6.png)

5.  브라우저를 닫고 Azure Portal로 돌아가세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)

6.  로그인 세부 정보는 Cloud Shell에 표시됩니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image8.png)

7.  환경 기본값을 **assigned Resource group**으로 설정하세요.

**+++az configure --defaults group="\<resource-group-name\>"
workspace="Azuremlws@lab.LabInstance.Id"+++**

![](./media/image9.png)

## **연습 2: 모델 학습 및 RAI 대시보드 생성을 위한 작업을 실행하기**

1.  아래 명령을 실행하여 **training dataset**를 Azure Machine Learning
    작업 영역에 등록하세요.

> **az ml data create -f cloud/train_data.yml**

데이터 자산이 생성되고 세부 정보가 cloud shell에 표시됩니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image10.png)

2.  아래 명령을 실행하여 **testing dataset**를 Azure Machine Learning
    작업 공간에 등록하세요.

> **az ml data create -f cloud/test_data.yml**

![](./media/image11.png)

3.  작업을 실행하기 위해**compute instance**를 생성하세요. 실행이 끝날
    때 나중에 사용할 컴퓨팅 *이름 (예: **compute-xxxxxxxxxxxx***)을
    복사하세요.

- **Compute**를 **create** 하기 위해 다음 명령을 실행하세요.

**az ml compute create --name compute@lab.LabInstance.Id --type
computeinstance --size Standard_E4ds_v4**

![A screen shot of a computer Description automatically generated with
medium confidence](./media/image12.png)

4.  Cloud Shell 메뉴에서 파일을 편집하기 위해 **Open editor** **{ }**
    창을 클릭하세요.

> ![Open editor](./media/image13.png)

5.  디렉토리를 확장하려면
    **RAI-Diabetes-Hospital-Readmission-classification** 폴더를
    클릭하세요.

![Expand directory](./media/image14.png)

6.  **cloud/training_job.yml** 파일로 이동하세요. 컴퓨팅 이름의 자리
    표시자를 이전에 복사한 **compute instance name**로 바꾸세요.

![Training job update](./media/image15.png)

7.  파일의 아무 곳이나 마우스 오른쪽 버튼으로 클릭한 후 **Save**옵션을
    선택하여 파일을 저장하세요. 

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image16.png)

8.  **cloud/rai_dashboard_pipeline.yml** 파일로 이동하세요. 컴퓨팅
    이름의 자리 표시자를 이전에 복사한 **compute instance name**로
    업데이트하세요.

![Rai pipeline update](./media/image17.png)

9.  파일의 아무 곳이나 마우스 오른쪽 버튼으로 클릭한 후 **Save**옵션을
    선택하여 파일을 저장하세요.

10. 파일의 아무 곳이나 마우스 오른쪽 버튼으로 클릭한 후 **Quit** 옵션을
    선택하여 편집기 창을 닫으세요.

![A screenshot of a computer program Description automatically generated
with medium confidence](./media/image18.png)

11. Cloud Shell 명령 프롬프트로 돌아가서 모델을 학습시키기 위한 작업을
    제출하세요. 학습 중에 작업이 실행 상태를 **Completed**으로
    업데이트할 때까지 기다리세요. 그렇게 하려면 아래 코드 블록을
    복사하세요.

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
> **참고:** 이 스크립트가 제대로 붙여넣어지지 않으면 수동으로 복사하여
> 붙여넣으세요
>
> **참고:** 이 스크립트를 실행하는 데 약 3-5분 정도 걸립니다.
>
> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image19.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image20.png)

12. **Azure Machine Learning Studio (**<https://ml.azure.com/>**)** -\>
    **Jobs**에서 Running 작업의 상태를 확인할 수 있습니다

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image21.png)

13. 학습 작업이 성공적으로 완료되면 Azure Machine Learning 작업 영역에
    모델을 등록하세요. 그렇게 하려면 아래 명령을 실행하세요.

**az ml model create --name rai_hospital_model --path
"azureml://jobs/$run_id/outputs/model_output" --type mlflow_model**

> 이 명령은 모델을 AML 작업 영역에 등록하고 아래 스크린샷과 같이
> 클라우드 셸에 세부 정보를 제공합니다.
>
> ![A picture containing text, screenshot, software, multimedia software
> Description automatically generated](./media/image22.png)
>
> ![A picture containing text, font, screenshot Description
> automatically generated](./media/image23.png)

14. **RAI dashboard**를 생성하려면 작업 파이프라인을 제출하세요. 그렇게
    하려면 아래 명령을 실행하세요.

az ml job create --file cloud/rai_dashboard_pipeline.yml

이 명령은 작업을 제출하고 Cloud Shell은 파이프라인의 초기 단계인
**Preparing** 상태로 채워집니다.

![A picture containing text, screenshot, software Description
automatically generated](./media/image24.png)

![A picture containing text, screenshot, software, font Description
automatically generated](./media/image25.png)

15. RAI 대시보드를 생성하기 위한 파이프라인 작업을 모니터링하려면
    <https://ml.azure.com/>에서 **Azure Machine Learning studio**에
    로그인하세요.

16. **Pipelines**를 선택하세요. RAI 대시보드를 생성하는 파이프라인
    작업의 진행 상황을 보려면 작업 **Display name**을 클릭하세요

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image26.png)

17. 실험은 **Running** 상태가 됩니다.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

18. 완료되고 RAI 대시보드가 생성해지면 상태가 **Completed**으로
    변경됩니다.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image28.png)

19. 왼쪽 탐색에서 **Models** 탭을 클릭하세요**.** 모델 이름을 클릭하여
    세부 정보 페이지를 엽니다.

> ![](./media/image29.png)

20. 상단 메뉴에서 **Responsible AI** 옵션을 선택하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image30.png)

21. 이제 **RAI dashboard**를 사용할 준비가 되었습니다.

## **연습 3: 오류 분석:**

RAI 대시보드의 오류 분석 섹션은 모델의 오류율에 기여하는 피처 그룹의
오류 분포를 제공하는 데 도움이 됩니다. 오류는 서로 다른 데이터 하위
그룹에 균등하게 분포되지 않는 경우가 많으며 오류 분석은 오류율이 가장
높은 기능을 식별하는 데 도움이 됩니다.

### 작업 1: 모델 오류 찾기:

이 작업에서는 오류 분석을 사용하여 학습된 모델에서 오류를 찾아 오류가
있는 위치를 식별하는 방법을 살펴보겠습니다. 또한 데이터 코호트를
생성하여 모델이 일부 코호트에서는 성능이 저조한 이유를 조사하고 다른
코호트에서는 그렇지 않은 이유를 조사하는 방법을 알아봅니다.

1.  **Diabetes Hospital Readmission** 이름을 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image31.png)

2.  **Compute**를 선택하세요.

![](./media/image32.png)

#### **작업 1.1: 오류가 가장 높은 트리 경로에 대한 코호트를 식별하고 생성하기**

분석을 시작하기 위해 루트 노드에서 모델을 평가하는 동안 총 994개의
테스트 데이터 중 168개의 잘못된 예측이 발견되었음을 확인할 수 있습니다.

1.  오류 수가 가장 많은 트리 경로를 찾으세요. 노드의 빨간색 음영이
    어두울수록 오류율이 높습니다.

2.  우리의 경우 가장 어두운 빨간색의 나무 경로는 오른쪽 하단에서 두
    번째에있는 잎 노드입니다.

![](./media/image33.png)

3.  이 **node**를 **Double click**하여 노드로 이어지는 **entire path**를
    선택하세요. 이렇게 하면 경로가 강조 표시되고 경로의 각 노드에 대한
    피쳐 조건이 표시됩니다.

4.  Error Analysis 섹션의 오른쪽 상단에 있는 **Save as a new cohort**
    버튼을 클릭하여 선택한 경로에서 코호트를 생성하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image34.png)

5.  **Cohort name**을 **+++Err: Prior_Inpatient \>0; Num_meds \>11.50 &
    \<= 21.50+++**로 입력하세요

**Click on Save.**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image35.png)

#### **작업 1.2: 오류가 가장 적은 트리 경로에 대한 코호트를 식별하고 생성하기**

대조를 위해 오류 수가 가장 적은 트리 경로로 다른 코호트를 만들어 모델이
한 코호트와 다른 코호트에서 잘 수행되는 이유에 대한 통찰력을 얻을 수
있는지 확인합니다.  트리의 맨 왼쪽에 있는 피쳐
조건이**num_lab_procedures ≤56.50 leaf node**는 오류가 가장 적은 트리의
경로입니다.

1.  노드를 **Double-click**하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image36.png)

2.  **Save as a new cohort**를 클릭하세요. 이 데이터세트의 **Filter**는:
    num_lab_procedures \<= 56.50, number_diagnoses \<= 6.50,
    prior_inpatient \<= 0.00입니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image37.png)

3.  코호트를 **Name**하고: **+++Prior_Inpatient = 0; num_diagnoses \<=
    6.50; lab_procedures \<= 56.50+++ Save**를 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image38.png)

#### **작업 1.3: 피쳐 목록을 사용하여 모델 오류에 기여하는 상위 피쳐를 식별하기**

1.  **Feature list**를 클릭하세요.

![](./media/image39.png)

2.  목록은 오류에 대한 기능의 기여도에 따라 정렬됩니다. 이 목록에서
    기능이 높을수록 모델 오류에 대한 기여도 중요도가 높아집니다.

3.  In our Diabetes Hospital Readmission 모델에서 **Feature List**은
    모델 오류의 주요 원인 중 하나인 다음 기능을 나타냅니다.

    - Age

    - num_medications

    - medicare

    - time_in_hospital

    - num_procedures

    - insulin

    - discharge_destination

### 작업 2: Heat map을 사용하여 오류 찾기

Feature List에서 **Age**은 가장 큰 오류 원인 중 하나였습니다. 따라서
Heat map 탭을 사용하여 어떤 연령대의 환자가 모델의 성능을 저하시키는지
살펴보겠습니다.

1.  **Error Analysis**에서 **Heat map**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)

2.  Heat Map 탭의 **Rows: Feature 1** 드롭다운 메뉴에서 **Age**를
    선택하여 모델의 오류에 어떤 영향을 미치는지 확인하세요.

3.  **Age**를 선택하면 대시보드에 가능한 조건에 따라 기능을 다른 셀로
    나누는 내장 인텔리전스가 어떻게 있는지 확인할 수 있습니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image41.png)

2.  각 셀 위로 마우스를 가져가면 셀에 표시된 데이터 그룹에 대한 올바른
    예측과 잘못된 예측의 수, 오류 적용 범위 및 오류율을 볼 수 있습니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)

3.  **Over 60 years**의 셀에는 **536**개의 올바른 모델 예측과
    **126**개의 잘못된 모델 예측이 있습니다. 오류 적용 범위는
    **73.81%**이고 오류율은 **18.79%**입니다

4.  **30-60**년이 있는 셀에는 **273**개의 올바른 모델 예측과 **25**개의
    잘못된 모델 예측 이 있습니다. 오류 적용 범위는 **25.60%**이고
    오류율은 **13.61%**입니다.

5.  **30**세 이하의 셀에는 **17**개의 올바른 모델 예측과 **1**개의
    잘못된 모델 예측이 있습니다**.**

> 우리의 관찰은 **Age** 가 모델의 잘못된 예측에 중요한 역할을 한다는
> 것을 보여주기 때문에 다음 실험실에서 추가 분석을 위해 각 연령 그룹에
> 대한 코호트를 생성할 것입니다.

#### ***작업 2.1: 연령 그룹을 기반으로 코호트 생성하기***

1.  **Over 60 years** 셀의 백분율 상자를 클릭하세요. 사각형 셀 주위에
    파란색 테두리가 표시됩니다.

2.  **Save as a new cohort**를 클릭하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

3.  Save as a new cohort 대화상자에서 다음을 입력하세요

    - Cohort name - **+++Age==Over 60 year+++**

**Save**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

4.  2단계와 3단계를 반복하여 다른 두 연령 셀 각각에 대한 코호트를
    생성하세요.

- **Cohort \#4:** Name - **+++Age == 30–60 years+++**

- **Cohort \#5:** Name - **+++Age \<= 30 years+++**

### 작업 3: 코호트 목록을 보기

1.  Error Analysis 섹셩의 오른쪽 상단 모서리에 있는 **Settings**
    톱나바퀴 아이콘을 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image45.png)

2.  그러면 생성한 모든 코호트 목록이 있는 **Cohort Settings** **window
    pane**이 열립니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image46.png)

## 연습 4: RAI를 사용하여 모델 분석을 수행하기

이 실습에서는 Azure Responsible AI (RAI) 대시보드의 **Model Overview**
섹션을 사용하는 방법을 살펴봅니다. 오류 분석 랩에서 생성된 코호트를
사용하여 한 코호트와 다른 코호트에서 모델의 동작이 더 나은 이유를
조사합니다.

## **연습 4.1: 모델 개요**

### 작업1: 모델 성능 메트릭 테이블 검토하고 비교하기

1.  Model Overview 섹션을 찾기 위해 Error Analysis 아래로 스크롤하세요.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

2.  Model Overview에서 **Dataset Cohorts** 창을 선택하세요. 이렇게 하면
    모델 메트릭과 함께 테이블에 생성된 다양한 코호트가 표시됩니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image48.png)

3.  오류가 가장 많은 코호트**Err: Prior_Inpatient \> 0; Num_Meds \> 11
    and ≤ 21.50** 가장 적은 오류 비교 **Prior_inpatient = 0;
    num_diagnose ≤ 6.50; lab_procedures \< 56.50.**

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image49.png)

4.  차트의 상자 표시선 위에 마우스를 올려 놓으면 측정 세부 정보를 볼 수
    있습니다.

![A screenshot of a computer Description automatically
generated](./media/image50.png)

1.  Erroneous cohort에 대한 정확도 점수가 0.806으로 좋지 않음을
    관찰합니다. **False Positive** 비율은 **very low**고 **False
    Negative** 값은 **high**입니다. 즉, 모델이 예측하는 환자의 대다수는
    30일 이내에 재입원하여 재입원하지 않을 환자를 예측하는 비율이
    높습니다.

> ![A red line in a white sheet Description automatically
> generated](./media/image51.png)

5.  다음으로, 오류가 **least errors**은 **cohort**의 정확도 점수가
    0.94인 메트릭을 살펴보면 모든 데이터가 있는 모델의 전체 정확도
    점수보다 훨씬 우수합니다. 그러나 이 코호트는 **0**에서 **False
    positive**비율도 낮습니다.

![A picture containing text, screenshot, line, number Description
automatically generated](./media/image52.png)

### 작업 2: Probability distribution를 검사하기

1.  아래로 스크롤하여 **Probability distribution**를 확인하세요.

2.  Probability distribution차트는 코호트의 환자가 30일 이내에 병원에
    재입원할지 또는 재입원하지 않을지 예측하는 모델의 확률을 보여줍니다.

3.  3개 코호트 모두에 대해 환자가 재입원하지 않을 확률을 비교합니다..

4.  모든 환자 테스트 데이터 세트가 포함된 **All data** 코호트는 대다수의
    환자가 30일 이내에 병원에 재입원하지 않을 것임을 보여주며, 환자가
    재입원하지 않을 확률의 중앙값은 0.854, 상위 사분위수는 0.986으로
    양호합니다.

5.  다음으로, 오류율이 가장 높은 코호트입니다: ***Err: Prior_Inpatient
    \>0; Num_meds \>11.50 & \<= 21.50***, 0.89에서 약간 낮은 확률과
    0.719의 중앙값을 보여줍니다.

6.  마지막으로 오류율이 가장 낮은 코호트입니다: ***Prior_Inpatient =
    0*; *num_diagnoses \<= 6.50*; *lab_procedures \<= 56.50***,
    재입원하지 않은 환자의 확률은 중앙값 0.90, 상위 사분위수 0.986을
    보여줍니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image53.png)

7.  3개 코호트에 대해 환자가 재입원할 확률을 표시하도록 차트를
    변경하려면 x축에서 **Choose Label** 버튼을 클릭하세요.

8.  **Probability: Readmitted** 라디오 버튼을 선택하세요. 팝업 창
    창에서.

9.  **Apply** 버튼을 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image54.png)

10. 3개 코호트에 대해 환자가 재입원할 확률을 비교하세요.

> ![A screenshot of a graph Description automatically generated with low
> confidence](./media/image55.png)

9.  3개의 코호트가 재입원할 확률이 0.55 미만인 것을 볼 수 있습니다. 모형
    오류 수가 가장 적은 코호트의 확률이 0.179로 가장 낮습니다. 오류가
    가장 많은 코호트의 확률이 0.543으로 가장 높습니다.

### 작업 3: 지표 시각화 차트를 검토하기

이제 메트릭 시각화 창으로 전환하여 모델의 성능에 대해 더 자세히
알아보겠습니다. 

1.  Metric visualizations 탭을 클릭하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image56.png)

2.  다른 메트릭을 선택하려면 x축에서 **Choose metric**을 클릭하여 사용
    가능한 다른 메트릭 목록에서 **Precision score**를 선택하세요.
    **Apply** 버튼을 클릭하세요.

> **참고**: 학습된 모델은 분류 문제이므로 RAI 대시보드에는 분류 메트릭만
> 표시됩니다.
>
> ![](./media/image57.png)

3.  차트를 검토하면 모든 테스트 데이터 코호트 및 오류 코호트에 대한 모델
    성능이 시간의 ~70%에서 정확함을 확인할 수 있습니다. 

4.  이전에 입원한 적이 없고 진단 건수가 7 미만인 환자의 경우 오류가
    **least erroneous cohort**에 대한 **Precision score**비율은
    **0.94**입니다. 이는 정확도 점수와 일치합니다

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image58.png)

5.  마지막으로, 메트릭을 **Recall**로 변경하여 코호트의 환자가 30일 후에
    병원에 다시 입원할 것임을 모델이 얼마나 정확하게 예측할 수 있었는지
    확인합니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image59.png)

6.  리콜은 재입원하는 환자의 모든 코호트에 대해 **model's prediction**이
    **correct less than 25%**을 보여줍니다. 이는 30일 이내에 재입원할
    환자를 예측하려고 할 때 모델의 예측이 대부분의 경우 정확하지 않다는
    것을 보여줍니다.

![A screenshot of a graph Description automatically generated with low
confidence](./media/image60.png)

### 작업 4: Confusion Matrix을 보기

Confusion Matrix은 올바른 예측을 수행하는 모델의 비율을 올바르게
확인하는 데 도움이 됩니다. 이를 통해 환자가 30일 이내에 병원에 재입원한
경우와 재입원하지 않은 경우에 대해 모델이 얼마나 잘 학습하고 있는지
확인할 수 있습니다.

1.  **Confusion matrix** 탭을 클릭하세요.

&nbsp;

2.  **Model**이 **Not Readmitted**환자가 **Readmitted**환자와 비교하여
    **better** 수행되고 있음을 확인할 수 있습니다

3.  False Negative의 수는 True Negative보다 작아야 합니다. 이 평균은
    모든 환자 데이터 중에서 이 모델은 \<30일 이내에 병원에 재입원할 수
    있는 환자 24명만 정확하게 예측할 수 있었습니다.

- The number of True Positive (TP) is: **802**

- The number of False Negative (FN) is: **159**

- The number of False Positive (FP) is: **9**

- The number of True Negative (TN) is: **24**

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image61.png)

## **연습 2: Feature Cohort**

오류가 가장 높은 코호트에는 *Prior_Inpatient \> 0*일의 환자 가 있고 약물
투여 횟수가 11일에서 22일 사이인 환자가 모델에서 오류율이 더 높았 기
때문에 *Prior_Inpatient* 및 *Num_medications* 자세히 살펴보면 문제가
있는 부분을 격리하는 데 도움이 됩니다. 이 실습에서는 Prior_Inpatient만
분석합니다*.*

1.  **Feature Cohorts** 탭을 클릭하세요.

2.  **Feature(s)** 드롭다운 메뉴에서 아래로 스크롤하여
    **prior_inpatient** 확인란을 선택하세요. 그러면 3개의 서로 다른 기능
    코호트와 모델 성능 메트릭이 표시됩니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image62.png)

3.  **prior_inpatient *\< 3*** 코호트의 표본 크기는 **943**입니다. 이는
    테스트 데이터에 있는 환자의 대다수가 과거에 입원 횟수가 3회 미만임을
    의미합니다. 이 코호트에 대한 **model's accuracy rate**는 **0.838**로
    양호합니다.

4.  테스트 데이터에서 39명의 환자만이 **prior_inpatient *≥ 3 and \< 6***
    코호트에 속합니다. 모델의 정확도는 **0.692**로 좋지 않습니다.

5.  마지막으로, 테스트 데이터에서 12명의 환자만이 이전에 6일 이상 입원한
    적이 있습니다. 이 코호트에 대한 **model accuracy**의 **0.75**는
    괜찮습니다.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image63.png)

### 작업 1: probability distribution 기능

데이터셋 코호트와 마찬가지로 "probability distribution"를 볼 수
있습니다.

1.  당뇨병 환자의 prior_inpatient 입원 횟수가 적을수록 환자가 30일
    이내에 재입원하지 않을 가능성이 높다는 것을 알 수 있습니다. 

![A screenshot of a computer Description automatically
generated](./media/image64.png)

### 작업 2: Feature Metrics visualizations

1.  **Metrics visualization**를 선택하세요. x축에서 **Choose metric**
    버튼을 클릭하세요. **Precision score** 메트릭을 선택하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image65.png)

2.  **prior_inpatient \< 3** 환자의 정밀도 점수가 0.40으로 매우 나쁜
    것을 볼 수 있습니다. 이는 모델이 수행한 모든 예측 중 40%만이 이
    코호트에 대해 정확했음을 의미합니다.

> ![A blue and white bar graph Description automatically
> generated](./media/image66.png)

3.  다른 2개 코호트의 정확도 점수는 양호합니다.

4.  다음으로, x축에 대한 **Recall** **score** 메트릭을 선택하세요.

> ![A screenshot of a computer Description automatically generated with
> medium confidence](./media/image67.png)

5.  반대로 **prior_inpatient \< 3**을 가진 환자의 재현율 점수는
    0.013입니다. 즉, 테스트 데이터에 있는 대부분의 환자에 대해 모델이
    환자가 30일 이내에 재입원할지 여부를 정확하게 예측하는 데 어려움을
    겪고 있습니다.

> ![A picture containing screenshot, software, line, text Description
> automatically generated](./media/image68.png)
>
> **요약**
>
> 이 실습에서는 기존 모델 성능 지표(예: 정확도, 재현율, 혼동 행렬 등)가
> 여전히 매우 중요한 이유를 보여줍니다. RAI 인사이트와 기존 성능
> 메트릭을 결합함으로써 대시보드는 보다 세분화된 수준에서 모델을
> 분석하고 디버그할 수 있는 전체적인 도구를 제공합니다.
