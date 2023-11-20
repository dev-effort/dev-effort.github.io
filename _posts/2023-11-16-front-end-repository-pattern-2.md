---
layout: single
title: 프론트엔드에 repository pattern 적용기 - 2
categories: architecture
tag: [react, front-end, architecture]
typora-root-url: ../
author_profile: false
sidebar:
  nav: "counts"
---

## Repository pattern 실제 적용

### 1. Repository의 구분과 repositoryContainer.ts

<img src="/images/2023-11-16-front-end-repository-pattern-2/image-20231116224623166.png" alt="image-20231116224623166" style="zoom:150%;" />

우선 src 하위에 repositories라는 폴더가 존재한다. repositories에는 다양한 repository 폴더들이 있고, repository를 구분짓는 기준(단위)는 리소스 단위로 했다. REST API를 기준으로 생각했을 땐, controller 단위로 repository를 만들었다.



<img src="/images/2023-11-16-front-end-repository-pattern-2/image-20231116225142064.png" alt="image-20231116225142064" style="zoom:50%;" />

예시를 들어보자면 job-controller는 jobRepository와 1대1 대응이 된다.

파일 repositoryContainer.ts를 살펴보면 다음과 같다.

```typescript
// repositoryContainer.ts

import 'reflect-metadata';

import { Container } from 'inversify';
import { IAuthRepository } from './authRepository/IAuthRepository';
import { TAuthRepository } from './authRepository/impl/TAuthRepository';
import { AuthRepository } from './authRepository/impl/AuthRepository';
import { TThreadRepository } from './threadRepository/impl/TThreadRepository';
import { IThreadRepository } from './threadRepository/IThreadRepository';

const SERVICE_KEYS = {
  AUTH_REPOSITORY: Symbol('AUTH_REPOSITORY'),
  THREAD_REPOSITORY: Symbol('THREAD_REPOSITORY'),
};

const _container = new Container();

_container.bind(SERVICE_KEYS.AUTH_REPOSITORY).to(TAuthRepository).inSingletonScope();
// _container.bind(SERVICE_KEYS.AUTH_REPOSITORY).to(AuthRepository).inSingletonScope();
_container.bind(SERVICE_KEYS.THREAD_REPOSITORY).to(TThreadRepository).inSingletonScope();

export const authRepo = _container.get<IAuthRepository>(SERVICE_KEYS.AUTH_REPOSITORY);
export const roomRepo = _container.get<IThreadRepository>(SERVICE_KEYS.THREAD_REPOSITORY);
```

Inversify를 이용해서 IoC 컨테이너를 생성하고 컨테이너에 각 repository들의 구현체를 등록한다.

그리고 각 repo들을 반환하는데 각 레포지토리들의 인터페이스가 반환되고 실제 동작은 바인딩 되어있는 구현체로 동작한다. 이로써 실제 repo들을 가져다 사용하는 곳에서는 인터페이스로 통하고 있기 때문에 어떤 구현체인지 알 필요가 없고 구현체 내용이 바뀌더라도 사용하는 쪽의 코드가 바뀔 필요도 없게 된다.  

AuthRepository는 실제 서버와 통신하는 구현체가 작성되어있고 TAuthRepository는 mock데이터로 작성된 구현체이다. 서버가 구성되지 않았음에도 TAuthRepository를 통해 개발을 할 수 있고, 추후에 AuthRepository를 만들어 교체하면 된다.

아래 코드는 authRepo를 가져다 사용하는 코드인데 구현체가 무엇인지 알 필요도 없고, 바뀌더라도 코드를 변경할 필요가 없다는 것을 알 수 있다.

```typescript
import { useMutation } from '@tanstack/react-query';
import { LoginRqData } from '../repositories/authRepository/types';
import { LoginModel } from '../repositories/authRepository/models/LoginModel';
import { authRepo } from '../repositories/repositoryContainer';

export const useLogin = () => {
  type RqData = {
    rqData: LoginRqData;
  };

  return useMutation({
    mutationFn: async (data: RqData) => {
      const resultFromServer = await authRepo.login(data.rqData);
      return new LoginModel(resultFromServer);
    },
  });
};

```



### 2. Repository

