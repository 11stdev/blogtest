---
layout: post
title: 'TEST 개념잡기'
author: Jaedo Aum
date: 2019-08-01 12:00
tags: [graphql]
---

GraphQL은 페이스북에서 만든 쿼리 언어입니다. GrpahQL은 요즘 개발자들 사이에서 자주 입에 오르내리고 있으나, 2019년 7월 기준으로 얼리스테이지(early-stage)임은 분명합니다. 국내에서 GraphQL API를 Open API로 공개한 곳은 드뭅니다. 또한, 해외의 경우, Github 사례([Github v4 GraphQL](https://developer.github.com/v4/))를 찾을 수는 있지만, 전반적으로 GraphQL API를 Open API로 공개한 곳은 많지 않습니다. 하지만 등장한지 얼마되지 않았음에도 불구하고, GraphQL의 인기는 매우 가파르게 올라가고 있다는 사실을 확인 할 수 있습니다.

![상승중인 GraphQL 인기 (출처: https://2018.stateofjs.com/data-layer/graphql/)](/files/graphql-popularity.png)

## GraphQL 이란?

Graph QL(이하 gql)은 Structed Query Language(이하 sql)와 마찬가지로 쿼리 언어입니다. 하지만 gql과 sql의 언어적 구조 차이는 매우 큽니다. 또한 gql과 sql이 실전에서 쓰이는 방식의 차이도 매우 큽니다. gql과 sql의 언어적 구조 차이가 활용 측면에서의 차이를 가져왔습니다. 이 둘은 애초에 탄생 시기도 다르고 배경도 다릅니다. sql은 **데이터베이스 시스템**에 저장된 데이터를 효율적으로 가져오는 것이 목적이고, gql은 **웹 클라이언트**가 데이터를 서버로 부터 효율적으로 가져오는 것이 목적입니다. sql의 문장(statement)은 주로 백앤드 시스템에서 작성하고 호출 하는 반면, gql의 문장은 주로 클라이언트 시스템에서 작성하고 호출 합니다.


```javascript
    requestPaymentSession: async (parent, { 
      pgId, name, sex, birthDay, phoneNumber, amount, productName, ref 
    }, context, info) => {
      const ret = await requestPaymentSession({ pgId, name, birthDay, phoneNumber, sex, amount, productName, ref })

      return removeSymbol(ret)
    },
    requestPaymentApprove: async (parent, {
      sessionKey, authNumber
    }, context, info) => {
      const ret = await requestApprovePayment({ sessionKey, authNumber })

      return removeSymbol(ret)
    }
```
*실제 구현한 비지니스 로직 관련 리졸버*

## 정리

gql은 퍼포먼스적인 장점이 분명 존재합니다. 하지만 개인적으로 더 관심이 가는 장점은 바로 생산성 향상입니다. gql은 기존 백앤드-프론트앤드 협업 문화를 많이 바꿀것으로 예상합니다. gql의 협업 구조상 프론트앤드쪽에 조금 더 할일이 많아지고 힘이 실리는 느낌입니다. 에자일하게 웹사이트 프로젝트를 진행하는데 gql이 많은 도움이 될 것이라고 생각합니다.

개발자 여러분에게 안타까운 소식을 전하자면, 이 글을 읽었다고 해서 gql을 바로 실전에서 사용하기는 쉽지 않을 것입니다. 이 글에서는 gql의 클라이언트 모듈에 대해서는 구체적으로 언급하지도 않았습니다. 특히 react를 혼합해서 사용하려면, 또 다시 가파른 고개를 넘어서야 제대로 사용 할 수 있을 것입니다. 요즘 react는 flux 아키텍쳐가 점령 했습니다. flux 아키텍쳐는 아름답지만, flux와 함께 gql을 구현하는 일은 또 다른 숙제가 될 것입니다. 개인적인 생각으로는 apollo client의 [local statment management](https://www.apollographql.com/docs/react/essentials/local-state) 기능은 flux 아키텍쳐를 구체화하여 구현한 redux를 완전히 대처 가능하다고 생각합니다. 추후에 기회가 된다면 react와 apollo 조합에 대해서 이야기 해볼까 합니다. 항상 공부하는 개발자의 모습은 아름답습니다. 이 세상의 모든 개발자 화이팅입니다.

참고 사이트 
* https://graphql.org

> 이 글은 핀플레이 결제 프로젝트를 진행시 Graphql을 도입하며 얻은 경험을 바탕으로 작성하였습니다.