---
layout: post
title: "[iOS] Pinning Constraints - Swift"
subtitle: "Auto Layout and Responsive UIs"
date: 2020-12-06 14:19:00
author: "dongjune"
header-img: "img/home-bg.jpg"
catalog: true
tags:
  - iOS
---
### <span style="color:rgba(0,0,200,0.4)">Constraints가 필요한 이유</span> 
아래의 초록색 Image View만 추가해 놓은 앱을 살펴봅시다.
<img width="600" alt="con-1" src="https://user-images.githubusercontent.com/53213397/101273647-b8566500-37da-11eb-8d69-8b69978f5cba.png">
현재 세로뷰에서는 정상적인 모습이지만 화면을 가로로 돌린다면 어떻게 될까요? 아래의 그림 처럼 이미지뷰가 화면에 고정되지 않고 밖으로 나가버리게 됩니다.   
<img width="600" alt="con-2" src="https://user-images.githubusercontent.com/53213397/101273646-b7253800-37da-11eb-889d-4185c3918326.png">
이와 같은 현상을 방지하기 위해 저희는 이미지뷰와 같은 컨텐츠 들을 고정 할 필요가 있는데 이것이 바로 **Constraint** 입니다.  


Constraint는 크게 **Pinning Constraint** 와 **Alignment Constraint** 로 나눌 수 있습니다. 둘 중 하나만 사용할 수도 있고 둘 다 사용할 수도 있습니다.   


**Pinning Constraint**는 거리를 고정하는 방식입니다.  
예를 들어 이미지뷰에 왼쪽면을 기준으로 50px의 Pinning Constraint를 설정한다면 세로뷰, 가로뷰, 디바이스 기종 등에 상관없이 항상 왼쪽면에 50px 만큼 떨어져 있게 됩니다.

  
**Alignment Constraint**는 말 그대로 정렬을 사용합니다. 예를 들어 버튼에 Horizontal center로 alignment constraint를 설정한다면 버튼은 어떤 상황에서도 항상 화면 왼쪽과 오른쪽의 중앙에 위치하게 됩니다.  vertical center를 설정한다면 항상 화면 윗면과 아랫면의 중앙에 위차하게 되겠죠.  
  
오늘은 Pinning Constraint를 살펴보고 [다음 포스팅](https://donggoolosori.github.io/2020/12/06/ios-align/)에서 Alignment Constraint를 살펴보겠습니다.
****


### <span style="color:rgba(0,0,200,0.4)">Constraints Setting</span> 
이제 위의 앱에 Constraint를 설정하여 가로 뷰에서도 초록바탕 이미지뷰가 화면에 꽉차도록 설정해보겠습니다.
이미지뷰를 선택한 상태로 화면 하단의 Add new constraints 버튼을 눌러줍니다.
<img width="600" alt="con-3" src="https://user-images.githubusercontent.com/53213397/101273645-b68ca180-37da-11eb-8809-e54456ae0b1c.png">
그러면 다음과 같은 창이 뜹니다. **상하좌우 숫자**가 있고 자세히 보면 각각에 연결된 **빨간색 점선**이 있습니다.  
<img width="600" alt="con-4" src="https://user-images.githubusercontent.com/53213397/101273644-b42a4780-37da-11eb-96a9-e3828bdedd06.png">
다음과 같이 점선 4개 **모두**를 클릭해주고 아래의 Add 4 Constraints 버튼을 클릭해주면 설정완료입니다.
<img width="600" alt="con-5" src="https://user-images.githubusercontent.com/53213397/101273637-ae346680-37da-11eb-8c8d-3c4e67d3faed.png">
빨간색 점선을 클릭하는 것은 그 **방향**에 Pinning Constraint를 걸어주겠다는 뜻입니다.
숫자의 의미는 상 하 좌 우 모서리에서 **얼마만큼 떨어지는지**를 설정하는 것입니다. 저희는 이미지뷰가 화면에 꽉 차도록 하고싶기 때문에 4 방향에 모두 0을 설정하는 것입니다.  

  
이제 좌측 빨간박스에 Constraints 설정이 생성됐고 화면을 돌려도 이미지뷰가 밖으로 나가는 현상은 사라졌습니다.  
<img width="600" alt="con-6" src="https://user-images.githubusercontent.com/53213397/101273634-aaa0df80-37da-11eb-8857-fec8540ebe52.png">
하지만 아직 1%가 아쉽습니다. 화면이 꽉차는게 아니라 흰색 여백이 생성됐습니다.  
이는 Constraints가 자동으로 **Safe Area**를 기준으로 생성됐기 때문입니다. Safe Area는 스마트폰 상단의 배터리, 와이파이 표시등을 위한 공간과 하단의 홈, 뒤로가기 버튼 등을 위한 공간입니다.  
  
이제 Constraints 기준을 Safe Area가 아닌 Super View로 변경하여 이미지가 화면에 꽉차도록 설정해봅시다.  
우선 화면 좌측에서 왼쪽면에 대한 Constraint 설정을 클릭합니다. 오른쪽 상단에 보시는 것처럼 Safe Area로 설정돼있는 것을 볼 수 있습니다.
<img width="600" alt="con-7" src="https://user-images.githubusercontent.com/53213397/101273632-a70d5880-37da-11eb-80d4-5c251aca7288.png">
이제 이것을 Super View 로 변경해주고
<img width="600" alt="con-8" src="https://user-images.githubusercontent.com/53213397/101273631-a4aafe80-37da-11eb-9bf4-afc62faa24cf.png">
기준이 변경됐기 때문에 얼마나 떨어져있는지를 나타내는 constant를 0으로 변경해줍니다.
<img width="600" alt="con-9" src="https://user-images.githubusercontent.com/53213397/101273630-a4126800-37da-11eb-97ce-75361d1f3886.png">
오른쪽면 Constraint 설정도 위와 동일하게 수행해줍니다.  

  
이제 세로 뷰와 가로 뷰 모두에서 꽉찬 이미지뷰를 보이는 것을 확인 할 수 있습니다.
<img width="600" alt="con-10" src="https://user-images.githubusercontent.com/53213397/101273629-a1b00e00-37da-11eb-97fe-b92ac046693e.png">
<img width="600" alt="con-11" src="https://user-images.githubusercontent.com/53213397/101273626-9b219680-37da-11eb-9879-06b354605962.png">





