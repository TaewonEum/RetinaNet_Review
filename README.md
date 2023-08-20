# RetinaNet_Review

## RetinaNet Summary

### Problem definition

Detection은 localization과 classification의 문제임

![image](https://github.com/eumtaewon/RetinaNet_Review/assets/104436260/ac49227c-cb3a-429f-abe5-e53007ece5d8)

위의 그림처럼 Detection은 어느 위치에 있는지 박스 생성과 동시에 어떠한 클래스의 객체인지 분류까지 한번에 진행이 가능

Detection 모델은 One-Stage-Detector와 Two-Stage-Detector로 나뉨

One Stage Detector는 물체의 위치와 클래스를 한 번에 예측하는 모델이며

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

ResNet은 CNN 기반의 모델로 이미지의 특징을 추출하는 모델로 생각하면 됨

ResNet은 모델의 층이 깊어져도 학습이 잘 되도록 구현한 모델이라고 생각하면됨

FPN은 다른 크기들의 객체를 탐지하는 시스템의 기본 요소임

FPN은 Resnet에서 추출된 이미지를 입력으로 받아 fully convolutional 방법으로 다양한 크기의

Feature map들을 출력한다. ResNet과는 독립적으로 수행됨

FPN은 다양한 크기의 이미지에서 특징을 추출하는 역할을 함

이를 통해 객체가 이미지의 어디에 위치하고 그 크기에 상관없이 객체를 탐지하는 데 도움을 줌

추가적으로 FPN에서 각 단계마다 다양한 크기와 비율의 앵커 박스를 정의합니다. 각 FPN 단계마다 여러개의 앵커 박스를 생성하고 이를 통해

다양한 크기와 비율의 물체를 탐지합니다.

- 정리

ResNet은 이미지를 입력 받아 이를 통해 특징을 추출함, 초기 Resnet 층에서는 간단한 특징에 대해 학습하고 이후에는 더 복잡한 특성들을 학습함

이렇게 학습된 특성은 피처맵으로 출력되고 각 FPN layer에 전달 추출

ResNet에서 추출된 피처 맵은 FPN layer를 통과하게됨, FPN은 피처 맵을 입력으로 받고 다양한 크기의 피처맵을 생성함

때문에 큰 객체, 작은 객체를 효과적으로 탐지할 수 있게됨

### Two task subnet

Backbone Network에서 최종 FPN을 통과한 피처 맵과 anchor box정보를 입력 받아 두개의 subnet network

Classification subnet과 Box regression subnet으로 전달됨

Classification subnet: 피처맵의 앵커 박스에 대해 물체가 있을 확률을 계산합니다. 또한 앵커 박스가 탐지한 객체에 대한 클래스에 대한 분류도 진행합니다.

Box regression subnet은 각 앵커 박스에 대한 Bounding Box의 위치를 조정함. 객체의 크기, 회전, 위치 등의 다양한 변형을 처리하고 이러한 변형을 보정하여 정확한 바운딩 박스를 얻는 역할을 함

### SAMPLE DATA

AI HUB 넙치광어 데이터로 진행

해당 데이터의 라벨링 데이터를 RetinaNet input구조에 맞추어 변형하고 파라미터를 수정하여 학습 진행함

