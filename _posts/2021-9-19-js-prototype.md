---
layout: post
title: "[Javascript] 프로토타입(Prototype)"
subtitle: "js prototype"
date: 2021-9-19 23:10:00
author: "dongjune"
header-img: "img/in_post/2.jpg"
catalog: true
tags:
  - Javascript
---
# 🧀 프로토타입 객체

자바스크립트의 모든 객체는 자신의 **부모 역할을 담당하는 객체**와 연결돼있습니다. 이를 통해 객체 지향의 상속 개념 처럼 부모 객체의 프로퍼티, 메소드를 상속받아 사용할 수 있습니다. 그 부모객체가 바로 프로토타입입니다.

```jsx
const student = {
  name: 'dongjune',
  score: 100,
};

// student에는 hasOwnProperty 메소드가 없지만 아래 구문은 동작한다.
console.log(student.hasOwnProperty('name'));

console.dir(student);
```

<img width="1028" alt="스크린샷_2021-09-18_오후_4 58 09" src="https://user-images.githubusercontent.com/53213397/133931263-ce489b1f-5bc4-4b9b-87ca-5d2a8465a754.png">


위의 그림을 보시면 student 객체가 `__proto__`라는 프로퍼티를 갖는것을 볼 수 있습니다. 이처럼 자바스크립트의 모든 객체는 `[[Prototype]]` (또는 `__proto__`) 이라는 인터널 슬롯을 갖습니다.

`[[Prototype]]`의 값은 null 또는 객체이며 상속을 구현하는데 사용됩니다. `[[Prototype]]` 객체의 **데이터 프로퍼티**는 get 액세스를 위해 상속되어 자식 객체의 프로퍼티 처럼 사용할 수 있습니다. 하지만 set 액세스는 허용하지 않습니다.
> 데이터 프로퍼티는 일반적으로 객체에 속해있는 프로퍼티를 의미합ㄷ다. 이와 반대로 접근자 프로퍼티가 존재하는데 get, set 키워드를 붙여 정의할 수 있습니다.  

  

`[[Prototype]]`의 값은 프로토타입 객체이며 `__proto__` accessor property로 접근할 수 있습니다. `__proto__` 프로퍼티에 접근하면 내부적으로 `Object.getPrototypeOf`가 호출되어 프로토타입 객체를 반환합니다. 위의 예제에서 `student` 객체는 `__proto__` 프로퍼티로 자신의 부모 객체(프로토타입 객체)인 `Object.prototype`을 가리키고 있습니다.

```jsx
const student = {
  name: 'dongjune',
  score: 90
}
console.log(student.__proto__ === Object.prototype); // true
```

객체를 생성할 때 프로토타입이 결정되며 프로토타입 객체는 다른 임의의 객체로 변경할 수 있습니다. 이러한 프로토타입의 동적인 특징을 활용하여 상속을 구현할 수 있습니다.

# 🧀 \[[Prototype]] vs prototype 프로퍼티

모든 객체는 자신의 프로토타입 객체를 가리키는 `[[Prototype]]` 인터널 슬롯을 갖습니다.
함수도 객체이므로 `[[Prototype]]` 인터널 슬롯을 갖는데 **함수 객체**는 일반 객체와 달리 `prototype` 프로퍼티 또한 소유하게 됩니다.

```jsx
function Person(name) {
  this.name = name;
}

var foo = new Person('dongjune');

console.dir(Person.prototype); // 존재
console.dir(foo.prototype);    // undefined
```

### \[[Prototype]] (`__proto__`)
- 함수를 포함한 모든 객체가 가지고 있는 인터널 슬롯
- **객체의 입장에서 자신의 부모 역할을 하는 프로토타입 객체를 가리키며 함수 객체의 경우** `Function.prototype` 을 가리킨다.

```jsx
console.log(Person.__proto__ === Function.prototype); // true
```

### prototype 프로퍼티
- 함수 객체만 갖는 프로퍼티
- **함수 객체가 생성자로 사용될 때 이 함수를 통해 생성될 객체의 부모 역할을 하는 객체(프로토타입)를 가리킨다.**

```jsx
console.log(Person.prototype === foo.__proto__); // true
```

# 🧀 constructor 프로퍼티

프로토타입 객체는 constructor 프로퍼티를 갖습니다. **이 constructor 프로퍼티는 객체의 입장에서 자신을 생성한 객체를 가리킵니다.**

```jsx
function Person(name) {
  this.name = name;
}

const foo = new Person('dongjune');

// 1
console.log(Person.prototype.constructor === Person);

// 2
console.log(foo.constructor === Person);

// 3
console.log(Person.constructor === Function);
```

