---
title: "[CI/CD] CloudType CI/CD"
excerpt: "Git Action 사용하여 CI/CD 구현하기"

categories:
  - Backend
tags:
  - [backend, spring, Git, CI/CD, CloudType]

permalink: /backend/CloudType/GitAction

toc: true
toc_sticky: true
 
date: 2023-05-29
last_modified_at: 2023-05-29
---

# CI/CD

저번에 이어 CloudType의 Git Action으로 CI/CD 하는 방법을 포스팅하겠다.

![1](https://jsw6701.github.io/assets/images/posts_img/230530/1.png)

CLI 탭의 2번째 코드를 보면 GitHub Action이라고 써져있다.

![2](https://jsw6701.github.io/assets/images/posts_img/230530/2.png)

이 코드를 복사한다.

참고로 이 과정을 진행하기 전에 귀찮아지닌 먼저 커밋했던 내용들은 배포에 다 추가해놓고 진행하길 바란다. 이전 커밋은 적용이 안되는 것 같다.

![3](https://jsw6701.github.io/assets/images/posts_img/230530/3.png)

나 같은 경우엔 Master 브랜치에 배포를 진행해놨다.  develop 브랜치는 개발용 master 브랜치는 배포용이다.

![4](https://jsw6701.github.io/assets/images/posts_img/230530/4.png)

여기 사진에 보면 Add File 버튼이 있다 클릭하여 Create new file을 선택하자.

![5](https://jsw6701.github.io/assets/images/posts_img/230530/5.png)

이제 이런 화면이 나올 것인데 꼭 Name your file…에 .github/workflows/deploy.yml 을 입력해주도록 하자. (물론 나처럼 메인 브랜치가 아닌 곳에 설정했다면 deploy-master.yml 같이 파일명이 되어있다.) 잘 보고 설정하자.

이름은 아까 본 CLI 쪽 Github Actions 바로 아래에 파일 경로와 이름까지 친절히 나와있다.

여기까지 마쳤다면 이제 다시 CloudType쪽에 넘어가서 환경설정을 들어가자.

![6](https://jsw6701.github.io/assets/images/posts_img/230530/6.png)

계정설정 → 인증으로 넘어가자

![7](https://jsw6701.github.io/assets/images/posts_img/230530/7.png)

![8](https://jsw6701.github.io/assets/images/posts_img/230530/8.png)

여기 새로운 API키 생성을 하고 나온 API키는 꼭 저장을 해주도록 하자. 다시 보기 불가능하다.

![9](https://jsw6701.github.io/assets/images/posts_img/230530/9.png)

다시 프로젝트로 넘어와서 settings로 가자.

![10](https://jsw6701.github.io/assets/images/posts_img/230530/10.png)

여기 보이는 Secrets and variables 누르고 Actions로 들어가자.

![11](https://jsw6701.github.io/assets/images/posts_img/230530/11.png)

New repository secret 클릭

전에 봤던 deploy.yml 코드를 살펴보면 우리가 무엇을 추가해야하는지 알 수 있다.

![12](https://jsw6701.github.io/assets/images/posts_img/230530/12.png)

![13](https://jsw6701.github.io/assets/images/posts_img/230530/13.png)

여기에 보이는 token쪽에 ${{ secrets.CLOUDTYPE_TOKEN}} 보면 secret. 을 제거한 CLOUDTYPE.TOKEN이 이름이고 값은 아까 복사한 API 키값을 넣으면 된다.

넣었다면 다음은 깃허브 설정으로 가자.

![14](https://jsw6701.github.io/assets/images/posts_img/230530/14.png)

settings로 들어가자.

![15](https://jsw6701.github.io/assets/images/posts_img/230530/15.png)

Developer settings 입장

![16](https://jsw6701.github.io/assets/images/posts_img/230530/16.png)

![17](https://jsw6701.github.io/assets/images/posts_img/230530/17.png)

형광펜 친대로 클릭하면 아래의 사진이 나온다.

![18](https://jsw6701.github.io/assets/images/posts_img/230530/18.png)

Note칸에는 원하는 이름을 쓰고 유효기간을 설정한다. (나는 사실 귀찮아서 그냥 영구로 하는…)

![19](https://jsw6701.github.io/assets/images/posts_img/230530/19.png)

![20](https://jsw6701.github.io/assets/images/posts_img/230530/20.png)

이 두개만 체크하고 생성해준다.

이제 그 생성하여 나온 값을 잘 저장해두고 프로젝트로 돌아가자.

아까와 똑같이 Settings에 Secrets and variables에 Actions로 들어가서 New repository secret 누르면

![12](https://jsw6701.github.io/assets/images/posts_img/230530/12.png)

여기 보이는 것에 ghtoken 값에 secrets를 제거하고 GHP_TOKEN을 이름으로 하고 키 값을 입력해주면 완성이다.

![21](https://jsw6701.github.io/assets/images/posts_img/230530/21.png)

이제 그럼 Action칸에 자신이 올린 것이 잘 배포에 들어가는 것을 볼 수 있다.