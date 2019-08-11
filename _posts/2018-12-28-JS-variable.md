---
title: 변수의 범위와 호이스팅
date: 2018-12-28
categories: javascript
tags: javascript
---
### 들어가기 전...
정말 오랜만에 블로그 포스팅을 하는 것 같다... 마지막으로 글을 올린지 벌써 5달이 되어갔다.. 
이번에 포스팅 할 내용은 **자바스크립트 변수의 범위(Variable Scope)**와 **호이스팅(hoisting)** 에 관련된 내용을 포스팅 해볼려고 한다.
<!-- more -->
# 목차
 - [JavaScript 변수의 범위](# JavaScript 변수의 범위)
 - [JavaScript 호이스팅-hoisting 이란?](# JavaScript 호이스팅-hoisting 이란?)

# JavaScript 변수의 범위
<hr />
 - 자바스크립트에는 2가지 변수의 범위가 존재한다.
 - 자바스크립트는 블럭-수준(block-level)의 범위를 가지고 있지 않다.
 - 다들 한번쯤 들어본 **전역변수**와 **지역번수**이다.
 - 함수 정의 기준으로 함수 외부에 생선된 변수를 **전역변수**라고 하고 
 - 함수 정의 기준 함수 내부에 생선된 변수를 **지역변수**라고 한다. 

## JavaScript 지역변수와 전역변수
우선 간단한 예제코드로 지역변수와 전역변수를 알아보자.
```javascript
var whereIam = 'function out!!';

function checkScope() {
    var whereIam = 'function in!!';
    return console.log(whereIam);
}

checkScope(); // 함수 내부에 선언된 function in!!이 출력된다.
console.log(whereIam); // 전역 변수로 선언된 function out!!이 출련된다.
```

지역변수를 선언하지 않을 시 문제점을 알아보자.
```javascript
var whereIam = 'function out!!';

console.log(whereIam); // function out!!

function checkScope() {
    var functionText = 'function in !!';
    whereIam = 'whereIam is change!!'; // 지역 변수를 var 키워드로 선언하지 않았기 때문에 지역 변수가 된다.
    console.log(functionText); // function in !!
    console.log(whereIam); // whereIam is change!!
}

checkScope();
```
var 키워드를 사용하여 변수 선언 시 지역변수로 선언되는 일을 막을 수 있다.

함수 외부에서 선언하거나 함수 내부에서 var 키워드를 사용하지 않고 선언한 변수는 지역 변수로 선언된다.
글로벌 객체는 자기 자신을 정의하는 속성을 가지고 있으며, HTML DOM에서는 window 속성이 global 객체 그 자신이다 즉 
**전역 변수는 window 객체를 통해 접근이 가능하다.**
```javascript
var nameTest; // 전역 변수 선언 방법
function getName() {
    name = 'blackMoja'; // 또다른 전역 변수 선언 방법
    console.log('name : ', name); // name : blackMoja
}

console.log(getName());
console.log(name); // 마찬가지로 전역변수로 선언되었으므로 blackMoja가 출력된다.
console.log(window.name); // window객체로 접근이 가능하다. 따라서 blackMoja가 출력된다.
```

**되도록이면 개발할때 전역변수의 생성은 피하자...**

# JavaScript 호이스팅-hoisting 이란?
<hr />
### **모든 변수 선언은 호이스팅 된다!!!**
호이스트란 변수의 정의가 범위에 따라 **선언**과 **할당**으로 분리되는 것을 말한다.
변수가 함수 내에서 정의 되었을 경우 선언이 함수의 최상위로, 
함수 외부에서 정의 되었을 경우 전역 컨텍스트의 최상위로 변경된다.
**변수의 선언이 초기화나 할당시에 발생하는것이 아니라, 최상위로 호이스트 된다.**

예제 코드로 확인해보자.
```javascript
function getName() {
    console.log('myName is : ', name); // myName is undefined
    var name = 'blackMoja';
    console.log('realName is : ', name); // realName is blackMoja
}

getName();
// myName is가 undefined인 이유는 지역변수 name이 호이스트 되었기 때문입니다.
```

자바스크립트 해석 순서
```javascript
function getName() {
    var name;
    console.log('myName is : ', name); // myName is undefined
    name = 'blackMoja';
    console.log('realName is : ', name); // realName is blackMoja
}

getName();
```

호이스트 되었을때, 함수 선언은 변수선언을 덮어 쓴다.
```javascript
var checkType;

function checkType() {
    console.log('function'); 
}

// 위 와 같이 두 변수 와 함수의 이름이 같은 경우 함수 선언 은 변수 선언을 덮어 쓴다.
console.log(typeof checkType); // function
```

반대로 변수에 값이 할당된 경우 변수가 함수 선언을 덮어쓴다.
```javascript
var name = 'blackMoja';

function name() {
    console.log('function');
}

// 위 와 같이 두 변수 와 함수의 이름이 같은 경우 변수 선언은 함수 선언을 덮어 쓴다.
console.log(typeof checkType); // string
```
**변수에 값을 할당 하기 전 항상 미리 선언하는 습관을 들이자.**

# 참고
<hr />
글로벌 변수와 window객체 관련된 글을 참고하였습니다.
http://unikys.tistory.com/324