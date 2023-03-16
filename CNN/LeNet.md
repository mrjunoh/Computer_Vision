# LeNet
- LeNet은 Yann Lecun이 개발한 CNN 아키텍처 이름이다.
- LeNet은 Lenet-1 부터 5까지 있다.

# LeNet-1
- 기존 DNN의 문제점을 해결하기 위해 Local receptive field, Shared wieghts, Subsampling 개념을 도입한 CNN을 만들었다.
- 1990년에 처음으로 CNN 개념이 반영된 LeNet-1을 발표했고 아키텍처는 아래 그림과 같다.
![image](https://user-images.githubusercontent.com/83350692/225621685-817b1876-f318-46ac-ac4b-8dff1a9292cd.png)

- 먼저 LeNet-1은 입력 이미지에서 filter를 활용한 convolution으로 feature map을 만든다.
- 여기서 여러 다른 특징을 추출하기 위해 다양한 filter를 사용하여 여러 개의 feature map을 만들 수 있다.
- 그리고 각 filter는 모든 이미지에 대해 동일하게 사용하여 Shared weights 개념도 반영되었다.
-
- 다음으로 subsampling으로 feature map에서 특정 winodw 크기의 값들의 평균들만 추출하여 feature map을 만들 수 있다.
- 그 결과, Feature의 크기를 줄이면서 동시에 Topological invariance를 얻을 수 있다.
- 또한, 하나의 feature map에서 subsampling을 통해 하나의 local feature를 얻고, 그 local feature에서 다시 convolution과 subsampling으로 feature를 얻는 과정을 통해 global feature를 얻을 수 있다.
- 결국 전체를 대표할 수 있는 강한 특징만 남게 된다.
-
- 마지막으로 전체를 대표할 수 있는 global feature를 10개의 class로 convolution하여 classifiacation이 가능하게 만든다.
- 정리하자면, LeNet-1은 크게 convolution layer, subsampling layer로 구성되어 있다고 할 수 있다.

# LeNet-5
- 1998년에 LeNet-5를 발표했다. 
- 5에서는 입력 이미지의 크기가 커졌고, fully connected layer가 추가되었다.
- 1에서는 16*16로 이미지를 줄이고 28*28 중앙에 위치시켰지만, 5에서는 28*28 이미지를 32*32의 중앙에 위치시켰다.
- 5에서 1보다 더 큰 이미지를 사용했기 때문에 이미지의 Detail한 부분까지 고려하여 성능이 더 높아진 부분도 있다.
- LeNet-5는 network가 크기 때문에 parameter 수는 6만개에 달한다.
![image](https://user-images.githubusercontent.com/83350692/225626913-4722f5a5-622f-4585-875a-764e134cd926.png)
- 위 그림이 LeNet-5의 아키텍처이다.
- convolution layer 3개, subsampling layer 2개, fully connected layer 1개로 구성됨
### c1, c2
- 5는 입력으로 32*32 크기 이미지를 받고, 5*5 filter로 convolution 하여 28*28 feature map 6개를 생성한다.
- 그리고 이를 2*2 subsampling(average pooling 사용) 으로 14*14 feature map 6개를 만든다.
### c3, s4
- 여기서 다시 5*5 FILTER로 convolution하여 10*10 feature map 16개를 만들고, 2*2 subsampling(avg pooling)하여 5*5 feature map 16개를 만든다. 
- 여기서 각 filter의 값들은 지정하는 것이아니라 학습을 통해 결정된다.
### c5, f6
- 한번더 5*5 feature map 16개를 5*5 filter로 convolution하여 1*1 feature map 120개를 만든다.
- 이렇게 만들어진 120개의 feature map을 크기가 84인 FC layer에 연결한다.
- 마지막으로 크기가 10인 layer와 연결하여 최종적으로 10개의 class를 구분할 수 있게 만들었다.