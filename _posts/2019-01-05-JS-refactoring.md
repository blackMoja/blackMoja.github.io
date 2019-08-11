---
title: 리펙토링 및 고도화 후기
date: 2019-01-05
---
## 2달에 걸친 프로젝트
드디어 첫 번째 대형 프로젝트를 마무리 했다!!
그동안 진행한 프로젝트들은 대부분 기간이 짧았거나 신규 개발하는데에 있어서 많은 시간과 리소스를 잡아먹진 않았지만
지금까지 사용한 구조가 아닌 새로운 구조로 다시 구조변경을하고 팀의 코딩가이드를 다시 새롭게 적용시키는 과정에서 시간이 오래 걸린것 같다.
첫 번째 대형 프로젝트 겸 리펙토링을 회고하고자 한다.

<!-- more -->

## 구조변경 및 리펙토링을 하게 된 계기
### 첫번째 이유는 기존 로직은 유지보수에 대한 내용이 제대로 반영되지 않은 로직이다.
리펙토링 및 구조변경 전 로직 구조는 유지보수에 대한 내용이 들어가 있지 않았고,
Ajax 모듈이나 format 모듈 그리고 기타 validation 모듈 등...
기본적으로 모든 페이지에서 사용하는 로직들이 공통으로 빠져 있지 않았다.
모든 페이지에서 위에 문제가 된 내용이 해당되어 있어서 참으로 난감했다.
그리고 팀 코딩 룰이 적용되어있지 않은 로직이라서 팀원들 또한 유지보수 하기 난감하는 의견이 많아서
리펙토링을 진행하게 되었다.

### 두번째 이유는 데이터의 불필요한 하드코딩에 대한 처리다.
분명 서버개발자분은 공통 리스트를 가져오는 API를 개발해서 주셧지만 해당 API를 사용하지 않는 상태였다.
난감했다 :( 있는데 안쓰다니... 혹시 몰라서 서버 개발자 분과 확인을 해보니 무조건 사용해야 했던 API였다.
후다닥 적용시키고 보니 눈에 보이는 이슈가 하나 더 발견됬다.
데이터에 따라 DOM을 생성해줘야하는데 이 동적 DOM을 hbs에 하드코딩으로 만들어 놓고 event로 show hide 시키고 있었다...
데이터에 따라 생성하는게 나중에도 편하다고 판단해서 나는 눈물을 머금고 동적으로 DOM을 생성하도록 구조 변경을 진행했다.

### 세번째 이유는 자바스크립트 파일에 대한 기능 정의였다.
정말 많은 공통파일 및 필요없는 라이브러리 파일들이 있었다. 한 15개....?
물론 내가 사용하는 jQuery, bxslider, jqueryUI등 필요한 라이브러리도 있었지만
불필요한 로직을 포함하고 애초에 사용조차 안하는 파일들이 많이 있었다.
페이지 진입시 필요없는 파일들까지 가져오니 페이지 최초 init시 리소스 낭비가 되는 문제도 있었다.
리소스가 쓸데 없이 낭비되고, 충분히 공통모듈에 있는 내용을 라이브러리를 가져다 쓴다는게 딱히 필요가 없다고 느껴졌다.

### 네번째 이유는 렌더링 방식이다.
기존에는 hbs 핸들바를 사용한 서버사이드 렌더링을 이용해 개발이 되어 있었다.
하지만 지금 다시보니 핸들바에 대한기능은 사용하지 않고... 단순 html용도로만 사용되고있었다.
핸들바에 기능을 사용하지 않고 무의미하게 서버사이드 렌더링은 오히려 서버 메모리만 쓸데없이 낭비될 것 같고
이 부분에 관하여도 쓸모없이 레이아웃 용도로만 사용하는 부분이면 차라리 서버사이드 렌더링이 필요 없다고 판단해서
hbs에서 html 구조로 변경 작업을 진행 하였다.