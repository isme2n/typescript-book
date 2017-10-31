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
이런 경우는 타입 추론에 대한 동기부여가 잘 된다. 예를들어 이 예제의 경우, 우리는 `foo`가 `number` 인지 `string`인지 확신할 수 없다. 이런 이슈가 거대한 멀티파일 코드에서 자주 발생한다. 타입추론에 대한 규칙은 다음에 더 자세히 살펴보자.

### Types can be Explicit
As we've mentioned before, TypeScript will infer as much as it can safely, however you can use annotations to:
1. Help along the compiler, and more importantly document stuff for the next developer who has to read your code (that might be future you!).
1. Enforce that what the compiler sees, is what you thought it should see. That is your understanding of the code matches an algorithmic analysis of the code (done by the compiler).

TypeScript uses postfix type annotations popular in other *optionally* annotated languages (e.g. ActionScript and F#).

```ts
var foo: number = 123;
```
So if you do something wrong the compiler will error e.g.:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

We will discuss all the details of all the annotation syntax supported by TypeScript in a later chapter.

### Types are structural
In some languages (specifically nominally typed ones) static typing results in unnecessary ceremony because even though *you know* that the code will work fine the language semantics force you to copy stuff around. This is why stuff like [automapper for C#](http://automapper.org/) is *vital* for C#. In TypeScript because we really want it to be easy for JavaScript developers with a minimum cognitive overload, types are *structural*. This means that *duck typing* is a first class language construct. Consider the following example. The function `iTakePoint2D` will accept anything that contains all the things (`x` and `y`) it expects:

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

### Type errors do not prevent JavaScript emit
To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript *will emit valid JavaScript* the best that it can. e.g.

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

will emit the following js:

```ts
var foo = 123;
foo = '456';
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and yet another reason to move to TypeScript.

### Types can be ambient
A major design goal of TypeScript was to make it possible for you to safely and easily use existing JavaScript libraries in TypeScript. TypeScript does this by means of *declaration*. TypeScript provides you with a sliding scale of how much or how little effort you want to put in your declarations, the more effort you put the more type safety + code intelligence you get. Note that definitions for most of the popular JavaScript libraries have already been written for you by the [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped) so for most purposes either:

1. The definition file already exists.
1. Or at the very least, you have a vast list of well reviewed TypeScript declaration templates already available

As a quick example of how you would author your own declaration file, consider a trivial example of [jquery](https://jquery.com/). By default (as is to be expected of good JS code) TypeScript expects you to declare (i.e. use `var` somewhere) before you use a variable
```ts
$('.awesome').show(); // Error: cannot find name `$`
```
As a quick fix *you can tell TypeScript* that there is indeed something called `$`:
```ts
declare var $:any;
$('.awesome').show(); // Okay!
```
If you want you can build on this basic definition and provide more information to help protect you from errors:
```ts
declare var $:{
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

We will discuss the details of creating TypeScript definitions for existing JavaScript in detail later once you know more about TypeScript (e.g. stuff like `interface` and the `any`).

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
