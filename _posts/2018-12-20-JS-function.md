---
title: 함수
date: 2018-12-20
categories:
    - javascript
tags:
    - javascript
---
### 들어가기 전...
요즘 자바스크립트에 대해 더 자세히 공부하면서 기본적인 내용 중에서 많이 사용하면서, 가장 중요한 **함수(Function)**에 대하여 복습하는 겸 내용을 정리하고자 한다.
<!-- more -->
# 목차
 - [JavaScript 함수](# JavaScript 함수)
 - [JavaScript 함수선언](# JavaScript 함수선언)
 - [JavaScript 함수표현](# JavaScript 함수표현)
 - [JavaScript 함수 선언식과 표현식의 차이점](# JavaScript 함수 선언식과 표현식의 차이점)
 - [참고](# 참고)

# JavaScript 함수
<hr />

>* 함수는 외부코드에 의해 함수는 외부코드에 의해 호출될 수 있는 '하위프로그램'이다.
>* 프로그램 그 자체처럼, 함수는 함수 몸통(function body)이라고 하는 일련의 문(statement)로 구성된다.
>* 값은 함수에 전달될 수 있고 함수는 값을 반환한다.
>* JavaScript에서, 함수는 다른 객체처럼 속성 및 메서드를 가질 수 있기에 <em>일급(first-class)</em> 객체 이다.
>  -- 다른 객체와 함수를 구별하는 것은 함수는 호출될 수 있다는 것이다.
>  -- 함수는 Function 객체이다.

### 일급 객체(First-Class)의 정의
1. 변수(variable)에 담을 수 있다.
2. 인자(parameter)로 전달할 수 있다.
3. 반환값(return value)으로 전달 할 수 있다.

일급 객체의 자세한 내용은 다음에 다루기로...

# JavaScript 함수선언
<hr />
함수(표현 및 선언)는 다음과 같은 3가지 키워드로 구성되어 있다.
* 함수의 이름
* 괄호에서 쉼표로 구분하는 매개변수
* 중괄호 {} 에서 함수를 정의하는 자바스크립트 표현

- 간단한 함수선언 예제
    ```javascript
    function showText(text) {
        return console.log(text);
    }
    ```
    1. 예제 코드에서 showText함수는 text라는 매개변수를 받고 
    console.log(text)를 return해주고 있다.

    2. 함수가 매개변수의 값을 바꾸더라도 이는 
    전역적으로 또는 함수를 호출하는 곳에는 반영되지 않는다.

- 매개변수로 Array나 Object 및 기본 자료형이 아닌 값을 전달하거나 함수가 객체의 속성을 변하게 하는 경우, 그 값을 변경할 수 있다.
    ```javascript
    function carBrand(brandItem) {
        brandItem.name= 'KIA';
    }

    var newCarBrand = {
        name: 'HYUNDAI',
        model: 'SONATA'
    };

    var temp_a, temp_b;

    temp_a = newCarBrand.name; // HYUNDAI

    carBrand(newCarBrand);
    temp_b = newCarBrand.name; // KIA, carBrand() 함수를 호출하여 값을 변경하였다.
    ```

# JavaScript 함수표현
<hr />
* 함수는 함수 표현식(function expression)에 의해서 선언할 수 있다.
* 이렇게 선언된 함수를 익명함수라고 한다. 모든 함수가 이름을 가질 필요가 없다는 말입니다.

함수 표현식으로 선언한 예제
```javascript
    var showTest = function(test) {
        return console.log(test);
    }

    var resultText = showTest('blackMoja'); // resultText는 blackMoja라고 console에 출력해줍니다.
```

* 함수 표현식에서 함수의 이름을 지정 할 수 있으며, 함수내에서 자신을 참조하는데 사용되거나, 
  디버거 내 스택 추적에서 함수를 식별하는 데 사용될 수 있습니다.

```javascript
var showTest = function showTest(test) {
    return console.log(test);
}

console.log(showTest(3));
```

* 함수 표현식은 함수를 다른 함수의 매개변수로 전달할 때 편리합니다. 
  다음 예제는 함수 표현식으로 정의된 함수를 인자로 받아, 2번 째 인자인 배열의 모든 요소에 대해 그 함수를 실행합니다.

```javascript
    function map(f, a) {
        var result = [],
            i;

        for (i = 0; i != a.length; i++) {
            result[i] = f(a[i]);
            return result;
        }
    }

    var f = function(x) {
        return x * x * x; 
    }

    var numbers = [0, 1, 2, 5, 10];
    var cube = map(f,numbers);
    console.log(cube);
```
* 함수는 [0, 1, 8, 125, 1000] 을 반환합니다.
* 함수를 정의하는것에 더하여 eval() 과 같이 런타임에 문자열에서 함수들을 만들기위해 Function 생성자를 사용할 수 있습니다.
* 객체내의 한 속성이 함수인 경우 메서드라고 합니다.

# JavaScript 함수 선언식과 표현식의 차이점
<hr />
### 함수 선언식은 호이스팅에 영향을 받지만, 함수 표현식은 호이스팅에 영향을 받지 않는다.
### 함수 표현식이 아니라 함수 선언식일 때는 함수 자체를 동째로 끌어올린다.

(호이스팅에 관련하여 자세한 내용은 더 찾아보고 다음에 정리하여 포스팅 해야겠당...)
호이스팅은 코드를 구현한 위치와 관계없이 브라우저가 자바스크립트를 해석할 때 맨 위로 올린다.

함수 선언식 호이스팅 전
```javascript
console.log(first);
second();
function second() {
    console.log('second');
}
var first = 'first';
```

함수 선언식 호이스팅 후
```javascript
function second() {
    console.log('second');
}

var first;

console.log(first);
second();
first = 'first';
```

이번에는 함수 표현식 예제입니다.
```javascript
first(); // first is not a function
second();
var first = function () {
    console.log('first');
}
function second() {
    console.log('second');
}
```
second() 함수는 함수 선언식 이므로 함수 생성 후 바로 대입이 됩니다. 하지만
함수 표현식으로 선언된 first() 함수는 대입되기 전에 호출을 먼저하여 에러가 발생합니다.

# 참고
<hr />
MDN web docs에 함수라는 타이틀의 글을 참고하였습니다.
url : https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%ED%95%A8%EC%88%98