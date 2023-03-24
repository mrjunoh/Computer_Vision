# GoogLeNet
- 2014년 ILSVRC에서 1위를 차지한 모델, 가장 큰 변화는 network의 깊이
![image](https://user-images.githubusercontent.com/83350692/225906749-a5e1f915-a993-4cf2-a66c-7b84598d5659.png)

- CNN의 성능을 향상시키는 가장 직접적인 방법은 network 크기를 늘리는 방법이다.
- 2013년까지는 CNN의 깊이가 10 미만이었지만, 2014년 googlenet, VGGNet이 각각 22 layer, 19 layer로 2배 이상 커졌다.
- 에러율도 각각 6.7%, 7.3% 낮아졌다. 
-
- 하지만 network가 깊어지면 trainable parameter수가 증가하게 되고, 그 결과 overfitting 문제가 발생하거나 연산량이 급격히 늘어날 수 있다.
- 단순히 network를 깊게 만들면 학습시간이 엄청나게 길어질 수 있다.
- 이러한 문제를 해결하기 위해서는 network의 구조적인 변화에 대한 고민이 필요했고, google에서 inception이라는 모듈로 구성된 GoogLeNet으로 이를 해결 했다.

# Contribution
GoogLeNet의 contribution은 다음 3가지이다.
- Inception 모듈
    - 1x1 convolution -> 파라미터 수 감소 (alexnet보다 12배 적음)
- Auxiliary classifier
    - vanishing gradient 문제 해결

# Architecture
- googleNet은 inception이라는 모듈로 구성되어 있다.
- inception은 "network in network"의 구조를 더 발전시킨 형태이다.

### NIN
- 보통 cnn에서는 convolution 연산을 통해 이미지 또는 feature map의 width, height의 크기는 줄이고 channel는 늘리는 형태를 취한다.
- max pooling은 w,h를 줄이는 역할을 한다.
- 이 때, 1x1 convolution 연산은로 c를 줄일 수 있고, 그 결과 c 단위로 FC 연산하여 C 또는 차원을 줄이는 효과(압출)를 얻을 수 있으며, 이는 학습해야 할 파라미터수의 감소로 이어진다.
![image](https://user-images.githubusercontent.com/83350692/225916581-ab6107d1-2070-4635-86c3-42e4f882fcf4.png)
- 결국 1x1 convolution으로 channel 수를 줄여 파라미터 수를 줄이는 것이 NIN의 핵심이다.
- GoogLeNe에서는 이를 반영하여 inception을 만들었다.

### Inception
- google 연구팀은 NIN 기반으로 network를 깊게 만들면서도 연산량의 수가 급격히 늘지 않는 Inception이라는 모듈을 만들었다. 
- 또한, local receptive field에서 더 다양한 feature를 추출하기 위해 여러 개의 convolution을 병렬적으로 사용 했다.
![image](https://user-images.githubusercontent.com/83350692/226178670-c0aaee25-c448-4e6a-a850-1596ffe18f90.png)
- 원래 초기 inception은 좌측 그림처럼 1x1 / 3x3 / 5x5 convolution, 3x3 max pooling을 나란히 놓는 구조를 만들었다.
- 이는 같은 layer에 다른 크기를 가지는 filter를 적용하여 다른 scale의 feature를 추출할 수 있게한다.
-
- Inception 모듈에는 연산량을 줄이기 위해 1x1 convolution으로 feature map의 차원 수를 줄였다.
- 위 우측 그림에서 볼 수 있듯이 다른 layer와 다르게 max pooling이 있는 layer에는 1x1 convolution 보다 max pooling을 먼저 실시한다.
- 그 이유는 max pooling이 feature map의 개수(channel 수)를 조정할 수 없기 때문이다.
- googlenet의 inception은 기존 cnn 구조에서 크게 벗어나지 않으면서 NIN의 특징을 반영했다.

###  GoogLeNet
- GoogLeNet은 지금까지 설명한 9개의 inception으로 구성되어 있다. 
- 밑에는 googleNet 그림이고, input image 크기는 224*224*3이다.
![image](https://user-images.githubusercontent.com/83350692/227533174-3a3243b4-531d-4a27-9946-361a7eee8aab.png)

각 표기가 의미하는 바는 아래와 같다.

    - 빨간 동그라미: Inception (위에 숫자: Feature map 수)

    - 파란색 모듈: Convolutional layer
    - 빨간색 모듈: Max pooling
    - 노란색 모듈: Softmax layer
    - 녹색 모듈: 기타 Function

- 그림에서 볼 수 있듯이 layer 초반에는 inception을 사용하지 않고, 일반적인 CNN에서 보이는 단순한 Conv-pool 구조를 가진다. 

- inception을 사용하지 않는 이유는 google팀에서 실험했을 때  초반에는 inception을 배치했을 때 효과가 없었기 때문이라고 한다. 이 영역을 Steam영역이라 부른다.
- 아래표가 googlenet의 상세한 구조와 parameter를 정리해놓ㅇ느 것이다.

![image](https://user-images.githubusercontent.com/83350692/227534066-c9f2ec07-4d61-4be0-88dc-479ffd15be32.png)

![image](https://user-images.githubusercontent.com/83350692/227534199-f7a349f8-c51b-462f-b224-d64fa91ee9ef.png)