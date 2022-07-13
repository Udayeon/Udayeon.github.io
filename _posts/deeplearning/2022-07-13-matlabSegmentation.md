---
layout: post
title: 
description: |
  (Matlab) 딥러닝을 사용한 Semantic Segmentation
hide_image: true
tags:
  - deeplearning
published: true
---

# (Matlab) 딥러닝을 사용한 Semantic Segmentation
* * *

# 1. Data 준비

## 1.1. image labeling
**Image Labelr**를 사용한다. 
1. 
![image](https://user-images.githubusercontent.com/69246778/178672956-9e5a1a56-94b2-434f-892c-84f70ecb215b.png)
**Import - From File - 원본 images 폴더 열기**   
   
2.
![image](https://user-images.githubusercontent.com/69246778/178673269-5175fba2-d2c4-4ac2-bfe1-93caf64fc94a.png)

3.
![image](https://user-images.githubusercontent.com/69246778/178673407-83f66226-5d1f-486c-ae0c-128391389208.png)

4.
![image](https://user-images.githubusercontent.com/69246778/178673481-c17c2f35-2245-47f0-8e15-e67e963ada32.png)
![image](https://user-images.githubusercontent.com/69246778/178673685-b197e856-f95d-4dc1-8aa3-7dbc9840000b.png)
**Pixel label** 선택하고 class name지정, 주의할 점은 클래스가 2개 이상이여야함! **background도 라벨링하기**

5.
![image](https://user-images.githubusercontent.com/69246778/178673982-c01db7ba-f939-4a67-b319-3d9701465dc6.png)
불러온 원본 이미지 모두 위와 같이 라벨링해주기.

6. 
![image](https://user-images.githubusercontent.com/69246778/178674124-5e9269da-03f4-4678-a586-cff509957e96.png)
라벨링 다 되면   
**Export - To file** : 바탕화면에 **PixelLabelData**라는 이름으로 라벨링된 이미지들이 저장되어 있는 폴더가 생성됨.   
**Export - To workspace** : gTruth.mat 파일 생성됨.   

## 1.2. dataset 준비
```
# 원본 이미지 폴더 열어서 image Directory로 만들기
imDir = fullfile('C:','Users','유다연','Desktop','images');
imds = imageDatastore(imDir);

# 라벨링된 이미지로 pixel Directory 만들기
classNames = ["lane" "background"];
pixelLabelID = [1 2];
pxDir = fullfile('C:','Users','유다연','Desktop','PixelLabelData');
pxds = pixelLabelDatastore(pxDir,classNames,pixelLabelID);


####### Image dataset #######
# Dataset 설정
rng(0); 
numFiles = numel(imds.Files);
shuffledIndices = randperm(numFiles);

# 전체 데이터의 60%는 훈련 데이터
numTrain = round(0.60 * numFiles);
trainingIdx = shuffledIndices(1:numTrain);
numVal = round(0.20 * numFiles);

# 검증 데이터, 테스트 데이터는 각각 20%
valIdx = shuffledIndices(numTrain+1:numTrain+numVal);
testIdx = shuffledIndices(numTrain+numVal+1:end);

trainingImages = imds.Files(trainingIdx);
valImages = imds.Files(valIdx);
testImages = imds.Files(testIdx);

# imageDatastore에 담아야 훈련에 쓸 수 있다.
imdsTrain = imageDatastore(trainingImages);
imdsVal = imageDatastore(valImages);
imdsTest = imageDatastore(testImages);


####### Label dataset #######
trainingLabels = pxds.Files(trainingIdx);
valLabels = pxds.Files(valIdx);
testLabels = pxds.Files(testIdx);

# label은 pixelLabelDatastore에 담아야 쓸 수 있다. 
pxdsTrain = pixelLabelDatastore(trainingLabels, classNames, pixelLabelID);
pxdsVal = pixelLabelDatastore(valLabels, classNames, pixelLabelID);
pxdsTest = pixelLabelDatastore(testLabels, classNames, pixelLabelID);


# 이미지와 레이블을 묶어서 훈련 및 검증 데이터로 만들어줌.
trainingData = combine(imdsTrain,pxdsTrain);
validationData = combine(imdsVal,pxdsVal);
```
   
데이터셋 만들고 잘 됐는지 확인 한번....
``` 
I = readimage(imds,1);
I = histeq(I);
imshow(I)


classNames = ["lane" "background"];
pixelLabelID = [1 2];
pxDir = fullfile('C:','Users','유다연','Desktop','PixelLabelData');
pxds = pixelLabelDatastore(pxDir,classNames,pixelLabelID);

C = readimage(pxds,1);
B = labeloverlay(I,C);
figure
imshow(B)


tbl = countEachLabel(pxds)
frequency = tbl.PixelCount/sum(tbl.PixelCount);

bar(1:numel(classNames),frequency)
xticks(1:numel(classNames)) 
xticklabels(tbl.Name)
xtickangle(45)
ylabel('Frequency')
```
![image](https://user-images.githubusercontent.com/69246778/178675988-4c19b1a0-6200-4f44-ae87-31516e50951f.png)
![image](https://user-images.githubusercontent.com/69246778/178675924-070e445c-d572-4b13-a785-0122b19742f8.png)
원본이미지 * label을 같이 보여주면서 클래스 빈도수를 나타내줌. 배경의 영역이 더 큰걸 알 수 있다.
   
   
# 2. training
```
% Specify the network image size. This is typically the same as the traing image sizes.
imageSize = [480 660 3];

% Specify the number of classes.
numClasses = numel(classNames);

% Create DeepLab v3+.
lgraph = deeplabv3plusLayers(imageSize, numClasses, "resnet18");


numFilters = 64;
filterSize = 3;

layers = [
    imageInputLayer([480 660 3])
    convolution2dLayer(filterSize,numFilters,'Padding',1)
    reluLayer()
    maxPooling2dLayer(2,'Stride',2)
    convolution2dLayer(filterSize,numFilters,'Padding',1)
    reluLayer()
    transposedConv2dLayer(4,numFilters,'Stride',2,'Cropping',1);
    convolution2dLayer(1,numClasses);
    softmaxLayer()
    pixelClassificationLayer()
    ];
opts = trainingOptions('sgdm', ...
    'InitialLearnRate',1e-3, ...
    'MaxEpochs',10, ...
    'MiniBatchSize',10);

trainingData = combine(imdsTrain,pxdsTrain);
net = trainNetwork(trainingData,layers,opts);
```
아주 간단하게 네트워크를 구성했음.   
![image](https://user-images.githubusercontent.com/69246778/178676205-49005076-1052-4d27-abd6-2342855f2720.png)

# 3. Test
```
I = readimage(imdsTest,19);
C = semanticseg(I, net);

B = labeloverlay(I,C,'Transparency',0.4);
figure
imshow(B)
```
test 데이터에서 임의의 사진에 대해 test
![image](https://user-images.githubusercontent.com/69246778/178676367-34f28c44-c6d4-47ce-baf8-53084b340c21.png)
결과 개노답처럼 보이지만 데이터가 부족한 점 + 아주 간단한 신경망이라는 점 등을 고려하면 ㄱㅊ....다음엔 성능을 높여보자!

# 참고자료
[딥러닝을 사용한 의미론적 분할](https://kr.mathworks.com/help/vision/ug/semantic-segmentation-using-deep-learning.html?s_tid=srchtitle_%25EB%2594%25A5%25EB%259F%25AC%25EB%258B%259D%20segmentation_4)
[pixelLabelDatastore](https://kr.mathworks.com/help/vision/ref/pixellabeldatastore.html)
[How Labeler Apps Store Exported Pixel Labels](https://kr.mathworks.com/help/vision/ug/labeler-pixel-label-data-storage.html#mw_26b58ab1-b59e-492a-984b-90f1baae97c2)