<img src="/images/2023-11-16-front-end-repository-pattern-2/image-20231116231647737.png" alt="image-20231116231647737" style="zoom:150%;" />

각 repository는 interface와 타입이 정의 되어있는 types.ts 파일이 있고 implementation 폴더와 models 폴더가 있다.

impl 폴더에는 interface의 실제 구현체가 존재한다. (models는 repository와 상관은 없는 부분인데 해당 프로젝트 구조 상 해당 경로에 있으면 좋겠다고 판단하여 둔 것이므로 models는 언급하지 않겠다.)

impl에는 AuthRepository와 TAuthRepository.ts가 있다. 

AuthRepository는 실제 구현체이고

TAuthRepository는 Mock 데이터로 구현된 Test 구현체이다.

각 구현체들은 interface를 구현해야한다.

```typescript
// IAuthRepository.ts
import { Login, LoginRqData } from "./types";

export interface IAuthRepository {
  login(rqData: LoginRqData): Promise<Login>;
}

```



### 3. interface 구현체, Faker.js를 이용한 mocking

```typescript
// TAuthRepository.ts
import { injectable } from 'inversify';
import { IAuthRepository } from '../IAuthRepository';
import { LoginRqData, Login } from '../types';
import { faker } from '@faker-js/faker';

// typescript에서 @ annotation을 사용할 수 있도록 설정 필요.
@injectable() 
export class TAuthRepository implements IAuthRepository {
  async login(rqData: LoginRqData): Promise<Login> {
    const mockData: Promise<Login> = new Promise(resolve => {
      resolve({
        id: faker.string.uuid(),
        status: faker.datatype.boolean(),
      });
    });

    const result = await mockData;

    return result;
  }
}
```

IAuthRepository를 implements를 통해 구현한다. 서버에서 데이터를 가져오는 것이기 때문에 async를 붙여준다. msw나 json-server와 같이 mock데이터를 만들어두고 마치 서버에서 가지고 오는 것과 같이 동작하는 라이브러리가 존재하지만 개인적으로 원하는 동작이 아니여서 해당 라이브러리를 이용하기 보다는 promise와 faker.js를 이용하였다.

faker.js를 통해 해당 타입에 맞는 랜덤한 값을 넘겨줄 수 있기때문에 json으로 mock데이터를 만들 필요가 없고, 다양한 케이스의 데이터가 들어오기때문에 더 다이나믹한 테스트를 할 수 있다는 장점이 있었다.

faker.js를 사용하면서 좋았던 점은 다양한 도메인의 데이터를 선택할 수 있다는 점이다. 

```typescript
faker.airline.flightNumber();
faker.company.name();
faker.phone.number();
```

다음과 같이 항공번호를 넘겨주기도 하고, 회사이름을 랜덤으로 주기도 하고, 핸드폰 번호도 랜덤으로 얻을 수 있다.



## Repository pattern을 적용해본 후의 회고

어떻게 해야될까 고민이 많았고 오랜 시간 고민을 했었다. 다행히 원하는 방향대로 구현을 할 수 있었고 잘 동작하여 뿌듯하였다. 

우선 백엔드와 병목이 없어서 좋았다. 지금 새롭게 시작된 프로젝트에 적용해보았을때, 백엔드의 API 구현 여부와 상관없이 API의 컨트롤러 명이나 인터페이스만 대충 맞춘다면, 테스트 구현체를 만들어서 직접 적용해 볼 수 있었고, API가 완성 되었을때도 구현체를 만들고 IoC 컨테이너에 구현체만 교체해주면 되기때문에 컴포넌트 단과 데이터를 불러오는 영역이 확실히 나뉘었음을 실감하였다.

또한 faker.js를 이용한 테스트가 생각보다 만족도가 높았다. Mock 데이터 파일을 직접 만들때면 이걸 왜 만들고 있어야하지? 하는 답답함이 항상 있었고, 명확하게 되는 데이터만으로 mock 데이터를 만들기때문에 이게 테스트로서의 목적이 전혀 없다고 생각되었기 때문이다.

하지만 faker.js를 통해 만들어낸 랜덤한 값은 올바른 타입으로 오지만 그 값이 랜덤으로 오기때문에 화면에서 고려해야할 다양한 케이스에 대해서 고려할 수 있어서 좋았다.
