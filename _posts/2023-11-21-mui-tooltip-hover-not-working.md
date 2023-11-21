---
layout: single
title: Mui tooltip hover not working
categories: issue-solution
tag: [react, front-end, Mui]
typora-root-url: ../
author_profile: false
sidebar:
  nav: "counts"
---

## 문제점

mui를 사용하다가 Tooltip 컴포넌트를 사용했는데 tooltip으로 감싼 컴포넌트를 hover시 나와야하는 tooltip이 나오지 않는 현상을 겪었다.

코드는 다음과 같았다.

```typescript
<Tooltip title={error.name.message} placement="bottom-start" arrow>
  <Textfield
    name="name"
    value={user.name}
    type="text"
    err={error.name.err}
    onChange={e => handleTable.changeUserRow(e, user.id)}
  />
</Tooltip>
```



## 원인

사용법에 특별한 이상은 보이지 않아서 혼란스러웠는데 stackoverflow에서 다음과 같은 답을 얻었다.

**The child of a Material-UI Tooltip must be able to hold a ref.**

mui의 tooltip의 자식요소는 ref를 유지할 수 있어야한다고 한다.

ref를 유지할 수 있는 컴포넌트의 종류는 다음과 같다.

- *Any Material-UI component*
- *class components i.e. React.Component or React.PureComponent*
- *DOM (or host) components e.g. div or button*
- *React.forwardRef components*
- *React.lazy components*
- *React.memo components*



## 해결법

ref를 유지할 수 없는 tooltip의 자식 컴포넌트를 div태그로 감싸주는 것이다.

```typescript
<Tooltip title={error.name.message} placement="bottom-start" arrow>
  <div>
    <Textfield
      name="name"
      value={user.name}
      type="text"
      err={error.name.err}
      onChange={e => handleTable.changeUserRow(e, user.id)}
    />
  </div>
</Tooltip>
```



### 참조

[https://stackoverflow.com/questions/57527896/material-ui-tooltip-doesnt-display-on-custom-component-despite-spreading-props](https://stackoverflow.com/questions/57527896/material-ui-tooltip-doesnt-display-on-custom-component-despite-spreading-props)