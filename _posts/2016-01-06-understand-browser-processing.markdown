---
layout: post
title:  "브라우저 동작방식"
date:   2016-01-06 17:11:33 +0900
categories: browser http study
---
우리가 흔히 아는 HTTP 통신의 기본 동작방식은 클라이언트가 서버에게 어떤형태로든 Request 
를 하게되면 그에 상응하는 Response 를 받아 처리 해 주는 방식이다.

OSI계층에서 Application 계층 이하의 내용은 원론적인 부분을 학부시절에 공부하여 별 
문제없이 이해는 하였지만, 정작 컴파일러 수업을 듣지않아(프로젝트로 웹 브라우저 만들기 
또한 가능하다고 했었습니다) 웹 브라우저가 HTML,CSS,JS를 어떻게 파싱하고 렌더링 하는지 
정확한 개념이 없었다.

  
![웹브라우저동작방식](https://lh3.googleusercontent.com/4rL64XKIe8n1axUbnqetQQALH6JQt23XhjbF5HFIal0oZocMVzFmo-Lef8BG5OnDzSwsK98tyab5Kmd-yUyHRlVqOiuY8D--Tmz7a-KONaLqPkv0TkPF4H_qVVlRWLoBvjl3TRc)
(출처 : <http://link.kut.ac.kr/>)

## 렌더링 엔진
> 요청받은 내용을 브라우저 화면에 표시합니다. HTML / XML 문자 및 이미지를 표시할 수 
> 있습니다.<br>
> Firefox: Gecko 엔진<br>
> Chrome, Safari: Webkit 엔진 (Chrome은 webkit, blink엔진) 

### 동작과정
![렌더트리 과정](https://lh4.googleusercontent.com/pGAyJ6ONKwYcQaFKU8rujuaylCnPR3ZL3KM5SLHPzttGXI41_4twlwMrds7lEyWpnQms5jUAHXVFkj6v1pWNl5AWmx51dNoKQydRDURIbLx8muFyPRijOy9tuOhqITq2RJgJl0I)

(출처 : <http://d2.naver.com/helloworld/59361> )

데이터를 받아 문서를 파싱하고, 트리 내부에서 태그를 DOM(Document Object Model) 노드로 
변환합니다. 그 후 CSS를 파싱하고, 렌더 트리라는 것을 생성한 후 그리기 작업을 진행하여 
사용자가 볼 수 있게 한다.

웹 브라우저에서 쓰이는 것이 인터프리터이기 때문에 모든 HTML이 파싱될 때 까지 기다리지 
않으며, 줄 별로 파싱하여 해당 되는 부분을 파싱하게 된다. 네트워크로부터 받아야 할 나머지
내용들이 전송되기를 기다리는 것 과 동시에 일부 내용을 화면에 표시하는 것이다.

하지만 HTML은 기본적으로 동기적 처리를 하고, 스크립트가 실행 될 때는 파싱이 중지된다.
(HTML5의 defer, async 속성으로 동시처리 가능)

파싱 중 스크립트가 CSS의 정보 요청 또한 가능하며 CSS가 처리중 일 때는 관련 스크립트는
중지하게 된다.


이에 우리가 흔히 아는 지식으로, CSS 로딩은 문서 상단에, 스크립트 로딩은 문서 하단부 
(```</body>``` 윗 영역)에 두는 것이 그 이유이다.

## 파싱과 DOM 트리 구축
> 문서 파싱은 브라우저가 코드를 이해하고 사용할 수 있는 구조로 변환하는 것을 의미한다.
> 파싱 후 노드 트리를 그리게 되는데 이것을 파싱 트리 또는 문법 트리라고 불린다.

![HTML 파싱과정](http://d2.naver.com/content/images/2015/06/helloworld-59361-9.png)

(출처 : <http://d2.naver.com/helloworld/59361> )

## Reflow & Repaint
### Reflow 발생
> 생성 된 DOM 노드의 레이아웃에 대한 값 변경 시 영향을 받는 모든 노드의 값을 다시 계산하여
> 렌더 트리를 재생성 하는 과정, 또한 Reflow 과정이 끝난 후 재생성 된 렌더트리를 다시
> 그리게 되는데 이것을 Repaint 라고 한다.


참고 문서<br>
[Reflow 원인과 마크업 최적화](https://lists.w3.org/Archives/Public/public-html-ig-ko/2011Sep/att-0031/Reflow_____________________________Tip.pdf)

### Repaint
> 모든 스타일 변경이 레이아웃 값에 영향을 받는 것은 아니다. background-color, visibility,
> outline 등 스타일 변경 시에는 레이아웃 값은 변하지 않기 때문에 Reflow 과정이 생략되고
> Repaint 만 일어나게 된다.

### Reflow 최적화 방법
1. 스타일에 대해 1개 이상의 속성과 값에 대한 변경이 있다면 class 로 변화를 주자
2. class 변화에 대한 타겟을 작은 단계의 DOM에 반영하자
3. 애니메이션을 시킬 때에는 ```position:absolute , position:fixed``` 속성을 주어 전체
	노드에서 분리를 시키자.
4. IE만의 표현식을 쓰지말자. (expression ...)
5. DOM 사용을 최소화 하여 Reflow 비용을 줄이자.
6. CSS 선택자는 최소한 줄이고 3개 이상 넘기지 말자.