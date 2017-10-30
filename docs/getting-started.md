* [Getting Started with TypeScript](#getting-started-with-typescript)
* [TypeScript Version](#typescript-version)

# 타입스크립트 시작하기

타입스크립트는 자바스크립트로 컴파일된다. 자바스크립트가 실제로 브라우저나 서버에서 실행되는 것이다. 따라서 다음과 같은 것이 필요하다:

* 타입스크립트 컴파일러 (오픈소스로 공개 된 [소스코드](https://github.com/Microsoft/TypeScript/)도 있고 [NPM](https://www.npmjs.com/package/typescript)에 있는걸 사용해도 된다.)
* 타입스크립트 에디터 (메모장을 사용해도 되지만 나는 [alm 🌹](http://alm.tools)을 사용한다. 또한 [다양한 IDE들이 타입스크립트 편집을 지원한다.]( https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support))


![](https://raw.githubusercontent.com/alm-tools/alm-tools.github.io/master/screens/main.png)


## 타입스크립트 버전

우리는 *안정화된* 타입스크립트 컴파일러를 사용하는 대신 아직 버전 숫자를 제공받지 않은 다양한 기능을 이 책에서 사용할 예정이다. 나는 사람들에게 자주 개발버전을 이용하는 것을 추천한다. 왜냐면 시간이 지날수록 테스트 버전이 더 많은 버그를 잡을 수 있기 때문이다.

커맨드 라인에 아래와 같이 타이핑해 타입스크립트를 설치할 수 있다.

```
npm install -g typescript@next
```

이제 커맨드라인에 `tsc`가 가장 최신이 되었다. 다양한 IDE가 이를 지원한다, 예를들어,

* `alm`은 항상 최신의 타입스크립트 버전을 제공한다.
* vscode에서 사용하기 위해서는 `.vscode/settings.json`에 다음과 같이 작성하면 된다:

```json
{
  "typescript.tsdk": "./node_modules/typescript/lib"
}
```

## 소스코드
이 책에서 사용되는 소스코드는 책의 깃 레포지토리인 https://github.com/basarat/typescript-book/tree/master/code 에서 받을 수 있다. 대부분의 코드 샘플들은 alm에 그대로 복사해서 실행해볼 수 있다. 코드에 npm modules같은 추가적인 세팅이 필요할 경우, 아래처럼 링크를 제시할 예정이다.

`this/will/be/the/link/to/the/code.ts`
```ts
// 이야기 중인 코드
```

개발 설정을 마쳤으면 이제 타입스크립트 문법을 배워보자.