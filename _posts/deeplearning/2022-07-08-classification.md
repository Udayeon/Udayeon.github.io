---
layout: post
title: 
description: |
  Swin Transformer - Image Classification
hide_image: true
tags:
  - deeplearning
published: true
---

# (Python) Swin Transformer - Image Classification
* * *

# 1. get_started.md
![image](https://user-images.githubusercontent.com/69246778/177749047-1a2905ef-df16-45f5-a3f5-ac643014c3ab.png)
연결된 MODEL HUB로 들어가서 사전 훈련된 모델들의 모음을 구경할 수 있음.     

![image](https://user-images.githubusercontent.com/69246778/177750465-a76b6bce-254b-4c51-abde-4577fcf9ebb4.png)
나는 siwn-V1의 swin-T를 사용하려 한다. ImageNet-1K로 사전훈련된 모델이다. 걸려진 링크의 **log**에서 훈련 과정 기록도
볼 수 있음.

# 2. Install
![image](https://user-images.githubusercontent.com/69246778/177751205-7b97fdc5-a80f-47b5-b3db-8663fe02d595.png)
설치조건을 살펴보자   
* python version = 3.7
* CUDA >= 10.2  (with cudnn >= 7)
* PyTorch >= 1.8.0 and torchvision >= 0.9.8 (with CUDA >= 10.2)

## 2.1. pytorch docker
각 프로젝트마다 Python과 라이브러리들의 버전이 다른 경우 가상환경(python venv나 conda 등)을 사용하면 
각 프로젝트의 환경을 분리해서 python의 여러 버전을 다룰 수 있다. 이때 사용하는 것이 Docker.

## 2.2. Clone
docker를 사용하면 좋지만 colab에서 시도해봄.   
게시물 [Colab에서 github 코드 사용하기](https://udayeon.github.io/2022/07/07/gitClone/) 참조.
![image](https://user-images.githubusercontent.com/69246778/177752506-c4fd2bcc-6a07-422b-975c-41fdedd13935.png)
SwinTransformer 폴더 안에 github을 clone해 놓음.

## 2.3. 각종 version 확인
```py
import torch
import torchvision

!python --version
print("Torch version:{}".format(torch.__version__))
print("cuda version: {}".format(torch.version.cuda))
print("cudnn version:{}".format(torch.backends.cudnn.version()))
print("Torchvision version:{}".format(torchvision.__version__))
```
![image](https://user-images.githubusercontent.com/69246778/177754942-10b9bcc6-65b3-4fe0-8bba-00fe95442ea6.png)
pytorch만 1.8이상으로 업데이트 해주면 되는 상황.   
   
```py
pip install timm==0.4.12
pip install opencv-python==4.4.0.46 termcolor==1.1.0 yacs==0.1.8
```
이렇게 설치를 다하면   
![image](https://user-images.githubusercontent.com/69246778/177756402-beafde75-f5ce-4c3d-9515-f8a3ae0ea3de.png)
런타임 다시 시작하라고 나옴. 

# 3. 필요한 module import
```py
import timm
import torchvision
from torchvision import transforms as T
import torch
from PIL import Image
```

# 4. Data준비
```py
URL = "https://raw.githubusercontent.com/SharanSMenon/swin-transformer-hub/main/imagenet_labels.json" # Imagenet labels
!wget https://www.allaboutbirds.org/guide/assets/photo/306327661-480px.jpg -O house_finch.jpg
```
imageNet label jason파일이랑 새 사진 한 장 가져옴. house finch로 분류되는 사진임. 근데 이 이미지를 바로 사용할 수는 없고 전처리가 필요함.

```py
trans_ = T.Compose([
                    T.Resize(256),
                    T.CenterCrop(224), # Model requries 224x224 images
                    T.ToTensor(), # Converts to pyTorch tensor
                    T.Normalize(timm.data.IMAGENET_DEFAULT_MEAN, timm.data.IMAGENET_DEFAULT_STD) # swin transformer was traiened on normalized data,
                    # therefore we must normalize our images too
])

image = Image.open("house_finch.jpg")
transformed = trans_(image) # Convert to pytorch tensor
transformed.size() #이미지가 tensor로 전환됨. 
```
```py
transformed.size()
```
```py
>> torch.Size([3, 224, 224])
```
tensor로 전환된 거에 배치를 추가해줌.

```py
batch = transformed.unsqueeze(0) # The model accepts a batch of image, so we create a batch of 1 image.
                                 # unsqueeze : 0번째 위치에 tensor 차원확장
```
```py
>> batch.Size([1, 3, 224, 224]) #이제 model에 넣을 수 있다.
```

# 5. model 구현
timm의 model list에서 swin~이라는 이름을 가진 모델들의 리스트를 확인함.
```py
timm.list_models("swin*", pretrained=True) # This will list all the swin transformer models available
```   
![image](https://user-images.githubusercontent.com/69246778/177760305-62245044-4898-434b-8466-2f74abee0139.png)
나는 이 중에 사전 훈련된 swin-T(swin_tiny_patch4_window7_224)를 사용할 거임.
```py
model = timm.create_model('swin_base_patch4_window7_224', pretrained=True)
```

```py
print(model)
```
```py
SwinTransformer(
  (patch_embed): PatchEmbed(
    (proj): Conv2d(3, 96, kernel_size=(4, 4), stride=(4, 4))
    (norm): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
  )
  (pos_drop): Dropout(p=0.0, inplace=False)
  (layers): Sequential(
    (0): BasicLayer(
      dim=96, input_resolution=(56, 56), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=96, out_features=288, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=96, out_features=96, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): Identity()
          (norm2): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=96, out_features=384, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=384, out_features=96, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=96, out_features=288, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=96, out_features=96, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((96,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=96, out_features=384, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=384, out_features=96, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(56, 56), dim=96
        (reduction): Linear(in_features=384, out_features=192, bias=False)
        (norm): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
      )
    )
    (1): BasicLayer(
      dim=192, input_resolution=(28, 28), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=192, out_features=576, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=192, out_features=192, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=192, out_features=768, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=768, out_features=192, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=192, out_features=576, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=192, out_features=192, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((192,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=192, out_features=768, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=768, out_features=192, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(28, 28), dim=192
        (reduction): Linear(in_features=768, out_features=384, bias=False)
        (norm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
      )
    )
    (2): BasicLayer(
      dim=384, input_resolution=(14, 14), depth=6
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (2): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (3): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (4): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (5): SwinTransformerBlock(
          (norm1): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=384, out_features=1152, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=384, out_features=384, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((384,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=384, out_features=1536, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=1536, out_features=384, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
      (downsample): PatchMerging(
        input_resolution=(14, 14), dim=384
        (reduction): Linear(in_features=1536, out_features=768, bias=False)
        (norm): LayerNorm((1536,), eps=1e-05, elementwise_affine=True)
      )
    )
    (3): BasicLayer(
      dim=768, input_resolution=(7, 7), depth=2
      (blocks): ModuleList(
        (0): SwinTransformerBlock(
          (norm1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=768, out_features=2304, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=768, out_features=768, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=768, out_features=3072, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=3072, out_features=768, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
        (1): SwinTransformerBlock(
          (norm1): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (attn): WindowAttention(
            (qkv): Linear(in_features=768, out_features=2304, bias=True)
            (attn_drop): Dropout(p=0.0, inplace=False)
            (proj): Linear(in_features=768, out_features=768, bias=True)
            (proj_drop): Dropout(p=0.0, inplace=False)
            (softmax): Softmax(dim=-1)
          )
          (drop_path): DropPath()
          (norm2): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
          (mlp): Mlp(
            (fc1): Linear(in_features=768, out_features=3072, bias=True)
            (act): GELU()
            (fc2): Linear(in_features=3072, out_features=768, bias=True)
            (drop): Dropout(p=0.0, inplace=False)
          )
        )
      )
    )
  )
  (norm): LayerNorm((768,), eps=1e-05, elementwise_affine=True)
  (avgpool): AdaptiveAvgPool1d(output_size=1)
  (head): Linear(in_features=768, out_features=1000, bias=True)
)
```
모델 아키텍쳐를 확인해보면 Swintransformr 블럭으로 구성된 총 4개의 stage(basic layer)가 있음.

# 6. Evaluate
```py
with torch.no_grad():
  output = model(batch) # torch.no_grad() incrases speed by disabling gradients.
  # pass in our 'batch' to the model
```
```py
output
```
```py
tensor([[-1.9016e-01,  1.9889e-01,  3.0785e-01,  5.0095e-01,  6.4167e-01,
          7.0143e-01, -1.1234e-02,  3.5708e-01,  1.0447e+00,  4.7405e-01,
          4.1761e+00,  1.4396e+00,  8.2452e+00,  1.1930e+00,  1.5377e+00,
          1.2554e+00,  8.3897e-01, -3.7446e-01, -1.3489e+00,  7.0715e-01,
          9.3417e-01,  7.1980e-01,  2.0256e-01,  7.6432e-01,  1.0556e+00,
          4.1342e-01,  7.8627e-01,  1.8811e-01,  5.9584e-01,  5.2687e-01,
          3.7547e-01, -9.9655e-01,  7.5490e-01, -2.6306e-02,  4.7791e-01,
          3.0842e-01,  5.6430e-01, -4.9928e-01,  6.1437e-01,  2.5116e-01,
          3.1124e-01,  5.7473e-01,  4.7467e-01,  5.8698e-01,  5.4986e-01,
          2.4520e-01, -2.3827e-01,  1.2559e+00,  7.9930e-02,  3.1247e-01,
          6.1390e-01,  3.2108e-01,  1.0920e+00, -1.2128e-01,  9.9993e-02,
         -5.6232e-01,  2.9175e-01, -1.9913e-01,  2.2639e-02,  8.9008e-01,
         -8.3613e-02,  1.1607e-01,  1.6977e-01,  4.4581e-01,  1.9575e-01,
          3.0114e-01,  5.2494e-01, -3.3385e-02,  6.9526e-01,  2.3393e-01,
          2.1891e-01, -1.5875e-02, -1.4747e-01,  1.1835e+00, -6.7120e-01,
          4.7606e-01,  4.5420e-01, -4.2010e-01,  8.1708e-01, -4.4538e-02,
          1.2209e+00,  5.4841e-01,  6.9280e-01,  7.4113e-01, -1.7145e-01,
          6.5327e-01,  1.3126e+00,  6.8698e-02,  3.3557e-01,  8.9700e-02,
          1.8628e-01,  2.2029e-01,  7.8222e-01,  2.9551e-01,  6.1749e-01,
          1.1113e-01, -8.3187e-01, -2.3362e-01,  7.1607e-01,  2.7000e-01,
          7.0056e-02,  6.5012e-01, -1.1278e-01,  5.5624e-01, -7.2767e-02,
          5.2746e-01,  2.3039e-01,  3.1528e-01, -1.1438e-02, -1.8073e-01,
          7.9599e-01,  4.5522e-01,  1.8323e-01,  4.5729e-01, -1.4132e-01,
          3.9605e-01,  4.5185e-01,  8.1261e-01, -3.0709e-01, -1.9369e-03,
         -6.2317e-02, -1.9239e-01,  1.2727e-01, -1.6577e-01, -4.2616e-01,
         -4.5357e-01,  1.1885e+00,  1.1287e-01,  3.7298e-02,  6.0257e-01,
          1.8089e-01, -2.7116e-01,  3.7338e-01,  1.1794e+00,  6.2387e-02,
          2.3052e-01,  3.3541e-01, -4.4384e-03, -1.0064e+00,  6.8106e-01,
          1.7592e+00,  4.7248e-01,  5.7414e-01, -7.9369e-01,  4.0171e-01,
          2.4801e-02,  2.6289e-01,  8.7113e-02,  2.1482e-01, -2.2497e-01,
          5.4912e-01,  3.6707e-01,  1.9378e-02,  4.2420e-01,  2.5855e-01,
         -4.1984e-01,  3.0794e-01,  8.5788e-02, -1.0902e-01,  4.0053e-02,
          2.1857e-01,  1.4636e-02,  8.1957e-02, -2.2590e-01, -2.3231e-01,
          2.4350e-01, -4.9512e-02,  4.8668e-01, -2.2230e-01,  1.1205e-01,
          5.9115e-01, -2.4379e-01,  1.2021e-01,  1.0780e-01, -1.9410e-01,
         -6.2192e-02,  6.2526e-01,  8.1567e-01,  6.0292e-01,  4.0620e-01,
         -2.8007e-01,  4.8430e-01,  2.1697e-01,  3.3052e-01,  1.0079e-01,
          6.2138e-01,  5.8198e-01,  2.3499e-01,  7.4986e-01,  8.5305e-01,
          7.4808e-01, -3.1625e-03,  1.3825e-01,  1.5847e-01,  4.1359e-01,
          1.7468e-02, -4.0559e-01,  7.9949e-01,  5.6901e-02,  7.2078e-01,
          3.6567e-01, -2.5864e-01,  2.4481e-01,  1.9460e-01, -7.2914e-02,
          4.1781e-01,  7.1999e-01, -8.2583e-02,  2.0628e-01, -1.1075e-01,
          3.7352e-01,  4.6917e-02,  2.0226e-01,  2.6312e-01,  2.9837e-01,
         -3.1440e-02,  3.6304e-01,  9.2160e-02,  1.0028e-01, -1.6261e-01,
          2.4298e-01,  4.3064e-01,  6.2832e-01,  3.5762e-01,  8.8712e-01,
         -1.2376e-01,  1.5457e-01,  1.5564e-01,  9.0174e-01,  3.5670e-01,
         -2.5416e-01,  2.4155e-01,  3.5712e-01,  6.8735e-01,  3.7985e-01,
          5.6126e-03, -1.3895e-01, -1.2159e-01, -4.2863e-01,  1.8999e-01,
          2.3478e-01,  2.7616e-01, -1.7466e-01,  1.3764e-02,  2.8008e-01,
         -8.1785e-02, -1.0094e-01,  2.1708e-02, -3.0793e-01,  8.8364e-02,
          3.6706e-01, -5.3914e-01,  7.2833e-01, -2.6394e-01, -1.7443e-01,
          1.6182e-01,  7.7405e-01,  5.2431e-01,  2.7311e-01,  1.7658e-01,
          4.7212e-01,  3.5499e-01,  1.9276e-01, -6.9727e-02,  3.7494e-01,
          8.2984e-01, -1.9308e-02,  5.8385e-01,  3.5803e-01,  1.1516e+00,
          1.3227e+00,  1.1154e+00,  1.1374e+00,  5.9013e-01,  7.6104e-01,
          4.2783e-01,  6.6518e-01,  1.0178e+00,  3.0298e-01,  1.1665e+00,
          8.5852e-01,  7.0953e-01, -6.7987e-02,  4.2619e-01,  2.7169e-02,
          7.5299e-01,  1.1239e+00,  3.3798e-01,  3.9194e-01,  4.5476e-01,
         -1.2264e-01,  1.5644e-01,  1.5443e-02,  5.5933e-01,  4.5060e-01,
          1.0366e+00,  7.4167e-01,  1.1647e+00,  1.4232e-01,  2.2146e-01,
          2.4077e-01,  6.0314e-01,  5.8562e-01,  7.3030e-01,  2.1645e-01,
         -3.8634e-01,  3.9612e-01,  1.5674e-01,  2.8146e-01,  2.1685e-01,
          4.0272e-01,  5.1490e-01,  1.8958e-01,  3.7581e-01,  1.0959e-01,
          3.2805e-01,  5.0887e-02,  3.9491e-01,  3.3445e-02,  4.7147e-01,
          7.0775e-01,  1.7443e-01,  1.5620e+00,  2.8595e-01,  1.0009e+00,
          4.4089e-01,  1.0637e+00, -1.2779e-01,  1.1127e-01,  3.8324e-01,
          7.8786e-01,  5.8636e-01,  3.7145e-01, -1.0142e-01,  3.4647e-01,
          8.3113e-02,  1.5621e-01,  6.0177e-01,  1.7819e-01, -2.9716e-01,
          3.7846e-01,  4.4441e-01,  2.2138e-01, -1.9090e-01,  2.8334e-01,
          2.3959e-01,  3.0895e-01,  7.1432e-01,  1.7525e-01,  5.2820e-01,
          6.4451e-01,  3.8495e-01,  5.0214e-01,  1.8482e-01, -2.3971e-01,
         -2.3657e-01,  9.1942e-01,  1.4066e+00,  1.1907e+00,  2.4637e-01,
          6.3798e-01,  1.0244e+00,  1.1758e+00, -7.6327e-03,  1.2490e+00,
          1.2514e-01,  9.7387e-01,  7.3128e-01,  1.3924e+00,  1.3600e+00,
          1.4896e-01,  9.4429e-01,  4.5068e-02,  1.1212e+00,  1.8605e-01,
          4.5022e-01,  1.2625e+00,  5.3233e-01,  3.7362e-01,  7.6741e-01,
          8.9300e-01,  4.0013e-01, -1.2323e-01,  3.7272e-01,  5.7332e-01,
          3.3325e-01,  4.2860e-01,  1.1771e+00,  6.5505e-01,  2.6137e-01,
         -5.6197e-01,  1.2330e-01,  3.9434e-01, -6.2766e-01, -1.7922e-02,
          5.6302e-01, -3.4868e-01, -1.0203e-01, -1.1533e-01,  3.9456e-01,
         -7.3878e-02, -3.4227e-01, -6.0979e-02, -4.5315e-01, -2.9820e-01,
          1.3319e-01, -2.2808e-01, -7.5696e-01, -6.3929e-01, -4.8852e-01,
         -5.9017e-01, -3.4703e-02,  1.1454e-01,  5.8063e-02, -4.5922e-01,
         -5.9126e-01, -7.2966e-01, -1.6626e-01, -1.6055e-01, -2.6499e-01,
         -1.2013e-01, -1.2224e-01, -3.6532e-01, -5.2364e-01, -4.4169e-01,
         -2.2341e-02, -5.8387e-02,  9.2884e-02, -2.5580e-01, -2.7870e-01,
         -9.0193e-01,  1.4743e-01,  1.8272e-01, -3.6225e-01,  7.2989e-02,
          1.2155e-02, -4.0540e-01, -3.4014e-01,  1.6540e-02,  1.7883e-01,
         -6.6057e-01, -7.4604e-01,  3.3700e-01,  6.1531e-01, -4.8099e-01,
         -5.6576e-01, -6.3594e-01, -7.3908e-01,  4.5265e-01, -6.9846e-01,
         -2.7634e-01,  4.3537e-01,  7.5218e-02, -1.9517e-01, -1.0576e+00,
         -5.7038e-01, -4.9136e-02,  1.0134e-01, -5.1726e-01,  7.3077e-01,
         -6.1172e-01,  9.1187e-02,  4.7658e-01,  4.2102e-01,  6.2047e-01,
          4.4012e-02, -4.6376e-01, -4.5354e-01, -5.5197e-01,  4.2342e-01,
          6.6338e-01, -7.8547e-01, -1.5879e-01, -6.8415e-01, -5.4500e-01,
         -1.9469e-01, -6.7532e-01, -3.6719e-01, -3.4788e-01, -8.8570e-01,
         -4.7493e-01, -5.0555e-01, -4.9782e-01, -4.3000e-01, -5.1897e-01,
         -9.2349e-01, -2.3556e-01, -1.9488e-01,  9.4515e-01,  1.1331e-01,
          8.7227e-02,  2.2582e-02, -8.5214e-02,  1.2412e-01,  3.8071e-01,
         -1.7089e-01, -2.1744e-01, -3.6068e-01, -7.2279e-01,  1.5909e-01,
         -1.3547e-01,  5.5003e-01, -1.2745e-01,  1.6347e-01,  5.3833e-02,
          5.9629e-01,  6.2980e-01, -7.8568e-01, -5.3709e-01, -5.3084e-01,
         -3.3307e-01, -7.1869e-01, -2.6294e-01, -7.4432e-01, -3.3571e-01,
         -3.0180e-01,  1.3953e-01, -1.3105e-01, -1.6679e-01, -5.8059e-01,
         -4.0118e-01, -4.0796e-01, -2.3450e-01, -3.7842e-01,  2.3672e-01,
         -5.2231e-01, -1.9563e-02, -2.5553e-01, -5.0407e-01, -1.2448e-01,
         -1.4636e-01, -7.0233e-01, -3.0430e-01,  1.8414e-01, -3.1405e-01,
         -5.3228e-01, -7.3455e-01, -2.9528e-01, -1.4198e-01, -8.1183e-03,
          1.3087e-01, -3.8116e-01, -1.8200e-01, -3.9521e-01, -2.0075e-01,
         -5.9787e-01, -4.2323e-01, -4.4982e-01, -5.2729e-01, -5.1305e-02,
         -1.2284e-01, -2.2798e-01, -5.5582e-01, -1.4420e-01, -6.3921e-01,
         -3.6248e-01,  2.3947e-02, -1.0276e-01,  1.4596e-01,  9.9152e-02,
         -2.9461e-01, -5.2575e-01,  2.2913e-02, -1.7880e-01, -3.9180e-01,
         -6.4333e-02, -2.7347e-01, -2.3845e-01,  2.2865e-01, -7.7296e-01,
         -3.8672e-02, -5.3160e-01,  2.4262e-01, -5.0585e-01,  7.9512e-01,
         -5.4792e-01, -3.5188e-01, -1.6749e-03,  5.7811e-01, -1.0483e-01,
          2.9546e-01, -9.9502e-01, -5.9201e-01, -1.9802e-01,  1.7315e+00,
         -9.1269e-01, -6.9820e-01,  1.1115e-01, -3.3711e-01, -3.0991e-02,
         -4.2238e-01,  5.0731e-01, -6.5463e-01, -6.5915e-01, -1.2187e-01,
         -3.6533e-01,  7.7418e-02, -1.2348e-01, -2.0200e-01,  6.2378e-01,
          3.3811e-01,  5.7402e-01, -2.7601e-01, -6.3712e-01, -1.5360e-01,
         -6.1983e-01, -2.5320e-01, -1.6812e-01,  4.3989e-02, -7.6879e-01,
          5.0545e-01,  3.9386e-02, -6.1623e-01, -4.1230e-01, -4.3297e-02,
         -3.6549e-01,  5.5354e-01,  6.5465e-02,  2.6579e-01,  2.3514e-01,
         -2.1405e-01, -8.1598e-01, -7.6677e-01,  1.7969e-01, -7.5569e-01,
         -5.7384e-01, -1.6247e-01, -7.2302e-01, -2.2339e-01,  2.6205e-02,
         -6.6659e-01, -6.8074e-01, -4.3061e-01,  1.0212e-01, -5.0308e-01,
         -3.8882e-01, -1.8231e-01, -3.7277e-01,  1.8769e-01, -2.1780e-01,
         -4.6009e-01, -5.6345e-01, -3.1182e-01,  2.4363e-01, -1.9799e-02,
         -3.2701e-01, -1.1985e-01, -1.0040e-01, -1.6500e-01, -5.7042e-02,
         -4.3935e-01, -3.0716e-01, -1.2662e-01,  5.3732e-01, -6.1773e-01,
          2.2006e-01, -3.8895e-01, -7.0388e-01, -9.6321e-02,  9.3200e-01,
         -2.4885e-01, -9.9200e-01, -5.6791e-01, -9.2939e-01,  1.1409e-01,
         -5.3784e-01,  6.5583e-01,  1.7979e-01, -5.9499e-01, -3.8889e-01,
         -4.0091e-01, -5.5202e-01, -6.6172e-01, -2.8358e-01,  1.3915e-01,
         -2.9885e-01,  4.0023e-01,  5.6519e-01, -3.1619e-02,  1.1973e+00,
         -2.3130e-01, -5.0424e-01, -1.4633e-01, -3.6287e-01, -3.7308e-02,
         -4.4300e-01, -6.4421e-01, -6.4729e-01, -2.2907e-01,  5.4190e-01,
         -3.4271e-01, -2.1642e-01, -7.2845e-01, -2.8447e-01, -5.7509e-01,
         -3.5847e-01, -1.5119e-01, -1.6043e-01, -2.4737e-03, -6.4167e-01,
          2.9207e-01, -2.5146e-03, -7.9427e-01,  1.9586e-01, -1.5453e-01,
         -4.0220e-01, -7.9659e-01, -6.0393e-01,  3.2927e-01, -6.0892e-01,
         -5.2895e-01,  1.0301e-01,  2.3389e-01, -4.2930e-01, -4.5528e-01,
          2.2037e-01, -2.1419e-01, -7.9580e-01, -5.3957e-01, -5.2205e-03,
         -5.0920e-01, -1.6658e-01, -2.6721e-01,  4.2789e-02, -3.3528e-01,
          7.5573e-01, -1.9973e-01, -1.5124e-01, -1.4544e-01,  1.2947e-02,
         -8.9012e-02, -1.6414e-01, -8.4208e-01, -6.3775e-02, -7.4057e-01,
          2.3504e-01, -7.8663e-01, -8.2063e-01,  1.3774e+00, -1.6367e-01,
         -9.9647e-01, -6.3561e-01, -4.6837e-01, -4.4509e-01, -1.7012e-01,
         -5.2070e-01, -5.3334e-01, -4.1822e-01,  2.5975e-01,  5.3876e-01,
         -4.7741e-01, -2.6495e-01, -5.4020e-01, -3.3319e-01, -4.0689e-01,
         -1.8129e-01,  3.9623e-01, -8.1908e-01,  8.5798e-02, -3.8031e-01,
         -2.6029e-02, -7.3476e-01, -1.3109e-01, -3.1236e-01, -4.4590e-01,
          2.8202e-01, -1.8046e-01, -1.2541e-01, -2.6163e-01, -2.8035e-01,
          5.1533e-02, -2.8707e-01,  1.8908e-01, -3.2279e-02, -1.4058e-01,
         -2.9554e-01,  4.2201e-02,  4.0757e-01, -5.5805e-01, -9.3672e-01,
         -2.2159e-01, -5.9347e-01, -5.2154e-01,  6.6839e-01,  2.8101e-02,
         -2.3748e-01,  6.8123e-02,  1.2480e-01, -5.1256e-01,  9.1172e-03,
         -1.0238e-02, -5.8544e-02,  5.1971e-01,  3.3778e-01,  1.3275e-01,
         -5.9229e-02, -5.1294e-01, -4.1083e-01, -4.6636e-02, -4.2335e-01,
         -1.2473e+00, -3.7039e-01, -2.0056e-02, -2.0438e-01,  2.3793e-02,
         -5.2832e-01, -3.1301e-01, -9.1200e-02, -9.5909e-02,  7.5334e-02,
         -2.6696e-01, -7.4490e-01, -1.6476e-01, -4.8951e-03, -6.0641e-01,
         -5.9582e-02, -2.6576e-02, -4.5913e-01,  1.1998e-01, -2.3965e-01,
         -7.2982e-01, -2.3401e-01, -2.9764e-01,  5.6456e-02,  8.9344e-01,
          2.8097e-01, -7.1086e-01, -1.1443e+00, -1.8808e-01, -5.4427e-01,
         -3.2140e-01,  5.1490e-02, -8.2018e-01, -1.6115e-01, -4.9608e-02,
          2.7496e-01, -1.7680e-01,  2.3059e-01, -5.1096e-01, -3.2908e-01,
          5.3168e-01, -5.8806e-01, -3.3796e-01, -1.4284e-01, -3.4888e-01,
         -4.8914e-01,  2.6956e-01, -8.2569e-01, -5.0551e-01, -5.4816e-02,
          5.7785e-01, -8.5647e-02, -3.3454e-01,  2.1260e-01, -2.8888e-01,
          1.8412e-01, -5.0054e-01, -5.7881e-01, -3.6718e-01, -7.9511e-01,
         -2.8605e-01,  8.7803e-02,  1.9759e-02, -4.7048e-01, -4.2033e-01,
         -6.2413e-01, -4.3208e-01, -2.5880e-01,  1.3739e-01, -2.8092e-02,
         -7.0037e-01, -2.1985e-01, -3.1013e-01, -4.5317e-01, -5.9855e-01,
         -2.1277e-01,  2.8163e-02, -2.7099e-01, -8.3828e-01, -3.0602e-01,
         -2.8236e-01, -8.1016e-01, -5.7811e-01,  4.1197e-01, -1.9368e-01,
          6.2221e-02, -5.8416e-01,  3.4758e-02, -6.7568e-01, -5.2671e-01,
         -5.7511e-01, -2.6696e-01, -7.0816e-02, -3.4019e-01, -3.0295e-02,
         -1.1176e-01, -3.5160e-01, -5.5518e-01, -6.5310e-01,  3.0679e-01,
          1.2405e-01, -2.5834e-01,  2.0472e-01,  3.8454e-01,  3.6679e-01,
         -1.8764e-01, -1.9986e-01, -2.8072e-01, -4.5951e-01, -1.6553e-01,
         -1.7848e-01,  3.6908e-01,  3.1331e-02, -7.5528e-02, -2.0375e-02,
         -2.0628e-01, -5.5666e-01, -8.6257e-01, -2.3624e-01, -6.9168e-01,
         -1.2105e-01, -4.1813e-01, -9.2024e-01,  1.8456e-02,  3.8384e-01,
         -4.1279e-01, -4.0707e-01, -4.5136e-02, -3.4236e-01, -2.3643e-01,
         -7.0221e-01, -2.1239e-01, -3.6989e-01, -1.8822e-01, -2.9873e-01,
         -2.3639e-01,  1.2757e+00,  1.3328e+00,  5.4690e-01, -1.7743e-02,
          1.1688e-01, -2.3275e-01,  2.0169e-01, -2.2757e-01, -2.2208e-01,
         -6.0521e-01,  2.8892e-01,  6.2703e-01, -4.3468e-01, -2.8957e-01,
          3.4508e-01,  5.1537e-02,  6.0912e-01, -3.3402e-01, -5.1515e-01,
         -5.4792e-01,  6.3807e-01, -2.1738e-01, -4.3179e-01, -2.8177e-01,
         -6.2700e-01, -4.1343e-01, -2.1286e-01, -3.4088e-01,  3.2639e-02,
         -1.1227e-01,  5.0254e-01, -2.2646e-01,  2.8809e-01,  4.3122e-01,
         -1.9679e-01, -4.5594e-01, -2.0170e-01,  2.7217e-02,  1.7588e-01,
          2.9440e-01, -1.8112e-01, -1.7842e-01, -3.1087e-01, -1.5393e-01,
          2.4738e-01, -5.0606e-01, -7.0970e-01, -4.8142e-01, -2.4103e-02,
          1.4307e-01, -2.1021e-01, -4.3767e-01,  2.3254e-01,  7.9012e-01,
         -1.9629e-01,  9.4204e-01, -5.9398e-02,  1.2561e+00,  1.3093e+00,
          8.0038e-01,  1.6374e+00,  6.1325e-01,  1.7617e-01, -4.9355e-01]])
```         
output은 이런 형태로 나오는데, 여기서 내가 제시한 이미지일 확률이 가장 높은 class가 큰 숫자를 가짐.

```py
class_ = output.argmax(dim=1) # argmax(dim=1) : 1행에서 최댓값 출력
class_
```
```py
>> tensor([12])
```
12번째 class가 정답일 확률이 가장 높은 class인 것. 이 때, 시작은 0부터여서 실제로는 13번째 위치에 있는 class임!   

# 7. Result
```py
import json
from urllib.request import urlopen

response = urlopen(URL) # remember i defined URL in the first cell
classes = json.loads(response.read())
classes[class_]
```
```py
>> 'house finch'
```
jason목록에서 해당 위치의 index를 출력하면 정답이 잘 나옴!
