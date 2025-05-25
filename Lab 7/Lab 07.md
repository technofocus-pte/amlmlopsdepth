# 실습 07: Azure Machine Learning Studio에서 프롬프트 플로우 개발하고 테스트하기

**목표:**

이 랩에서는 Azure Machine Learning 스튜디오에서 프롬프트 플로우를
사용하는 주요 사용자 경험에 대해 알아봅니다. Azure Machine Learning 작업
공간에서 프롬프트 플로우를 사용하도록 설정하고 프롬프트 플로우를 생성 및
개발하고 플로우를 테스트 및 평가한 후 프로덕션에 배포하는 방법을
알아봅니다.

예상 소요 시간 – 60분

## 작업 1: Azure 리소스를 준비하기

### 작업 1.1: Azure Machine Learning 작업 공간을 생성하기

이 작업은 Azure Machine Learning 작업 공간을 생성하는 데 집중합니다.
기계 학습 프로젝트를 효과적으로 구성하고 관리하기 위해 전용 작업 공간을
설정하는 방법을 알아봅니다. 이 작업 공간은 공동 작업, 실험 및 배포를
위한 중앙 허브 역할을 합니다.

1.  +++https://portal.azure.com[++에서 Azure
    Portal](https://portal.azure.com)에 로그인하고 관리 테넌트 자격
    증명으로 로그인하세요.

2.  Azure portal 홈페이지에서 **+ Create a resource**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

3.  On the **Create a resource** 페이지에서 검색 바로 +++Azure Machine
    Learning**+++**를 찾고**Azure** **Machine Learning**를 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image2.png)

4.  **Marketplace**에서 **Create dropdown**을 클릭하고 **Azure Machine
    Learning**을 선택하세요.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)

5.  새 작업 공간을 구성하려면 다음 정보를 제공합니다:

    - **Subscription**: **할당된 Azure subscription**를 선택하세요

    - **Resource group**: **할당된 Resource Group**를 선택하세요

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> **Workspace Details:**

- **Workspace name:** +++**Azuremlws@lab.LabInstanceId**+++

- **Region**: 가장 가까운 지역을 선택하세요 **(**여기 **North Central
  US**가 선택됩니다)

&nbsp;

- **Container registry: Select Create new. Enter
  +++azuremlcr@lab.LabInstanceId+++**

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

![A screenshot of a computer Description automatically
generated](./media/image6.png)

6.  작업 공간 구성이 완료되면 **Review + Create**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

7.  Validation이 통화되면 **Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

8.  새 작업 공간을 보기 위해**Go to resource**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image9.png)

9.  **On the Microsoft.MachineLEarningServices | Overview page**에서
    **Work with your model in Azure Machine Learning studio**에서
    **Launch studio**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image10.png)

### 작업 1.2: 컴퓨팅을 생성하기

이 작업은 Azure에서 컴퓨팅 리소스를 생성하는 방법을 보여 줍니다. 가상
머신 또는 관리형 컴퓨팅 클러스터와 같은 다양한 컴퓨팅 옵션을 탐색하고
기계 학습 워크로드를 효율적으로 실행하기 위해 리소스를 구성하고
프로비저닝하는 방법을 이해합니다.

1.  **Azure Machine Learning Studio**가 열리면 왼쪽 창의 **Manage**에서
    **Compute**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

2.  **Compute instances** 화면에서 **+ New**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image12.png)

3.  Create compute instance 화면에서 다음 세부 정보를 입력하세요.

    1.  Compute name – +++**pfcompute**+++

    2.  Virtual machine type – **CPU**

    3.  Virtual machine size – **Standard_E4ds_v4**를 선택하세요

> **Review + Create**를 클릭하세요.

**참고:** 나중에 사용할 수 있도록 이 컴퓨팅 이름을 기록해 두세요.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

4.  컴퓨팅을 생성하려면 다음 화면에서**Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image14.png)

**참고:** 컴퓨팅은 Running 상태가 되는 데 약 10분이 걸립니다.

![A screenshot of a computer Description automatically
generated](./media/image15.png)

**중요:** **중요:** 컴퓨팅이 실행되면 다음 작업을 계속할 수 있습니다.
그러나 실습 실행을 중단하는 경우 컴퓨팅 인스턴스를 **중지**하고 휴식 후
다시 시작하세요.

### 작업 1.3: Azure OpenAI 리소스를 생성하기

1.  Azure portal +++https://portal.azure.com+++에서
    +++**AzureOpenAI**+++를 검색하고 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image16.png)

2.  **+ Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

3.  다음 세부 정보를 입력하고 **Next**를 클릭하세요.

- Resource group - 할당된 Resource group를 선택하세요

- Region – 지역을 선택하세요 (여기 North Central US가 사용됩니다)

- Name - +++**AOAI-PF@lab.LabInstanceId**+++

- Pricing tier - **Standard**

