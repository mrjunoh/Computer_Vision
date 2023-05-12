# 객체 탐지(object detection)
- 컴퓨터비전의 하위 분야 중 하나인 object detection은 이미지 및 비디오 내에서 유의미한 특징 객체를 감지하는 작업을 한다.
- 객체 감지에는 얼굴 인식, video tracking, 사람 수 세기 등 다양한 분야의 문제를 해결하기 위해서 사용
- 자율 주행 자동차, 의료 데이터에서 사용함

![image](https://user-images.githubusercontent.com/83350692/235354226-145dba57-7f01-41f4-98b6-c47abb98a5f9.png)

    object detection은 비디오나 사진에서 어떤 물체가 있는지 분류(classification)을 하고 분류된 object가 어디 있는지 bounding box를 통해 위치를 파악하는(Localization)과정을 통해 object를 감지할 수 있다.

    object detection은 object의 수에 따라서 하나의 물체를 찾는 Single-object detection과 여러 물체를 찾는 Multi-object Detection이 있다.

    즉 다음과 같이 정의함
    Object Detection = Multipled label Classification + Bounding Box Regression

### 객체 탐지 공부 순서(빨간글씨 중점으로)
![image](https://user-images.githubusercontent.com/83350692/235354359-117dfa57-e392-4836-aa3b-527f4938f296.png)



## 1-stage Detector vs 2-stage Detector
![image](https://user-images.githubusercontent.com/83350692/235354675-77ad515a-a4c6-40b7-82ed-f59849988b5e.png)
- object detection은 1-stage(아래)와 2-stage(위)방식으로 나눌 수 있음

### 1-stage Detector
![image](https://user-images.githubusercontent.com/83350692/235354738-4dc5ee97-2d3b-4c03-8861-358e821cfd4a.png)

- 1-stage는 Region proposal과 detection을 동시에 수행한다.
- 대표적으로는 YOLO, SSD 계열이 있다. 
- 2-stage보다 속도가 빠르지만 정확도가 좋지 않다는 특징을 가짐
- 속도가 뛰어나 자율 주행 및 많은 곳에서 사용됨


### 2-stage Detector
![image](https://user-images.githubusercontent.com/83350692/235354797-ecb8c7de-f0fd-48fd-8d46-f5c8f6c6c3c5.png)

- 2-stage는 Region proposal과 detection을 순차적으로 수행한다.
- 대표적으로는 R-CNN 계열이 있다.
- 2-stage는 순차적으로 수행하기 때문에 연상량이 많아 속도는 느리지만 정확도가 좋다는 특징을 가짐