1. `Person()` 생성자에 의해 생성 된 객체의 프로토타입을 생성한 객체는 `Person()` 생성자 함수이다.
2. `foo`를 생성한 객체는 `Person()` 생성자 함수이다.
3. `Person`을 생성한 객체는 `Function()` 생성자 함수이다.

# 🧀 Prototype chain

자바스크립트는 어떤 객체의 프로퍼티나 메소드에 접근하려 할 때 해당 객체에 요소가 존재하지 않으면 `[[Prototype]]`이 가리키는 링크를 따라 프로토타입의 프로퍼티나 메소드를 차례대로 검색합니다. 이것을 **Prototype chain** 이라고 합니다.  
아래의 코드를 보면 `student`에는 `hasOwnProperty` 메소드가 존재하지 않지만 자신의 프로토타입인 `Object.prototype`에서 `hasOwnProperty` 메소드를 찾습니다.

```jsx
const student = {
  name: 'dongjune',
  score: 90
}

// Object.prototype.hasOwnProperty()
console.log(student.hasOwnProperty('name')); // true
```

## 🤔 객체 리터럴 방식으로 생성된 객체의 프로토타입 체인

### 객체 생성 방법 3가지

- 객체 리터럴

    ```jsx
    const Person = { name: "dongjune" };
    ```

- 생성자 함수

    ```jsx
    const person = new Person("dongjune");
    ```

- Object() 생성자 함수

    ```jsx
    const person = new Object();
    ```

- 객체 리터럴 방식은 결국 `Object()` 생성자 함수를 사용하여 객체를 생성한다.

- `Object()` 생성자 함수는 함수이기 때문에 `prototype` 프로퍼티가 존재한다.

- 객체 리터럴로 생성된 객체의 프로토타입 객체는 `Object.prototype`이다.

<img width="867" alt="스크린샷_2021-09-19_오전_10 46 29" src="https://user-images.githubusercontent.com/53213397/133931268-2575b2c3-09b5-4275-a772-e235516980c6.png">


## 🤔 생성자 함수로 생성된 객체의 프로토타입 체인

### 함수를 정의하는 방식 3가지

- Function() 생성자 함수
- **함수표현식**: 함수를 정의할 때 함수 리터럴 방식을 사용한다.

    ```jsx
    var hi = function(name){
    	return 'Hi' + name;
    }
    ```

- **함수선언식**: 자바스크립트 엔진이 내부적으로 기명 함수표현식으로 변환한다.

    ```jsx
    function hi(name){
    	return 'Hi' + name;
    }

    // 위의 함수선언식은 자바스크립트 엔진이 내부적으로 아래의 기명 함수표현식으로 변환
    var hi = function hi(name){
    	return 'Hi' + name;
    } 
    ```

- 결국 함수선언식, 함수 표현식 모두 함수 리터럴 방식을 사용하며, 함수 리터럴 방식은 Function() 생성자 함수를 단순화한 방식이다.

> 즉 3가지 함수 정의 방식 모두 Function() 생성자 함수를 사용합니다. 따라서 모든 함수 객체의 `prototype` 객체는 `Function.prototype`입니다.

```jsx
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
  this.sayHello = function(){
    console.log('Hi! my name is ' + this.name);
  };
}

var foo = new Person('dongjune', 'male');

console.dir(Person);
console.dir(foo);

console.log(foo.__proto__ === Person.prototype);                // ① true
console.log(Person.prototype.__proto__ === Object.prototype);   // ② true
console.log(Person.prototype.constructor === Person);           // ③ true
console.log(Person.__proto__ === Function.prototype);           // ④ true
console.log(Function.prototype.__proto__ === Object.prototype); // ⑤ true
```

<img width="964" alt="스크린샷_2021-09-19_오전_11 05 46" src="https://user-images.githubusercontent.com/53213397/133931269-4a420d14-f778-41d8-9ed9-989a11fbdc82.png">


1. `foo` 객체의 프로토타입은 `Person.prototype`이다.
2. `Person.prototype`의 프로토타입은 `Object.prototype`이다.
3. `Person.prototype`은 `Person()` 생성자 함수에 의해 생성됐다
4. `Person`은 함수 리터럴로 생성됐으므로 `Person`의 프로토타입 객체는 `Function.prototype`이다.
5. 프로토타입 체인은 모든 객체의 부모 객체인 `Object.prototype` 객체에서 끝난다. 따라서 `Function.prototype`의 프로토타입 객체는 `Object.prototype`이다.

# 🧀 프로토타입 객체의 확장

