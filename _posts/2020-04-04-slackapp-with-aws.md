---
layout: post
title: 'AWS를 활용한 Slack 공개 App 제작 가이드'
author: 유광엽
date: 2021-04-04 13:57
tags: [aws, slack]
---

```
> 목표: Slack의 Slash command 기능을 가진 추첨 App을 만들어서 공개 런칭해보기

> 필요사항: Slack 및 AWS 계정

> 사용 Spec: Python3, Slack API, AWS Lambda, API Gateway, S3, CloudFront, Dynamodb

> 참조문서

Slack App 디렉터리: https://[회사slack주소]/apps
Slack App 런칭 공식 가이드 문서: https://api.slack.com/start/distributing/guidelines
Slack App 런칭 공식 체크리스트 문서: https://api.slack.com/reference/slack-apps/directory-submission-checklist#new-slack-app-directory-checklist__app-submission-checklist__help-us-review-your-app
Slack API 공식 문서: https://api.slack.com/
```
---


#### 목차 - App 런칭 방법 ( https://api.slack.com/tutorials/submitting-apps-to-the-directory )

* 1.추첨 Slack APP 만들기  
    * 1-1. 새로운 App 만들기  
    * 1-2. 새로운 App 공개배포 활성화 시키기  
    * 1-3. App제출 진행하기  
   
* 2.추첨서비스 구축하기  
    * 2-1. 사전준비  
    * 2-2. Slack 슬래시커맨드 호출 흐름도  
        * 2-2-1. 공통 Lambda 생성  
        * 2-2-2. 추첨서비스 Lambda 생성  
        * 2-2-3. API Gateway 생성  
    * 2-3. 트러블슈팅 사항  
* 3.소개용 랜딩페이지 구축하기  
    * 3-1. S3 버킷 생성하기  
    * 3-2. CloudFront 배포하기  
* 4.OAuth 인증 서버 구성하기  
    * 4-1. AWS RDS 구성 시 참고사항  
    * 4-2. AWS Dynamodb 구성 시 참고사항  
* 5.앱 제출 후 Slack팀 Review 처리내용  


***