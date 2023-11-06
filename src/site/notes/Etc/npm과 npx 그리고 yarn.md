---
{"tags":["Etc"],"dg-publish":true,"dg-hide":true,"permalink":"/etc/npm-npx-yarn/","hide":true,"dgPassFrontmatter":true,"noteIcon":""}
---


# npm
NPM은 Node Package Manager의 줄임말로, Node.js의 기본 패키지 관리자이다.
npm은 명령 줄 클라이언트(우리가 `npm ~` 으로 명령을 실행 시킬 수 있는)와 `npm registry`로 구성된다.

다들 처음 npm을 사용할 때 `npm install ...` 명령어로 시작하여 필요한 모듈을 다운로드 받을 것이다. 이 모듈이 저장되어 있는 장소가 [`npm registry`](https://www.npmjs.com/)다. 

npm은 기본적으로 다음과 같은 기능을 제공한다.
- 라이브러리 설치 및 관리
- 라이브러리의 의존성(dependency) 관리
- 라이브러리의 업데이트 및 삭제
- 라이브러리의 버전 확인
- 라이브러리의 배포

> **의존성(dependency)**
> 의존성이란 한 모듈에서 다른 모듈의 기능을 사용할 때의 관계를 말한다.
> 예를 들어 A모듈에서 B모듈의 기능을 사용한다면 A는 B에 종속되어있다, 의존적이다 라고 한다. 이 의존성은 낮을 수록 좋다.

## npm이 주는 의의
만약 npm이 없다면 우리는 다른 사람이 만든 라이브러리를 이용하기 위해 github등 저장소로 직접 들어가 코드를 내려받거나, 제공하는 CDN을 통해 html에 직접 삽입해줘야 할 것이다.

또한 라이브러리가 만약 업데이트 된다면, 다시 일일히 찾아가 업데이트된 코드를 내려받아야 한다. 혹시라도 그 라이브러리가 다른 라이브러리를 의존하고 있다면 그 의존하는 라이브러리도 찾아가서 업데이트를 반복 해줘야 한다...


# npm init
`npm init` 혹은 `npm install [모듈 이름]`을 하고나면 `package.json`, `package-lock.json`, `node_modules` 폴더가 생긴다.

## package.json
`package.json`은 프로젝트의 이름, 버전, 설명, 종속된 라이브러리 등의 정보를 담은 파일이다. 이 파일을 통하여 협업에서 프로젝트의 정보를 일관되게 공유할 수 있다.
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My first Node.js project",
  "dependencies": {
    "express": "^4.17.1"
  },
  "scripts": {
    "start": "node server.js",
    "test": "jest"
  },
  "license": "MIT"
}

