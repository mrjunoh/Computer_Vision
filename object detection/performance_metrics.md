# IoU(Intersction Over Union)
iou는 객체 탐지 분야에서 2개의 영역이 "얼마나 겹쳐져 있는가"를 표시하는 지표이다.

![image](https://user-images.githubusercontent.com/83350692/236678245-d4797b83-c093-4c68-9584-aeed66e125d0.png)
- iou는 정답영역과 예측 영역이 겹쳐진 부분이 클 수록 IoU값이 커진다.
- 정답 영역내에서도 너무 작은 영역이 겹친다거나 정답 영역에 포함되어 있어도 너무 크면 iou의 값이 낮아진다.
- 즉, IoU값이 클수록 "물체가 잘 검출되고 있다"고 할수 있음

## 예시
![image](https://user-images.githubusercontent.com/83350692/236678530-edad2e24-7feb-4669-82fc-08300192d7c8.png)
- 초록색이 예측한 bounding box, 노란색이 정답 bounding box
- 초록 영역중에서 노란 영역이 차지하는 비율이 IoU임
- 초록 영역이 bounding box의 합집합 영역, 노란 영역이 두 bounding box의 교지밥 영역이라서 이 비율이 IoU라고 명명되었음


# mAP(mean Average Precision)
mAP는 precision의 평균으로 많이 사용하는 지표 중 한가지이다.
이를 계산하기 위해서는 다른 개념을 알야아함

    True Positive (TP) : 맞다고 추측하고 실제로 맞음. IOU >= Threshold
    False Positive (FP) : 맞다고 추측하고 실제로는 틀림. IOU < Threshold
    False Negative (FN) : 아니라고 추측하고 실제로는 맞음. Ground Truth를 아예 detect 못함
    True Negative (TN) : 아니라고 추측하고 실제로 아님.

    여기서 Threshold 는 일반적으로 50%, 75%, 95%로 설정된다. 

### precision
- 주로 예측된 결과가 얼마나 정확한지를 나타내는데 사용이 됨
- 검출된 것들 중에서 정답을 맞춘것들의 비율이 어느정도인지를 알 수 있기에 검출 결과가 얼마나 정확한지를 알 수 있다.
- 계산식은 true positive(실제 positive를 positive로 잘 예측한 경우, TP)를 TP와 False Positive(실제 Negative를 positive로 잘못 예측한 경우, FP)의 합으로 나눠줘서 계산을 하게됨
- 즉 precision을 높이기 위해선 모델이 예측 box를 신중하게 쳐서 FP를 줄여야한다.

![image](https://user-images.githubusercontent.com/83350692/236679160-c45596a4-10fa-4e1e-bada-8750c3f53d19.png)

### Recall
- 검출되어야 할 객체들 중에서 제대로 검출된 것의 비율들 뜻함
- recall은 precision과는 다르게 입력으로 positive를 주었을 때 얼마나 positive로 예측하는지를 나타내는 데 사용이 됨
- 계산식은 TP를 TP와 False Negative(실제 positive를 negative로 잘못 예측한 경우, FN)의 합으로 나눠줘서 계산을 하게됨
- 즉, recall을 높이기 위해선 모델 입장에서는 되도록 box를 많이 쳐서 정답을 맞혀서 FN을 줄여야한다.
- 그러므로 precision과 recall은 반비례 관계를 갖게됨
- 두 값 모두 높은 모델이 좋은 모델임

![image](https://user-images.githubusercontent.com/83350692/236679174-59708b42-1cb1-46be-ad04-e029edf17594.png)

### AP(AVERAGE PRECISION)와 mAP
- AP의 계산은 recall을 0부터 0.1 단위로 증가시켜서 1까지(총 11개의 값) 증가 시킬 때 필연적으로 precision이 감소하는데, 각 단위마다 precision 값을 계산하여 평균을 내어 계산을 한다.
-  즉 11가지의 Recall 값에 따른 Precision 값들의 평균을 AP라고 한다.
- 하나의 클래스 마다 AP 값을 계산 할 수 있으며 전체 클래스 갯수에 대해 AP를 계산하여 평균을 낸 값이 바로 mAP 이다. 


# FPS
- 속도를 나타낼 때는 보통 초당 몇장의 이미지가 처리 가능한지를 나타내는 FPS(Frame Per Second)를 사용함