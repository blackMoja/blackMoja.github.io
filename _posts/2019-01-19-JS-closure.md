---
title: 클로저(Closure)
date: 2019-01-19
categories:
    - javascript
tags:
    - javascript
---
클로저(Closure)에 대하여 알아보고자 한다.
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/javascript.png)
<!-- more -->
# 클로저란 무엇일까?
간단하게 설명해보자면 외부함수안에 내부함수를 선언해서 리턴을 해주면 그 자체로 클로저가 된다.
무슨 이상스러운 소리일까 간단하게 예제를 보자
```javascript
function outter() {
    var outer = 1;
    function inner() {
        console.log(outer)
    }
    return inner;
}

outter() // 1;
```
위에 정의한 outter 함수는 익명함수를 반환하고, 반환된 함수에서는 내부에서 선언된 변수를 참조하고 있다.
함수 실행이 끝났다고 해서 사라지지 않고 제대로 된 값을 반환하고 있다.

여기서 반환된 함수가 **클로져** 이다!!

# 은닉화
우리 모두가 알듯 함수 외부에서 함수 내부 변수를 접근 할 수가 없다. 그 변수들을 private 변수라고 한다.
아래 예를 보자 우리는 클로저 안 내부 변수에 직접접근 할 수 없는 상황을 보게 된다.
```javascript
function outter() {
    var outterValue = 123;
    return function {
        console.log('number : ', number)
    }
}

outter(); // number : 123
```
우리는 여기서 더이상 내부 변수에 접근 할 방법이 없다. 
클로저를 사용하면 은닉화도 손쉽게 가능하다.

하지만 우리는 개발을 하게 되면 private변수에 접근을 해야 할 일이 생기는데 클로저를 사용하면 이 문제도 해결 할 수 있다.
```javascript
function outter(name) {
    return {
        inner() {
            console.log('inner function : ', name);
        }
    };
}

var test = outter('blackMoja');
test.inner(); // inner function : blackMoja
```
예제에서 처음 outter에 'blackMoja'를 전달했다.
outter로 생성된 test객체는 return되는 함수를 통해 outter에 전달된 name을 가져왔다.
여기서 inner함수가 outter함수의 외부에 노출되는 함수(클로저)이고, 이런 함수를 **특권 함수**라고 한다.
##### 여기서 특권이란?
단지 비공개 멤버에 접근권한을 가진(즉 일종의 특권을 부여 받은)공개 메서드를 가리키는 이름이다.
위 예제에서는 inner함수가 outter함수의 name에 접근권한을 부여받은 특권 함수이다.

# 클로저를 활용한 반복문 
클로저에 환경에 대해서도 알아볼 수 있는 좋은 예제이다.
다음 예제를 보고 생각해보자.
```javascript
var i;
for (i = 1; i < 10; i += 1) {
    setTimeout(function add() {
        console.log(i);
    }, i*100);
}
```
우리는 보통 for문을 1~10까지 도니까 당연히 우리는 1~10까지 찍힐 거라고 예상한다.
하지만 실형 결과를 보면 많이 다르다.
add함수에 인자(i)로 넘긴 아이는 0.1초 뒤에 실행된다. 하지만 그 0.1초에 모두 연산이 마무리되어 i는 전부 10이 된 상태 
그렇기 때문에 우리가 실제로 출력되는 값을 보면 10이 10번 찍히는 상황을 보게된다.

우리는 클로저를 이용하여 정상적으로 출력이 가능하게 끔 처리 할 수 있다.
```javascript
var i;
for (i = 1; i < 10; i += 1) {
    function(changeNumber) {
        setTimeout(function add() {
            console.log(changeNumber;
        }, i*100);
    }(i)
}
```
setTimeout위에 IIFE를 활용해서 add함수를 클로저로 만들어버렸다. 여기서 **클로저는 만들어진 당시 환경을 기억한다**
예제에서 IIFE함수는 i를 받아서 changeNumber라는 형태로 들어온다. 클로저의 각기 다른 환경에 들어가게 된다.
for문은 총 10번 클로저도 총 10개가 생성됨으로 각기 다른 10개의 환경의 changeNumber가 생성된다.
따라서 우리가 원하는 1~10 까지의 결과물을 출력할 수 있게된다.

## 마치면서..
자바스크립트 개발하면서 클로저를 들어봤지만 실제로 제대로 공부하고 개념을 새우기는 어려운 개념이다.
포스팅을 하는 지금 까지도 내가 실제로 코딩하면서 적용해보지 않는 이상 완벽하게 정리하고 내껄 만들었다고 하기에는 아직은 이른 것 같다.
코딩을 하면서 나오는 새로운 내용이나 포스팅 하면서 틀리거나 추가할 내용은 꼭 추가하도록 해야겠다.
다음 포스팅때는 **클로저(Closure)**와 밀접한 관계가 있는 **스코프(Scope)**에 대해서 알아 볼 예정이다.

## 참고한 자료
[클로저-MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)
[자바스크립트의 스코프와 클로저 - Toast Meetup!](https://meetup.toast.com/posts/86)
[[번역] 자바스크립트 스코프와 클로저(JavaScript Scope and Closures)](https://medium.com/@khwsc1/%EB%B2%88%EC%97%AD-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-javascript-scope-and-closures-8d402c976d19)