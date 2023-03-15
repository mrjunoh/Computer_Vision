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

# convolution 과정 설명
![image](https://user-images.githubusercontent.com/83350692/225311282-e90ab5c8-885d-4ef1-a592-ebd3ff332054.png)

![image](https://user-images.githubusercontent.com/83350692/225311437-9de4872a-7aa8-439b-b600-1f3ad4ebeeca.png)

![image](https://user-images.githubusercontent.com/83350692/225311969-fee7262c-d220-498e-b0a0-3855c85766fb.png)

- 필터(커널)이 있음 이미지상은 3*3임
- 그 필터가 한칸씩 이동을하면서 각 칸에 매칭되는 수끼리 곱해져서 다 더해지게 돼서 새로운 feature map이 생성됨
- 한칸씩 이동은 stride=1이여서 한칸씩 이동 만약 stride=2면 2칸씩 이동함
- 5*5 이미지가 3*3으로 바뀌게 된다.

![image](https://user-images.githubusercontent.com/83350692/225312184-f7bdaa3c-aae0-4aec-a1ae-15c0be60c331.png)
- stride=2라고 하면 2칸씩 이동을해서 5*5 -> 2*2가 된다.
- 이렇게 되면 이미지의 특징을 놓칠 가능성이 증가한다.

### convolution layer 예제
![image](https://user-images.githubusercontent.com/83350692/225312805-602baecd-3679-4571-aaf2-19be8830cd83.png)
- 4*4의 input 이미지가 있고
- 필터는 3*3이다.
- 여기서 뽑은 feature map은 2*2이다.
  - 계산을 직접 보면
![image](https://user-images.githubusercontent.com/83350692/225313191-36c275a8-57bb-4782-a244-0169d77eb482.png)

![image](https://user-images.githubusercontent.com/83350692/225313340-570aabaa-630b-4b23-b8f2-c58b8eb6917c.png)
- 필터를 원본 이미지에 적용을 시킨다. 즉 convolution(합성곱)을 한다.
- 각 위치에 있는 픽셀들 끼리 곱하고 다 더해준다.
- 그 값을 새로운 feature map의 한칸에 넣어준다.
- stride=1로 한칸 옆으로 옮기고 다시 반복을 한다.
- 그럼 4*4는 3*3 필터를 걸쳐서 최종적으로 2*2 feature map이 생성된다.

![image](https://user-images.githubusercontent.com/83350692/225313711-71fcd267-19ad-446a-a33c-d80e7b2f8e23.png)
- 필터(커널) 안에 있는 숫자들은 weight(파라미터)이다. 
- 그래서 최적의 weight(파라미터)를 찾는 것이 인공지능이 할 일이다.

## Activation
![image](https://user-images.githubusercontent.com/83350692/225314844-7fe5a34d-e738-492c-b8ea-62a62ffee444.png)
- convolution 과정이 끝난 후 pooling 전에 activation을 통과해서 비선형 변환을 해야한다.
- 위 이미지에서는 relu를 사용해서 오른쪽 결과값이 나오게 된것이다.

## Padding
- Convolution 결과 이미지가 입력 이미지의 크기와 같으면 편리할 때가 많음
- 가장자리에 있는 픽셀들은 중앙에 위치한 픽셀들에 비해 convolution 연산이 적게 수행됨
- 이를 위한 해결책으로 원 이미지 테두리에 0의 값을 갖는 픽셀 추가
- 테두리에 추가된 0값의 픽셀 -> pad, 테두리에 추가된 0값의 필셀 추가 -> zero padding
![image](https://user-images.githubusercontent.com/83350692/225319012-23f0b100-476f-445c-8ef8-33fce81e404d.png)
- 이미지에 0이 zero padding처리를 한 것이다.
- 보통 convolution을 하면 이미지 크기가 줄어드는데 padding을 통해 입력 이미지 크기와 feature map 크기가 같아 진다.
![image](https://user-images.githubusercontent.com/83350692/225319246-c272d596-2e07-48c3-a716-519ad1ebf86a.png)
- padding 예시이미지
