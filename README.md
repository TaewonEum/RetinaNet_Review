# RetinaNet_Review

## RetinaNet Summary

### Problem definition

Detection은 localization과 classification의 문제임

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/ac49227c-cb3a-429f-abe5-e53007ece5d8)

위의 그림처럼 Detection은 어느 위치에 있는지 박스 생성과 동시에 어떠한 클래스의 객체인지 분류까지 한번에 진행이 가능

Detection 모델은 One-Stage-Detector와 Two-Stage-Detector로 나뉨

One Stage Detector는 물체의 위치와 클래스를 한 번에 예측하는 모델이고(임

Two Stage는 두 단계를 나누어 수행하는 모델임

RetinaNet은 One Stage Model이며, 정확도가 낮은 One Stage Model의 단점을 극복한 모델임

One Stage Model은 속도가 빠른 반면 정확도가 비교적 낮은 편임

One Stage Model Yolov2와 SSD에서는 각각 416x416, 300x300 이미지에서 845개, 8732개의 anchor박스를 생성하여

Prediction을 진행함

이러한 경우 아래와 같은 background error 발생 가능성이 매우 높음

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/66027f3d-df7d-4181-a3db-7fe2d3624587)

객체를 포함하고 있는 anchor box 개수보다 배경을 담고있는 anchor box수가 훨씬 더 많음

둘의 비율차이가 크기 때문에 class imbalance 문제가 발생함

RetinaNet은 이러한 foreground ,background class imbalance 는 2가지의 문제를 야기한다고 정의함
 
1. training is inefficient as most locations are easy negatives that contribute no useful learning signal

2.the easy negatives can overwhelm training and lead to degenerate models.


배경을 담고있는 (background) anchor의 수가 훨씬 더 많을

### RetinaNet Architecture

RetinaNet은 두 부분으로 구성됨. Backbone network와 Two-task subnet

Backbone network 일반적으로 ResNet과 FPN의 조합으로 구성됨

### Backbone network

ResNet->이미지에서 특성 추출

FPN-> 다양한 크기와 비율의 객체를 탐지하는 데 도움이 되는 다양한 해상도의 특성맵 생성

### Two-task subnet
