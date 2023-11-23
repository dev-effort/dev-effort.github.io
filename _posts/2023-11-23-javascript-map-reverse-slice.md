---
layout: single
title: javascript map()을 역순으로. reverse(), slice()
categories: javascript
tag: [react, front-end, javascript]
typora-root-url: ../
author_profile: false
sidebar:
  nav: "counts"
---

## map을 역순으로 순회하는 방법

간혹 map()을 역순으로 반복했으면 하는 순간이 있다.

우선 이러한 요구사항이 생겼을 때, 실제 어떠한 방법이 있는지 바로 찾아보기보다는 스스로 어떠한 방식으로 그 방법이 존재할 지 생각해보는 훈련을 해보는 것이 좋다.

다음과 같은 경우에는 두가지 정도가 떠올랐다.

1. 객체를 역순으로 재정렬(반전)
2. map()에서 index가 역순으로 동작하는 옵션 제공

찾아보면 2번 방법은 제공되지 않는 방법이고 1번 방법으로 할 수 있는 api가 제공된다.

## reverse()

Array.prototype.reverse()를 사용하면 순서가 반전된 배열이 반환된다.

주의 사항으로는 **원본 배열이 변형된다는 점**이다.

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

```javascript
const sample = ['one', 'two', 'three'];
const result = sample.reverse().map(num => num);
console.log(result);
// output: Array ["three", "two", "one"]

console.log(sample);
// output: Array ["three", "two", "one"]
```

## slice()

원본이 손상되는 것은 우리가 원치 않는 경우가 많다. 그리고 권장되지도 않는다. 사이드이펙트를 야기하는 경우가 많기때문이다.

그렇다면 원본을 복사해서 복사본을 reverse()하면 되겠다. 이때 slice()를 사용하면 쉽게 배열을 복사가 가능하다.

slice() 메서드는 어떤 배열의 begin부터 end까지(end 미포함)에 대한 **얕은 복사본을 새로운 배열 객체로 반환**하고 **원본 배열은 바뀌지 않는다.**

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

```javascript
const sample = ['one', 'two', 'three'];
const result = sample.slice(0).reverse().map(num => num);
console.log(result);
// output: Array ["three", "two", "one"]

console.log(sample);
// output: Array ['one', 'two', 'three']
```

