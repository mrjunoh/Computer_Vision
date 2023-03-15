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

