# LESRCNN : https://arxiv.org/abs/2007.04344

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Lightweight image super-resolution with enhanced CNN
----------------------------------------------------

Abstract
--------
  * 딥러닝의 발전으로 SISR의 성능이 많이 올라감
  * 과도한 conv의 사용과 다수의 파라미터는 높은 계산비용과 많은 메모리가 뒤따름
  * 위와 같은 단점을 갖고 있는 SR model들은 실시간, 저전력 컴퓨터에 적용하는데 비효율적임
  * 이러한 문제를 해결하기 위해 lightweight enhanced SRCNN(LESRCNN)을 제안하고 LESRCNN은 information extraction and enhancement block(IEEB), reconstruction block(RB), information refinement block(IRB)로 구성되어 있음
  * IEEB는 계층적으로 저주파수 특징들의 추출하고 딥 레이어에서 얕은 레이어의 메모리 능력을 증가시키기 위해 단계별 특징들을 모음
  * RB는 전역 및 지역 특징들을 융ㅇ합시킴으로써 저주파수 특징들을 고주파수 특징들로 변환 시키는 역할을 하고 IEEB와 long term dependency problem을 해결하는데 있어 보완적임
  * IRB는 RB로부터 얻어진 coarse 고주파수 특징들을 학습하여 더 정확한 SR 특징들을 만듦
  
  
Contribution
------------
  * LESRCNN은 IEEB(Information extraction and enhancement block), RB(Reconstruction Block), IRB(Information refinement Block)을 cacading 시킴으로써 파라미터의 수를 감소시키고 좋은 성능을 만들었으며 또한 적은 계산량과 적은 메모리 소비를 달성하였음
  * IEEB는 계층적으로 저주파수 특징들을 추출하며 융합시켜 깊은 레이어에서 얕은 레이어의 메모리 능력을 향상시켰음, heterogeneous architecture를 사용하여 계산비용과 메모리 소비를 감소시켰음, LR patch를 사용하여 SR model을 학습시켜 학습과정에 있어 충분히 가속화시킴
  * 전역 및 지역 특징들을 잔여학습으로 combine하고 sub-pixel을 통해 저주파수 특징들을 고주파수 특징으로 변형시켜 IEEB의 long term dependency problem을 방지함
  * IRB는 RB로부터 추출된 coarse 고주파수 특징들을 더 정확한 고주파수 특징으로 학습시킴


