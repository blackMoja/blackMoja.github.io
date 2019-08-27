---
title: Watch와 Computed
date: 2019-08-26
categories:
    - Vue
tags:
    - Vue
---

정말 오랜만에 글 쓰는 것 같다.. @.@
더운 날씨와 나태해진 정신상태를 바로 잡고자 다시한번 블로그를 정리한다! ㅎ.ㅎ

최근 담당하고 있는 프로젝트 관련하여 Vue + typescript 구조로 고도화를 진행하는 도중
Vue에 관하여 조금씩 정리를 해볼려구 한다!

<!--more-->

# computed? watch?

## computed
Vue js 공식 사이트에서는 computed의 속성은 getter를 가지고 있습니다.
또한 템플릿 표현식에서 복잡한 계산식을 가지게 될 경우 computed를 권장 합니다.

간단한 예제를 통해 우선 보겠습니다.
```html
<div id="example">
    <p>원본 메세지 : "{{ message }}" </p>
    <p>역순 메세지 : "{{ reservedMessage }}" </p>
</div>
```

```javascript
var vm = new Vue({
    el: '#example',
    data: {
        message: '하위'
    },
    computed: {
        // getter 속성을 가집니다.
        reservedMessage: function() {
            // this 는 vm 인스턴스를 의미함다.
           return this.message.split('').reverse().join(''); 
        }
    }
});
```

### 결과
```
원본 메세지 : 하위
역순 메세지 : 위하
```

위 예제를 통해서 우리는 "vm.reservedMessage"의 값은 항상 "vm.message"를 의존하고 있다는 걸 알 수 있으며,
또한 "vm.message"의 값이 바뀌면 "vm.reservedMessage"의 값이 바뀐다는 걸 예제를 통해 확인했습니다!

## computed의 캐싱 vs 메소드
표현식에서는 메소드를 호출하여 computed와 같은 결과를 얻을 수 있습니다.

```
<p>뒤짚힌 메세지 : "{{ reservedMessage() }}"</p>
```

```javascript
methods: {
    reservedMessage: () => {
        return this.message.split('').reverse().join('');
    }
}
```

위 예제를 통해서 우리는 computed와 methods로 선언한 방식은 같은 결과를 낸다는 것을 알 수 있습니다.
하지만 중요한 차이점이 있습니다. 바로 **computed로 선언한 방식은 값에 대하여 저장(캐싱)한다는 것 입니다.**

computed로 선언한 방식의 값은 실제로 값이 변경되지 않는 이상 여러 번 요청해도 값을 계산하지 않고,
이미 저장되어 있는 값을 반환합니다.

```
<div id="test">
    <p>테스트를 해보아요</p>
    <p>computed : "{{ reservedText }}"</p>
    <p>computed : "{{ reservedText }}"</p>
    <p>methods : "{{ reservedText() }}"</p>
    <p>methods : "{{ reservedText() }}"</p>
</div>
```

```javascript
new Vue({
    el: '#test',
    data: {
        text: '누구인가'
    },
    computed: {
        reservedText() {
            console.log('------- computed -------')
            return this.message.split('').reverse().join('');
        }
    },
    methods: {
        reservedText: () => {
            console.log('------- methods -------')
            return this.message.split('').reverse().join('');
        }
    }
})
```

### 결과
위 예제의 실행결과로 콘솔창을 보면 실제로
------- computed -------
------- methods -------
------- methods -------
라는 결과를 얻을 수 있습니다.

## computed vs watch 그 차이?!
Vue는 computed와 비슷한 역할을 하는 watch라는 속성을 제공합니다.
Watch는 다른 데이터 기반으로 변경할 필요가 있는 데이터가 있는 경우
그 데이터가 바뀌면 이런 함수를 실행하라는 방식으로 소프트웨어 공학에서 이야기하는 ‘명령형 프로그래밍’ 방식 입니다.

```
<div id="test">
    " {{ city }}"
</div>
```

**watch 예제**
```javascript
var vm = new Vue({
    el: '#test',
    data: {
        fullAddress: '',
        country: 'Korea',
        city: 'Seoul'
    },
    watch: {
        country: function (val) {
            this.fullAddress = val + ' ' + this.city
        },
        city: function (val) {
            this.fullAddress = this.country + ' ' + val
        }
    }
})
```

**computed 예제**
```javascript
var vm = new Vue({
    el: '#demo',
    data: {
        country: 'Korea',
        city: 'Seoul'
    },
    computed: {
        address: function () {
            return this.country + ' ' + this.city
        }
    }
})
```

## Watch 속성
Vue는 watch 옵션을 통해 데이터 변경에 반응하는 보다 일반적인 방법을 제공합니다.
이는 데이터 변경에 대한 응답으로 비동기식 또는 시간이 많이 소요되는 조작을 수행하려는 경우에 가장 유용합니다.

```
<div id="watch">
    <p>
        아무거나 입력하십셔 :
        <input v-model="text">
    </p>
    <p>"{{ text }}"</p>
</div>
```

```javascript
var vm = new Vue({
    el: '#watch',
    data: {
        text: '입력하라우 어서!'
    },
    watch: {
        text: (newValue, oldValue) => {
            console.log('watch result => ', newValue);
        }
    }
});
```

예제는 아래와 같이 나옵니다.
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/watch_example.png)

# 참고한 자료
[computed와 watch](https://kr.vuejs.org/v2/guide/computed.html)