![A screenshot of a computer Description automatically
generated](./media/image18.png)

4.  다음 페이지에서 기본값을 수락하고 **Review + submit** 페이지에서
    **Create**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image19.png)

5.  배포가 완료되면 **Go to resource**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image20.png)

6.  왼쪽 창에서 **Keys and Endpoint**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

7.  **Key**와 **Endpoint** 복사하고 랩의 이후 부분에서 사용할 수 있도록
    메모장에 저장하세요.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

8.  **Azure Machine Learning Studio**에서 왼쪽 창에서 **Model
    catalog**를 선택하고 **gpt-4o**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image23.png)

9.  모델을 배포하기 위해 **Deploy**를 클릭하세요.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

10. 배포 이름을 수락하고**Deploy**를 선택하세요. 나중에 사용할 수 있도록
    이 이름을 기록해 두세요.

![A screenshot of a computer Description automatically
generated](./media/image25.png)

![A screenshot of a computer Description automatically
generated](./media/image26.png)

## 작업 2: Prompt flow 연결을 설정하기

1.  Azure Machine Learning Studio의 왼쪽 탐색 창에서 **Prompt flow**를
    선택하세요. 메뉴 바에서 **Connections**를 선택하세요. **Create**
    옆의 드롭다운을 선택하고 **Azure OpenAI**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image27.png)

2.  Add Azure OpenAI connection wizard에서 다음 세부 정보를 입력하여
    **Save**를 선택하세요.

- Name – +++**AoaiML_pf**+++

- Provider – **Azure OpenAI**를 선택하세요

- Subscription ID – **할당된 subscription**를 선택하세요

- Azure OpenAI Account Name – **AOAI-PF@lab.LabInstanceId**를 선택하세요

- Auth Mode – **API Key**를 선택하세요

- API Key – **Azure OpenAI resource**에서 저장된 **key**를 제공하세요

- API base – **Azure OpenAI resource**에서 저장된 **endpoint**를
  제공하세요

> ![A screenshot of a computer Description automatically
> generated](./media/image28.png)

![A screenshot of a computer Description automatically
generated](./media/image29.png)

3.  연결 생성이 성공적으로 이루어졌는지 확인하세요.

![A screenshot of a computer Description automatically
generated](./media/image30.png)

## 작업 3: Prompt flow를 생성하고 개발하기

1.  **Prompt flow** 홈페이지의 **Flows** 탭에서 프롬프트 플로우를
    생성하려면 **Create**를 선택하세요. **Create a new flow** 페이지에는
    생성할 수 있는 플로우 유형, 플로우를 생성하기 위해 복제할 수 있는
    내장된 샘플 및 플로우를 가져오는 방법이 표시됩니다.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

2.  **WebClassification** 범주에서 **Clone**를 선택하세요.

**Explore gallery**에서 기본 제공 샘플을 찾아보고 타일에서 **View
detail**를 선택하여 시나리오에 적합한지 미리 볼 수 있습니다.

이 실습에서는 **Web Classification** 샘플을 사용하여 기본 사용자 경험을
안내합니다.

Web Classification는 LLM을 사용한 다중 클래스 분류를 보여주는
플로우입니다. URL이 주어지면 플로우은 몇 번의 샷, 간단한 요약 및 분류
프롬프트를 사용하여 URL을 웹 범주로 분류합니다. 예를 들어, URL
https://www.imdb.com 이 주어지면 URL을 Movie로 분류합니다.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

3.  **Folder name**에 채워진 이름을 수락 한 후**Clone**을 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image33.png)

4.  Flow 실행을 위해서는 컴퓨팅 세션이 필요합니다. 연산 세션은 필요한
    모든 의존성 패키지를 포함하는 Docker 이미지를 포함하여
    애플리케이션을 실행하는 데 필요한 컴퓨팅 리소스를 관리합니다.

5.  Flow authoring 페이지에서 **Start compute session**을 선택하여
    컴퓨팅 세션을 시작하세요.

![A screenshot of a computer Description automatically
generated](./media/image34.png)

**참고:** 연산 세션이 Running 상태로 설정되는 데 약 **10분**이
소요됩니다.

![A screenshot of a computer Description automatically
generated](./media/image35.png)

## 작업 4: 플로우 작성 페이지를 검사하기

컴퓨팅 세션을 시작하는 데 몇 분 정도 걸릴 수 있습니다. 컴퓨팅 세션이
시작되는 동인 플로우 작성 페이지의 각 부분을 확인합니다

- 페이지 왼쪽의 **Flow **또는 *flatten* 보기는 노드를 추가 또는
  제거하거나 노드를 인라인으로 편집 및 실행하거나 프롬프트를 편집하여
  플로우를 작성할 수 있는 기본 작업 영역입니다. **Inputs** 및
  **Outputs** 섹션에서는 입력 및 출력을 보고, 추가하고, 제거하고, 편집할
  수 있습니다.