프로토타입 객체도 일반 객체와 같이 프로퍼티나 메소드를 추가, 삭제 할 수 있습니다.

```jsx
function Person(name) {
  this.name = name;
}

var foo = new Person('dongjune');

Person.prototype.sayHello = function(){
  console.log('Hi! my name is ' + this.name);
};

foo.sayHello();
```

![스크린샷_2021-09-19_오후_10 30 38](https://user-images.githubusercontent.com/53213397/133931270-74816c6b-8b37-43d0-90d8-a4225f25b710.png)


# 🧀 원시타입의 확장

자바스크립트에서 원시 타입(숫자, 문자열, boolean, null, undefined)을 제외한 모든것은 객체입니다.  
하지만 아래의 코드와 같이 원시타입인 문자열이 객체와 유사하게 동작하는 것을 볼 수 있는데요.

```jsx
var str = 'test';
console.log(typeof str);                 // string
console.log(str.constructor === String); // true
console.dir(str);                        // test

var strObj = new String('test');
console.log(typeof strObj);                 // object
console.log(strObj.constructor === String); // true
console.dir(strObj);
// {0: "t", 1: "e", 2: "s", 3: "t", length: 4, __proto__: String, [[PrimitiveValue]]: "test" }

console.log(str.toUpperCase());    // TEST
console.log(strObj.toUpperCase()); // TEST
```
이는 원시타입으로 프로퍼티나 메소드를 호출할 때 원시 타입과 연관 된 객체로 일시적으로 변환되어 프로토타입 객체를 공유하기 때문입니다.  
하지만 원시타입은 객체가 아니므로 프로퍼티나 메소드를 직접 추가할 수 없습니다.

```jsx
var str = 'test';

// 에러가 발생하지 않는다.
str.myMethod = function () {
  console.log('str.myMethod');
};

str.myMethod(); // Uncaught TypeError: str.myMethod is not a function
```

이때 우리는 prototype을 이용할 수 있습니다. String.prototype에 메소드를 추가하면 원시타입, 객체 모두 메소드를 사용할 수 있습니다.

```jsx
var str = 'test';

String.prototype.myMethod = function () {
  return 'myMethod';
};

console.log(str.myMethod());      // myMethod
console.log('string'.myMethod()); // myMethod
console.dir(String.prototype);
```

# 🧀 프로토타입 객체를 이용한 상속 구현

객체를 생성할 때 프로토타입 객체가 결정되지만 이 객체는 다른 임의의 객체로 변경 가능합니다. 이것은 부모 객체인 프로토타입을 동적으로 변경가능하다는 것을 의미하며 이를 이용하여 상속을 구현할 수 있습니다.

### 🤔 프로토타입 객체를 변경하면

- 프로토타입 변경 전에 생성한 객체 → 기존 프로토타입 객체를 [[Prototype]]에 바인딩한다.
- 프로토타입 변경 이후에 생성된 객체 → 변경된 프로토타입 객체를 [[Prototype]]에 바인딩한다.

```jsx
function Person(name) {
  this.name = name;
}

var foo = new Person('Lee');

// 프로토타입 객체의 변경
Person.prototype = { gender: 'male' };

var bar = new Person('Kim');

console.log(foo.gender); // undefined
console.log(bar.gender); // 'male'

console.log(foo.constructor); // ① Person(name)
console.log(bar.constructor); // ② Object()
```

![스크린샷_2021-09-19_오후_11 19 18](https://user-images.githubusercontent.com/53213397/133931271-c031d7f3-94b8-48d0-b3db-b750faa40304.png)

1. `foo`의 `constructor`는 `Person()` 생성자 함수를 가리킨다.
2. `Person.prototype` 객체가 변경되면서 `Person.prototype.constructor` 프로퍼티도 삭제됐다. 따라서 `bar`의 `constructor`는 프로토타입 체이닝으로 `Object.prototype`의 `constructor`가 된다. `Object.prototype`의 `constructor`는 `Object()` 생성자 함수이다.

# 🧀 프로토타입 체인 동작 조건

객체의 프로퍼티를 참조하는데 해당 객체에 프로퍼티가 존재하지 않는 경우 프로토타입 체인이 동작합니다.
객체의 프로퍼티에 값을 할당할 때는 프로토타입 체인이 동작하지 않습니다. 이는 자바스크립트 엔진이 객체에 할당하려는 프로퍼티가 존재하지 않으면 그냥 프로퍼티를 동적으로 추가하기 때문입니다.

# Reference
- https://poiemaweb.com/js-prototype
- https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Object_prototypes
