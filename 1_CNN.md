# Introduction
- CNN은 이미지를 분석하는데 활용되는 Deep neural network라고 할 수 있다.
- 합성공 신경망(Convolution neural network,cnn)은 1989년 LeCun의 논문에서 제안한 모델이다. 

# DNN의 문제점
1. 위치 정보 손실
- DNN은 이미지 정보를 입력으로 받을 때 그 위치의 중요도는 모두 동일하다고 간주하고 1차원 Vector로 표현하여 실제 위치 정보를 잃어버리는 것이다.
- 그러다보니, 글자 이미지의 위치를 조금만 이동시키거나 크기가 달라지거나, 변형이 생겨도 모델이 다른 이미지라고 판단할 수 있기 때문에 변형된 이미지로 새롭게 학습해야 한다.
- 이처럼 많은 데이터로 학습시켜야 하기 때문에 시간이 오래 걸린다.
2. Parameter수 증가
- DNN은 간단한 모델 학습을 위한 parameter의 수가 엄청나게 많다.
- 만약, image 크기가 크고, hidden layer 개수가 많아지면 parameter 수는 급격하게 늘어날 수 있다.
- parameter수가 많아지면 학습 시간도 오래 걸리고 과적합 문제도 발생할 수 있다.
### 해결책
- parameter의 수를 줄이기 위해 low level 변수(픽셀)를 조합하여 보다 적은 수의 parameter를 생성
- feature enginerring: 기계학습, transform기법 등에 이용하여 feature 생성
                       여기서 나온 feature은 오리지널보다 적은 데이터임 즉, 차원을 줄이는데 사용됨
- feature engineering 기법을 통해 차원을 줄인 새로운 feature를 생성해서 모델에 input하는데 이 방법이 매우 힘듬
- 그래서 CNN을 사용함

# CNN(Convolution Neural Network, 합성곱신경망)개념
- 이미지 데이터의 특성을 잘 반영할 수 있는 인공신경망 모델
- 2D or 3D 구조를 유지하면서 학습
- CNN은 conolution 연산, activation 연산, pooling 연산의 반복으로 구성됨
- 일정 횟수 이상의 convolution 이후에는 Flatten 과정을 통해 이미지가 1차원의 벡터로 변환됨
예시이미지
![image](https://user-images.githubusercontent.com/83350692/225309341-a1d0c862-e0b8-4b17-84b4-919aab98221d.png)
- 밑에 9개의 블럭중 4개의 블럭을 뽑아서 그것을 한개의 feature로 만든 것이다.
- 처음 3*3에서 새로운 2*2 feature로 바뀌게 된다.

## convolution 과정 설명
![image](https://user-images.githubusercontent.com/83350692/225311282-e90ab5c8-885d-4ef1-a592-ebd3ff332054.png)

![image](https://user-images.githubusercontent.com/83350692/225311437-9de4872a-7aa8-439b-b600-1f3ad4ebeeca.png)

![image](https://user-images.githubusercontent.com/83350692/225311969-fee7262c-d220-498e-b0a0-3855c85766fb.png)

- 필터(커널)이 있음 이미지상은 3*3임
- 그 필터가 한칸씩 이동을하면서 각 칸에 매칭되는 수끼리 곱해져서 다 더해지게 돼서 새로운 feature map이 생성됨
- 한칸씩 이동은 stride=1이여서 한칸씩 이동 만약 stride=2면 2칸씩 이동함
- 5*5 이미지가 3*3으로 바뀌게 된다.

