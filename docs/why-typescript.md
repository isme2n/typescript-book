# 왜 타입스크립트를 사용하는가
타입스크립트는 두개의 주요 목표가 있다:
* 자바스크립트용의 *타입시스템*을 제공한다.
* 미래의 자바스크립트 에디션에 제공 될 기능들을 현재의 자바스크립트 엔진에서 제공한다.

이런 목표들에 대한 열망은 다음과 같다.

## 타입스크립트 타입 시스템

"**왜 자바스크립트에 타입을 추가해야하지?**"라고 의문을 가질 수 있다.

타입은 코드의 퀄리티를 향상시키고 이해하기 쉽게만든다. 구글,마이크로소프트,페이스북같은 거대한 팀은 이런 문제를 지속적으로 만나게 되었다. 특히:

* 타입은 리팩토링시에 민첩성을 향상시킨다. *컴파일러가 에러를 잡아내는게 런타임중에 실패하는 것보다 낫다*.
* 타입은 최고의 문서중에 하나이다. *함수의 시그니쳐는 하나의 정리이고 함수는 증명인 것이다*.

어찌되었든 타입은 불필요하게 격식을 차리는 방식이다. 타입스크립트는 진입장벽을 가능한 낮게 유지하는것을 중요하다:

### 자바스크립트는 타입스크립트다
타입스크립트는 컴파일시 자바스크립트코드에 타입 안전성을 제공한다. 타입스크립트라는 이름이 주어진 이유기도하다. 엄청난 것은 그 타입들이 완전히 선택적이라는 것이다. `.js` 파일을 `.ts`로 바꿀 수 있고 타입스크립트는 이를 원래 파일처럼 유효한 `.js` 파일을 반환한다. 타입스크립트는 *쓰고싶을때 쓸 수 있는* 엄격한 자바스크립트 타입체킹을 위한 수퍼셋이다.

### 타입은 드러나지 않을 수 있다
타입스크립트는 개발시에 생산성을 높이기 위해 제대로 된 타입안전성을 제공하려고 타입정보를 추론한다. 예를들어, 아래 예제에서 타입스크립트는 foo가 `number` 타입이라는 것을 알기 때문에 두번째 줄에서 에러가 날 것이다. 

```ts
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```
이런 경우는 타입 추론에 대한 동기부여가 잘 된다. 예를들어 이 예제의 경우, 우리는 `foo`가 `number` 인지 `string`인지 확신할 수 없다. 이런 이슈가 거대한 멀티파일 코드에서 자주 발생한다. 타입추론에 대한 규칙은 추후에 더 자세히 살펴보자.

### 타입은 명시적이어야 한다
이전에 이야기했듯, 타입스크립트는 최대한 안전하게 타입을 추론하지만, 어노테이션을 사용해서 추론할 수도 있다:
1. 컴파일러를 돕고 다음 개발자가 당신의 코드를 읽을때(아마 미래의 `나`일 것이다).
1. 컴파일러가 보는 것을 당신이 생각한 것과 맞춘다. 코드에 대한 이해는 컴파일러가 하는 알고리즘 분석과 비슷하다.

타입스크립트는 ActionScript와 F# 같은 다른 유명한 *선택적* 어노테이션 언어처럼 후위 타입 어노테이션을 사용한다.

```ts
var foo: number = 123;
```
예를 들어 아래식처럼 사용한다면 컴파일러는 오류를 뿜어낼 것이다:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

타입스크립트에서 지원하는 모든 어노테이션구문은 추후에 더 자세히 다루어 보자.

### 타입은 구조다
몇몇 언어들의(특히 명목상 타입언어들의) 정적 타입지정은 불필요한 과정을 만든다. 코드가 잘 작동하는 것을 알고 있더라도 언어의 의미가 타입을 복사하게 만들기 때문이다. 이게 [C#의 automapper](http://automapper.org/)가 왜 C#에 *치명적인가*의 이유이다. 타입스크립트는 자바스크립트개발자가 최소한의 오버로드만으로 쉽게사용하길 바라기 때문에 *구조적*이다. 타입스크립트는 *덕타이핑*을 사용하며 덕타이핑은 최고의 언어구조이다. 예제를 보며 생각해보자. 함수 `iTakePoint2D`는 `x` 와 `y`를 포함하는 모든 것을 받아들일 것이다:

```ts
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### 타입에러는 자바스크립트의 실행을 막지않는다
자바스크립트 코드를 타입스크립트로 마이그레이션하는 것을 쉽게하기위해 타입스크립트는 에러가 있더라도, 가능한 최선을 다해 *유효한 자바스크립트*로 바꾸어 준다. 예를들어:

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

will emit the following js:

```ts
var foo = 123;
foo = '456';
```

이로인해 우리는 점진적으로 자바스크립트코드를 타입스크립트로 업그레이드 할 수 있다. 이런 부분이 다른 많은 언어의 컴파일러와 가장 큰 차이라고 할 수 있다. 또한 타입스크립트로 옮겨가는 또 하나의 이유이기도 하다.

### 타입은 고요하다
타입스크립트의 주요 설계 이슈는 개발자가 안전하고 쉽게 자바스크립트라이브러리를 타입스크립트에 사용하게 만드는 것이었다. 타입스크립트는 이를 *선언*을 이용해 구현했다. 타입스크립트는 선언문에 얼마나 많은 노력을 기울이는지에 따른 경사각을 제공한다. 더 많은 노력을 기울여서 작성한 선언문은 더 뛰어난 타입의 안전성과 코드 인텔리전스를 얻을 수 있다. 대부분의 인기있는 자바스크립트 라이브러리의 정의는 이미 [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped)에 의해 작성되어있다:

1. 정의파일이 이미 존재한다.
1. 또는 최소한의 잘 검토된 타입스크립트 선언 템플릿이 이미 있다.

어떻게 선언 파일을 직접 작성하는지 간단한 예제를 통해 보자.[jquery](https://jquery.com/) 예제이다. 기본적으로 좋은 JS코드 기대되는대로 타입스크립트는 변수를 사용하기 전에 어디선가 `var`를 선언할 것이라고 기대한다. 
```ts
$('.awesome').show(); // Error: cannot find name `$`
```
*타입스크립트라고 부를 수 있는* 빠른 해결책은 실제 `$`를 부르는 것이다:
```ts
declare var $:any;
$('.awesome').show(); // Okay!
```
만약 에러로부터 스스로 보호하기 위고 싶다면, 이 기본적인 선언과 더불어 다양한 정보를 제공하면 된다:
```ts
declare var $:{
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

이미 존재하는 자바스크립트를 타입스크립트 선언 만들기는 `interface` 나 `any`등과 더불어 타입스크립트에 대해 조금 더 알게 되었을 때 더 자세히 다루어 보자.

## Future JavaScript => Now
TypeScript provides a number of features that are planned in ES6 for current JavaScript engines (that only support ES5 etc). The typescript team is actively adding these features and this list is only going to get bigger over time and we will cover this in its own section. But just as a specimen here is an example of a class:

```ts
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```

and the lovely fat arrow function:

```ts
var inc = (x)=>x+1;
```

### Summary
In this section we have provided you with the motivation and design goals of TypeScript. With this out of the way we can dig into the nitty gritty details of TypeScript.

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
