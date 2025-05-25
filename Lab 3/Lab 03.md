# 실습 03 – 괸리형 기능 저장소로 기능 세트를 개발 및 등록하고 기능을 사용하여 모델을 학습시키기

이 실습에서는 사용자 지정 변형을 사용하여 기능 사양을 생성하는 방법을
설명합니다. 해당 기능 세트를 사용하여 학습 데이터를 생성하고 구체화를
활성화하고 채우기를 수행합니다. 구체화는 기능 창에 대한 기능 값을 계산한
후 해당 값을 구체화 저장소에 저장합니다. 모든 기능 쿼리는 구체화
저장소의 해당 값을 사용할 수 있습니다.

구체화가 없으면 기능 집합 쿼리는 값을 변환하기 전에 기능을 계산하기 위해
즉석에서 원본에 변환을 적용합니다. 이 프로세스는 프로토타입 생성
단계에서 잘 작동됩니다. 그러나 프로덕션 환경에서 학습 및 추론 작업의
경우 안정성과 가용성을 높이기 위해 기능을 위해 기능을 구체화하는 것이
좋습니다.

예상 소요 시간 – 50분

## 연습1: 필요한 역할을 할당하기:

1.  Azure portal Home 페이지에서 **Resources** 탭에서 할당된 **Resource
    group**를 선택하세요. 왼쪽 창에서 **Access control(IAM)**를
    선택하세요. **Add** 옆의 드롭다운을 클릭하고 **Add role
    assignment**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  +++**AzureML Data Scientist**+++를 검색하고 선택하세요. **Next**를
    클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  Members 탭에서 **+ Select members**를 클릭하고 **User name**
    <+++@lab.CloudPortalCredential(User1).Username>+++를 검색하세요.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  **Username**를 선택하고 **Select** 버튼을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

5.  다음 2개의 화면에서 **Review + assign**을 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  할당된 메시지가 추가되면 할당이 수행됩니다.

7.  동일한 단계를 반복하여 역할 +++**Storage Blob Data Reader**+++ 및
    +++**Storage Blob Data Contributor**+++를 추가하세요.

## 연습 2: 기능 세트를 개발하고 관리되는 기능 저장소에 등록하기

이 자습서는 관리되는 기능 저장소 자습서 시리즈의 첫 번째 부분입니다.
여기에서 다음 방법을 알아봅니다:

- 최소한의 새 기능 저장소 리소스를 생성하기

&nbsp;

- 기능 변환 기능을 사용하여 기능 세트를 개발하고 로컬에서 테스트하기

- 기능 저장소 엔터티를 기능 저장소에 등록하기

- 개발한 기능 세트를 기능 저장소에 등록하기

- 생성한 기능을 사용하여 샘플 학습 DataFrame을 생성하기

- 기능 세트에서 오프라인 구체화를 사용하도록 설정하고 기능 데이터를 다시
  채우기

### 작업 1: 환경을 준비하기

1.  Azure Machine Learning Studio의 왼쪽 창에서 **Authoring**에서
    **Notebooks**를 선택하세요. 사용자 이름 옆에 있는 세 개의 점을
    클릭하고 **Upload folder**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

2.  **C:\Labfiles**에서 **featurestore** 폴더로 찾고 선택하여
    **Upload**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

3.  **featurestore-\> notebooks-\>sdk_and_cli**로 이동하고 notebook 1를
    여세요. Develop-feature-set-and-register.ipynb

![](./media/image8.png)

4.  **Compute**에서 **Serverless Spark Compute**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

5.  사전 요구 사항에 따라 세션을 구성하려면 **Configure session**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

6.  **Python packages -\> Upload Conda file**를 선택하세요. **Browse**를
    클릭하고 **C:\Labfiles**에서 **conda.yml**를 선택하고 **Apply**를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

7.  Notebook의 첫 번째 셀을 **Execute**하세요. 그러면 모든
    **dependencies**이 설치 되고 실행이 완료됩니다. 완료하는 데 약 **10
    minutes** 정도 걸립니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image12.png)

![A screenshot of a computer Description automatically
generated](./media/image13.png)

