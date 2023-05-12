# object localization
![image](https://user-images.githubusercontent.com/83350692/235355194-677577a4-a2f7-45c9-9d6b-459e1f7f03dd.png)
1. 위 그림 첫 번째는 그냥 차인지 아닌지 구분하는 classification이고 
2. 두 번째 그림은 그림 내의 차에 위치를 bounding box로 표시하는 작업이 Localization이다.
3. 세 번째 그림은 모든 object들을 Localization하는 것이 object detection이다.

먼저 classification 개념을 이용해 Localization을 이해하기.
![image](https://user-images.githubusercontent.com/83350692/235356205-23c74456-04e5-4123-8e00-784286263141.png)

- 위 그림에'서 자동차 위치를 알아내려면 자동차즤 중심점 위치와 가로, 세로 길이를 알면 자동차의 위치를 알 수 있다.

![image](https://user-images.githubusercontent.com/83350692/235356259-d3af5aae-b07c-49e2-a8eb-6e297eed8acf.png)

    - 결론적으로 변경이 필요한 부분은 예측하고자한 정답인 label y이다.
    - Label y에 필요한 정보는 자동차 위치에 해당하는 중심점(bx, by)과 가로/세로 길이 (bh,bw)그리고 어떤 object인지에 대한 class 정보와 해당 class가 존재하는지 여부에 대한 정보 (pc)가 필요하다.
    - 그래야 어떤 object가 비디오나 사진 내 어디에 몇 %확률로 존재하는지 알 수 있기 때문이다.
    - 이를 정리하면 아래 그림과 같다.

![image](https://user-images.githubusercontent.com/83350692/235356484-fd2c7d9f-f7c5-40cd-a03f-49110624d184.png)


### cost function
- 먼저 obejct가 존재하는 경우에는 일반적인 cross entropy를 사용했다.
- 하지만 object가 존재하지 않는 경우에는 Pc값만 차이가 나는지 확인하면 된다.
- 왜냐하면 object가 없으면 나머지 값들은 무의미 하기 때문이다.
- 각 항목에 따라 다르게 cost function을 적용할 수 있다.

## Landmark detection
- Landmark detection은 비디오나 사진 내에서 object의 landmark(특징점) 위치를 찾아내는 것을 의미한다.
- Landmark detection도 이전에 했던 object localization과 유사한 형태를 띈다.
![image](https://user-images.githubusercontent.com/83350692/235356770-1cdb0151-7521-416b-a49f-761b44fa7454.png)

- object localization에서도 class에 대한 정보와 해당 class의 그림 내 중심점 및 가로/세로 길이를 예측하여 object의 위치를 예측하는 모델을 만들었다.
- landmark detection을 할 대도 마찬가지로 landmark의 class에 대한 정보와 각 landmark의 위치를 output으로 하는 ConvNet 모델을 만들면 landmark의 위치를 예측하는 모델을 만들 수 있다.