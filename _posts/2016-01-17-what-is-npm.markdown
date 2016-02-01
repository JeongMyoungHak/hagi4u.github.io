---
layout: post
title: NPM 으로 package.json 생성하기
date: "2016-01-17 01:16:00 +0900"
categories: study npm package.json
published: true
---


(수정예정)
## package.json?
프로젝트 소스가 모듈에 의존하고 있는경우, 각각의 버전에 따라 문제가 생길 수 있다. 때문에 버전과 관련한 의존성을 해결하기 위해 버전관리를 해 줄 필요가 있습니다.

NPM (Node Peopleckage Manager)은 프로젝트에 대한 설정을 package.json 이라는 파일에 의존하고 있고, 프로젝트에 대한 관리가 가능합니다.

### package.json 예시
![example-package-json.png]({{site.baseurl}}/assets/example-package-json.png)

### 어떻게 만들까요?
손수 JSON파일을 생성해서 사용해도 되지만 저 형식을 만들기 위해서는 외워야 할게 많으니, `npm`명령어를 기준으로 만드는 방법을 알아 보도록 합니다.

```npm init``` 명령을 입력하면 아래 이미지와 같은 대화형 프롬프트를 통해 필요에 부분을 입력하게 되면, 해당 내용이 모두 package.json 파일에 저장됩니다.

!["npm init"](/assets/example-npm-init.png)

위 내용에서는 단순히 프로젝트 정보에 대한 내용만 나오지만, npm을 통한 install 명령 수행 시에는 해당 패키지가 dependencies 영역에 추가됩니다.

### 왜?
일반적으로는 npm install '모듈명'으로 모듈을 설치하지만, 프로젝트의 가장 맨 위 경로에 package.json 파일이 있고, npm install 명령만 입력할 시에는 package.json 에 명시되어 있는 dependencies 곳의 모듈을 모두 설치 해 줍니다. 

이는 github 과 같은 버전관리 시스템이나 협업툴에서 node_modules 과 같은 복잡한구조(폴더 트리수도 많고, 파일도 많음)를 올리지 않고, 다른 개발 환경에서 해당 package.json 파일만 있으면 동일한 환경의 모듈을 설치할 수 있습니다.

### 의존성 표시 방법
> - version (=version) : 해당 버전과 완전히 일치하는 버전
> - \>version : 해당 버전보다 큰 버전
> - \>=version : 해당 버전보다 크거나 같은 버전
> - <version : 해당 버전보다 작은 버전
> - <=version : 해당 버전보다 작거나 같은 버전
> - ~version : 버전의 범위 (~0.5 : 앞 버전과 0.5 버전 사이의 범위를 의미)
