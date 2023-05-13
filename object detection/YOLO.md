# Yolo
- yolo는 you only look once의 약자로 다른 모델들에 비해 빠른 처리 속도를 보여 실시간 객체탐지가 가능하다.
- yolo는 Resize한 이미지를 ConvNet을 이용해 object를 감지하는 방법이다.
- 이는 기존 object detection 방법들보다 그 과정이 단순하여 bounding box를 그리는 속도가 더 빠르면서 정황석을 유지하는 방법이다.


# contribution(기여도)
- Localization + Classification problem → Regression problem (Bounding box 위치와 Class probability 예측)

- Only one feedforward using whole image → Global context

- Unified detection → Fast → Real-time detection

- Generalizable representation → Other domain works (예: Art works)

# Unified detection
YOLO는 이미지를 Gird로 나누고 각 grid에 대해 bounding box의 위치와 그 확률 그리고 어떤 class에 해당하는지 확률 정보를 예측해서 bounding box를 찾아내는 방법이다.
- 예) 아래 그림처럼 9개의 영역으로 나누고 각 영역에 대해 8가지 정보(object 중심점, object 가로/세로 길이 등)를 예측하도록 모델을 만들면, object가 있는경우 정확한 위치를 알아낼 수 있다.
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/ab15d5d2-244a-4c1f-93e0-622fe9db07a0)

### 예시
S=7, B=2, C=20 output은 7x7x30 tensor
- image = s x s grid
- grid cell
    - b: bounding box location and cofidence score
        - x,y,w,h는 bounding box의 위치
        - confidence score는 해당 bounding box가 object를 얼마나 많이 포함하고 있는지 정도
        - confidence = Pr(object) x IOU
        - 아래 그림에서 confidence score가 높은 bounding box는 더 진하게 표기됨
    - c: class probabilities
        - 특정 object에 대해 각 class가 얼마나 나타날 수 있는지 확률
        - Pr(Classi|Object)
        - 아래 그림에서 각 object 별 나타날 확률이 높은 것을 class probability map으로 표현함

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/da931fa0-ee4a-45a2-9c19-193a7d0e7269)

    1.	하나의 이미지 데이터를 입력하면 물체들을 찾아 bounding box와 가장 높은 확률의 클래스를 찾는다. (중앙 상단 이미지)
    2.	각 grid cell에서 물체의 확률과 IOU값을 비교해 가장 높은 확률을 가진 클래스를 검출해 해당 grid cell의 클래스를 확정 (중앙 하단 이미지)
    3.	확정된 클래스의 확률이 일정한 값(threshold) 이상의 grid cell을 이용해 bounding box를 그린다. (우측 이미지)


- 예측할 때는 하나의 grid에 대한 확률을 예측하여 각 class에 대한 confidence score를 계산할 수 있다.
- 이를 기반으로 bounding box가 어떤 class의 bounding box인지를 알아낼 수 있다.
- 수식
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/aac1623f-6a66-41c4-8a04-32db6f18e412)

# Network design
- yolo는 GoogLeNet을 약간 변형하여 사용함
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/24491ced-0886-42b3-a86f-645de4244c3d)
- 위 그림에서 1x1 conv layer로 이전 layer보다 channel 수를 줄여 parameter 수를 줄였다.
- 초반 20개의 conv layer는 googleNet의 구조를 반영했고, 뒤에 4개의 conv layer와 FC layer를 사용하고 있다.

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/9b99c86f-29f1-479f-9296-2a856690fa45)
그림 출처: https://oi.readthedocs.io/en/latest/computer_vision/object_detection/yolo.html

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/574106f1-fe4b-4eb9-a876-1167c9a6d5c4)
- 위 그림은 yolo의 속도를 더 높이기 위해 24개의 conv layer를 9개로 줄인 fast yolo의 구조임
- 1x1 conv layer와 일부 3x3 conv layer를 삭제했지만 max pooling은 그대로 사용하고 있음

