# My-Daily-Research

## Overview

- [Management of Cyber risk based on natural disasters](#Management-of-Cyber-risk-based-on-natural-disasters)
  - [2023.09](#2023-09-06)
  - [2023.08](#2023-08-31)
  - [2023.07](#2023-07-28)
- [Research on the robustness of Diffusion Generative models](#Research-on-the-robustness-of-Diffusion-Generative-models)
  - [2023.04](#2023-04-01)
  - [2023.03](#2023-03-27)

## Management of Cyber risk based on natural disasters
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

연구를 하면서 내가 다루게 될 데이터셋은 미국 Advisen에서 만든 데이터셋(https://www.advisenltd.com/data/)이며, 교수님께서 말씀하신 나의 task는 아래 2가지 이다.

- Advisen 기반의 사이버리스크 관련 논문들 읽고 흐름 잡기
- Advisen 데이터셋 전처리하기

사실 메인은 2번 째 전처리 하기이며, 현 날짜로부터 3주 뒤인 8월 21-22일 사이에 연구 진행과정을 review하기로 하였다.

<hr>
## Research on the robustness of Diffusion Generative models

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