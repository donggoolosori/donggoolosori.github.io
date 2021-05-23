---
layout: post
title: "[DLCV] Faster R-CNN"
subtitle: "Deep Learning, Computer Vision"
date: 2020-10-22 18:29:00
author: "dongjune"
header-img: "img/in_post/code-bg.jpg"
catalog: true
tags:
  - Machine Learning
  - Computer Vision
---
# Faster R-CNN 개요
Faster R-CNN은 **RPN(Region Proposal Network)** + **Fast R-CNN**이 합쳐진 네트워크이다.  
![1](https://user-images.githubusercontent.com/53213397/117608083-4f638680-b198-11eb-97ea-1cb17189a8e5.png)

Region Proposal 영역을 딥러닝 네트워크에 포함시켜서 Fast R-CNN의 문제점을 개선했다. [(Fast R-CNN 포스팅)](https://donggoolosori.github.io/2020/10/21/fast-rcnn/){:target="_blank"}  
  
따라서 Region Proposal에 GPU를 사용할 수 있고, 전체적으로 End-to-End 구조가 완성된다. 
# Faster R-CNN 구조
![2](https://user-images.githubusercontent.com/53213397/117608122-6609dd80-b198-11eb-96fb-d39b9233e634.png)
1. CNN을 통과하여 원본 이미지의 Feature Map을 추출
2. RPN(Region Proposal Network)를 통해 Region Proposal 영역 추출
3. Feature map과 Region Proposal 이미지들을 맵핑해준다.
4. 맵핑된 이미지들을 ROI Pooling을 통해 같은 크기로 할당해준다.
5. Softmax와 bounding box regressor를 통해 학습한다.

# Region Proposal Network 구현 이슈
- Selective Search는 Color, Texture(무늬), Size, Shape와 같은 다양한 근거를 토대로 높은 수준의 Region Proposal을 수행해준다. [(selective search 포스팅)](https://donggoolosori.github.io/2020/10/21/dlcv-ss/)
- **하지만** RPN에서는 Feature map에 대한 pixel 값과 Ground Truth Bounding Box만이 데이터로 주어지는데 **어떻게** selective search 수준의 높은 Region Proposal을 수행할 수 있을까?
- 이것을 해결하기 위한 것이 Anchor box이다.


# Anchor Box
Object가 있는지 없는지에 대한 후보 box. 총 9개이다.
![3](https://user-images.githubusercontent.com/53213397/117608129-6a35fb00-b198-11eb-88f3-4c3844d62698.png)
### - Feature Map에서의 Anchor Box
![4](https://user-images.githubusercontent.com/53213397/117608133-6ace9180-b198-11eb-994b-969403a9f08a.png)
아주 촘촘하게 포인트마다 9개의 Achor Box를 만들어준다.  
위의 그림에서는 총 1900개의 포인트마다 9개의 Achor box를 생성하여 전부 1900x9 = 17100개의 Anchor box가 존재한다.
### - Positive, Negative Anchor Box
![5](https://user-images.githubusercontent.com/53213397/117608134-6bffbe80-b198-11eb-84e6-6c28eda43b83.png)
Ground Truth Bounding Box와 겹치는 IoU에 따라 Anchor box를 Positive와 Negative로 구분한다.
- IoU가 0.7이상이면 Positive
- IoU가 가장높은 Anchor는 Positive
- IoU가 0.3보다 낮으면 Negative

### - Anchor Box를 Reference로 한 Bounding Box Regression
![6](https://user-images.githubusercontent.com/53213397/117608137-6c985500-b198-11eb-845f-442c61ca0f7f.png)
**Predicted Anchor Box**를 최대한 **Positive Anchor box**와 가깝게 하는것이 bounding box regression  
여기서 주의할 점은 **Ground Truth Box**를 따라가는 것이 **아니라는** 점이다.
### - RPN Classification과 Bounding Box Regression
![7](https://user-images.githubusercontent.com/53213397/117608139-6c985500-b198-11eb-8fc1-8ac667aebb8b.png)
- Classification을 통해 Positive box를 구해준다.
- Bounding Box Regression은 Positive Anchor box와의 간격을 줄인다.

### - RPN 의 Output
![8](https://user-images.githubusercontent.com/53213397/117608140-6d30eb80-b198-11eb-9488-7dd5c1d89f0d.png)
### - Faster R-CNN Training
![9](https://user-images.githubusercontent.com/53213397/117608143-6dc98200-b198-11eb-8e24-ff194392828e.png)
- RPN 과 Fast R-CNN 이 네트워크를 공유하기 때문에 만일 어느 한 쪽이 학습되어 Weight가 그 영역에 최적화되면 다른 영역은 성능이 떨어지게 된다.
- 각각의 영역을 Fine Tunning을 통해 해결한다. faster R-cnn은 FC layer부분, RPN은 1x1 fully convolutional layer부분만 Fine Tunning 해준다.