# Python TensorFlow기반 Yolo 모델 구현 프로젝트

!!! Darknet 파일은 https://github.com/thogood212/darknet 에서 확인 가능합니다.

![1](https://user-images.githubusercontent.com/87019897/175307608-85cd433b-550c-4a39-9927-166c5cb1fffb.png)

## 프로젝트 선정 이유

1. 관심사

- 자율주행과 물류

2. 문제점

- 물류 분야에서는 코로나로 인해 물동량이 코로나 이전보다 급격히 상승되었고 상승한 물동량을 제대로 처리하지 못하는 물류 대란이 일어나기도 했습니다.
- 이러한 과부하로 업무량이 늘어나면서 물류 종사자들의 파업 과 과도한 업무가 문제가 되기도 했습니다.

3. 해결방법

- 이러한 문제를 해결하기 위해 이전부터 글로벌 물류 기업인 아마존, 알리바바 등에서는 로봇AI를 사용하여 창고 수용량과 효율성을 높히기 위해 노력하고 있습니다.
- 물류 창고에서의 상품 보관 및 선별, 포장, 배송, 재고관리를 모두 관리하는 풀필먼트(통합물류서비스)에서 업무의 효율성 증진, 종사자의 업무 편의성 증진을 위해서 AI로봇의 도입은 필요합니다.

4. WHY?
- 위에서 설명한 물류, 자율주행에서는 Object Detection 기술을 적극 활용하여 기술 개발을 통한 해결방안을 마련하고 있습니다. 그렇기 때문에 이러한 기술의 기초를 쌓기 위해서 이 프로젝트를 진행합니다.

---

## 프로젝트 목표

- 자율 주행과 물류AI로봇에서 사용되는 **Object Detection 기술을 구현**함으로써 차세대 중요 기술의 **기초적인 지식**을 쌓는 것에 목표를 두고 있습니다.

## 프로젝트 진행 방향

1. **Object Detection에 대한 기본 지식과 Yolo v3,4모델에 대한 이해**
2. C언어로 작성된 물체 인식 오픈 소스 신경망(Darknet)으로 학습된 대중적인 가중치를 사용하여 구현예정.
3. C언어로 작성된 가중치를 TensorFlow에 적용하기 위해 변환이 필요하고 변환 후 TensorFlow를 사용하여 YOLO v3,4 모델을 통해 **Object Detection 구현** (Basic Target)
4. **추가 기능 Total Object counting, Class counting 구현**
5. **커스텀 학습 후 실험** (Advanced Target)

---

## Yolov4의 구조 이해

### **1. backbone network : Darknet53**

- backbone 이란? **classification용으로 만든 pre-trained 모델**(ResNet등)을 FC layer부분을 바꿔 **다른 용도인 Detection, Segmentation, Pose Estimation 등으로 사용**하는 것.
- 기존의 목적이었던 분류 기능(FC layer)를 유지하되 사전학습된 Dataset 가중치를 그대로 가져가 사용하는 Transfer-learning과 사전학습된 가중치를 새로운 Dataset을 통해 조정하여 다른 용도로 사용하는 Fine-tuning과 구분해야한다.
- C언어를 기반으로 하기 때문에 Python에서 구현한 모델(TensorFlow, Pytorch)을 사용할 때에 가중치를 변환시켜주어야 합니다.

### **2. Head : Yolo v4**

Object Detection에선 **classification과 localization을 어떻게 수행**하느냐에 따라서 **two-stage detector(나누어 수행)와 one-stage detector(동시 수행)**로 모델이 구분되고 그 하위에는 **Anchor의 사용 유무**로 Anchor-based, Anchor-free 로 분류 됩니다.


Yolo 특징

- Yolo v4는 **Anchor-based One-stage detector**로 two-stage detector보다 실행 속도가 빠릅니다.(가장 빠른 객체 검출 알고리즘)
- 단점으로는 작은 물체를 구별하기 어렵다는 점이 있습니다.
- Yolo는 **backbone 모델을 기반**으로 하며 **특징 추출기(Feature Extractor)**라고도 불립니다.

### **3. 평가지표**

- IOU(Intersection Over Union) : 실측값(Ground Truth)과 모델이 예측한 값이 얼마나 겹치는지를 나타내는 지표

![2](https://user-images.githubusercontent.com/87019897/175308285-902d530f-748d-47ad-bd6e-87716953c1c8.png)

- Precision(정밀도), recall(재현율) : Object Dectection 기술이 사용되는 분야에서는 특징에 따라 정확도를 높여 필요한 객체를 탐지해야하는 경우, 기준을 낮추어 최대한 많은 객체를 탐지 해야하는 경우도 있습니다. 이러한 경우 임계값 조정을 통해 원하는 결과를 얻도록 조정해야합니다.
- AP (Average Precision, 평균 정밀도) 와 mAP(mean Average Precision)
    - ROC 곡선 그래프의 아래 영역에 해당하는 값
    - 단일 클래스의 AP 값을 구하고 평균을 통해 mAP를 구하여 평가지표로 활용합니다.

---

## **결과 확인**

**<YOLO v4 Deep sort model 구현>**

1. 기본적인 sort 진행

![3](https://user-images.githubusercontent.com/87019897/175308884-eeff450a-9e45-42ba-add0-2a4865842059.png)

2. Object counting 기능 구현

![4](https://user-images.githubusercontent.com/87019897/175308914-5d74cd28-b998-432a-b308-048bc087e11b.png)

3. 클래스 별 counting 기능 구현

![5](https://user-images.githubusercontent.com/87019897/175308943-bc466ea5-7d0d-4707-b7ec-459728da0fad.png)

3-1. 클래스 별 counting 기능 구현 다른 영상 ([링크](https://tv.kakao.com/channel/3954198/cliplink/426258400))

<br/>

## **YOLO v4 customized 구현**

Darknet(https://github.com/AlexeyAB/darknet)을 통해 기본 가중치 값을 활용하여 약 160개 이미지가 있는 마스크 데이터셋(roboflow 출처)을 학습시키고 최종적으로 그래프와 test 이미지를 통해서 확인했습니다.

<br/>

### <**Customize 순서>**

위에서 Darknet을 clone하고 커스텀 학습할 이미지 데이터셋이 준비된 후에 해당 과정을 진행하시면 됩니다.

**1. Darknet cfg 복사 ( cfg, config = 소프트웨어에 특정한 설정을 저장하는 데 사용되는 구성 파일 )**

cfg의 파라미터를 조정하여 커스텀 학습하기 위해 기존 yolov4-custom.cfg 파일을 복사하여 yolov4-obj.cfg 파일을 만들어줍니다.

```python
!cp ./darknet/cfg/yolov4-custom.cfg ./darknet/cfg/yolov4-obj.cfg
```

**2. 복사한 yolov4-obj.cfg의 파라미터 조정**

1. batch size = 64 or 32 ( size가 작을 수록 학습시간이 오래걸리나 좋은 성과를 낼 수 있습니다.)
2. subdivisions=16
3. **max_batches = 6000** (최소 6000 권장, 클래스 개수x2000 정도로 설정)
4. **steps = 4800, 5400**(max_batches의 80%, max_batches의 90%로 설정)
5. **classes = 2(클래스 개수)** , **filters = 18((클래스 개수 + 5)*3)**
6. image width = 416, image height = 416(최소32, 416,608이 기준)

**3. 이미지 데이터셋 이름 리스트 만들기 (train.txt, valid.txt, test.txt)**

**4. obj.data, obj.nams 파일 작성하기**

**5. 가중치 다운로드(conv layer 가중치)**

**6. 데이터 학습**

<br/>

### **YOLO v4 customized** 모델 결과 확인

제대로 학습되지 않았을 경우

![6](https://user-images.githubusercontent.com/87019897/175309145-5fb1a7d8-8212-40b3-bf3e-f51cf5ad553a.png)

학습을 끝마친 경우

![7](https://user-images.githubusercontent.com/87019897/175309164-2b1210bd-63f6-40a8-b97b-5f6f5a141800.png)

### **느낀 점**

1) 어렵다고 느꼈던 Object Detection task를 수행할 수 있는 모델을 구현해봄으로써 모델에 대한 기초 이해 및 사용법을 알게 되었습니다.

2) 오픈소스 코드를 통해 복잡한 모델을 구현하여 커스텀해보아 Computer Vision 분야 외에도 다양한 분야에서 해당 경험을 활용하여 진행할 수 있다고 느꼈습니다.