현재 Web Classification샘플을 복제할 때 입력 및 출력은 이미
설정되었습니다. 플로우에 대한 입력 스키마는 name: url입니다. type:
string, 문자열 유형의 URL. 사전 설정된 입력 값을 수동으로
https://www.imdb.com 와 같은 다른 값으로 변경할 수 있습니다.

- 오른쪽 상단의 **Files** 은 플로우의 폴더와 파일 구조를 보여줍니다. 각
  플로우 폴더에는 *flow.dag.yaml* 파일, 소스 코드 파일 및 시스템 폴더가
  포함되어 있습니다. 테스트, 배포 또는 협업을 위해 파일을 생성, 업로드
  또는 다운로드할 수 있습니다.

- 오른쪽 아래에 있는 **Graph** 보기는 플로우이 어떻게 보이는지
  시각화하기 위한 것입니다. 확대 또는 축소하거나 자동 레이아웃을 사용할
  수 있습니다.

**Flow**  또는 병합 보기에서 파일을 인라인으로 편집하거나 **Raw file
mode**토글을 켜고 **Files** 에서 파일을 선택하여 편집을 위해 탭에서
파일을 열 수 있습니다.

**Flow** 또는 병합 보기에서 파일을 인라인으로 편집하거나 **Raw file
mode**토글을 켜고 **Files** 에서 파일을 선택하여 편집을 위해 탭에서
파일을 열 수 있습니다.

![A screenshot of a computer Description automatically
generated](./media/image36.png)

## 작업 5: LLM 노드를 설장하기

각 LLM 노드에 대해 **Connection **을 선택하여 LLM API 키를 설정해야
합니다. Azure OpenAI 연결을 선택하세요.

연결 유형에 따라 드롭다운 목록에서 **deployment_name** 또는 모델을
선택해야 합니다. Azure OpenAI 연결의 경우 배포를 선택하세요. 

1.  summarize_text_content 경우 아래 세부 정보를 입력하세요

Connection – **AoaiML_pf**를 선택하세요

Api – **chat**를 선택하세요

deployment name – **gpt-4o-2024-11-20**를 선택하세요

![A screenshot of a computer Description automatically
generated](./media/image37.png)

2.  LLM 노드 **classify_with_llm**에 대해서도 비슷하게 연결을
    설정하세요.

![A screenshot of a computer Description automatically
generated](./media/image38.png)

3.  단일 노드를 테스트하고 디버그하려면 **Flow** 보기에서 노드 맨 위에
    있는 **Run** 아이콘을 선택하세요. **Inputs** 를 확장하고 플로우 입력
    URL을 변경하여 다른 URL에 대한 노드 동작을 테스트할 수 있습니다

4.  실행 상태는 노드 맨 위에 나타납니다. 실행이 완료되면 노드
    **Output** 섹션에 실행 출력이 나타납니다 .

5.  플로우의 시작 부분으로 이동하여 **fetch_text_content_from url**을
    실행하고 블록을 실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

**Graph **보기에는 단일 실행 노드 상태도 표시됩니다.

6.  **Inputs** 섹션에서 **Value** 필드의 값을
    +++https://play.google.com/store/apps/details?id=com.spotify.music+++로
    제공하세요

전체 플로우를 테스트라고 디버그하려면 오른쪽 위에서 **Run**을
실행하세요.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

## 작업 5: 플로우 출력 보기

플로우 출력을 설정하여 한 곳에서 여러 노드의 출력을 확인할 수도
있습니다. 플로우 출력은 다음을 지원합니다.:

- 단일 테이블에서 대량 테스트 결과를 확인하기

- 평가 인터페이스 매핑을 정의하기

- 배포 응답 스키마 설정하기

1.  자세한 입력, 출력, 플로우 실행 및 오케스트레이션 정보를 확인하려면
    위쪽 배너 또는 위쪽 메뉴 모음에서 **View outputs**를 선택하세요.

![A screenshot of a computer Description automatically
generated](./media/image41.png)

2.  Outputs 화면의 Outputs 탭에서 플로우은 **category**및 **evidence**가
    있는 입력 URL을 예측합니다. ![A screenshot of a computer Description
    automatically generated](./media/image42.png)

3.  **Outputs** 화면에서 **Trace** 탭을 선택한 후 **node name** 아래에서
    **flow** 을 선택하여 오른쪽 창에서 자세한 플로우 개요 정보를
    확인하세요. **Flow**을 확장하고 단계를 선택하여 해당 단계에 대한
    자세한 정보를 확인하세요

![A screenshot of a computer Description automatically
generated](./media/image43.png)

**요약:**

이 실습에서는 Azure Machine Learning Studio의 프롬프트 플로우를 사용하여
간단한 요약 및 분류 프롬프트를 사용하여 URL을 웹 범주로 분류하는 방법을
배웠습니다.