```

- name: 프로젝트 이름
- version: 프로젝트 버전
- description: 프로젝트의 간략한 설명
- dependencies: 프로젝트를 실행하기 위해 필요한 라이브러리 목록
- scripts: 프로젝트를 실행하거나 테스트하는데 사용되는 명령어
- license: 프로젝트를 사용하는데 적용되는 라이선스

## package-lock.json
`npm install`을 실행하면 `package.json`에 명시되어 있는 버전으로 설치 된다. 정확히는 명시되어 있는 버전 위로, 메이저 버전 아래로 설치가 된다.

예시로 다음과 같이 의존 라이브러리가 적혀 있다고 보자.
```json
"dependencies": {
  "express": "^4.17.1"
},
```
이는 express 라이브러리를 설치하는데, `4.17.1` 버전 위로, `5.0.0` 버전 아래로 설치하라는 뜻이다.
즉, `npm install` 시점에 express 라이브러리의 버전이 `4.20.0`으로 업데이트 됐다면, 그 버전을 설치하게 된다. 이는 `package.json` 파일을 사용해도 설치 시점에 따라 서로 다른 버전이 설치될 수 있다는 말이다.

그래서 `npm i`후 보면 `package-lock.json` 만들어져 있다. 이는 설치 시점에 상관없이 명시되어 있는 버전으로만 설치할 수 있게 해준다.

즉, 협업을 할 경우 이 `package-lock.json`까지 공유를 해야 모두가 같은 버전의 라이브러리를 사용하여 오류를 방지할 수 있을 것이다.

## node_modules
`npm install`후 생기는 이 폴더는 라이브러리와 그 라이브러리가 의존성으로 가지는 라이브러리를 저장하는 폴더다. 그래서 많은 라이브러리를 설치하게 될 경우 점점 커지게 된다.
사용자는 `npm install`을 통해 언제든 이 폴더를 다시 생성할 수 있으므로 공유할 필요가 없다. 따라서 이 폴더는 깃허브등에 올릴시 공유하지 않도록 주의해야 한다.

## npm 명령어 목록
- **npm init** : 프로젝트를 생성
- **npm init -y** : 기본적인 package.json 파일을 생성
- **npm install -g** : 전역으로 라이브러리를 설치
- **npm uninstsall -g** : 전역으로 라이브러리를 제거
- **npm list -g** : 설치된 전역 라이브러리를 확인
- **npm publish** : 라이브러리를 npm registry에 배포
- **npm run [script-name]** : 지정된 스크립트를 실행
- **npm run list** : 프로젝트의 스크립트를 확인
- **npm help** : 명령어에 대한 도움말을 표시

```bash
# 라이브러리 설치
npm install express
# 라이브러리 업데이트
npm update express
# 라이브러리 제거
npm uninstall express
# 프로젝트 생성 
npm init 
# 전역 라이브러리 설치
npm install -g express 
# 전역 라이브러리 제거 
npm uninstall -g express 
# 라이브러리 배포 
npm publish 
# 스크립트 실행 
npm run start 
# 명령어 도움말 
npm help install
```

# npx
![Pasted image 20231101123822.png](/img/user/Pasted%20image%2020231101123822.png)
create-react-app 공식 홈페이지에는 `npm install`이 아닌 `npx`로 설치할 것을 권장하고 있다. 

`npx`는 `npm` 버전 5.2부터 같이 설치되어 있는 **명령어**이다. `npm`이 패키지를 설치하고 삭제하는 관리로서의 명령어 라면 **`npx`는 패키지를 실행하는데 사용되는 명령어다.**

예를 들어 `create-react-app`의 경우는 처음 한번 설치 할 때만 필요하다. 따라서 굳이 `npm install`로 설치하여 남겨둘 필요가 없다. 또 버전이 바뀔 경우, 설치 전에 항상 확인을 해줘야 한다.

이럴 경우 `npx` 명령어를 사용하면 편리하다.
`npx` 명령어는 다음과 같은 순서를 따른다.

`npx create-react-app`을 했을 경우
1. 로컬 패키지 체크: 현재 프로젝트의 node_modules에서 해당 패키지가 설치 되었는지 검색한다.
2. 전역 패키지 체크: 전역에 설치되어 있는지 확인한다.
3. 원격 패키지 다운로드: 패키지를 npm cahe에 저장하고, 바로 실행한다.

![Pasted image 20231101130023.png| 300](/img/user/Pasted%20image%2020231101130023.png)

# yarn
`npm`처럼 패키지를 관리할 수 있는 또다른 패키지 관리 도구이다. 페이스북에서 개발하였으며, `npm`보다 속도, 안정성, 유연성 등에 더 나은 점을 내세우며 등장했다.

## yarn.lock
`npm`이 `package-lock.json`를 생성했다면, `yarn`은 `yarn.lock`을 생성한다. 이를 통해 버전의 통일성을 지켜준다.

## yarn 명령어 목록
- **yarn init** : 프로젝트 생성 
- **yarn or yarn intall** : `package.json`에 있는 종속성 설치 
- **yarn global add** : 전역으로 라이브러리를 설치
- **yarn global remove** : 전역으로 라이브러리를 제거
- **yarn global list** : 설치된 전역 라이브러리를 확인
- **yarn publish** : 라이브러리를 npm registry에 배포
- **yarn run [script-name]** : 지정된 스크립트를 실행
- **yarn help** : 명령어에 대한 도움말을 표시


# yarn과 npm 무엇을 써야할까?
yarn이 npm보다 속도, 보안등 다방면에서 개선되어 나왔다고 하지만 npm도 계속 발전 해왔기에 일반적인 수준에서는 둘 중 무엇을 써도 상관없을 것 같다.

하지만 한 프로젝트에서 yarn을 쓰려고 했다면 앞으로 계속 yarn만, npm을 쓰기로 했다면 npm만 쓰는 것이 중요하다.