---
title: rxjs 개념 정리
date: 2019-10-03
categories:
    - rxjs
tags:
    - rxjs
---

회사 프로젝트를 진행하면서 상태관리 관련 라이브러리 vuex를 사용하지 않고
rxjs로 대신 그 역할을 하기위해 사용하고 있는데 간단한 개념도 잊어버리기 쉬운 라이브러리라
천천히 정리해볼려구 한다.

<!--more-->

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/RxJS.gif)

## rxjs
**RxJS는 Observable를 사용하여 비동기 및 이벤트 기반 프로그램을 작성하기 위한 라이브러리이다.**

RxJS는 상태전파를 위해 **리액티브 프로그래밍** 기법과 로직 오류 방지를 위해 **함수형 프로그래밍** 기법을 함께 사용하고 있으며,

Observer 패턴, Interater 패턴 그리고 위에 작성한 함수형 프로그래밍을 제공해 준다.

프로젝트 진행하면서 사용한 키워드 4가지에 대해서 정리해볼려구 한다.
 - Observable
 - Observer
 - Subscription
 - Subject 

하나하나 차례차례 살펴보자.

### 1.Observable
**Observable**은 시간을 인덱스로 둔 컬렉션을 추상화한 클래스이다.

이 클래스는 동기나 비동기의 동작 방식으로 전달된 데이터를 하나의 컬렉션으로 바라볼 수 있게 해준다.

또한 관찰자(Observer)에게 데이터를 전달해 주는 역할을 한다.

```typescript
import { Observable } from 'rxjs';

// 1. Observable 생성
const observable = new Observable(
  (subscriber: any) => {
      subscriber.next(1);
        setTimeout(() => {
            subscriber.next(4);
            subscriber.complete();
        },
    1000
    );
});
```

### 2.Observer
**Observer**는 Observable에 의해 전달된 데이터를 소비하는 주체이다.

크게 3가지 객체를 가지게 된다.
 - next : Observable에 의해 데이터가 전달 되는 상황.
 - error : Observable에 의해 에러를 전달한다. next, complete는 발생하지 않는다.
 - complete : Observable에 의해 성공을 전달한다. next 이벤트는 발생하지 않는다.

Observer는 Observable과 **subscription**메소드를 통해 연결된다.

``` typescript
const observer = {
    next: (resp: any) => {
        console.log('Observer가 Observable로부터 받은 데이터여 : ', resp);
    },
    error: (error: any) => {
        console.log('Observer가 error를 줬어요 : ', error);
    },
    complete: () => {
        console.log('Observer가 종료가 되었어용');
    }
}
```

### 3.Subscription
Subscription객체는 자원의 해제를 담당한다.

등록된 Observable의 데이터를 더이상 전달받고 싶지 않을 경우 unsubscribe 메소드를 호출하여 자원을 해제한다.
``` typescript
const subscription = currentTarget$.subscribe(observer);

// subscription으로 자원 해제가 가능
subscription.unsubscribe();
```

### 4.Subject
RxJS의 Observable은 옵서버 패턴의 Subject와 닮았지만 조금은 다르다.

하지만 상태를 Observer에게 전달하는 관점에서 둘은 비슷하다.

Subject는 여러 Observable과 데이터를 공유한다. 

뿐만 아니라 Subject는 Observable과 다르게 새로운 상태를 전달 할 수 있다.

Observable이 읽기 전용(read-only)라면 Subject는 읽기 쓰기(write-read)가 가능한 Obseravble이다.

~~~ typescript
const { Subject } = rxjs;
const subject = new Subject();

// observerA를 등록
subject.subscribe(
    {
        next: (resp: any) => {
            console.log(`observerA: ${resp}`);
        }
    }
)
// 데이터 1을 전달
subject.next(1);

// observerB를 등록
subject.subscribe(
    {
        next: (resp: any) => {
            console.log(`obserberB: ${resp}`);
        }
    }
)
// 데이터 2를 전달
subject.next(2);

// 결과
// "observerA: 1"
// "observerA: 2"
// "observerB: 2"
~~~
위에 예시를 통해 알아낸 것이 있다.
1. subject는 next를 통해 구독하고 있는 observer에게 전달할 수 있다.
2. 데이터를 모든 observer와 공유한다.
3. subject를 구독하는 시점에 따라 observer가 받는 데이터의 내용은 다를 수 있다.

이러한 이유는 Subject가 Observable을 상속하고 Observer가 구현되어 있기 때문에 가능하다.
**Subject는 Observable하기도 하면서 동시에 Observer이다.**

## 마무리 
오늘은 RxJS를 사용하면서 중요하다고 생각하는 개념을 정리해 보았다.
사실 RxJS는 더 많은 옵션들을 가지고 있는데 그 옵션들은 아직 내가 실무를 통해
사용해본 적이 없어서 우선은 이렇게 4개만 정리하고자 한다.

또 다른 RxJS 이벤트나 메소드를 사용한다면 또 다시 정리하도록 하겠다. :)

## 참고
**Quick Start RxJS**라는 책을 참고하여 중요한 내용을 정리하였다.