# Training
yolo가 어떻게 학습되는지 알아보자
- 우선 앞에 있는 20개의 conv layer는 imageNet 1000-class competition dataset을 활용하여 pretraining 해서 사용한다.
- 이렇게 pretrain된 layer 뒤에 4개의 conv layer와 2개의 FC layer를 추가한 뒤, VOC dataset으로 Fine tuning한다.
- 그리고 input은 기존처럼 224x244 image를 사용하는 것이 아니라 resolution을 높인 448x448 image를 사용하면서 조금 더 세밀한 정보도 파악하려 했다.
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/89aeb791-2489-48be-abb6-06e7a1950bf2)

- 위 그림을 간소화해서 표현
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/22bee4a3-cec7-41c1-9761-9229d87a3d12)
- 448x448 image는 수정된 googleNet 모델의 20개 layer들을 거쳐 feature map이 나온다.
- 여기에 4개의 conv layer와 2개의 FC layer를 거쳐 최종적으로 7x7x30인 tensor를 얻을 수 있고 이것이 obejct detection에 활용된다.

# Prediction
- 학습된 모델을 이용하여 입력으로 이미지를 넣으면 object에 해당하는 bounding box를 그릴 수 있다.
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/e0907578-e77e-4401-8621-63f94c297d4a)

- 첫 번째 grid를 먼저 예측해보면 파란 vector처럼 Pc 값이 0으로 나와 object가 없다고 판단할 수 있다.
- 차가 있는 grid를 예측했을 때 bounding box 정보가 초록 vector처럼 Pc=1로 나오고 나머지bx, by, bh, bw 값들과 class에 대한 확률값이 나온다.
- 이를 통해 object에 대한 bounding box를 그릴 수 있다.

### 실제 yolo에서 bounding box를 그리는 방법
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/744e6a41-8493-4028-8846-c325e4a5ecc6)
- 이미지를 학습된 모델에 입력하면 7x7x30 크기의 tensor가 결과로 나온다.
- 그 중 하나의 vector만 살펴보면 처음 5개는 첫 번째 bounding box에 대한 정보이고,
다음 5개는 두 번째 bounding box에 대한 정보이다.
- 그리고 그 다음 20개는 현재 grid에 해당하는 object가 어떤 class인지 그 정도를 확률로 나타낸 값들이다.

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/cc40a57b-63be-4bc5-a060-3b71af1a6501)
- 모든 grid에 대해 각 grid에서 각 bounding box의 conference score와 class probability 20개를 곱하면 20x1 크기 vector를 98개 얻을 수 있다.
- 여기서 곱한 값의 의미는 각 grid에서 하나의 bounding box에 특정 class의 object가 나타날 확률을 의미한다.

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/0691b8b4-f68a-42f5-af60-39538118c988)
- 이렇게 추출한 98개의 20차원 vector를 이용하여 최종 object의 위치를 나타낼 bounding box를 선별하게 된다.
![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/4c633d4a-a4da-427a-b4e1-6e308441bb2b)
- 위 그림에서 98개의 vector에 첫 번째 차원의 값들은 모두 하나의 class에 해당하는 bounding box에 대한 정보다. 
- 이 중에서 값이 0.2보다 작은 경우에는 모두 값을 0으로 변경한다.
- 그리고 나서 현재 class의 object를 가장 잘 감지한 bounding box 순으로 내림차순 정렬한다.
- 그리고 non max suppresion을 적용하여 중복되는 bounding box의 값을 0으로 변경한다.

# Non-max suppresion
yolo에서는 object가 여러 개의 grid에서 감지될 수 있어 이를 해결하기 위해 Non-max suppresion사용함

![image](https://github.com/mrjunoh/Computer_Vision/assets/83350692/6953af51-d2ad-4a7c-8595-6bd18211eba4)
- 각 grid 마다 2개의 bounding box를 예측했다.
- 이 중에서 확률값이 낮은 것을 먼저 제거한다.
- 그리고 나서 각 class의 bounding box중 나타날 값이 가장 큰 값과 많이 겹치는 bounding box를 모두 제거한다.
- 이것이 Non-max suppresion이고 위 예제 이미지에서는 class가 3개 이므로 Non-max suppresion을 3번 실시한다.
- 그 결과 위 우측 이미지처럼 확률이 높은 bounding box가 차와 사람을 감지할 수 있다.
