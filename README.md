# 코드 스플리팅

CRA(Create React App)의 기본 웹팩 설정에 SplitChunks 라는 기능이 적용되어있다.

node_modules에서 불러온 파일, 일정 크기 이상의 파일, 여러 파일 간에 공유된 파일을 자동으로 따로 분리시켜서 캐싱의 효과를 제대로 누릴 수 있게 해 줍니다.

파일을 분리하는 작업을 코드 스플리팅 이라고 한다.

코드 스플리팅은 단순히 효율적인 캐싱 효과를 가진다.

A, B, C로 구성된 싱글 페이지 애플리케이션(SPA)을 개발한다고 가정한다면 A 페이지에 방문할 때 B 페이지와 C 페이지의 컴포넌트 정보는 필요없다.

하지만 리액트 프로젝트에 별도로 설정하지 않으면 A, B, C 컴포넌트에 대한 코드가 모두 한 파일(main)에 저장된다.

애플리케이션의 규모가 처진다면 필요없는 컴포넌트 정보를 불러와 파일 크기가 매우 커진다. 성능도 저하되어 사용자 경험이 안 좋아지고 트래픽도 늘어난다.

이러한 문제를 해결할 방법이 코드 비동기 로딩이다.


## 자바스크립스 함수 비동기 로딩

코드 비동기 로딩을 통해 자바스크립트 함수, 객체, 컴포넌트를 처음부터 불러오지 않고 필요할 때 불러와 사용할 수 있다.

notify.js 파일을 만들고 App.js에서 사용하려고 한다.

```javascript
const onClick = () => {
    // 상단에 import 하지 않고 import() 함수 형태로 사용하면 파일을 따로 분리시켜서 저장한다.
    // 함수가 필요한 지점에 파일을 불러와서 함수를 사용한다.
    import('./notify').then(result => result.default());
  };
```
위와 같이 onClick 함수에 notify를 적용하고 빌드하면 build/static/js에 새로운 파일이 생긴 것을 볼 수 있다.


## React.lazy와 Suspense를 통한 컴포넌트 코드 스플리팅

리액트에 내장된 기능으로 유틸 함수인 React.lazy 와 컴포넌트인 Suspense가 있습니다.

React.lazy와 Suspense 는 state를 따로 선언하지 않고 컴포넌트 코드 스플리팅을 할 수 있다.


**React.lazy**는 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해주는 유틸 함수다.

```javascript
const SplitMe = React.lazy(() => import('./SplieMe'));
```

**Suspense**는 리액트 내장 컴포넌트로 코드 스플리팅된 컴포넌트를 로딩하도록 발동시킬 수 있고, 로딩이 끝나지 않았을 때 보여줄 UI를 설정할 수 있다.

```javascript
import React, { Suspense } from 'react';

(...)
<Suspense fallback={<div>loading...</div>}>
  <SplitMe />
</Suspense>
```

React.lazy 와 Suspense는 서버 사이드 렌더링을 지원하지 않는다.

### Loadable Components

Loadable Components: 코드 스플리팅을 편하게 하도록 도와주는 서드파티 라이브러리

이 라이브러리의 이점은 서버 사이드 렌더링을 지원한다는 점이다.

렌더링 전에 스플리팅된 파일을 미리 불러오는 기능도 있다.

>서버 사이드 렌더링이란... 
>
>웹 서비스의 초기 로딩 속도 개선, 캐싱 및 검색 엔진 최적화를 가능하게 해 주는 기술
>
>서버 사이드 렌더링을 사용하면 웹 서비스 초기 렌더링을 서버가 처리하기 때문에 사용자는 서버의 렌더링한 html 결과물을 그대로 받아와 초기 로딩 속도가 개선된다.

```
$ yarn add @loadable/component
```






