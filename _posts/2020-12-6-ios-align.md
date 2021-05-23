---
layout: post
title: "[iOS] Alignment Constraints - Swift"
subtitle: "Auto Layout and Responsive UIs"
date: 2020-12-06 16:19:00
author: "dongjune"
header-img: "img/in_post/ios.jpg"
catalog: true
tags:
  - iOS
---
[지난 포스팅](https://donggoolosori.github.io/2020/12/06/ios-contraint/){:target="_blank"}에서 Pinning Constraints에 대해 알아봤습니다.  
하지만 Auto Layout을 위해서는 Pinning Constraint 만으로는 충분하지 않습니다. 
### <span style="color:rgba(0,0,200,0.4)">Pinning Constraint의 문제점</span>
버튼을 화면 중앙에 위치시키는 것을 생각해봅시다.만약 Pinning으로 이것을 설정한다면 어떻게 될까요?  
다음과 같이 세로뷰에서는 잘 작동하지만
<img width="700" alt="align-1" src="https://user-images.githubusercontent.com/53213397/101274677-77168300-37e3-11eb-8761-ac4a3b3fbdc2.png">
가로 뷰에서는 다음과 같이 중앙에 위치되지 못하는 것을 볼 수 있습니다.  
그 이유는 Pinning 은 거리를 통해 constraint를 설정하기 때문입니다.  
따라서 가로로 화면을 돌리게 되면 버튼이 이처럼 벗어나게 됩니다.
<img width="700" alt="align-2" src="https://user-images.githubusercontent.com/53213397/101274682-7c73cd80-37e3-11eb-8e5d-ce403f007a3d.png">

### <span style="color:rgba(0,0,200,0.4)">Alignment Constraint</span>
위의 문제점은 Alignment를 사용해서 해결할 수 있습니다.
<img width="700" alt="align-3" src="https://user-images.githubusercontent.com/53213397/101274780-31a68580-37e4-11eb-87da-16854e3e4ffb.png">
[지난 포스팅](https://donggoolosori.github.io/2020/12/06/ios-contraint/)에서 만든 앱에 간단한 label을 추가하고 이를 항상 중앙에 위치하도록 만들어 보겠습니다.  
  
#### 1. label 추가
<img width="700" alt="align-4" src="https://user-images.githubusercontent.com/53213397/101274929-52230f80-37e5-11eb-8052-1e7c5a350261.png">
#### 2. 아래의 Align 버튼 클릭
<img width="700" alt="align-5" src="https://user-images.githubusercontent.com/53213397/101274933-57805a00-37e5-11eb-90b2-0fc26cf996de.png">
#### 3. 수평, 수직 정렬 클릭 후 Add 버튼 클릭
<img width="700" alt="align-6" src="https://user-images.githubusercontent.com/53213397/101274936-58b18700-37e5-11eb-9553-4d44e20a1134.png">
#### 4. Align 완료
<img width="700" alt="align-7" src="https://user-images.githubusercontent.com/53213397/101274937-59e2b400-37e5-11eb-9486-58c89ab06e10.png">

  
  
