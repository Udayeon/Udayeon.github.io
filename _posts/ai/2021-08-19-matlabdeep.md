---
layout: post
title: YOLOv2
description: |
  YOLOv2를 활용한 Multi objet Detector
hide_image: true
tags:
  - ai
published: true
---


# 1. Training Data labeling
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
매우 시간이 오래 걸리기 때문에 아까 다운로드했던 training labels of object data set을 이용한다.
   
![image](https://user-images.githubusercontent.com/69246778/129996148-ec7dfdeb-3469-414a-afc9-c55692beacb0.png)
사진마다의 Label이 이렇게 메모장 형식으로 저장되어 있다. 
   
![image](https://user-images.githubusercontent.com/69246778/129997808-0de27663-baa3-4932-868d-65ce83451d7c.png)
이 txt들을 어떻게 활용할지 모르겠어서 일단 하나 열어보았다. Training data set의 000000 사진에 해당하는 label이 들어있는데 각 숫자가
의미하는 바가 뭔지 확인할 필요가 있겠다.
![image](https://user-images.githubusercontent.com/69246778/129997985-4c8ae3c7-c195-4214-9007-af427a049cf0.png)
image에 Pedestrian을 직접 라벨링 해보았다. 그리고 이걸 file로 Export해본다.
   
![image](https://user-images.githubusercontent.com/69246778/129998531-350bb31e-fd28-4a35-8d29-b056cfd1cd5b.png)
그러면 이렇게 gTruth라는 data로 저장된다. 이렇게 직접 labeling하고 file로 export하면 ground truth가 된다는 걸 확인할 수 있다.
이gTruth와 다운받았던 label txt를 비교해보면   
![image](https://user-images.githubusercontent.com/69246778/129998757-c490957a-5c53-410e-b1f8-832ee210b463.png)
이 부분이 label의 위치?를 결정하는 어떤 유의미한 data란 걸 추측할 수 있겠다.
   
하나만 더해보자.
![image](https://user-images.githubusercontent.com/69246778/129998907-88e9301a-1116-40c2-9130-75b59e30c144.png)
000013 image에 car를 labeling하고 마찬가지로 file로 export 해본다.
![image](https://user-images.githubusercontent.com/69246778/129999083-07a57bfe-2d2f-44ca-9340-10f6d170c7b9.png)
기존의 gTruth에 이미지 000013에 해당하는 label data가 추가되었다. image이름이 000013이지만 중간에 누락된 것들이 있어서 
순서상으론 9번째 이미지라 9행에 입력되었다.

그래서 든 생각이, 거꾸로 gTruth 파일을 만들어서 import하면 알아서 라벨링이 되지 않을까 싶었다. 이 역과정이 되는지 확인하기 위해 
Label Data를 먼저 저장하고 Image Labeler에서 import할 수 있는지 확인한다.
![image](https://user-images.githubusercontent.com/69246778/129999829-2be6df8e-564e-463b-9bd5-16dcf238d4d7.png)
   
저장한 Label을 Import Lables로 불러오니까 data set에 Labeling이 되는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/69246778/130000007-f29886f7-240c-4755-bc57-994e42be5212.png)
   
결론적으로, gTruth file을 수정해서 불러오면 Training data set에 labeling이 한번에 되겠다는 걸 알 수있다.
그럼 이제 gTruth file의 나머지 부분도 label txt를 참고해서 수정해보자.
gTruth file에 필요한 값은 [x position, y position, width, height]
    
예를들어, 000000 image의 label data가 이런식이면
![image](https://user-images.githubusercontent.com/69246778/130002874-5af81e12-4c58-40e8-a772-faac5aef08c4.png)
gTruth table 첫번째 행에 [712.40, 143.00, 810.73-712.40, 307.92-143] 즉, [712.40, 143, 99, 165]를 입력하면 되는 것이다.
   
근데 문제가 이 gTruth file이 table형식이면 안되고 groung truth 형식이여야 하는데 형식 전환하는 법을 몰라 막혔다.


