기존 바닐라 js + jquery + fullpageJS로 된 랜딩 페이지를 리액트로 리팩토링 한다.

CRA환경 대신 yarn berry를 사용한 pure react project로 작업해볼 것.

https://kasterra.github.io/setting-yarn-berry/
https://mmsesang.tistory.com/entry/Yarn-berry-yarn-pnp-%ED%99%98%EA%B2%BD%EC%9C%BC%EB%A1%9C-React-Typescript-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%84%B8%ED%8C%85%ED%95%98%EA%B8%B0
https://toss.tech/article/node-modules-and-yarn-berry
참고하는 url

prerequisite)
Yarn Berry의 PNP(Plug n Play)

node_modules의 불편함(소스 코드 확인 밑 인스톨, 빌드 시간의 증가)
를 해결하기 위해서 모든 코드를 다운받는 node_modules 대신
.yarn/cache에 dependency가 압축 파일 형태로 저장,
.pnp.cjs에 해당 위치 정보가 저장됨.

### 1. 기존 package.json 구조를 벗어나는 yarn berry react 만들기

```
rm -rf node_modules
rm -rf package.lock.json

// 이후

yarn set version latest

```

의 커맨드로

```
C:.
│  .yarnrc.yml
│  package.json
│  README.md
│
└─.yarn
    └─releases
            yarn-3.2.3.cjs
```

구조가 만들어짐./

ps) powershell에서 

```
tree /F 로 위 구조 만들어줌.
```

ps2) cjs는 commonJS 모듈 파일.

이후 yarn install로 .pnp.cjs 만들어준다.

### 2. 타입스크립트 플러그인 설치

```
yarn plugin import typescript
```

타입스크립트 플러그인을 Import 해줍니다. 이 플러그인은 자체 types를 포함하지 않는 패키지를 추가할 때 @types/ 패키지를 package.json폴더에 종속성으로 자동으로 추가해줍니다. 설치 하지 않아도 상관 없지만 설치하면 편하고 유용하기 때문에 설치해줍니다.

플러그인이므로 zip 파일 생성 안됨.

vs code extension ZipFS 설치

ps) astro, error lens도 설치

이후

```
yarn dlx @yarnpkg/sdks vscode
```

으로 berry dependency 세팅.




### CRA없이 프로젝트 시작

https://blog.bitsrc.io/create-react-app-without-create-react-app-b0a5806a92

#### webpack 환경설치

```
yarn add -D webpack webpack-cli webpack-dev-server
```

으로 설치해주면 berry 환경이므로 package.json은 생기지만, node_modules 대신

.yarn/cache에 추가되는 것을 볼 수 있음.

webpack — Will allow us to bundle all of our code into a final build
webpack-cli — CLI tool for providing a flexible set of commands for developers to increase speed when setting up a custom webpack project. If you’re using webpack v4 or later and want to call webpack from the command line, you need this
webpack-dev-server — Webpack dev server is a mini Node.js Express server.It uses a library called SockJS to emulate a web socket. Will enable us to create a localhost dev environment

#### babel 설치

```
yarn add -D babel-loader @babel/preset-env @babel/core @babel/plugin-transform-runtime @babel/preset-react @babel/eslint-parser @babel/runtime @babel/cli
```

babel-loader — allows transpiling JavaScript files using babel and webpack. exposes a loader-builder utility that allows users to add custom handling of Babel’s configuration for each file that it processes.
@babel/preset-env — allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s). This both makes your life easier and JavaScript bundles smaller!
@babel/core — core package
@babel/plugin-transform-runtime — A plugin that enables the re-use of Babel’s injected helper code to save on codesize.
@babel/preset-react — use react preset when we are using Reactjs. Helps in converting html files to react based file
babel-eslint — parser that allows ESLint to run on source code that is transformed by Babel
@babel/runtime — package that contains a polyfill and many other things that Babel can reference.
@babel/cli — command line interface to use babel

바벨 설치

#### react 설치

```
yarn add react react-dom
```

#### eslint 설치

```
yarn add -D eslint eslint-config-airbnb-base eslint-plugin-jest eslint-config-prettier path
```

#### react-testing-library 설치

https://www.daleseo.com/react-testing-library/

jest와 묶어서 설치함.

```

yarn add -D @testing-library/react jest @testing-library/jest-dom
```

#### public/index.html 추가

https://blog.bitsrc.io/create-react-app-without-create-react-app-b0a5806a92

에 맞춰서 설치해준다.

다만 ts 관련 세팅이 없으므로 

https://webpack.js.org/guides/typescript/

에서 타입스크립트 세팅 해준다.

```
yarn add -D typescript ts-loader
```

https://github.com/Microsoft/TypeScriptSamples/tree/master/react-flux-babel-karma

을 참고해서 webpack-config.js 세팅


## bugs

https://bobbyhadz.com/blog/typescript-cannot-find-module-react

react type declaration 에러



이 내용 해결 안되었음.

아마 webpack 세팅 자체가 잘못된듯함

새롭게 해야할것!