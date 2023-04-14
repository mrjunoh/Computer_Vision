# DenseNet
DenseNet은 기존의 LeNet, AlexNet, VGG, Inception, ResNet의 장점을 살려 만든 CNN architecture이다.

# 구조
- 기본적인 CNN과 ResNet, DenseNet의 구조를 비교해보면 아래 그림과 같음
![image](https://user-images.githubusercontent.com/83350692/231961088-9b9d69af-0ccc-4ae3-bf5b-ade65597576c.png)

- Resnet은 이전 결과와 현재 결과를 summation으로 이어주는 구조 였다면, 
- Densenet은 모든 결과를 Concatenation으로 연결시키는 구조라고 볼 수 있다.
- Res는 최종적으로 마지막 layer의 결과로 classifiaction을 하는데,
- Dense는 전체 layer의 결과를 모두 반영하여 classifiaction을 할 수 있다. 그래서 dense가 더 좋은 성능을 낼 수 있다고 주장함
- 모든 layer를 다 연결하면 parameter 수가 늘어나기 때문에 DneseNet은 layer의 channel수(= depth)를 줄인다.( channel 수를 많이 늘리지 않아도 모든 layer를 고려하기 때문에 성능에 문제는 없다고 함)
![image](https://user-images.githubusercontent.com/83350692/231962905-6f78067b-708e-4936-9435-c14e5e67eed9.png)

# 장점
1. 모든 layer를 다 연결하기 때문에 gradient의 전달이 잘 이루어짐
2. 파라미터 수가 적음
3. low한 feature도 반영 가능