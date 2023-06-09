# RetinaNet_Review

## RetinaNet Summary

### Problem definition

Detection은 localization과 classification의 문제임

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/ac49227c-cb3a-429f-abe5-e53007ece5d8)

위의 그림처럼 Detection은 어느 위치에 있는지 박스 생성과 동시에 어떠한 클래스의 객체인지 분류까지 한번에 진행이 가능

Detection 모델은 One-Stage-Detector와 Two-Stage-Detector로 나뉨

One Stage Detector는 물체의 위치와 클래스를 한 번에 예측하는 모델이고

Two Stage는 두 단계를 나누어 수행하는 모델임

RetinaNet은 One Stage Model이며, 정확도가 낮은 One Stage Model의 단점을 극복한 모델임

One Stage Model은 속도가 빠른 반면 정확도가 비교적 낮은 편임

### RetinaNet Architecture

RetinaNet은 두 부분으로 구성됨. Backbone network와 Two-task subnet

Backbone network 일반적으로 ResNet과 FPN의 조합으로 구성됨

### Backbone network

ResNet->이미지에서 특성 추출

FPN-> 다양한 크기와 비율의 객체를 탐지하는 데 도움이 되는 다양한 해상도의 특성맵 생성

### Two-task subnet