8.  Spark 세션이 시작되면 **User name**을 사용자 이름으로 바꾸고 다음
    셀을 실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

![A screenshot of a computer error Description automatically
generated](./media/image15.png)

9.  다음 3개의 셀을 실행하여 Azure CLI를 설정하세요.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

10. 다음 셀에서 **output**의 단계에 따라 **Azure**에 로그인하세요.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

![A screenshot of a computer program Description automatically
generated](./media/image18.png)

### 작업 2: 최소 기능 저장소를 생성하기

1.  기능 저장소의 이름, 위치 및 기타 값을 설정하려면 **first** 번째 셀을
    **Execute**하세요.

![A screenshot of a computer program Description automatically
generated](./media/image19.png)

2.  **Creates the feature store**하는 다음 셀을 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

3.  다음 셀은 **initializes AzureML feature store core SDK client**.
    **Execute**하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image21.png)

### 작업 3: 이 Notebook에 있는 트랜잭션 롤링 집계 기능 세트를 프로토타입하고 개발하기

1.  **Transaction** 소스 데이터를 탐색하려면 이 섹션 아래의 첫 번째 셀을
    **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  로컬에서 **Develop a transactions feature set**하려면 두 번째 셀을
    실행하세요.

![A screenshot of a computer code Description automatically
generated](./media/image23.png)

3.  기능 세트 사양에서 **generate a spark dataframe**하려면 다음 셀을
    실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  기능 집합 사양을 기능 저장소에 등록하려면 특정 형식으로 저장해야
    합니다. 생성된 트랜잭션을 검사하세요. FeatureetSpec: 파일 트리에서
    이 파일을 열어 사양을 확인하세요:
    featurestore/featuresets/accounts/spec/FeaturesetSpec.yaml.

피쳐 세트 사양으로 내보내려면 다음 셀을 실행하세요.

![A screenshot of a computer program Description automatically
generated](./media/image25.png)

### 작업 4: 기능 저장소 엔터티를 등록하기

1.  Entity는 동일한 조인 키 정의가 동일한 논리적 엔터티를 사용하는 기능
    집합에서 사용되도록 하는 모범 사례를 적용하는 데 도움이 됩니다. 기능
    저장소 엔터티를 등록하려면 셀을 실행하세요.

> ![A screen shot of a computer Description automatically
> generated](./media/image26.png)

### 작업 5: 트랜잭션 기능 세트를 기능 저장소에 등록하기

1.  Azure portal (+++https://portal.azure.com+++)에서 할당된 Resource
    그룹 아래의 **featureset**으로 시작하는 **Storage account**으로
    이동하세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image27.png)

2.  왼쪽 창에소 Access Control(IAM)를 선택하세요. **Add** -\> **Add role
    assignment**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

3.  +++**Storage Blob Data Reader**+++를 검색하고 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image29.png)

4.  연습 1에서 수행한 것과 유사한 역할 할당을 완료하세요.

5.  마찬가지로 +++**Storage Blob Data Contributor**+++ 역할을
    추가하세요.

6.  Azure Machine Learning Studio로 다시 이동하세요.

7.  다른 사용자와 공유하고 다시 사용할 수 있도록 기능 집합 자산을 기능
    저장소에 동록할 것입니다. 또한 버전 관리 및 구체화와 같은 관리되는
    기능을 사용할 수 있습니다. 기능 세트 자산에는 이전에 생성한 기능
    세트 사양과 버전 및 구체화 설정과 같은 추가 속성에 대한 참조가
    있습니다.

8.  **Register the transaction feature set**하려면 다음 셀을
    **Execute**하세요

> ![A screenshot of a computer Description automatically
> generated](./media/image30.png)

### 작업 6: 기능 저장소 UI 살펴보기

1.  브라우저에서 새 탭을 열고 +++https://ml.azure.com/home++의 Azure ML
    글로벌 방문 페이지로 이동하세요.

2.  왼쪽 탐색에서**Feature stores**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

3.  **featurestore**를 클릭하세요.

**참고:** 기능 저장소 자산 (기능 집합 및 엔터티)을 만들고 업데이트하는
것은 SDK 및 CLI를 통해서만 가능합니다. UI를 사용하여 기능 저장소를
검색/찾아볼 수 있습니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

