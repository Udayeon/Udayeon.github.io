---
layout: post
title: opencv에 extra module설치하기
description: |
hide_image: true
tags:
  - programming
published: true
---

opencv에 extra module을 설치하면서 겪은 고난들....


# 문제상황 1
* * *
Tracker 사용을 위해 추가적인 모듈을 설치할 필요가 있었음. 기존에 opencv3를 쓰다가 최신 버전이 좋을거 같아서 삭제하고 opencv4를 재설치 했는데 tracker 모듈은 opencv3에서만 사용 ㄱㄴ했음
그래서 또다시 opencv4를 삭제하고 opencv3를 다시 설치함...   
[opencv4설치](https://udayeon.github.io/2021/09/02/opencv4/)
[opencv3설치](https://j-remind.tistory.com/57)

# 문제상황 2
* * *
opencv3 설치까지 잘 마쳤는데 내가 하고자 한 extra module설치가 어려웠음. 구글링해서 찾은 방법은 대개 cmake를 깔고 그걸로 어쩌고저쩌고....
결론부터 말하면 삽질이였고 다음의 코드로 해결함

```
$ pip3 install opencv-contrib-python
```
위의 코드를 입력했을 때

![스크린샷, 2021-09-24 15-10-20](https://user-images.githubusercontent.com/69246778/134626693-5055533a-5f45-4aae-88fc-f88f8f23d6d3.png)
이런 오류가 뜨면

```
$ pip3 install scikit-build
```
를 입력하고 다시 install하면 해결됨.     
근데, 설치가 완료되지 않고 계속 이 상태에 멈춰있음;;;
![스크린샷, 2021-09-24 15-12-35](https://user-images.githubusercontent.com/69246778/134627043-5c2b78ac-6b14-45e0-93e4-c2f3f50e2eab.png)

```
$ pip3 install --upgrade pip
```
그럴 땐 이 코드로 pip를 업그레이드 시켜주고 다시 설치하면 됨

![스크린샷, 2021-09-24 15-15-35](https://user-images.githubusercontent.com/69246778/134627170-01a4f174-1d98-4072-94ee-44dd1214866c.png)
설치완료임...;
