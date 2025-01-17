---
layout: single
title: 애자일(agile)에서 스토리 포인트란
categories: software-engineering
tag: [software engineering, agile, story point]
typora-root-url: ../
author_profile: false
sidebar:
  nav: "counts"
---

## 서론

팀을 매니징하는데 있어서 우리 팀, 각 팀원들의 역량을 정확히 파악할 줄 알아야 한다. 그 역량을 기반으로 어떠한 일을 할때 업무를 누구에게, 몇명에게 할당 할지, 기간 산정은 어떻게 할지 판단 할 수 있기 때문이다. 하지만 그 수치를 정확하게 파악하기란 쉽지 않다.

일반적으로 이러한 수치로 맨먼스(man month)를 이용해왔다.

man month는 단순하게 한 사람이 한달동안에 할 수 있는 작업의 양을 나타낸다. 하지만 개발자의 능력에 따라 그 작업의 양이 천차만별이기때문에 좀 더 정확하게 하기 위해 개발자의 등급을 세분화하기도 한다. 초급, 중급, 고급으로 분류를 하여 이용하기도 한다. 그럼에도 불구하고 맨머스에 맞는 생산성이 나오지 않기도하고 일정을 맞추지 못하는 경우가 비일비재하다.

애자일 방법론에서는 맨먼스보다 스토리 포인트라는 것을 활용함으로써 좀 더 정확히 개발자의 생산성을 수치화하고 이용한다.



## 스토리 포인트란?

스토리 포인트는 스토리(애자일에서 말하는 스토리, 간단하게 기능단위라고 생각해도 무관함)를 완전히 구현하는데 필요로 하는 전반적인 노력의 추정치를 표현하기 위한 단위이다. 이것은 굉장히 추상적이고 직관적이지만 사람의 경험적인 직관은 생각보다 정확하다는 것을 알아야한다.

다만 이것은 추정치일뿐 피의 맹세가 아니라는 것을 명심해야한다. 때문에 해당 스토리가 몇점의 포인트가 정확할지 엄청난 스트레스를 받으면서 스토리 포인트를 정할 필요도 없고 절대 수정이 불가능한 값이 아니라는 점이다.

스토리 포인트를 만드는 것은 팀 활동이고 팀이 모두 참석해서 여러가지 사항을 고려해서 그 값을 추정하는 것이 원칙이다.

우선 스토리 포인트는 단순한 작업 시간이 아니라는 것을 명심해야한다.

**스토리 포인트의 고려요소**

- 작업 복잡성
- 작업량
- 위험 또는 불확실성

등등 이러한 요소들을 가지고 스토리 포인트의 수치를 정한다. 스토리를 작은 단위로 쪼갤수록 불확실성을 줄일 수 있다.



## 스토리 포인트와 MD(man day)의 차이

MD는 공수를 나타내는 단위로서 한 사람이 특정 기능을 구현하기 위해 얼마나 많은 시간이 필요한지를 나타내는 값이다. 즉, MD는 사람과 시간이라는 두개의 척도로 공수를 표현하는 방식이다.

스토리 포인트는 MD와 다르게 사람과 시간이라는 척도를 배제하고 구현하고자 하는 스토리 그 자체에 온전히 집중한다. 어떤 사용자 스토리를 구현하는데 있어서 얼마나 어려운지를 나타내기 위한 기준이 필요한데 그 기준과 비교하여 얼만큼 상대적인 어려움이 있는지를 표현하는 값이 바로 스토리 포인트인 것이다.

다음 예시를 통해 좀 더 직관적으로 이해할 수 있다.

```
서울과 부산까지의 직선 거리는 약 325.5km. 매우 길다는 것을 알고있다. 왜냐면 1km라는 기준을 알고 있기 때문이다.

서울에서 부산으로 이동하는게 걸리는 시간은 이동수간에 따라 다르다.
```

비행기, 자동차, 기차 등. 이동 수단 별로 걸리는 시간을 MD라고 볼 수 있다.

이동 수단에 관계없이 변하지 않는 값인 325.5km라는 거리가 스토리 포인트라고 할 수 있다.

사용자 스토리를 구현하는데 걸리는 시간은 각 개발자마다 다르겠지만, 사용자 스토리 그 자체에 의존적인 스토리 포인트는 변하지 않는다.