### 작업 7: 등록된 기능을 사용하여 학습 데이터 dataframe을 생성하기

1.  먼저 관측 데이터를 탐색합니다. 관측 데이터는 일반적으로 학습 및 추론
    데이터에 사용되는 핵심 데이터입니다. 기능 데이터와 결합하여 전체
    학습 데이터를 생성합니다. 관측 데이터는 이벤트 시간 동안 캡처된
    데이터로, 이 경우 거래 ID, 계정 ID, 거래 금액을 포함한 핵심 거래
    데이터가 있습니다. 이 경우 학습용이므로 target 변수도
    추가됩니다(is_fraud).

2.  출력 데이터를 관찰하려면 cel land를 **Execute**하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  **Registered feature set** 및 **list its features**를 받으려면 다음
    셀을 **Execute**하세요.

![A screenshot of a computer program Description automatically
generated](./media/image34.png)

4.  **Sample values**를 **print**하려면 다음 셀을 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

5.  다음 셀을 **Execute**하세요. 이 단계에서 **training data**의 일부가
    될 **select features**하고 기능 저장소 SDK를 사용하여 학습 데이터를
    생성합니다.

![A screenshot of a computer program Description automatically
generated](./media/image36.png)

6.  기능 데이터와 관측 데이터를 사용하여 학습 데이터 프레임을 생성하려면
    다음 셀을 실행하세요.

![A screenshot of a computer program Description automatically
generated](./media/image37.png)

### 작업 8: 트랜잭션 기능 세트에 대한 오프라인 구체화를 활성화하기

기능 세트에서 구체화가 활성화되면 채우기를 수행하거나 구체화 작업을
예약할 수 있습니다.

1.  기능 데이터 크기에 따라 yaml 파일의 spark.sql.shuffle.partitions를
    설정하려면 다음 셀을 실행하세요

2.  spark 구성 spark.sql.shuffle.partitions는 기능 집합이 오프라인
    저장소로 구체화될 때 생성되는 parquet 파일 수(일당)에 영향을 줄 수
    있는 OPTIONAL 매개 변수입니다. 이 매개 변수의 기본값은 200입니다.
    가장 좋은 방법은 작은 쪽모이 세공 파일을 많이 생성하지 않는
    것입니다. 기능 세트가 구체화된 후 오프라인 기능 검색이 느려지는
    것으로 판명되면 오프라인 저장소의 해당 폴더로 이동하여 작은 쪽모이
    세공 파일(일당)이 너무 많은 문제인지 확인하고 그에 따라 이 매개
    변수의 값을 조정하세요.

**참고:** 이 Notebook에 사용된 샘플 데이터는 크기가 작습니다. 따라서 이
매개변수는 featureset_asset_offline_enabled.yaml 파일에서 1로
설정됩니다.

> ![A screenshot of a computer Description automatically
> generated](./media/image38.png)

3.  구체화는 지정된 기능 창에 대한 기능 값을 계산하고 이를 구체화
    저장소에 저장하는 프로세스입니다. 기능을 구체화하면 신뢰성과
    가용성이 향상됩니다. 모든 기능 쿼리는 구체화 저장소의 구체화된 값을
    사용합니다. 이 단계에서는 18개월의 기능 창에 대해 일회성 백필을
    수행합니다.

4.  다음 코드 셀은 정의된 기능 창에 대해 현재 상태, None 또는
    Incomplete로 **materialize data**합니다. **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

5.  다음 셀에 있는 기능 세트의 **print sample data** 해보겠습니다.
    **Execute**하세요. 출력 정보에서 데이터가 구체화 저장소에서
    검색되었음을 알 수 있습니다. 학습/추론 데이터를 검색하는 데 사용되는
    get_offline_features() 메서드도 기본적으로 구체화 저장소를
    사용합니다.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 연습 3: 기능을 사용하여 모델을 실험하고 학습시키기

이 notebook에서는 다음 방법을 알아봅니다:

