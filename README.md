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
 
1.training is inefficient as most locations are easy negatives that contribute no useful learning signal

2.the easy negatives can overwhelm training and lead to degenerate models.

즉, easy negative 배경이 사진에서 비율이 높기 때문에 객체를 찾는 것이 비효율적이고 

easy negative에 모델이 압도되어서 성능이 떨어짐

RetinaNet은 이러한 문제를 focal Loss 방식을 도입하여 해결함

### Focal Loss

Focal Loss는 쉽게 말하자면 잘 분류되는 것들-> 더 작은 loss를 주어서 분류하기 쉬운 문제에 대한 학습 비중을 줄이는 것이 focal Loss의 개념임

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/4459ab2a-6e40-4d5d-a7e6-4a5afd244f7a)

x가 1이면 잘 분류가 된 것이고, 0이면 잘 분류가 안된 것을 나타내는 위 그래프에서

x가 잘 분류될수록 Loss가 점점 더 작아집니다.

즉 쉽게 판단할 수 있는 sample에 대해서는 loss를 조금주어 영향력을 낮춥니다. 반면에 어려운 문제에는 Loss값을 크게주어 학습이 집중될 수 있도록 함

### RetinaNet Model Architecture

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/9d4cdbd5-b1ac-4402-8dfd-ac73f57a22fe)

RetinaNet은 두 부분으로 구성됨. Backbone network와 Two-task subnet

Backbone network 일반적으로 ResNet과 FPN의 조합으로 구성됨

Two-task subnet은 class subnet, box subnet으로 구성됨

backbone Network는 Feature map을 추출하고

class subnet에서 클래스 분류를 진행, box subnet에서 bounding box regression을 수행함

### Backbone network

- Resnet Backbone: 이미지 특성 추출
- FPN(Feature Pyramid Network): Resnet에서 추출된 특성을 입력받아 다양한 크기의 피처 맵 생성