## 스토리 포인트의 장점

스토리 포인트를 활용했을 때 얻을 수 있는 이점은 팀의 현재 움직임과 팀의 향후 일정을 쉽고 빠르게 파악할 수 있고 예측할 수 있으며 계획할 수 있다는 장점이 있다. 개개인이 스프린트 기간동안 처리할 수 있는 스토리 포인트의 평균치, 팀이 한달동안 소화하는 스토리 포인트를 파악하고 있다면 전체적인 스토리가 가지고 있는 스토리 포인트를 통해 기간을 산정 할 수 있기 때문이다.

또한 외부 환경 요소에 덜 의존하기 때문에 보다 객관적인 값을 지속적으로 이용 할 수 있다. 좀 더 쉽게 설명하자면, 스토리 포인트를 이용해서 예측하고 계획하면 스프린트 기간에 발생할 수 있는 너무 많은 회의나 갑자기 발생한 장애, 병가 등의 모든 예외 상황을 전부 수용할 수 있다는 점이다.

기존 MD 방식은 사람과 시간이라는 척도를 이용하기 때문에 철수가 ‘사용자 가입’ 이라는 스토리를 구현하기 위해 1MD가 필요하다고 추정했을 때, 영희는 기존 시스템을 잘 모르고 구현 언어도 익숙하기 않기 때문에 3MD가 필요할 수도 있다. 이처럼 MD는 같은 사용자 스토리라도 사람에 따라 다른 값이 도출되게 된다. 프로젝트에 참여하는 사람에 따라 다른 값이 도출되기 때문에 프로젝트의 규모를 객관적인 값으로 도출하기 어렵고, 개개인에 따라 달리 스케줄링 되기 때문에 예측 및 계획이 불안정하다. 하지만 스토리 포린트에선 사용자 스토리를 구현하는 담당자가 누구든 상관없이 무조건 동일한 값으로 관리된다. 가입 사용자 스토리가 3 스토리 포인트라고 가정하면, 영희가 구현하든 철수가 구현하든 가입 사용자 스토리는 3 스토리 포인트이다. 이러한 특성 때문에 사용자 스토리와 스토리 포인트에 기반해 계획을 전략적으로 수립 할 수 있다.



## 대부분의 잘못된 사용

대부분 추정하는 작업이 어려워서 처음에는 시간을 대입해 스토리 포인트를 추정하고 사용하는 방식을 채택한다. 이를테면 4시간 짜리 업무를 1 스토리 포인트라고 설정해 사용하는 것이다. 그런 방식으로 스토리 포인트를 사용해보고나선 기존에 추정하던 방법과 별 다를게 없다고 생각하며 스토리 포인트를 더 이상 활용하지 않게되는데 이는 매우 잘못된 방식이다.

스토리 포인트가 5점이라고 해서 1점짜리보다 5배의 시간이 걸린다는 의미가 아니고 1점보다 5배 더 복잡하다는 의미로 생각해야한다. 일반적으로 5점짜리가 1점짜리보다 더 많은 시간이 걸릴 가능성이 매우 높기에 이를 시간으로 환산하려 했던 점이 잘못된 경우이다.



## 디버그와 리팩토링에도 스토리 포인트를 적용해야하는가?

디버그와 리팩토링에는 스토리 포인트를 부여하지 않는다.

원래 스토리에서 수행되었어야 할 것들이 기술적 부채를 갚는 행위로 이루어지기 때문에 0 스토리 포인트를 부여한다.

디버그와 리팩토링에 스토리 포인트를 부여하지 않기 때문에 스토리 포인트의 획득 양을 가지고 개개인을 평가하기 위한 도구로 사용하면 안된다. 스토리 포인트는 개개인을 평가하기 위한 도구가 아니며 팀이 스프린트당 잘 나아가고 있는지 보여주기 위한 팀을 위한 수치라는 충분한 공감대가 형성되어야 한다.



### 참조

[https://www.atlassian.com/ko/agile/project-management/estimation](https://www.atlassian.com/ko/agile/project-management/estimation)

[https://engineering.linecorp.com/ko/blog/user-story-point-in-line-pay-team](https://engineering.linecorp.com/ko/blog/user-story-point-in-line-pay-team)