- 미리 계산된 기존 값을 기능으로 사용하여 새로운 계정 기능 세트 사양의
  프로토타입을 생성합니다. 로컬 기능 집합 사양을 기능 저장소에 기능
  집합으로 등록합니다. 이 프로세스는 사용자 지정 변환이 있는 기능 집합을
  만든 첫 번째 자습서와 다릅니다.

- 트랜잭션 및 계정 기능 집합에서 모델에 대한 기능을 선택하고 기능 검색
  사양으로 저장합니다.

- 기능 검색 사양을 사용하여 새 모델을 학습시키는 학습 파이프라인을
  실행합니다. 이 파이프라인은 기본 제공 기능 검색 구성 요소를 사용하여
  학습 데이터를 생성합니다.

### 작업 1: 환경을 설정하기

1.  Notebooks 창에서**Experiment and train models using features**
    notebook를 여세요.

2.  **Configure session**를 클릭하고 이전 이전 Notebook에서 했던 것과
    유사한 방식으로 **conda.yaml**을 업로드하세요.

3.  세션을 시작하려면 첫 번째 셀을 **Execute**하세요. 약 10분 정도
    소요됩니다.

![A white rectangular object with green text Description automatically
generated](./media/image41.png)

4.  다음 셀에서 폴더 구조의 **username**으로 **\< your_user_alias \>**의
    자리 표시자를 바꾸고 셀을 **Execute**하세요.![A screenshot of a
    computer program Description automatically
    generated](./media/image42.png)

5.  **Setup CLI**하려면 다음 **3**개의 셀을 **Execute**하세요.

6.  다음 셀은 프로젝트 작업 공간 변수를 초기화합니다. **Initialize the
    variables**하려면 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image43.png)

7.  다음 셀은 기능 저장소 변수를 초기화합니다. 그것을 실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image44.png)

8.  **Initialize the feature store consumption client**하려면 다음 셀을
    실행하세요.

![A screenshot of a computer screen Description automatically
generated](./media/image45.png)

### 작업 2: 미리 계산된 데이터에서 로컬로 계정 기능 세트를 생성하기

미리 계산된 기능을 온보딩하려면 변환 코드를 작성하지 않고도
featureset사양을 생성할 수 있습니다. Featureset사양은 featurestore에
연결하지 않고 완전한 로컬/개발 환경에서 featureset를 개발하고 테스트하기
위한 사양입니다. 이 단계에서는 기능 세트 사양을 로컬로 생성하고 여기에서
값을 샘플링합니다.

1.  **Explore the source data for accounts**하려면 다음 셀을
    실행하세요**.**

![A screenshot of a computer Description automatically
generated](./media/image46.png)

2.  이러한 미리 계산된 기능에서 로컬로 **create accounts feature set
    spec**하려면 다음 셀을 실행하세요

![A screen shot of a computer code Description automatically
generated](./media/image47.png)

![A screenshot of a computer Description automatically
generated](./media/image48.png)

3.  기능 세트 사양에서 **generate a spark dataframe**하려면 다음 셀을
    **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

4.  기능 세트 사양을 기능 저장소에 등록하려면 특정 형식으로 저장해야
    합니다. 작업: 아래 셀을 실행한 후 생성된 계정 FeatureSetSpec: 파일
    트리에서 이 파일을 열어 사양을 확인합니다:
    featurestore/featuresets/accounts/spec/FeatureSetSpec. 다음 셀을
    **Execute**하세요.![A screenshot of a computer program Description
    automatically generated](./media/image50.png)

### 작업 3: 등록되지 않은 기능을 로컬에서 실험에서 실험하고 준비가 되면 기능 저장소에 등록하기

기능을 개발할 때 기능 저장소에 등록하거나 클라우드에서 학습 파이프라인을
실행하기 전에 로컬에서 테스트/유효성 검사를 수행할 수 있습니다. 이
단계에서는 등록되지 않은 로컬 기능 집합(계정)과 기능 저장소에 등록된
기능 집합(트랜잭션)의 기능 조합에서 ML 모델에 대한 학습 데이터를
생성합니다.

1.  **Model**에 대한 **select features**하려면 **Execute**하세요**.**

![A screenshot of a computer program Description automatically
generated](./media/image51.png)

