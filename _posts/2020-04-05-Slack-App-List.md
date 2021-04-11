---
layout: post
title: 'Slack User App 리스트 추출'
author: Jaedo Aum
date: 2021-04-05 10:22:18
tags: [slack]
---

## 로그 추출 히스토리

11번가 Slack Workspace의 `유저 생성 앱`과 `Incoming-webhook`, [`Jenkins-CI`](https://11st-pf.slack.com/apps/A0F7VRFKN-jenkins-ci)와 같은 `사용자 지정 통합 앱` 의 리스트를 추출하여 분석함으로서, 사용자 계정이 비활성 처리될 때 문제가 될만한 앱을 찾아보고자 하였다. 

슬랙의 앱 관리 WEB-UI는 검색, Export 되지 않으므로 수백개의 설정이 존재하는 Incoming-Webhook의 리스트만 확인하려 해도 수십 페이지에 걸친 정보를 취합하는 반복 작업이 필요하기에 엄두가 나지 않는다. 

Slack 측에 추출방법에 대해 문의해 보니 아래와 같은 답변을 들을 수 있었다. 

> You can call the team.integrationLogs ([https://api.slack.com/methods/team.integrationLogs](https://api.slack.com/methods/team.integrationLogs)) API to get the integration activity logs for a team and then filter that by user by passing the user ID as the user value. You will need to navigate through the results as they go back all the way to the start of your workspace, but hopefully this would be of use.


[team.integrationLogs](https://api.slack.com/methods/team.integrationLogs) API를 통해 확인해보니, 앱 리스트는 6158건이 나왔다. 이것은 생성/삭제/업데이트 내역이 모두 카운트 된 횟수로서 숫자 자체로서의 의미는 없다. 

추출된 리스트에서 의미있는 값만 필요했으므로, 단순하게 리스트를 종류별로 추출해 보는 코드를 작성하였다. 

## 로그 분석을 위한 로그추출 버전

```python
import requests
import json, time

# document: https://api.slack.com/methods/team.integrationLogs
#USER_TOKEN = ""  # Basic Auth - User Token

def callSlackTeamIntegratinLogs(page):
    url = "https://slack.com/api/team.integrationLogs?count=100&page=" + str(page)
    headers = {
        "Accept": "application/json", 
        "Authorization": "Bearer " + USER_TOKEN 
    }

    response = requests.request(
        "GET",
        url,
        headers=headers
    )

    return response.text

response = json.loads(callSlackTeamIntegratinLogs(1))

logs = response.get("logs")
paging = response.get("paging")

total = paging.get("total")
pages = int(paging.get("pages"))
app_list = []
service_list = []

for log in logs:

    if log.get("service_type"):
        print("[Service]==>", log)

    elif log.get("app_type"):
        print("[APP]==>", log)
    else:
        print("other==>", log)

print(json.dumps(app_list, indent=4)) # json beautify print

```

## CSV 추출을 위한 메인 버전

```python
import requests
import json, time

# document: https://api.slack.com/methods/team.integrationLogs
#USER_TOKEN = ""  # Basic Auth - User Token

def callSlackTeamIntegratinLogs(page):
    url = "https://slack.com/api/team.integrationLogs?count=100&page=" + str(page)
    headers = {
        "Accept": "application/json", 
        "Authorization": "Bearer " + USER_TOKEN 
    }

    response = requests.request(
        "GET",
        url,
        headers=headers
    )

    return response.text

app_list = []
service_list = []
pcnt = 1
while True:
    # break
    if(pcnt > pages):
        break

    response = json.loads(callSlackTeamIntegratinLogs(pcnt))

    if pcnt == 1:
        paging = response.get("paging")

        total = paging.get("total")
        pages = int(paging.get("pages"))

        print(paging, total, pages)

    logs = response.get("logs")

    for log in logs:
        app_list.append(log)

    print(len(app_list), len(service_list))

    # keys = app_list[0].keys()
    keys = ['app_id','app_type','service_id','service_type','channel','user_id', 'user_name', 'date', 'change_type', 'reason', 'scope', 'rss_feed_url', 'rss_feed', 'rss_feed_change_type', 'rss_feed_title']
    
    with open('slack-app-list.csv', 'w', newline='') as cf:
        dict_writer = csv.DictWriter(cf, keys)
        dict_writer.writeheader()
        dict_writer.writerows(app_list)

    pcnt += 1
    time.sleep(5)   # rate limit 2 이므로 5초씩 쉰다. 그냥 추출하면, 1500건 추출된 후 retelimit

```

## 참고 

User APP의 Incoming-Webhook 설정화면
![slack app functionality](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/71d122b6-e03d-4480-9178-11c9ddbf48c8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210407%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210407T021716Z&X-Amz-Expires=86400&X-Amz-Signature=911e16690bb02c54aab5e78412e89e086d83263d19cb33f15a282c6fa85e11dc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
