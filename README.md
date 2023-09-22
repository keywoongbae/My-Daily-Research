# My-Daily-Research

## Overview

- [Analysis of Cyber risks related to natural disasters (in POSTECH)](#analysis-of-cyber-risks-related-to-natural-disasters-postech) 
  - [2023.09](#2023-09-06)
  - [2023.08](#2023-08-31)
  - [2023.07](#2023-07-28)
  
- [Research on the robustness of Diffusion Generative models (in Inha University)](#research-on-the-robustness-of-diffusion-generative-models-inha-university) 
  - [2023.09](#2023-09-18-)
  - [2023.04](#2023-04-01)
  - [2023.03](#2023-03-27)

## Analysis of Cyber risks related to natural disasters (POSTECH)
### 2023-09-18

- 교수님과의 미팅을 하기 전, 어떤 부분들을 점검해야하는지 정리했다.
  - (1) 데이터셋에 해당되는 자연재해 키워드가 여러 개라고 해서 이것들을 복제하여 따로 두는 것이 맞는지에 대한 검토 필요.
  - (2)  BERT를 Pre-training시킬 때 `test.csv`를 `train.csv`처럼 "자연"재해와 관련된 것들과 아닌 것들로 구분해서 다시 학습을 시켜야 함.
  - (3) 과연 내가 학습시킨 BERT 모델이 robust한지도 검증.
- 교수님과의 미팅에서 아래의 내용들을 다루었다.
  - **우리만의 Text Mining Method**을 구축하자.
    - 교수님이 스위스 연구소에 계실 때 한 석사과정이 텍스트데이터 마이닝과 관련된 논문을 작성하였음.
    - "asset", "outcome", "actor" 이렇게 세 분야로 구분해놓고, 세 분야에 공통으로 포함되는 단어들만 추출하는 방식을 채택했다고 함.
    - BERT를 사용한 것 자체가 학술적으로 논문이 되기는 어려우나, 수리적인 모형(Pseudo Code)으로 논문에 작성하면 좋을 것 같음.
  - BERT를 사전학습을 시켜 자연재해 분류에 사용했던 것처럼 **사이버리스크에도 적용**할 수 있을까?
    - 웹상에는 사이버리스크 분류에 적합한 데이터셋이 없음.
    - 따라서 기존 사이버리스크 데이터셋 (Advisen, PIC)등을 사용하여 BERT를 학습시켜보자.
  - **Description으로부터 리스크 타입을 분류하는 과정이 중요**한 이유는?
    - 본 연구의 목적이 Cyber Risk, Natural disaster Risk, Operational Risk 사례들의 특징을 추출하는 것이므로, 처음에 분류를 잘해야 나중에 문제가 없을 것 같기 때문.
    - 이는 전에 디퓨전모델 연구하는 과정에서 초반에 대충 넘어간 것들이 막판에 수면 위로 올라와 고생하면서 얻은 교훈이 반영된 나의 결론임. 😁
- 이번 미팅을 통해 **이번주 내가 해야할 Task**는 다음과 같다.
  - **(1) 자연재해 트위터 데이터셋 test.csv에서  자연재해 사례들과 아닌 것들로 분류해서 BERT 학습 다시 시키기.**
    - validation을 위한 데이터셋도 준비해서 BERT의  robustness를 검증하는 것도 나쁘지 않을듯.
  - **(2) 사전학습 데이터셋은 다음과 같이 구성해서 BERT 학습시키기.**
    - Cyber Risk : 어드바이젠 데이터셋
    - Natural Risk : Tweets Natural disaster Risk 
    - Operational Risk : Tweets non-Natural disaster Risk (아니면 추가로 찾아봐도 될 듯).

### 2023-09-17

- 현재 **SAS데이터셋에서 Natural Disaster과의 relation여부를 분류**하는 작업을 하고 있다.
  - SAS 데이터셋은 총 3만개정도이므로, 데이터셋 하나하나를 읽고 분류하는 것은 불가능함.
  - 그러기에 대표적인 NLP모델인 [BERT](https://www.kaggle.com/code/yinchienpai/disaster-tweets-prediction-bert-pytorch)를 사용하여 분류를 진행하였음.
  - <img src=".\img\bert_pretraining.png" alt="Cross-lingual Information Retrieval with BERT – arXiv Vanity" style="zoom:50%;" />
  - Natural Disaster Classification 데이터셋을 [Kaggle](https://www.kaggle.com/competitions/nlp-getting-started)에서 다운로드 한 후, 이를 BERT에 넣어 사전학습시킨 후, SAS 데이터셋에 fine-tuning 하였음.
  - 아쉽게도, Kaggle에는 Cyber Risk와 관련된 데이터셋은 없었음. (대신 Cyber Security와 관련된 데이터셋은 있었음.)
  - 이렇게 해서 기존에 분류된 것과 BERT가 분류한 것을 비교하여 그 유사도를 검증할 예정임.
  - 총 데이터셋이 38056개라서 전처리하는데 시간이 좀 많이 걸림. 전처리 돌려놓고 저녁이나 먹고 와야겠다.

### 2023-09-16

- **한 개의 사건에서 여러 개의 자연재해 키워드가 추출되는 문제가 발생**하였다.

  - | ID   | Description of Event                                         | keyword               |
    | ---- | ------------------------------------------------------------ | --------------------- |
    | 1    | There was an `earthquake` and ......<br />... also `typhoon` occured ...... | (earthquake, typhoon) |
    | 2    | The `fire` was ...                                           | fire                  |
    | 3    | This was...                                                  | None                  |

  - 위와 같이 자연재해와 관련된 키워드가 여러 개 있는 경우가 있었다. 이런 경우는 **아래와 같이 셀을 복사하여 해결**하였다. 총 데이터셋은 늘어났다. ($37648 \rightarrow 38056$)

  - | ID   | Description of Event                                         | keyword    |
    | ---- | ------------------------------------------------------------ | ---------- |
    | 1    | There was an `earthquake` and ......<br />... also `typhoon` occured ...... | earthquake |
    | 2    | There was an `earthquake` and ......<br />... also `typhoon` occured ...... | typhoon    |
    | 3    | The `fire` was ...                                           | fire       |
    | 4    | This was...                                                  | None       |

### 2023-09-11

교수님과 미팅을 하였고 다음과 같이 연구를 진행하기로 하였다.

- SAS 데이터셋을 <u>(1) cyber risk</u>, <u>(2) Nat-Cat(Natural-Catastrophe)</u>, <u>(3) Operational Risk(일반적인 리스크)</u>로 분류한다.
  - 이때, 각 risk에 대한 분포가 어떠한 분포(포아송분포, 감마분포 등등)를 따르는지 fitting한다. 데이터 개수는 50개 이상이면 분포를 따른다고 가정한다.
  - Loss-Frequency 그래프
  - SAS는 전반적인 리스크를 다룬 데이터셋이다. Description은 주요한 몇몇 케이스는 eye2eye로 읽되, systematic한 방법을 구축한다. 
- 어떤 분포를 따르는지를 분석하였으면, 이를 이용하여 요인을 분석한다. 
  - 이때 사용할 모델은 GLM, GAM을 사용한다.
- 추정한다.
  - 연년별 손실을 추정한다.
- 그리고 시나리오 분석을 한다.
  - 이를 통해 Nat-Cat을 통해 발생한 Cyber risk경우를 바탕으로 서로의 관계를 분석한다.

다음과 같이 진행하기로 하였으며, 우선은 이번 일주일은 SAS 데이터셋을 전처리한다.

### 2023-09-08

노가다 분류를 하면서 발견함 점

- 대부분의 단어가 1차원적 의미가 아니라 다른 비유적 의미로 사용됨.
- 1차원적 의미로 쓰이는 사례들도 대부분 "자연"재해는 아님.
- flood는 대부분 트래픽이 넘치다의 표현으로 쓰임. fire는 실행, 혹은 총격과 관련된 표현으로 쓰이며, 자연적으로 발생한 불이 아닌 인위적, 고의적으로 일으킨 방화가 다수임.
- `iffy`는 자연재해가 아니거나, 사건의 근본적인 원인이 아닌 경우에 라벨링하였음.
- 또 데이터셋 설명이 대부분 돌고 도는 것 같음. 비슷한 것들이 자주 보임.
- 과연 이 데이터셋이 이머징 리스크 연구에 적합할까? 이런 생각도 듦.

결과적으로 다음과 같은 의문점이 듦.

- 어드바이젠 데이터셋을 이용하여 자연재해 리스크를 연구한 전례/논문이 있는지?
  
  - 생각보다 자연재해가 직접적인 원인이 되어서 발생하는 사이버리스크가 없기 때문.
  - 접근 방향 : 주제 변경 or **데이터셋 변경** or 강행 or 예상치못한 솔루션?!
  
- 내가 정리해 본 **어드바이젠 데이터셋 관련 논문 리스트**

  - | Title                                                  | Jrnl/Cnf                             | Description                                                  |
    | ------------------------------------------------------ | ------------------------------------ | ------------------------------------------------------------ |
    | The drivers of cyber risk                              |                                      | 두터운 꼬리 분포를 띄는 사이버리스크는 회사의 크기가 클수록, 사건이 우발적일수록(극단적인 케이스 제외), 디지털 사용 점유율(클라우드 서비스)이 클수록 커지며, 피싱/스키밍 분야의 사건이 cyber cost가 높다. |
    | Time dynamics of cyber risk                            |                                      | Advisen, SAS, PRC 총 세 개의 데이터셋을 사용함.<br />사이버 위험 빈도가 더 빠르게 증가함을 보여줌.<br />사이버 위험 관리와 사이버 위험의 불안정성을 이해하는데 중요하다. |
    | Cyber Risk Frequency, Severity and Insurance Viability | Insurance: Mathematics and Economics | Advisen을 통해 미국의 사이버 보험산업에 대한 보험 위험 이전에 대한 탐구를 수행.<br />1) 사이버 손실 이벤트의 빈도와 심각도를 설명할 수 있는 가장 중요한 공변량<br />2) 필요한 보험료, 위험 풀 크기에 관련하여 사이버 위험의 보험 가입 여부<br />사이버 위험의 불안정성과 본질에 대한 몇 가지 새로운 핵심 통찰력을 제공하고 위의 질문을 해결. |
    
    

### 2023-09-07

키워드 분류 문제는 다음과 같이 해결한다.

- 독일 친구가 한 내용을 보니, 키워드 당 해당되는 데이터가 최대 197개, 200개 정도였으므로 **<u>하나하나 읽어보면서 분류를 했을 가능성이 커보임.</u>** 
- 따라서 **<u>deepl을 적극 활용하여 일일이 분류를 해야 할 것 같음</u>**. 총 504개의 데이터이며, 중복되는 것도 꽤 있음. 하루~이틀 정도 날 잡고 빡세게 분류를 한다. 도저히 생각해봐도 다른 방법이 떠오르지를 않는다.
- 데이터 전처리 방식은 **다음 4가지 조건**을 가지고 있어야 함.
  - price가 ice로 분류되는 경우를 방지하기 위해 순수하게 단어만 포함되도록 설정 (확인 ✅)
  - 특수 부호(,.!)를 제외하고 단어만 취급 (확인 ✅)
  - 데이터셋을 전부 소문자로 변환하여 탐색
  - 병합된 데이터셋에 셀을 직접 추가하여 Yes / No로 채워넣은 후, 완성되면 import하여 다시 전처리를 시작함

### 2023-09-06

오늘은 데이터 merge후, 자연재해와 관련된 키워드를 이용해서 `result["CASE_DESCRIPTION"]`에서 키워드가 포함이 되는지를 판단 후, <u>자연재해와의 관련여부</u>와 <u>어떤 자연재해와 관련있는지</u>를 데이터프레임에 추가하는 작업을 하였다. 하는 과정에서 다음과 같은 문제가 발생하였다.

- 키워드가 중복이 되는 경우는 어떻게 할 것인가?
  - case description에서 `Fire`과 `Earthquake` 두 개의 키워드가 포함되어 있으면 어떻게 구별해야 할까?
  - 사실 이 문제는 나중에 해결해도 됨. 그리고 심할 경우, 셀을 복붙해서 자연재해 키워드만 다르게 해도 될듯.
- 키워드가 있다고 과연 그 데이터셋이 진짜 자연재해와 관련된 데이터셋일까?
  - 이게 제일 큰 문제. 예를 들면, ice cream. ice makers. 이런 경우도 `ice`에 분류가 된다.
  - "Further, Plaintiff also began receiving a flood of calls and text messages from Defendants offering mortgage refinancing." 이 경우는 "전화가 쏟아진다"는 비유적 표현인데도 `flood`로 포함이 되어 자연재해 데이터셋으로 분류가 됨.
  - 이런 경우에는 어떻게 해결하지...?
    - 일일히 읽어서 분류한다..? -> 너무 시간낭비
    - 딥러닝을 이용해서 파인튜닝...? -> 과연 할 수 있을까?
    - 독일친구는 어떻게 분류를 했지?

내일 혼자 좀 더 고민해본 후 교수님께 말씀드려보아야 할 것 같다.

### 2023-08-31

오늘 오후 3시에 교수님과 줌미팅을 하였다. 줌미팅 내용은 지난 한 달동안 어떤 것들을 하였는지를 말씀드리는 것이었다. 

크게 (1) 논문을 읽고 들었던 생각과 (2) 어드바이젠 데이터셋을 이용하여 논문의 내용들을 어떻게 구현하였는지를 말씀드렸다. 

교수님께서는 줌미팅 이후, 크게 세 가지를 말씀해주셨다.

- 자연재해와 cyber cost를 연결지을 것.
  - 나 이전의 독일 출신이셨던 석사생께서 하시던 분야이다. 자연재해와 cyber cost를 연결 짓기. 그러기 위해선 우선 데이터셋 내 자연재해에 대한 정보를 긁어와야 할 것 같다. 음 그런데 자연재해와 관련된 칼럼이 없는 것 같은데.... 이 부분은 내일 따로 말씀드려봐야 할 것 같다. 그러고 난 후, 키워드로 조합하기.
- 테이블 join해서 하나의 데이터 프레임으로 구축하기.
  - 어드바이젠 데이터셋은 총 4개의 테이블이 있다. 이 테이블들을 공통된 칼럼을 기준으로 join을 해보라고 하셨다. 그렇게 하면 새로운 안목이 생길 수 있기 때문.
- 딥러닝 시계열 모델을 이용한 연구도 괜찮은 contribution
  - 논문을 읽으면서 딥러닝과의 접목 가능성에 대해서도 생각해보았고, 교수님께서도 괜찮다고 하셨다. 이 부분은 나중에 더 발전시켜봐야지.

### 2023-07-30

현재 "The drivers of cyber risk"논문을 읽으면서 정리하고 있다. 이 논문을 간단하게 읽으면서 (1) cyber risk란 무엇인지, (2) advisen dataset을 어떻게 다루는지 두 가지를 알아보려고 한다. 정리한 내용은 markdown으로 정리하면서 깃허브에 commit한다. 

그리고 advisen 데이터셋 구성을 잠깐 보았는데, 어떻게 전처리를 해야하는지 감이 안잡힌다. 내일 일어나서 폴더 안에 전처리 가이드가 있는지 확인한 후에 교수님께 방향성에 대해 질문을 드리고자 한다.

### 2023-07-28

대학원 입학 전, 교수님과 연구 주제에 대해서 사전 미팅을 하였다. 내가 맡게 될 연구 주제는 **기후 변화로 인해 발생하는 사이버 리스크의 관리**이다. 교수님께서 원하는 연구주제가 있다면 편하게 말하라고 하셨으나, 아직 이 분야에 대해 모르는 부분들이 많기 때문에 일단 해본 후에 판단을 해보고 싶다고 말씀드렸다.

연구를 하면서 내가 다루게 될 데이터셋은 미국 Advisen에서 만든 데이터셋 (https://www.advisenltd.com/data/) 이며, 교수님께서 말씀하신 나의 task는 아래 2가지 이다.

- Advisen 기반의 사이버리스크 관련 논문들 읽고 흐름 잡기
- Advisen 데이터셋 전처리하기

사실 메인은 2번 째 전처리 하기이며, 현 날짜로부터 3주 뒤인 8월 21-22일 사이에 연구 진행과정을 review하기로 하였다.

<hr>

## Research on the robustness of Diffusion Generative models (Inha University)

### 2023-09-22

실험을 하고 있는 동안, 논문을 수정하는 중이다. 수정은 Open Review 평가를 최대한 반영해서 하고자 한다.

- | Review                                                       | My Feedback                                                  |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | 분석을 통해 향후  DDPM 관련 연구에 어떤 영감을 줄 수 있나?   |                                                              |
  | DDPM의 본질과 연결지을 수 있나?                              |                                                              |
  | 본 논문은 설명할 수 없는 분석을 수행하며 손상된 데이터에서 DDPM 생성 또는 성능을 개선할 수 있는 방법을 제시하지 않음. | (1) 설명이 불가능하다.<br />-> 어느정도는 인정. 딥러닝이 블랙박스이기 때문에 최대한 다양한 환경에서 실험을 하였음. 그리고 귀납적으로 성능을 평가하고자 함.<br />(2) DDPM의 성능 개선<br />본 논문에서는 DDPM의 성능 개선하는 것이 목적이 아니라, DDPM의 성능저하 시점을 알고자 하는 것. |
  | 동기가 불분명하다.                                           | ????????????????????<br />왜 동기가 부족하지?<br />아니 이사람들이... 논문 제대로 읽어보지도 않았으면서!!!!! |
  | 대규모 고해상도 데이터셋으로 실험을 진행할 것.               | 시간이 남으면 corrupted imagenet으로 학습시킬 것.            |
  | Preliminary에 관련없는 내용들이 많음. 더 간결하게 만들 수 있음. 견고성에 대한 연구와 같은 유용한 정보가 부족함. | B.1.을 더 줄이고, B.2.를 좀더 강조해서 쓸 것.                |
  | 기술적 기여가 부족하고, 손상프로세스만 추가하고 그 방법이 매우 간단하다. |                                                              |
  | 부패의 강도가 DDPM의 성능에 어떤 영향을 미치는지에 대해 더 자세히 설명해야 함. | DDPM모델에 가장 치명적이었던 fog corruption을 강도에 따라 FID 스코어를 측정하는 실험을 부록에 추가할 것. |
  | 그림4를 더 고해상도의 그림으로 대체할 것.                    | 이건 뭐 하면 되고... 정 안되겠으면 직접 그리자.              |
  | Introduction에서 견고성을 이야기하다가 갑자기 DDPM에 대한 설명으로 넘어가서 혼란스러움. |                                                              |

그리고 아래는 논문을 퇴고하면서 수정할 사항과 수정완료 여부를 정리해놓은 표이다.

- | 수정사항                                                     | 이유 / Details                                               | ✅    |
  | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
  | Return point -> Return state로 수정하기                      | figure 3을 수정하려고 하는데, 이때 수직선을 사용해서 DDPM-C의 메커니즘을 설명하려고 한다. <br />DP수업 때 주로 state라는 용어를 많이 사용하는데, 이를 적용해보면 어떨까 한다. 수정하면서 더 좋은 아이디어 있으면 오케이. |      |
  | Introduction에서 Robustness▶DDPM부분 연결 매끄럽게 하기      | Review 반영사항.                                             |      |
  | Introduction에서 굳이 DDPM에 대한 설명을 해야하나? DDPM-C를 설명하면 안되나? | 뒤에서 계속 반복해서 나와 좀 지루하기도 하고, 중요하지 않아 보임.<br />(notation에는 딱히 안해도 될 거 같음.)<br />Methodology에서 설명해도 될 것 같음. |      |
  | Introduction에서 DDPM-C의 contribution을 설명할 때, 96개의 실험 환경 세팅했다, | + severity에 따른 DDPM 성능 저하 정도 실험도 추가<br />어필할 건 적극적으로 논문 내에서 어필하자. |      |
  | Introduction에서 DDPM-C 동기 설명할 때 "순수한 접근(naive approach)"라는 내용 적으면 좋을 듯. | 노이즈가 DDPM Return ability에 끼치는 영향력 + severity에 따라 그 변화의 정도를 알고자 하는 순수한 접근. |      |
  | 용어 설명                                                    | Original state, original image, return state, return image, return ability, <br />2.1장에서 Return state뭐 이런것들 설명하기.<br /> |      |
  | 3장에서 Figure2 해상도 높이기                                | Review 반영사항.                                             |      |
  | 3장에서 왜 FID를 사용하는지 작성하기                         | Review 반영사항.                                             |      |
  | B장에서 adversarial examples에 대한 related work 조사해서 넣기. | Review 반영사항.                                             |      |
  | C장 Extra experiments 넣기.                                  | Review 반영사항 (부패의 강도에 따른 DDPM의 성능변화).        |      |
  | Introduction과 Abstract에 실험 더 추가한거랑 contribution 등등 추가해서 넣기 | 어필할 건 적극적으로 어필하자.                               |      |
  | D장 그림 다시 수정하기                                       | 엉터리였다. 수정 필요. (근데 왜 언급이 없지..?)              |      |
  | A장에서 mechanism 수직선으로 표현하기.                       | 보다 더 나은 이해를 위해...<br />단순히 왔다 갔다 한다기 보단, 노이즈의 시점으로 이 메커니즘을 이해하면 좋을 것 같기 때문에.. |      |

  

### 2023-09-21

Fog-severity 실험을 진행하고 있다. 실험 진행상황은 다음과 같다.

- 실시간으로 수정할 예정.

- | 데이터셋 ($\lambda$)  | 실험 완료 | 생성 완료 | PNG 변환($x_g$) | PNG 변환($x_0$) |  FID  |
  | :-------------------: | :-------: | :-------: | :-------------: | :-------------: | :---: |
  |     **MNIST** (1️⃣)     |     ✅     |     ✅     |        ✅        |        ✅        | 33.7  |
  |     **MNIST (2️⃣)**     |     ✅     |     ✅     |        ✅        |        ✅        | 16.39 |
  |     **MNIST (3️⃣)**     |     ✅     |     ✅     |        ✅        |        ✅        | 16.30 |
  |     **MNIST (4️⃣)**     |     ✅     |     ✅     |        ✅        |        ✅        | 17.77 |
  |     **MNIST (5️⃣)**     |     ✅     |     ✅     |        ✅        |        ✅        | 24.15 |
  |                       |           |           |                 |                 |       |
  | **Fashion MNIST (1️⃣)** |     ✅     |     ✅     |        ✅        |        ✅        | 42.8  |
  | **Fashion MNIST (2️⃣)** |     ✅     |     ✅     |        ✅        |        ✅        | 22.02 |
  | **Fashion MNIST (3️⃣)** |     ✅     |     ✅     |        ✅        |        ✅        | 24.24 |
  | **Fashion MNIST (4️⃣)** |     🔄     |     🔄     |                 |        ✅        |       |
  | **Fashion MNIST (5️⃣)** |     🔄     |           |                 |        ✅        |       |
  |                       |           |           |                 |                 |       |
  |   **CIFAR-10 (1️⃣)**    |     ✅     |     ✅     |        ✅        |        ✅        | 39.23 |
  |   **CIFAR-10 (2️⃣)**    |     ✅     |     ✅     |        ✅        |        ✅        | 37.60 |
  |   **CIFAR-10 (3️⃣)**    |     ✅     |     ✅     |        ✅        |        ✅        | 39.07 |
  |   **CIFAR-10 (4️⃣)**    |     ✅     |     ✅     |        ✅        |        ✅        | 40.12 |
  |   **CIFAR-10 (5️⃣)**    |     ✅     |     ✅     |        ✅        |        ✅        | 38.66 |
  |                       |           |           |                 |                 |       |
  |   **CIFAR-100 (1️⃣)**   |     ✅     |     ✅     |        ✅        |        ✅        | 44.11 |
  |   **CIFAR-100 (2️⃣)**   |     ✅     |     ✅     |        ✅        |        ✅        | 48.41 |
  |   **CIFAR-100 (3️⃣)**   |     ✅     |     🔄     |                 |        ✅        |       |
  |   **CIFAR-100 (4️⃣)**   |     🔄     |     🔄     |                 |        ✅        | 48.40 |
  |   **CIFAR-100 (5️⃣)**   |     ✅     |     ✅     |        ✅        |        ✅        | 45.15 |
  |                       |           |           |                 |                 |       |
  
- 학습하는데 한 에폭당 6분, 총 50에폭이므로 300분 = 5시간 정도가 걸린다. 

### 2023-09-18-

- 현재 severity에 따른 FID score변화 정도를 확인하기 위해 실험을 진행하고 있다. 그 과정에서 아래 오류들이 발생하였다.

  - ```
    DNN library is not found.
    ```

  - ```
    ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    tensorflow-datasets 4.9.2 requires tensorflow-metadata, which is not installed.
    tensorflow-text 2.13.0 requires tensorflow-hub>=0.8.0, which is not installed.
    tf-models-official 2.13.2 requires tensorflow-hub>=0.6.0, which is not installed.
    tensorflow-datasets 4.9.2 requires protobuf>=3.20, but you have protobuf 3.19.6 which is incompatible.
    tensorflow-text 2.13.0 requires tensorflow<2.14,>=2.13.0; platform_machine != "arm64" or platform_system != "Darwin", but you have tensorflow 2.11.0 which is incompatible.
    tf-models-official 2.13.2 requires tensorflow~=2.13.0, but you have tensorflow 2.11.0 which is incompatible.
    ```

  - ```
    ImportError: cannot import name 'convert_to_tensor_v2_with_dispatch' from 'tensorflow.python.framework.ops' (/home/osanie/.local/lib/python3.8/site-packages/tensorflow/python/framework/ops.py)
    ```

- 100% 버전 문제라 확신하고 아래의 조치들을 취하여 해결하였다.

  - (1) `! pip uninstall tensorflow tensorflow-estimator tensorflow-gpu tensorflow-hub tensorflow-metadata tensorflow-probability`
  - (2) `! pip install tensorflow==2.11.0`
  - (3) `! pip uninstall tensorflow-datasets`
  - (4) `! pip uninstall tensorflow-text -y`
  - (5) `! pip uninstall tensorlfow-datasets -y`

- Log들을 차분하게 잘 살펴보자라는 나의 교훈이 실현된 하루였다 😁

### 2023-04-06

| Title                           | Success/Fail | Detail&Feedback |
| ------------------------------- | :----------: | --------------- |
| Re-train DCGAN and WGAN         |      🔄       |                 |
| Translate and make up for paper |      🔄       |                 |

### 2023-04-05

| Title                           | Success/Fail | Detail&Feedback                                              |
| ------------------------------- | :----------: | ------------------------------------------------------------ |
| Re-train DCGAN and WGAN         |      🔄       | - DCGAN의 generator 마지막 layer의 활성화 함수가 `sigmoid`로 되어있었음<br />- Input image는 -1과 1 사이로 정규화하였기 때문에 `tanh`로 설정해주어야 함.<br />- 변경 후, 재학습 진행 중임. WGAN도 여러 부분에서 애매하다는 느낌이 들어 재학습을 진행하였음. |
| Translate and make up for paper |      🔄       |                                                              |

### 2023-04-04

| Title                               | Success/Fail | Detail&Feedback                                              |
| ----------------------------------- | :----------: | ------------------------------------------------------------ |
| Calculating FId score again.        |      ✅       | - fashionM에서 예상치 못한 결과가 발생함. $^{1}$             |
| 데이터셋 확인 및 정리하기           |      ✅       | 1. np.uint8로 변환<br />2. 크기 (5000,64,64,3)으로 통일      |
| fashionM identity 학습 및 생성 진행 |      🔄       | - fashionM identity 생성 이미지가 잘못 생성되었음. 학습부터 생성까지 다시 진행하고 있다. |

**$^{1}$ 결과 원인** 

1) 데이터 타입이 다른 데이터셋은 np.uint8인 반면, fashionM은 np.float32였음.  $\rightarrow$ <u>np.uint8로 예쁘게 만들어준다.</u>

<img src="./img/figure1.png" alt="Github_Logo" style="zoom:33%;" />

2) 한 데이터셋은 (5000,28,28,3)이었고, 다른 한 데이터셋은 (5000,64,64,3)이었다. $\rightarrow$ <u>모든 데이터셋 (5000,64,64,3)으로 통일시켰음.</u>
3) 생성된 이미지 중 하나를 출력했는데 아래와 같이 순수 노이즈 이미지가 나왔다. $\rightarrow$ <u>다시 학습시켜 생성 중.</u>

<img src="./img/figure3.png" alt="Github_Logo" style="zoom:33%;" />

**<u>오늘의 교훈 : 데이터 전처리는 통일된 형태, 규칙적인 형태로 진행하는 것이 좋다. 아니면 오늘과 같은 현상 반복될 듯.</u>** 

### 2023-04-03

| Title            | Success | Detail&Feedback                                              |
| ---------------- | :-----: | ------------------------------------------------------------ |
| GPU Set_Up Again |    ✅    | - 현재 사용하고 있는 버전은 nvidia-smi 510.85.02 / cuda 11.6 / cudnn 8.8.0 /  tensorflow-gpu 2.11.0. 이렇게 하면 호환이 되지 않으며, cudnn버전을 8.2.0으로 변경하여 재설치하였음. |

**오류 원인**

- nvidia driver version, cuda version, cudnn version, tensorflow-gpu version 등 버전을 맞추기 위해서 계속 설치하고 했는데도 잘 안됨.
- `import tensorflow as tf` 를 입력하고 나오는 에러를 자세히 읽어보니 `from tensorflow.keras.scipy` (자세히 기억 안남)에서 오류가 잡힌 것을 발견.
- `pip install scipy`해서 최신 버전으로 재설치 하니 돌아갔다.
- 오류 원인은 아마도 `pip install fid-score`를 할 때 requirement내 scipy버전이 최신버전이 아니여서 발생한 것 같다. 
- **<u>오늘의 교훈 : 오류가 발생했을 때는 컴퓨터가 나에게 하는 말을 찬찬히 자세히 읽어보자!</u>**

### 2023-04-02

| Title                | Success/Fail | Detail&Feedback                                              |
| -------------------- | :----------: | ------------------------------------------------------------ |
| Calculate FID scores |      ✅       | - 원하는 결과가 안나왔음. <br />- 연구주제 변경 및 보완 필요함. |

### 2023-04-01

| Title                | Success/Fail | Detail&Feedback |
| -------------------- | :----------: | --------------- |
| Calculate FID scores |      🔄       |                 |

### 2023-03-31

| Title                                                | Success/Fail | Detail&Feedback                                              |
| ---------------------------------------------------- | :----------: | ------------------------------------------------------------ |
| Generating images of FashionM motion blur from dcgan |      ✅       |                                                              |
| Generating images of FashionM motion blur from wgan  |      ✅       |                                                              |
| Generating images of mnist identity from ddpm        |      ✅       | - 검토를 해보니 images of mnist identity from ddpm의 shape이 (3000,64,64,3)로 찍혔음. 다시 생성하였음. |
| Generating images of cifar100 motion blur from ddpm  |      ✅       |                                                              |
| Calculate FID scores                                 |      ❌       | - Kernel Dead 현상을 방지하기 위해 batch size를 적절하게 줄인다. |

### 2023-03-30

| Title                             | Success/Fail | Detail&Feedback                                              |
| --------------------------------- | :----------: | ------------------------------------------------------------ |
| Generating images from ddpm       |      🔄       | - 내일 오후정도면 이미지 추출 완료예정.<br />- 현재 4개 중 2개의 데이터셋 추출 완료. |
| Translating papers (part 1,2,3)   |      🔄       |                                                              |
| Illustrating figures with draw.io |      🔄       |                                                              |

### 2023-03-29

| Title                                                        | Success/Fail | Detail&Feedback                                              |
| ------------------------------------------------------------ | :----------: | ------------------------------------------------------------ |
| Generating images from ddpm                                  |      🔄       | - 아침에 생성 알고리즘에서 오류 발견<br />- 생성 중단하고 오류 수정 뒤 다시 진행. |
| Translating papers (part 1,2,3)                              |      ❌       |                                                              |
| Summarizing papers related to the vulnerability of Deep learning |      ❌       |                                                              |

### 2023-03-28

| Title                                     | Success | Detail&Feedback                |
| ----------------------------------------- | :-----: | ------------------------------ |
| GPU Set UP with installing cudnn          |    ✅    | ipynb 파일 3개 이상 실행 못함. |
| Training dcgan with fashion mnist dataset |    ✅    |                                |
| Training wgan with fashion mnist dataset  |    ✅    |                                |
| Training ddpm with fashion mnist dataset  |    ✅    |                                |
| Generating images from ddpm               |    🔄    |                                |

### 2023-03-27

| Title                            | Success/Fail | Detail&Feedback                                              |
| -------------------------------- | :----------: | ------------------------------------------------------------ |
| GPU Set_UP with installing cudnn |      🔄       | - GPU Set Up시에는 tensorflow-gpu, CUDA, cudnn 세 개의 버전이 다 호환이 되어야 함.<br />- cudnn의 버전 재설치 및 설정 필요 |
