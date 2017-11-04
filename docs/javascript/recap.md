# 당신의 자바스크립트는 타입스크립트다

지금부터 아주많은 문법들이 자바스크립트 컴파일러와 비교가 될 것이다. 타입스크립트는 문법적인 차이와는 다른 이야기로 자바스크립트는 타입스크립트이다. 쉽게 다이어그램을 보자.

![](https://raw.githubusercontent.com/basarat/typescript-book/master/images/venn.png)

그러니까 이 다이어그램의 의미는 당신이 타입스크립트를 배우기 위해 자바스크립트를 배워야 한다는 것이다(좋은 점은 **오직** 자바스크립트만 배우면 된다는 것이다). 타입스크립트는 자바스크립트에 대한 모든 훌륭한 문서를 표준화하는 것 뿐이다.

* 그저 새로운 구문을 제공할뿐 버그를 고쳐주진 않는다 (커피스크립트처럼).
* 새로운 추상언어를 만든다는 것은 커뮤니티와 개인에게 많은 차이가있다 (다트를 보라).

타입스크립트는 그냥 자바스크립트와 문서이다.

## 자바스크립트를 더 좋게 만들자

타입스크립트는 당신을 결코 제대로 작동하지 않는 자바스크립트 부분으로부터 보호할 것이다. 그래서 이런걸 기억할 필요가 없다.

```ts
[] + []; // JavaScript will give you "" (which makes little sense), TypeScript will error

//
// other things that are nonsensical in JavaScript
// - don't give a runtime error (making debugging hard)
// - but TypeScript will give a compile time error (making debugging unnecessary)
//
{} + []; // JS : 0, TS Error
[] + {}; // JS : "[object Object]", TS Error
{} + {}; // JS : NaN or [object Object][object Object] depending upon browser, TS Error
"hello" - 1; // JS : NaN, TS Error

function add(a,b) {
  return
    a + b; // JS : undefined, TS Error 'unreachable code detected'
}
```

본질적으로 타입스크립트는 자바스크립트를 린팅하는데, 타입정보가 없는 다른 린터들보다 더 낫다.

## 여전히 자바스크립트를 배워야 할 필요가 있다

타입스크립트는 자바스크립트를 작성하기에 매우 실용적이다. 그래서 부주의를 막기위해 자바스크립트에 대해 알아야 할 것들이 있다. 그것들에 대해 다음번에 논의해 보자.

> 노트: 타입스크립트는 자바스크립트의 수퍼셋이다. 컴파일러와 IDE에서 실제로 사용되는 문서가 포함된 ;)
