---
layout: single
title: javascript map에서 if문. filter()
categories: javascript
tag: [react, front-end, javascript]
typora-root-url: ../
author_profile: false
sidebar:
  nav: "counts"
---

## map() 안에서 if문

javascript의 기본기가 탄탄하지 않았던 시절의 코드를 보다보면 부끄러운 코드가 많이 있는데 그 중에 하나가 배열을 특정 조건으로 필터링하여 새로운 배열을 반환하기 위해 map()안에서 if문을 사용하는 코드이다.

```javascript
const sample = [{name: "a", age: 22, role: "member", pay: 100},
  {name: "b", age: 25, role: "member", pay: 120},
  {name: "c", age: 30, role: "leader", pay: 130},
];

// JavaScript map if 예시               
const result = sample.map(data => {
  if(data.role !== "leader") return <div>{data.name}</div>;
  }
);
```

주니어들의 코드 리뷰를 해줄때에도 간혹 보이는데, 역시 사람은 같은 실수를 반복하는구나 싶다.



## filter()

이런 경우에는 filter()를 사용하면 된다.

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

filter() 메서드는 주어진 함수의 조건문을 통과하는 모든 요소를 모아 새로운 배열로 반환한다. 원본은 바뀌지 않는다.

```javascript
const sample = [{name: "a", age: 22, role: "member", pay: 100},
  {name: "b", age: 25, role: "member", pay: 120},
  {name: "c", age: 30, role: "leader", pay: 130},
];
               
const result = sample.filter(data => data.role !== "leader")
  .map(data => {return <div>{data.name}</div>;};
);
```

