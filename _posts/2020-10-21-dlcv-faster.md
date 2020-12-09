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
![1](/assets/img/faster1.png)

Region Proposal 영역을 딥러닝 네트워크에 포함시켜서 Fast R-CNN의 문제점을 개선했다. [(Fast R-CNN 포스팅)](https://donggoolosori.github.io/2020/10/21/fast-rcnn/)  
따라서 Region Proposal에 GPU를 사용할 수 있고, 전체적으로 End-to-End 구조가 완성된다. 
# Faster R-CNN 구조
![2](/assets/img/faster2.png)
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
![3](/assets/img/faster3.png)
### - Feature Map에서의 Anchor Box
![4](/assets/img/faster4.png)
아주 촘촘하게 포인트마다 9개의 Achor Box를 만들어준다.  
위의 그림에서는 총 1900개의 포인트마다 9개의 Achor box를 생성하여 전부 1900x9 = 17100개의 Anchor box가 존재한다.
### - Positive, Negative Anchor Box
![5](/assets/img/faster5.png)
Ground Truth Bounding Box와 겹치는 IoU에 따라 Anchor box를 Positive와 Negative로 구분한다.
- IoU가 0.7이상이면 Positive
- IoU가 가장높은 Anchor는 Positive
- IoU가 0.3보다 낮으면 Negative

### - Anchor Box를 Reference로 한 Bounding Box Regression
![6](/assets/img/faster6.png)
**Predicted Anchor Box**를 최대한 **Positive Anchor box**와 가깝게 하는것이 bounding box regression  
여기서 주의할 점은 **Ground Truth Box**를 따라가는 것이 **아니라는** 점이다.
### - RPN Classification과 Bounding Box Regression
![7](/assets/img/faster7.png)
- Classification을 통해 Positive box를 구해준다.
- Bounding Box Regression은 Positive Anchor box와의 간격을 줄인다.

### - RPN 의 Output
![8](/assets/img/faster8.png)
### - Faster R-CNN Training
![9](/assets/img/faster9.png)
- RPN 과 Fast R-CNN 이 네트워크를 공유하기 때문에 만일 어느 한 쪽이 학습되어 Weight가 그 영역에 최적화되면 다른 영역은 성능이 떨어지게 된다.
- 각각의 영역을 Fine Tunning을 통해 해결한다. faster R-cnn은 FC layer부분, RPN은 1x1 fully convolutional layer부분만 Fine Tunning 해준다.