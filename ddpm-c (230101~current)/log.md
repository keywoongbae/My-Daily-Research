# DDPM-C


> Research from 2023.01~</br>
> Written since 2023.03.27~

### 2023.03.27

| Title                            | Success | Detail&Feedback                                              |
| -------------------------------- | ------- | ------------------------------------------------------------ |
| GPU Set UP with installing cudnn | 🔄       | - GPU Set Up시에는 tensorflow-gpu, CUDA, cudnn 세 개의 버전이 다 호환이 되어야 함.<br />- cudnn의 버전 재설치 및 설정 필요 |

### 2023.03.28

| Title                                     | Success | Detail&Feedback                |
| ----------------------------------------- | ------- | ------------------------------ |
| GPU Set UP with installing cudnn          | ✅       | ipynb 파일 3개 이상 실행 못함. |
| Training dcgan with fashion mnist dataset | ✅       |                                |
| Training wgan with fashion mnist dataset  | ✅       |                                |
| Training ddpm with fashion mnist dataset  | ✅       |                                |
| Generating images from ddpm               | 🔄       |                                |

<hr>

### 2023.03.29

| Title                                                        | Success | Detail&Feedback                                              |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| Generating images from ddpm                                  | 🔄       | - 아침에 생성 알고리즘에서 오류 발견<br />- 생성 중단하고 오류 수정 뒤 다시 진행. |
| Translating papers (part 1,2,3)                              | ❌       |                                                              |
| Summarizing papers related to the vulnerability of Deep learning | ❌       |                                                              |

<hr>

### 2023.03.30

| Title                             | Success | Detail&Feedback                                              |
| --------------------------------- | ------- | ------------------------------------------------------------ |
| Generating images from ddpm       | 🔄       | - 내일 오후정도면 이미지 추출 완료예정.<br />- 현재 4개 중 2개의 데이터셋 추출 완료. |
| Translating papers (part 1,2,3)   | 🔄       |                                                              |
| Illustrating figures with draw.io | 🔄       |                                                              |

<hr>

### 2023.03.31

| Title                                                | Success | Detail&Feedback                                              |
| ---------------------------------------------------- | ------- | ------------------------------------------------------------ |
| Generating images of FashionM motion blur from dcgan | ✅       |                                                              |
| Generating images of FashionM motion blur from wgan  | ✅       |                                                              |
| Generating images of mnist identity from ddpm        |         | - 검토를 해보니 images of mnist identity from ddpm의 shape이 (3000,64,64,3)로 찍혔음. 다시 생성하였음. |
| Generating images of cifar100 motion blur from ddpm  |         |                                                              |
| Calculate FID scores                                 |         | - Kernel Dead 현상을 방지하기 위해 batch size를 적절하게 줄인다. |

<hr> 

