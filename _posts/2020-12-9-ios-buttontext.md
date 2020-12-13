---
layout: post
title: "[iOS] Change button title - Swift"
subtitle: "setTitle"
date: 2020-12-09 22:19:00
author: "dongjune"
header-img: "img/in_post/ios.jpg"
catalog: true
tags:
  - iOS
---
setTitle 명령어를 통해 UIButton의 title을 바꿀 수 있습니다.
```swift
button.setTitle("Change a title",for:.normal)
```


다음 코드는 앱이 로드 됐을 때 (**viewDidLoad**) UIButton의 title을 바꾸는 코드입니다.

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var choice1Button: UIButton!
    @IBOutlet weak var choice2Button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        button.setTitle("Change a title",for:.normal)
    }
}
```