Propsed Method
--------------
![image](https://user-images.githubusercontent.com/61686244/111970401-96d97900-8b3e-11eb-846a-353d12959ff8.png)

  * LESRCNN은 information extraction and enhancement block(IEEB), reconstruction block(RB), information refinement block(IRB)로 구성되어 있음
  * IEEB는 딥 레이어에서 얕은 레이어의 메모리 능력을 높이기 위해 잔여학습을 통해 단계별 저주파수 특징들을 계층적으로 추출하고 모음, 계산비용의 증가 없이 SR의 성능을 높임, 얻어진 정보들 중 불필요한 부분들을 제거하는 역할을 함
  * RB는 전역 및 잔역 특징들을 융합함으로써 저주파수 특징들을 고주파수 특징으로 변형시킴
  * IRB는 RB로부터 얻어진 coarse한 고주파수 특징들을 refine하는 역할을 함, 이를 통해 더 정확한 SR특징들을 얻게됨

Network Architecture
--------------------
  * LESRCNN은 전체적으로 23개의 층을 가지고 있고, 3개의 블록으로 구성(IEEB, RB, IRB)
  * IEEB는 17개의 층으로 구성, 저 주파수특징들을 추출하고 향상시키는 역할, 적은 계산량으로 추출된 저주파수 특징들을 refine함
  * RB는 1개의 층으로 구성, 저주파수 특징들 고주팟후 특징으로 변환하는 역할
  * IRB는 5개층으로 구성, RB로부터 얻어진 coarse한 고주파수 특징들을 refine하여 더 정확한 SR특징을 얻어냄
  
![image](https://user-images.githubusercontent.com/61686244/111970595-c8eadb00-8b3e-11eb-8b01-a40542c0fdce.png)

  * Loss Function

![image](https://user-images.githubusercontent.com/61686244/111970690-e324b900-8b3e-11eb-855d-af7caaaf7ace.png)
  
Information extraction and enhancement block(IEEB)
--------------------------------------------------
  * 일반적으로 네트워크의 깊이가 깊을수록 얕은 계층의 메모리 능력이 떨어짐
  * 이러한 문제를 해결하기 위해 17개의 IEEB를 제안, 좋은 성능과, 높은 효율성을 얻음
  * IEEB는 저주파수 특징들 계층적으로 추출하며 그때 잔여학습을 사용하여 계층적인 특징을 융합시켜 깊은 네트워크에서 얕은 계층의 effect를 보존함
  * heterogeneous architecture사용하여 얻은 기능을 추출해 파라미터, 계산비용 및 메모리 소비를 줄임 
  * IEEB는 2개의 유형의 Conv를 가지고 있음 : 1) 3x3 Conv + ReLU(홀수 layer에 사용), 2) 1x1 Conv + ReLU(짝수 layer에 사용)
  
![image](https://user-images.githubusercontent.com/61686244/111970775-fdf72d80-8b3e-11eb-8375-e03d06d12030.png)

![image](https://user-images.githubusercontent.com/61686244/111970797-03ed0e80-8b3f-11eb-98e5-e2dc12e08978.png)

![image](https://user-images.githubusercontent.com/61686244/111970812-094a5900-8b3f-11eb-860b-db53364b319a.png)

![image](https://user-images.githubusercontent.com/61686244/111970821-0d767680-8b3f-11eb-9084-df212d8818ac.png)

![image](https://user-images.githubusercontent.com/61686244/111970839-149d8480-8b3f-11eb-9c85-7c1a21792ff0.png)
  
Reconstruction Block(RB)
------------------------
  * sub pixel conv를 사용하여 저주파수특징을 고주파수특징으로 변형함
  * 그림 2와 같이 scale별 sub pixel 구성이 다름
  * long term dependency problem을 해결하기 위해 전역 및 잔역 특징들 통합시켜 딥 레이어에서 얕은 레이어의 메모리 능력을 높임
  * RB는 1st, 16th 층의 전역 및 지역 특징들의 출력을 upsample 하기위해 sub-pixel conv를 사용 
  * SR의 성능을 향상시키기 위해 전역 및 지역 특징을 융합하기 위해 잔여학습을 사용
  
![image](https://user-images.githubusercontent.com/61686244/111970918-2b43db80-8b3f-11eb-9a79-adfccb8bb85b.png)

Information refinement block(RB)
--------------------------------
  * 높은 quality 영상을 만들기 위해 고주파수 특징과 저주파수 특징을 combining하는 것은 SISR의 효과적인 방식
  * IEEB는 저주파수 특징을 추출하기 위해 LR영상만을 사용
  * RB는 IEEB로 부터 얻은 저주파수 특징을 coarse 고주파수 특징으로 변환시키는데, coarse 고주파수 특징들은 고 주파수 특징의 섬세한 정보들이 부족함
  * 위와 같은 문제를 해결하기 위해 IRB를 이용하여 더 정확한 SR특징들을 학습하고 SR 영상을 복원함
  * IRB는 5 layer로 구성되어 있으며 4개 layer는 입ㆍ출력 64개 채널을 갖고, 1개 layer는 입ㆍ출력 64, 3개 채널을 갖음  
  
![image](https://user-images.githubusercontent.com/61686244/111971000-40206f00-8b3f-11eb-8c98-35ebf6904cdb.png)

Experiments
-----------
  * Train : DIV2K dataset
  * Test : Set5, Set14, BSD100, Urban100
  
Network Analysis-Information extraction and enhancement block
-------------------------------------------------------------
  * 실시간 또는 저전력 컴퓨팅에서 성능과 계산비용을 중요한 요소
  * 구성된 IEEB의 design은 낮은 계산 비용과 적은 메모리 소비 높은 성능에 대해서 break rule한 design임 
  * 1x1 Conv는 training cost를 줄이는데 적합하지만, 위치를 선정하는데 어려움이 따름 
  * heterogeneous architecture(HN)를 사용하여 문제 해결, heterogeneous convolution은 P=2로 IEEB에 사용되고 3x3 Conv와 1x1 Conv가 연결되어 있음,8개의 3x3 Conv, 8개의 1x1 Conv, 1개의 standard Conv 3x3   
  * Standard convolutional network(SN)는 HN과 깊이와 구성은 같지만 모두 3x3 Conv 사용

Compare HN and SN
-----------------
![image](https://user-images.githubusercontent.com/61686244/111971252-8249b080-8b3f-11eb-9bff-b407b78d374f.png)

![image](https://user-images.githubusercontent.com/61686244/111971274-8675ce00-8b3f-11eb-815e-cd717cb1f138.png)

  * 깊이가 커짐에 따라 얕은 레이어의 메모리 능력이 약해져 이미지 응용 프로그램의 성능이 저하되는 것으로 알려져 있음
  * 위와 같은 문제를 해결하기 위해 multi-level feature fusion을 IEEB에 적용하여 추가적이 계산 없이 계층적인 정보를 사용하여 얕은 레이어의 성능을 올림
  
Network Analysis-Reconstruction block
-------------------------------------
  * SR model을 학습하기위해 저주파수 특징들만 사용하는건 부적절한 결과를 만들어 낼 수 있음
  * 네트워크의 중간부부에 해당하는 RB를 제안하며, 저주파수 특징들을 고주파수 특징으로 변환하는 역할을 함 
  * RB는 sub-pixel conv를 IEEB의 출력(global)과 IEEB의 첫 번째 layer(local)에 사용 
  * 잔여 학습을 이용하여 전역 및 지역 특징을 gather함 
  * 전역 및 지역 특징을 이용하여 sub-pixel을 진행하여 잔여학습을 통해 gather하는 방법을 통해 expressive aility를 향상 시킬 수 있음

Network Analysis-Information refinement block
----------------------------------------------
  * IEEB는 저주파수 특징들의 effect를 만들어내며, RB는 저주파수 특징을 coarse 고주파수 특징을 만들어내는데, 이는 고주파수 특징의 영향을 무시할 수 있음 
  * IRB는 4개의 Conv+ReLU로 구성되어 있고 RB로부터 얻은 coarse 고주파수 정보를 이용하여 더 정확한 고주파수 특징들을 학습할 수 있음

![image](https://user-images.githubusercontent.com/61686244/111971450-b58c3f80-8b3f-11eb-9ba1-9cbc71add047.png)

![image](https://user-images.githubusercontent.com/61686244/111971482-bd4be400-8b3f-11eb-9c4c-e3fac11a844e.png)

![image](https://user-images.githubusercontent.com/61686244/111971500-c1780180-8b3f-11eb-9656-918befcaca30.png)

![image](https://user-images.githubusercontent.com/61686244/111971516-c5a41f00-8b3f-11eb-9912-d61d25d2a171.png)

![image](https://user-images.githubusercontent.com/61686244/111971531-c9d03c80-8b3f-11eb-81ac-644476425a49.png)

![image](https://user-images.githubusercontent.com/61686244/111971546-ce94f080-8b3f-11eb-8fb4-e7c46313ddb9.png)

![image](https://user-images.githubusercontent.com/61686244/111971558-d2c10e00-8b3f-11eb-8666-2d1adfb24821.png)


Conclusions
-----------
  * lightweight enhanced super resolution CNN(LESRCNN)제안
  * IEEB는 계층적으로 저주파수 특징들을 추출하고 모아 long-term dependency problem을 해결
  * IEEB는 heterogeneous architecture를 사용하여 파라미터의 수와 SR 모델을 학습하는데 복잡성을 줄였음
  * RB는 전역 및 지역 특징들을 융합함으로써 저주파수 특징을 고주파수 특징으로 전환 시키며 이는 깊은 레이어에서 얕은 레이어의 메모리 능력을 향상시킬 수 있음
  * IRB는 RB로부터 얻은 coarse 고주파수 특징을 이용하여 더 정확한 SR특징을 만들어냄

 
