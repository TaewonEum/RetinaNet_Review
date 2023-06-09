# RetinaNet_Review

## RetinaNet Summary

### Problem definition

Detection은 localization과 classification의 문제임

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/ac49227c-cb3a-429f-abe5-e53007ece5d8)

위의 그림처럼 Detection은 어느 위치에 있는지 박스 생성과 동시에 어떠한 클래스의 객체인지 분류까지 한번에 진행이 가능합니다.

### RetinaNet Architecture

RetinaNet은 두 부분으로 구성됨. Backbone network와 Two-task subnet

Backbone network 일반적으로 ResNet과 FPN의 조합으로 구성됨

### Backbone network

ResNet->이미지에서 특성 추출

FPN-> 다양한 크기와 비율의 객체를 탐지하는 데 도움이 되는 다양한 해상도의 특성맵 생성

### Two-task subnet
