object detection은 비디오나 사진에서 object를 감지하는 작업이고, 그 과정은 크게 classification과 Localization으로 나뉜다.

딥러닝을 사용하기 전 sliding window detection을 많이 사용함

# Sliding window detecion

- 일반적인 ConvNet을 이용하여 자동차를 분류하는 모델을 만들면 아래 그림과 같은 결과를 얻음

![image](https://user-images.githubusercontent.com/83350692/236675330-b9660a4a-30f8-4d13-84d5-1c3fc14608a0.png)

- 이렇게 학습된 ConvNet을 이용하여 이미지를 sliding 하면서 각 부분이 자동차인지 아닌지를 분류하면서 자동차의 위치를 찾을 수 있다. 
- 하지만 이 방법은 수 많은 영역을 잘라내고 ConvNet으로 계산해야 되기 때문에, computational cost가 많이 든다.
- 이를 줄이기 위해 slide 간격을 크게 하여 띄엄띄엄 확인하면 성능이 저하될 수 있다.
![image](https://user-images.githubusercontent.com/83350692/236675507-25b7a244-687f-4a66-863d-e50a5ca99e24.png)

- 그래서 이전에는 object detection을 위해 많은 연산을 요구하지 않는 Linear classifier를 사용하거나 직접 특징을 설정했다. 이 방법은 한계가 있다.
- 이 문제는 Convolution을 이용하여 해결할 수 있다.

- Computational cost 감소를 위해 FC layer를 convolutional layer로 바꿀 수 있다.
![image](https://user-images.githubusercontent.com/83350692/236675662-0924177b-dc3f-45d1-b4c4-79538bec35a3.png)

- 결론적으로 FC layer를 conv layer로 바꾼 ConvNet으로 sliding window를 적용하는 경우, 여러 번 sliding window를 적용하는 것이 아니라 한 번에 해결할 수 있다.
![image](https://user-images.githubusercontent.com/83350692/236675729-1c00bf19-862a-4a6b-93af-8c3c6f4e1947.png)
- 이 방법도 bounding box의 위치가 정확하지 않을 수 있다는 문제점을 가지고 있음

# Non-max suppresion
- 객체 탐지에 발생할 수 있는 또 다른 문제점은 알고리즘이 같은 object를 여러 번 감지하는 것임.
- non - max suppresion은 알고리즘이 각 object를 한 번씩만 감지하게 보장하는 방법
- non-max suppresion은 bounding box 중 확률이 최대인 경우를 찾고 그렇지 않은 것들은 IoU를 활용하여 억제하는 방법
![image](https://user-images.githubusercontent.com/83350692/236675952-5f0da64c-71cc-46aa-bef4-e362f28042e8.png)
1. 위 첫번째 그림처럼 각 객체에 대해 여러 개의 bounding box가 감지될 수 있다.
2. 이 때 각 bounding box의 확률 중 가장 높은 것만 밝게 표시함
3. 선택된 bounding box와 IOU가 높은 bounding boxㄹ르 찾아 어둡게 표현함
4. 그 후 어둡게 표시된 bounding box를 제거하면 나음 bounding box가 해당 객체의 최종 boudning box가 된다.

## non-max suppression 알고리즘
- pc<=0.6 인 Bounding box는 모두 제거함
- 다음 과정을 남아있는 Bounding box가 없을 때까지 반복함
  - pc 가 가장 큰 Bounding box 선택
  - 선택된 Bounding box와 IoU>=0.5 인 Bounding box 모두 삭제

# Anchor boxes
- 지금까지 object detection의 문제점은 각 Grid가 하나의 object만 감지할 수 있는지 여부다.
- 중요한 이유는 여러 개의 grid에서 하나의 object을 감지하면 어떤 grid가 감지했다고 판단해야 되는지 애매할 수 있기 때문이다.
- 그러면 하나의 grid로 여러 개의 object를 감지하려면 Anchor box를 사용해야 한다.

![image](https://user-images.githubusercontent.com/83350692/236677463-1327e4a2-7ba2-47f0-b3a6-41a6e32f857f.png)

- 기존에는 label y가 하나의 object에 대한 bounding box 정보를 담고 있었는데
- Anchor box 개념을 이용하면 label y가 2개의 object에 대한 bounding box 정보를 담고 있다.
- 여기서는 2개의 anchor box만 이용해서 한grid에서 2개의 object를 판단할 수 있게 된다. 더 많은 anchor box를 사용할 수도 있다.

- 즉, 하나의 grid에 anchor box와 IOU가 가장 유사한 object에 대한 정보를 예측하는 것이다.
- 예를 들어 기존에 3x3x8 output을 가졌다면 이제는 3x3x(2x8)인 output을 가지게 된다.(gird와 anchor box는 한 쌍)
