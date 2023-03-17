# AlexNet
- alexnet은 imagenet의 120만 개 이미지를 1000개의 class로 분류하는데 CNN을 사용했고 압도적인 성과를 얻었다.
![image](https://user-images.githubusercontent.com/83350692/225638792-2f4b34e3-5b26-42c8-a047-d8249b0f8a01.png)
- 오른쪽 그림에서 supervision이 alexnet이다.
- 또한 alexnet의 gpu 사용과 소스 코드 공개 후로 많은 연구자들이 gpu를 사용하기 시작했고
- 우측 그래프를 보면 ILSVRC에 참여자의 gpu 사용률이 높아졌고 그에 따른 error rate가 낮아진 것을 볼 수 있다.
- alexnet은 5개의 convolution layer(일부 max pooling), 3개의 FC layer로 구성되어 있는데, LeNet-5와 유사하다. 특히 성능 개선을 위해 크게 5가지를 고려함

    - relu + local response normalization
    - multiple gpu
    - overlapping pooling
    - dropout
- 위 같은 방법을 적용하여 alexnet은 ILSVRC 2012에서 TOP-5 ERROR RATE 15.3%로 우승했다.

# Materials
- alexnet은 학습과 테스트를 위해 ILSVRC에서 제공하는 일부 imagenet 데이터를 사용했다.
- 원래 imagenet에는 22000개 category를 갖는 15 million개의 이미지가 있다.
- 하지만 이 중에서 하나의 category에 약 1000개의 이미지를 추출하여 training에 1.2 million개, validation에 50000개, 테스트에 150000개의 이미지를 사용했다.

- 이러한 imagenet의 이미지를 바로 alexnet을 학습하고 테스트 하는데 사용할 수 없다.
- 그 이유는 이미지 크기가 다양하고 그 개수가 부족해 여러 가지 문제가 생길 수 있다.

# 아키텍처
![image](https://user-images.githubusercontent.com/83350692/225889578-036197e6-ec58-4885-9826-f7b691d995f3.png)
- AlexNet의 전체 구조는 5개의 convolutional layer와 3개의 FC layer로 구성되어 있다.
- 첫 번째, 두 번째, 다섯 번째 convolution layer 다음에 max pooling을 사용한다.
- alexnet은 방대한 network(뉴런 65만개, 파라미터 6천만개, connection 6억3천만개)를 학습하기 위해 2개의 gpu를 병렬로 사용함
- LeNet과 다르게 성능 개선을 위해 5가지를 고려 했다.
    1. relu + local response normalization
    2. multiple gpu
    3. overlapping pooling
    4. dropout

## 아키텍처 디테일
![image](https://user-images.githubusercontent.com/83350692/225890257-6ba6b010-c844-4624-bf11-0d403513e412.png)
### input layer
- alexnet은 rgb 3가지 색상을 가지는 image를 input으로 사용하는 것이 LeNet과 다르다. 그래서 이미지의 depth가 3이고 이를 convolution 하기 위해 filter의 depth도 3이 된다.
### 1st conv layer
- alexnet의 input image는 224x224x3으로 크기 때문에 각 gpu에 대해 11x11x3 filter 48개 (stride=4)로 convolution 했고 (LeNet-5:5x5 filter)
- 이 때 convolution 결과에 relu를 적용시켰다. 그 결과로 각 gpu에서 55x55 feature map을 48개(총 96개)를 생성했다.
- 여기에 3x3 max pooling(stride=2)과 local response normalization를 적용시켜, 각 gpu에 27x27 feature map 48개(총 96개)를 만든다.

### 2nd conv layer
- 각 gpu의 27x27 feature map 48개(총 96)에 각각 5x5x48 filter 128개로 convolution했고, 여기에 relu를 적용시켰다.
- 그 결과로 각 gpu에서 27x27 feature map 128개(총 256개)를 생성했고, 여기에 3x3 max pooling (stride=2)과 local response normalization을 적용시켜 각 gpu에 13x13 feature map 128개 (총 256개)를 만든다.

### 3rd conv layer
- 각 gpu의 13x13 feature map을 128개 (총 256개)에 각각 3x3x128 filter 192개로 convolution 했고, 여기에 relu를 적용시켰다.
- 그 결과로 각 gpu에서 13x13 feature map 192개(총 384개)를 생성했다.
- 세 번째 layer는 다른 layer와 다르게 두 gpu에서 convolution한 결과를 연결했다.
- validation을 통해 세 번째 conv layer에서 두 gpu의 convolution 결과를 연결하는 것으로 선택함
- 이번 layer에서는 max pooling과 local response normalization을 하지 않는다.

### 4th conv layer
- 네 번째 layer에서는 각 gpu의 13x13 feature map 192개(총 384개)에 3x3x192 filter개로 convolution 했고, 여기에 relu를 적용시킴
- 그 결과로 각 gpu의 13x13 feature map 192개 (총 384개)를 생성했다.

### 5th conv layer
- 5 번째 layer에서는 각 gpu의 13x13 feature map 192개(총 384개)에 3x3x192 filter 128개로 convolution 했고, 여기에 relu를 적용시켰다.
- 그 결과로 각 gpu에서 13x13 feature map 128개 (총 256개)를 생성했다.
- 여기에 3x3 max pooling(stride=1)을 적용시켜 각 gpu에 6x6 feature map 128개(총 256개)를 만든다.

### Fully connected layer
- FC layer와 연결하기 위해 각 gpu의 6x6 feature map 128개(총 256개)를 Flatten 시킨다.
- 아래 그림처럼 좌측 상단 값부터 순차적으로 추출하여 일렬로 나열하면 됨
![image](https://user-images.githubusercontent.com/83350692/225896384-d542032f-d1d6-4578-847d-0017711a29e8.png)

- 각 gpu의 6x6 feature map 128개(총 256개)를 모두 flatten 하면 9216개의 뉴런을 가지는 layer를 만들 수 있다.
- 이렇게 flatten된 layer는 4096개의 뉴런을 가지는 첫 번째 fc layer에 연결시킨다.
- 또한, 4096개의 뉴런을 가지는 첫 번째 fc layer와 마찬가지로 4096개의 뉴런을 가지는 두 번째 fc layer를 연결한다.

### output layer
- 마지막으로 4096개의 뉴런을 가지는 두 번째 fc layer와 1000개의 class를 구별할 수 있게 1000개의 뉴런을 가지는 output layer와 연결한다.
- 여기서 1000개의 뉴런은 각 category를 의미한다.
- 그리고 이 때 softmax를 사용하여 output layer로 전달되는 모든 값의 합이 1이 되게 값들을 변경해준다.
- 이는 각 1000개 뉴런이 나타나는 정도를 확률적으로 표현하여 해석하기 용이하게 만들기 위해서이다.
![image](https://user-images.githubusercontent.com/83350692/225897038-44ce9998-b931-4e7a-9092-66e2c80261fa.png)


# All parameters
- 위에서 설명한 구조의 모든 파라미터를 정리하면 다음과 같다.
![image](https://user-images.githubusercontent.com/83350692/225897210-d41bcae2-9ac6-41ea-a614-808987ae82ef.png)