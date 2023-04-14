# VGGNet
- VGGNet은 ILSVRC에서 2위를 차지한 모델
- 하지만 GoogleNet보다 구조적으로 간단하여 이해가 쉽고 테스트가 용이하기 때문에 더 많이 사용되었다.
- 추후에 나온 Inception-V2나 Inception-V3에서 VGGNet의 단순한 구조를 일부 적용했다.

# Contribution
- VGGNet은 CNN 성능에 network의 깊이(depth)가 어떤 영향을 주는지에 초점을 맞췄다.
- VGG는 이전 CNN 모델보다 깊이가 훨씬 깊은 19개의 Layer를 가진다.

![image](https://user-images.githubusercontent.com/83350692/231937239-117470c0-e277-4d23-9e52-7b9104bf59ff.png)

VGG의 핵심적인 Contribution은 Architecture와 Dataset과 관련해 각각 5가지로 정리할 수 있다.

- Architecture
    - Factorizing convolution → Implicit regularization
    - Pre-initialization (Like auxiliary classifier in GoogLeNet)
    - FC layer → Conv. layer

- Dataset
    - Scale jittering
    - Multi-crop (GoogLeNet) and dense evalutation (OverFeat)

# Materials
- VGG는 구조가 단순하기 때문에 Train/Test dataseta 만드는데 많이 신경썼다.
- ILSVRC-2012는 각각의 Class에 약 1,000장의 이미지를 3개의 Dataset으로 나눠서 사용했다.
    - Training: 1.3M images
    - Validation: 50K images
    - Testing: 100K images with held-out class labels
- 과적합이 발생하는걸 data augmentation 기법을 활용해서 해결하였다.

# Training
- VGG는 Alex와 유사하게 이미지를 다양한 방법으로 Scaling 한 후 <br>
  “224x224 크기로 랜덤하게 선택 → 좌우반전 (Horizontal flipping)”하거나 RGB값을 조작하여 이미지 수를 늘렸다.
- 추가로 각 RGB 값에 각각의 RGB mean값을 빼주는 전처리를 했다.
### Alex, GoogLe의 data augmentation
AlexNet
- Scaling
    - 모든 이미지를 256x256 크기로 만듦 (Single scale)
- Data augmentation
    - 256x256 이미지에서 224x224 이미지를 랜덤하게 추출 (1개 → 2048개)
    - RGB 값을 주성분 분석하여 RGB 데이터 조작

GoogLeNet
- 가로/세로 비를 [3/4, 4/3] 범위를 유지하며 원 이미지의 8% ~ 100%를 포함하는 Patch 추출 후 학습에 사용
- Photometric distortion으로 학습 데이터 늘림

VGG는 scale방법이 Alex와 조금 다른데, 이유는 single scale 외에도 Multi scale을 사용하기 때문이다. 

# single scale
- Training scale을 S로 표현했을 때, S를 256이나 384 중 하나를 선택 해서 사용하는 방법을 Single scale이라고 한다.
- S=256으로 학습 후 S=384로 학습시킨다.
- 이러면 Learning rate를 줄이고 학습시킨다.

        Training scale(S): 기계학습 모델을 훈련하는 데 사용되는 훈련 데이터의 크기를 나타냄

# Multi scale
- multi scale은 S를 Smin=256 과 Smax=512 범위에서 랜덤하게 선택할 수 있게 하는 방벙이고, 이를 Scale jittering이라고 한다.
- 보통 object들이 다양한 크기를 가질 수 있기 때문에, scale jittering으로 모델이 다양한 크기의 이미지에 대응 가능하게 예측 정확도를 더 높일 수 있다.
- s=384로 학습 후, S를 무작위로 선택하여 Fine tuning 한다.

# Test 
- VGG는 alex, google과 유사하게 Scaling 후 Crop과 좌우 반전으로 test 이미지 수를 늘렸다.

# Scale
- Q를 test scale이라고 했을 대 Q는 s(training scale)와 같을 필요가 없고 각 S에 대해 여러 개의 Q를 사용하면 결과가 더 좋아진다.