2.  로컬로 **generate training data**하려면 다음 2개의 셀을
    **Execute**하세요.

![A close-up of a computer code Description automatically
generated](./media/image52.png)

![A screenshot of a computer Description automatically
generated](./media/image53.png)

3.  featurestore에 **register the accounts featureset**하려면
    **Execute**하세요. 로컬에서 다양한 기능 정의를 실험하고 온전한
    테스트를 수행한 후에는 기능 저장소에 등록할 수 있습니다. 이를 위해
    기능 세트에 자산 정의를 피처 스토어에 등록합니다.

![A screenshot of a computer Description automatically
generated](./media/image54.png)

4.  등록된 featureset 및 온전성 테스트를 가져오려면 다음 2개의 셀을
    **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image55.png)

### 작업 4: 학습 실험을 실행하기

1.  SDK에서 기능 검색하려면 다음 셀을 실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image56.png)

2.  이전 단계에서는 로컬 실험 및 테스트를 위해 등록되지 않은 기능 집합과
    등록된 기능 집합의 조합에서 기능을 선택했습니다. 이제 클라우드에서
    실험할 준비가 되었습니다. 선택한 기능을 기능 검색 사양으로 저장하고
    학습/추론을 위해 mlops/cicd 흐름에서 사용하면 모델 제공의 민첩성이
    향상됩니다.

3.  **Select features for the model**하려면 다음 셀을 **Execute**하세요.

![A screenshot of a computer program Description automatically
generated](./media/image57.png)

4.  선택한 기능을 **feature-retrieval spec**으로 내보내려면 다음 셀을
    **Execute**하세요.

![A screenshot of a computer program Description automatically
generated](./media/image58.png)

### 작업 5: 파이프라인을 사용하여 클라우드에서 학습하고 만족스러운 경우 모델을 등록하기

이 단계에서는 학습 파이프라인을 수동으로 트리거합니다. 프로덕션
시나리오에서는 소스 리포지토리의 feature-retrieval 사양 변경 사항을
기반으로 하는 ci/cd 파이프라인에 의해 트리거될 수 있습니다.

1.  **Run the training pipeline**하려면 다음 셀을 **Execute**하세요.

![A screenshot of a computer Description automatically
generated](./media/image59.png)

![A screenshot of a computer program Description automatically
generated](./media/image60.png)

2.  스튜디오의 왼쪽 창에서 **Jobs**을 마우스 오른쪽 버튼으로 클릭하고 새
    탭에서 여세요. 실험**training_on_fraud_model**을 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image61.png)

3.  **Training job**을 클릭하고 세부 정보를 탐색하세요. 실험이 완료되는
    데 약 5분에서 15분 정도 걸립니다.

![A screenshot of a computer Description automatically
generated](./media/image62.png)

![A screenshot of a computer Description automatically
generated](./media/image63.png)

4.  완료될 때까지 기다리세요. 완료되면 왼쪽 창에서 **Models**을
    선택하세요. 목록에서 **fraud_model** 선택하세요. 이것은 지금
    만들어진 모델입니다.

![A screenshot of a computer Description automatically
generated](./media/image64.png)

5.  **Feature sets** 탭을 선택하세요. 여기에서 이 모델이 의존하는
    **transactions**및 **accounts** featuresets를 모두 볼 수 있습니다.

![A screenshot of a computer Description automatically
generated](./media/image65.png)

6.  +++https://ml.azure.com/home+++에서 **feature store UI**를 여세요.
    **Feature stores** -\> **featurestore**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image66.png)

7.  왼쪽 창에서 **Feature sets**를 선택하고 **feature sets** 중 하나를
    선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image67.png)

8.  **Models** 탭을 클릭하세요. 기능 세트를 사용하는 모델 목록을 볼 수
    있습니다(모델이 등록되었을 때 기능 검색 사양에 따라 결정됨).

![A screenshot of a computer Description automatically
generated](./media/image68.png)

요약:

이 실습에서는 관리형 기능 저장소를 사용하여 기능 세트를 개발 및 등록하고
기능을 사용하여 모델을 학습시키는 방법을 배웠습니다.
