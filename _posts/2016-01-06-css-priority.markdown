---
layout: post
title:  "CSS 적용 우선순위"
date:   2016-01-06 13:51:33 +0900
categories: css study
---
CSS는 가능한 적용 순서에 대해 고려하여 작성해야합니다. 적용 우선순위를 무시하는 (```!important```) 것과 같은 속성을 사용 할 때에는 규모가 큰 사이트 제작시에 충돌아 발생 할 여지가 있으며, 속도적인 면에서 느릴 수 있으니(요즘은 그렇다고 티날만한 환경은 아니지만) 가급적이면 사용을 자제해야합니다. 

##적용순서(Like Score)
- Tag Selector : 1점
- Class Selector: 10점
- ID Selector : 100점
- Inline Style : 1000점
- ```!important``` : 위 순서 무시 **(사용 권장 X)**
	> 이미 렌더링 된 DOM에서 !important 로 인해 우선순위가 무시되어 Reflow 가 추가적으로 일어날 수 있기 때문

 

##적용순서 계산(Calculate Score)
```li{}```<br>
1점 (1)

```ul  li{}```<br>
2점 (1+1)

```ul li.list{}```<br>
12점 (1+1+10)

```#examples ul.list li.list__elem{}```<br>
122점 (100 + 1 + 10 + 1 + 10)

```.. style=""```<br>
1000 점
  