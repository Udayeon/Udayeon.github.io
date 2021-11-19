---
layout: post
title: (Matlab) YOLOv2를 활용한 Multi objet Detector 1 - labeling
description: |
  
hide_image: true
tags:
  - programming
published: true
---

(Matlab) YOLOv2를 활용한 Multi objet Detector 1 - labeling

# Training Data labeling
* * *
Machine Learning의 방법론 중 하나인 Deep Learning에서, 지도학습(Supervised Learning)을 하기 위해선, 주어지는 데이터에 대해 Label이 
필요하다. 지도학습이란, 기계에게 사진을 보여주고 이 사진에 대한 정답을 함께 알려주는 식으로 학습하는 것으로,
Label은 여기서 '정답'에 해당하는 것이다. 그러니까, 정답을 잘못알려주면 즉 Label을 정확히 하지 못한다면 모델의 성능이 떨어질 수 
있으므로 정확한 Label이 중요하다. 기계를 학습시키는 데에 사용하는 Training data에 labeling한 것을 학습에 활용하고, 
학습을 마치면 Test data를 이용해 학습이 잘 되었는지 평가하게 된다. 따라서, 학습의 첫 단계는 Training data에 정확한 labeling을 
하는 것이라 할 수 있겠다.   
   
Training Data를 학습시키기 위해 KITTI의 left color images of object data set를 사용한다. image dataset을 다운로드받고
training labels of object data set을 다운로드 한다.

