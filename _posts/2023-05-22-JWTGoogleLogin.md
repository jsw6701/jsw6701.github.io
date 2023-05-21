---
title: "[Spring] JWT 구글 로그인 토큰 유효성 검사 에러"
excerpt: "JWT Google Login 디버그 지옥 코드 뜯어보기"

categories:
  - Backend
tags:
  - [backend, Error, spring, JWT, GoogleLogin]

permalink: /backend/JWT

toc: true
toc_sticky: true
 
date: 2023-05-22
last_modified_at: 2023-05-22
---

약 2주동안 골 때린 디버그 지옥에서 돌아온 글…

## JWT 로그인 토큰 ID 유효성 검사

## 임무

먼저 내가 했던 일은 지금 안드로이드에서 토큰 ID 값을 받고 그 토큰 ID 값을 유효성 검증하여 accesstoken, refreshtoken을 주는 것이 임무였다.

## 사건 발발

하지만 안드로이드에서 토큰 ID 값을 주었음에도 불구하고  이게 정말 내 문제가 맞나 싶을 정도로 코드에는 문제가 없었고 세부적인 코드 직접 다 들어가서 디버그를 찍었음에도 원인을 알 수 없었다.

~~정확하겐 원인은 알겠는데 이게 내 문제가 아니다! 이거지…~~

## 해결 과정

### AuthService

![1](https://jsw6701.github.io/assets/images/posts_img/230522/1.png)

디버그의 흔적이 보이겠지만 지금은 성공한 이후라… 다시 안되는 포인트를 돌리고 찍기엔 귀찮으니 그냥…ㅎㅎ

1. verifier에 들어가는 토큰 ID는 정상이였다.
2. setAudience도 정상적으로 설정해 둔 구글 클라이언트 아이디로 들어갔다.
3. ***verify 함수를 사용하여 검증을 했는데 돌아온 googleIdToken 값이 null 값이였다.***

정말 무슨 일이야…이게 싶은 왜냐하면 유효성 검증도 개인키로 사이트에서 다 검증해봤고 유효 시간도 남아있었고 **토큰에는 문제가 없는데 verify가 문제가 있다고 판별**한다.

내부로 들어가보자

### GoogleTokenVerifier

클래스 내부에 verify 함수를 보게 되면 아니라고 판단되면 null값을 보내는 것을 알 수 있다.

![2](https://jsw6701.github.io/assets/images/posts_img/230522/2.png)

boolean을 반환하는 verify를 보게 되면 첫 번째 디버그 건 곳에 verifyPayload에서 false가 리턴되는 것 같았다.

![3](https://jsw6701.github.io/assets/images/posts_img/230522/3.png)

더 들어가 보자

### IdTokenVerifier

여기서 사실 디버그를 총 3군데 찍었었는데 첫번째 디버그 건 issuers에만 통과됐다고 뜨고 두 세번 째 줄에서는 X 표시가 떴다. 하지만 issures도 제대로 통과한 것이 아닌 거 같아서 확인해봤다.

![4](https://jsw6701.github.io/assets/images/posts_img/230522/4.png)

### IdToken

![5](https://jsw6701.github.io/assets/images/posts_img/230522/5.png)

문제의 근원지인지도 모르는 근원지를 찾았다.

getIssuer를 통하여 가져온 그 값이 정확히는 기억이 나지 않지만 “accounts.google.com” 으로 왔을 것이다.

하지만 버전 업그레이드 이후 현재는 토큰의 페이로드에 “https://accounts.google.com”이 들어오는 것이라고 들어서 서로 맞지 않아 결국엔 null값이 리턴되는 것 같다.

## 해결책

해결책은 돌고 돌아 버전 업그레이드였다…

원래는 google-api-client-jackson2 버전을 1.20.0을 사용하고 있었다.

이것을 최신 버전인 2.2.0을 바꿔주었다.

```java
implementation group: 'com.google.api-client', name: 'google-api-client-jackson2', version: '2.2.0'
```

## 해결

![6](https://jsw6701.github.io/assets/images/posts_img/230522/6.png)
