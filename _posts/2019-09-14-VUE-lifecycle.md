---
title: Vue 라이프사이클
date: 2019-08-26
categories:
    - Vue
tags:
    - Vue
---

Vue를 사용하면서 나의 기준에서 중요하다고 생각하는 라이프사이클 메소드를 정리해볼려구 한다.

<!--more-->
# VUE 라이프사이클

## 라이프사이클 구조
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/lifecycle.png)

라이프사이클 구조는 위 다이어그램처럼 이루어져 있다.
내 기준에서 중요하다고 생각하는 **4가지**를 적어봤다.
모든 라이프사이클 훅은 자동적으로 this를 인스턴스에 바인딩 함으로
화살표 함수가 부모 컨텍스트를 바인딩 하기 때문에 this는 예상대로 Vue 인스턴스가 아니게 됩니다.

#### 컨텍스트?
간단하게 말하자면 **자바스크립트의 실행하게 해주는 환경** 이다.

- created
- mounted
- updated
- destroyed

### created
인스턴스가 작성된 후 동기적으로 호출된다.

이 실행 단계에서 인스턴스는 데이터 처리, 계산된 속성, 메서드, 감시/이벤트 콜백 등과 같은 옵션 처리를 진행한다.
마운트가 시작되지 않았으므로 "$el" 속성을 아직 사용할 수 없습니다.

### mounted
인스턴스가 마운트 된 직후 호출됩니다.

mounted는 모든 자식 컴포넌트가 마운트 된 상태를 보장하지 않습니다. 
mounted 내부에서 vm.$nextTick를 사용하면 전체가 렌더링된 상태를 보장합니다.
```javascript
mounted: function () {
    // 렌더링 완료를 보장합니다.
    this.$nextTick(function() {
        console.log('마운트 완료');
    });
}
```
**이 라이프사이클 메소드와 "beforeMounted"메소드는 서버사이드렌더링을 지원하지 않는다.**

### updated
데이터가 변경되어 가상 DOM이 재 렌더링되고 패치되면 호출되는 메소드이다.

엘리먼트의 DOM이 업데이트 된 상태가 되어 이 훅에서 DOM 종속적인 연산(템플릿 연산)을 할 수 있습니다.
그러나 대부분의 경우 무한루프가 발생할 수 있으므로 훅에서 **상태**를 변경하면 안됩니다.
상태 변화에 반응하기 위해서 계산된 속성 또는 Watch를 사용하는 것이 더 효율적이다.

updated는 모든 자식 컴포넌트가 재-렌더링 된 상태를 보장하지 않는다.
따라서 $nextTick를 사용하면 모든 컴포넌트(자식)이 업데이트 된 상태를 보장한다.
```javascript
updated: function () {
  this.$nextTick(function () {
    // 리 렌더링 된 후 코드 실행
    console.log('리 렌더링 완료');
  })
}
```

**SSR 중 호출되지 않습니다**

#### 가상 돔?
가상 돔은 직접 돔에 접근하는 방식이 아닌 메모리에 돔에 대한 변경 내용은 담아두고
그 결과 차이난 부분의 돔만 수정하는 동작을 한다.
중요한 내용이니 다음 블로그에서 포스팅 해야겠다 :)

### destroyed
인스턴스가 제거된 후 호출된다.

이 훅이 호출되면 Vue 인스턴스의 모든 디렉티브가 바인딩 해제 되며
모든 이벤트 리스너가 제거되며 모든 하위 Vue 인스턴스도 삭제된다.
**rxjs를 사용한다면 이 구간에서 unsubscrible()을 사용하여 구독 해제를 해준다.**

# 마치며
오늘은 뷰 JS 공식 사이트에도 정리가 잘 되어 있지만 나름 대로 정리하며 다시 개념들을 확인하였다.
다음 블로그 포스팅에는 가상 돔을 포스팅하여 기본적인 내용을 좀더 정리할려구 한다.

더 자세한 API doc는 아래 참고한 자료에 추가해놨으니 정확한 설명이 좀더 필요하면 참고하자!

# 참고한 자료
[VueJs-Api](https://kr.vuejs.org/v2/api/)