![image](https://user-images.githubusercontent.com/69246778/129994047-e0e7fda3-5777-4803-b022-f9808f6ae6ef.png)
   
![image](https://user-images.githubusercontent.com/69246778/129994099-0f54164e-7886-42ba-9696-d7462e5bb8df.png)
이렇게 image data set을 저장했는데, 사정상 노트북으로 학습해야하고 project 기한이 짧아 이 중 일부 사진만 학습할 예정이다.
   
Training Data에 Labeling을 해주기 위해 Matlab의 **Image Labeler**를 이용한다.
![image](https://user-images.githubusercontent.com/69246778/129994850-842a8e26-8856-44de-9d29-14fd8018d7e2.png)
Image Labeler를 실행하고 Data set을 Loading 해준다.
![image](https://user-images.githubusercontent.com/69246778/129996008-f59cd6d3-7c86-4d64-a93c-69767e1fbe6d.png)
중간에 번호가 누락된게 있는데 대략 300개 정도 받아왔다. 이제 여기서 Labeling을 해주면 되는데 일일히 박스를 그릴 수도 있지만
매우 시간이 오래 걸리기 때문에 GroundTruth 파일을 사용한다.

⭐시작부터 난항⭐   
Image labler에 Image를 불러와서 수동으로 labeling한 후, 이를 ground Truth로 export하는 것은 간단하다. 걍 Export label버튼을 누르고 gTruth형식으로 설정만 해주면
되기 때문이다. 근데 내가 하고 싶은 것은 table로 주어진 ground Truth를 Image labeler로 불러와서 자동으로 챡챡 bbox가 그려지게 만드는 것인데 이 과정이 생각보다 어려웠다.   
결론만 말하면, Image labeler에서 ground Turth를 챡챡 그리기 위해선 이 값을 **table이 아닌 groundTruth** 클래스로 불러와야만 한다. 따라서, 내가 갖고 있는 table로
새로운 ground Truth 객체를 만들어야 한다. [Matlab 도움말](https://kr.mathworks.com/help/vision/ref/groundtruth.html?s_tid=doc_ta)을 참고했다.
   
![image](https://user-images.githubusercontent.com/69246778/130350038-b7a0c014-5d46-4e8e-9594-6478e64bb8e0.png)
최종적으로 하고 싶은게 이렇게 gTruth class를 갖는 객체를 생성하는 것이다. 만들어진 **gTruth**를 Image labeler에서 **Import Labels**하면 bbox가 챡챡 그려질 것이다.
gTruth를 만들 때 필요한 것들이 **dataSource, labelDefs, labelData**라는 것을 확인하고 하나씩 생성해보자.

## 1. dataSource   
dataSource는 각 image마다의 label값을 가지고 있는 data다. 내가 table 형태로 갖고있던 car와 pedestrian에 대한 ground Turth를 사용하면 된다.
![tempsnip](https://user-images.githubusercontent.com/69246778/130350265-7f5212fe-d82a-47d0-8ae2-bc16242904aa.png)
나는 KITTI dataset을 이용했는데 어쩐 일인지 다운받는 과정에서 이미지 몇 개가 누락되었다. 그래서 groundTruth도 수정해주었다. 
(자세히보면 이미지가 000000,000001,000002,000003...이 아니라 000000,000003,000005... 로 띄엄띄엄임...)
이미지가 저장되어 있는 경로도 잘 설정되었는지 확인하자. 수정을 마치고 groundTruth.mat로 저장한 후 다음과 같은 code를 작성한다.

```
>> data = load('groundTruth.mat');
>> imageFilenames = data.groundTruth.imageFilename(1:184)
>> dataSource = groundTruthDataSource(imageFilenames);

```
이 code는 imageFile을 **groundTruth.mat**에서 받아와서 이걸로 dataSource를 만들겠다는 의미. 
![image](https://user-images.githubusercontent.com/69246778/130350446-465af634-8906-4584-a9f0-85ed09cb3a3e.png)
이렇게 dataSource를 만들었다.


## 2. labelDefs   
다음은 label을 정의할건데 다음의 code로 쉽게 작성가능 하다.
   
```
>> ldc = labelDefinitionCreator();
>> addLabel(ldc,'Car',labelType.Rectangle);
>> addLabel(ldc,'Pedestrian',labelType.Rectangle);
>> labelDefs = create(ldc)
```
![image](https://user-images.githubusercontent.com/69246778/130350478-ae369501-e203-43bb-ba84-a735bcb45684.png)
난 car랑 pedestrian만 사용했음.

## 3. labelData
마지막으로 labelData를 만들어주자.

```
>> CarTruth = data.groundTruth.Car(1:184)
>> PedestrianTruth = data.groundTruth.Pedestrian(1:184)
>> labelNames={'Pedestrian';'Car'}
>> labelData=table(PedestrianTruth,CarTruth,'VariableNames',labelNames)
```

![image](https://user-images.githubusercontent.com/69246778/130350557-04ef16f3-0588-47e6-8711-4c58b58ce81c.png)

## 4. gTruth
위에서 만든 3가지를 이용해 최종적으로 gTruth를 만든다. 이 과정에서 오류가 발생했었는데 걍 첨부터 천천히 다시하니까 됐음.

```
>> gTruth = groundTruth(dataSource,labelDefs,labelData)
```

![image](https://user-images.githubusercontent.com/69246778/130350622-2d7bdf85-393c-4e19-82d3-9d00c33009ea.png)
이렇게 되면 성공임   
   
![image](https://user-images.githubusercontent.com/69246778/130350639-e89857d3-3e39-40bf-b02c-122a76d48b96.png)
작업 공간을 보면, 파란색으로 동그라미한 게 내가 기존에 갖고있던 table형식의 groundTruth(image labeler에서 사용 불가)이고   
빨간색이 그로부터 새로만든 ground Truth형식의 data임.

## 5. Image labeler
Image labeler 앱을 실행하고 이미지를 불러온 후 **Import Labels -> From Workspace -> gTruth** 하면 다음과 같이 bbox가 챡챡 생성된다.
   
![image](https://user-images.githubusercontent.com/69246778/130349797-03b894fc-b497-4caf-ac75-843df345fe3c.png)
![image](https://user-images.githubusercontent.com/69246778/130349805-e3f399c7-8992-4066-994e-a7ccf77ec01f.png)
![image](https://user-images.githubusercontent.com/69246778/130350748-e614c600-c7e9-460b-b55d-1b007319097a.png)
200장 정도되는 사진에 일일히 손으로 라벨링 했으면 매우 오래 걸리고 정확도도 떨어졌을텐데 이렇게 하니까 한번에 챠라락 완성
