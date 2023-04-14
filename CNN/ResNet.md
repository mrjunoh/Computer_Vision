# ResNet
- 2015년에 나온 모델
- R-CNN의 속도 문제를 개선한 SPPNet으로 Detection 분야 성능을 개선시켰다.
- Resnet은 classification 뿐만 아니라 Localization, detection 분야에서도 압도적인 성능을 보였음
        
        Localization: computer vision 및 처리 작업으로 image 내에서 대상 또는 관심 영역의 위치와 범위를 식별하는 것.

        이미지 내에서 물체를 찾는 것 뿐만 아니라 물체를 분류하는 것이 목표인 object detection에서 자주 사용됨
![image](https://user-images.githubusercontent.com/83350692/231937239-117470c0-e277-4d23-9e52-7b9104bf59ff.png)

- RES는 classification에서 152 layer를 사용하여 top-5 error가 3.57%가 나왔고
- 사람의 이미지 분류 오차율인 5%보다 더 좋은 결과다.
- 2014년 우승한 google보다 성능이 두배 정도 좋아짐
- Network의 depth가 7배 이상 깊어짐

# Motivation
- network가 깊어지면 Gradient vanishing/exploding, overfitting 등의 문제가 생긴다. 
- 이를 해결하기 위해 ResNet은 Residual learning 개념을 언급함

# Residual learning
- 기존 CNN은 아래 좌측 그림처럼 x를 입력받고 2개의 weight layer를 거친 후 H(x)라는 출력을 내며, 학습을 통해 최적의 H(x)를 찾기 위해 weight의 값들을 수정한다.
![image](https://user-images.githubusercontent.com/83350692/231946446-92a92702-6e11-4762-8214-89a9b2d783df.png)

- 하지만 최적의 H(X)를 찾는 것이 아니라 H(x)-x를 찾는 것으로 변경하면,
- 최적의 출력과 입력의 차를 찾도록 학습되는 것이다.
- 이 때, F(x)= H(x)- x 라고 하면, 출력 H(x) = F(x) + x가 된다.
- 즉 우측 상단 그림처럼 입력에서 출력으로 연결되는 선이 생기고,
- 이는 덧셈만 추가되어 연산량의 차이가 없어 forward/ backward path가 단순해지는 효과를 얻는다.
- 이 것이 Residual learning의 기본 Block이다.

- 최적의 f(X)=0이므로 F(x)가 0이되는 방향으로 학습하게 되면 입력의 작은 움직임을 쉽게 검출 할 수 있게 된다. 이때 여기서 F(x)의 작은 움직임, 나머지(residual)을 학습한다는 관점에서 residual learning이라고 불리게 된다.

결론: Residual learning을 사용해서 더 deep한 network를 쉽게 최적화 할 수 있게 되고, 깊이가 늘어나서 정확도도 향상할 수 있게 됐다.