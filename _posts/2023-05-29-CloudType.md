---
title: "[Spring] CloudType 프로젝트 배포"
excerpt: "CloudType에 간단하게 MariaDB, SpringBoot 배포"

categories:
  - Backend
tags:
  - [backend, spring, mariadb, CloudType]

permalink: /backend/CloudType

toc: true
toc_sticky: true
 
date: 2023-05-29
last_modified_at: 2023-05-29
---

# CloudType 배포를 해보자

## 클라우드 서버를 이용하는 방법

두 가지 방법으로 클라우드 서버를 이용할 수 있다.

첫 번째 방법은 IaaS 방식을 이용하는 것이고, 두 번째 방법은 PaaS 방식을 이용하는 것이다.

### IaaS

Iaas는 Infrastructure as a Service의 약자로, 인프라를 서비스로 제공하는 것이다.

즉, 서버를 빌려서 직접 관리하는 방식이다.

예를 들어, AWS, GCP, Azure 등이 있다.

### PaaS

PaaS는 Platform as a Service의 약자로, 플랫폼을 서비스로 제공하는 것이다.

즉, 서버를 빌려서 관리를 해주는 방식이다.

예를 들어, Heroku, Firebase, CloudType 등이 있다.

이번에는 PaaS 방식을 이용하여 배포를 해보자.

## CloudType 사용법

![1](https://jsw6701.github.io/assets/images/posts_img/230529/1.png)

먼저 배포하기 위해 들어가면 이런 식으로 템플릿이 많이 준비되어 있고, 정말 편한 점은 깃허브를 연동하여 배포할 수 있다는 것이다.

물론 다른 Heroku 같은 PaaS를 사용해봤을 때도 깃허브 연동이 있었지만 여러므로 정말 Heroku는 해외에서 제공을 해줬던 것이기에 사용하는 데 직관성도 많이 떨어지고 속도도 매우 느렸다.

CloudType은 한국에서 제공을 하는 것이라 **직관성이 매우 뛰어나고 한국인이 사용하기에 매우 편리**하게 되어있는 것 같다. (속도는 아직 제대로 측정을 안해봐서 모르겠지만 Heroku보다는 빠르다고 한다…맞나?)

SpringBoot를 배포하기에 앞서 먼저 DB를 배포하고자 한다.

### 1. MariaDB 선택

CloudType에서 사용할 수 있는 DB는 총 4가지다.

MariaDB, MongoDB, PostgreSQL, Redis

나는 항상 MySQL을 자주 사용해서 MariaDB를 사용하기로 했다.

![2](https://jsw6701.github.io/assets/images/posts_img/230529/2.png)

클릭하면 이런 식으로 딱 나오는 데 정말 뭐야 왜 이리 간단하냐? 싶은 인터페이스다.

나는 현재 이미 만들어놔서 프로젝트 같은 것이 선택되어 있지만, 처음 하시는 분들은 직접 써줘야하는 것으로 기억한다.

방식은 하나의 프로젝트 안에 무료는 총 4가지 리소르를 넣을 수 있는 것으로 보인다. 나는 총 2가지로 MariaDB, SpringBoot를 넣을 것이다.

알맞게 빈칸을 채워주면 된다 일단 기본적으로 입력할 것은 그냥 서비스 이름, root password 정도면 될 것이다.

### 2. TCP 외부 접속 허용

![3](https://jsw6701.github.io/assets/images/posts_img/230529/3.png)

먼저 프로젝트 쪽에 들어오면 방금 생성한 MariaDB가 보일 것이고 우측 상단에는 톱니바퀴도 보일 것이다.

![4](https://jsw6701.github.io/assets/images/posts_img/230529/4.png)

외부에서 이 생성한 DB를 사용하고 싶다면 이걸 무조건 따라 해야한다.

1. 톱니바퀴 클릭 (환경설정)
2. 아래로 쭉 스크롤
3. TCP 외부 접속 허용 클릭

   ![5](https://jsw6701.github.io/assets/images/posts_img/230529/5.png)

   ![6](https://jsw6701.github.io/assets/images/posts_img/230529/6.png)

4. 적용하기 클릭

이제 외부에서도 접속할 수 있는 프로젝트가 되었다.

### 3. MySQL Workbench에서 연동하는 법

![7](https://jsw6701.github.io/assets/images/posts_img/230529/7.png)

생성한 DB를 클릭하여 들어오면 이런 화면이 보일 것이다. 여기서 도메인을 클릭한다.

![8](https://jsw6701.github.io/assets/images/posts_img/230529/8.png)

앞에 형광펜 표시한 [svc.sel4.cloudtype.app](http://svc.sel4.cloudtype.app) 이 호스트 이름이고, 뒤에 빨간색으로 가려놓은 3xxxx가 포트 번호이다.

![9](https://jsw6701.github.io/assets/images/posts_img/230529/9.png)

여기서 커넥션이름은 알아서 정하시고, 말한대로 호스트네임과 포트번호를 바꿔주고 마지막으로 아까 생성할 때 입력했던 Password를 Store in Vault를 눌러서 입력하고 저장해주면 끝이다.

### 4. Spring Boot 연동

1. application.properties

```java
spring.datasource.url=jdbc:mariadb://${HOST}:${PORT}/${NAME}?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul
spring.datasource.username=${USER}
spring.datasource.password=${PASSWORD}
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
```

나는 배포할 목적이기도 하고 깃허브 코드를 public으로 열어놨기에 중요한 내용은 환경변수 처리하여 볼 수 없도록 해놨다.

1. build.gradle

```java
implementation 'org.mariadb.jdbc:mariadb-java-client:3.0.3'
```

### 5. Spring Boot 배포하기

이제 처음에 보인 사진에서 내 GitHub 저장소 배포하기를 누른다.

![10](https://jsw6701.github.io/assets/images/posts_img/230529/10.png)

여기 보이는 곳에서 자신이 속한 그룹과 저장소를 선택한다.

나는 GDSC라는 Google Developer School Clubs에 가입되어 있어서 저렇게 되어있고 보통은 자신의 깃허브가 표시될 것이다.

![11](https://jsw6701.github.io/assets/images/posts_img/230529/11.png)

사진과 같이 골라주면 언어/프레임워크를 자신이 사용하는 것을 고르면 밑에 애플리케이션 설정에서 언어가 자동으로 설정되었는데 버전을 고쳐줘야한다.

나는 Spring 3.x.x 버전을 사용하고 있기에 Java17버전을 사용하니 변경해주도록 한다.

![12](https://jsw6701.github.io/assets/images/posts_img/230529/12.png)

이제 환경 변수 설정으로 넘어가겠다.

이 글을 읽으시는 분들은 다들 알테니 넘어가고 환경변수 이름과 값을 입력해주자.

![13](https://jsw6701.github.io/assets/images/posts_img/230529/13.png)

나 같은 경우에는 아까 보여준 [application.properties](http://application.properties) 에 사용한 HOST, NAME 등과 같은 환경변수의 값을 입력하였다.

![14](https://jsw6701.github.io/assets/images/posts_img/230529/14.png)

마지막은 그냥 포트가 다르다면 달리 입력해주면 되고 아니면 그냥 배포하기를 눌러주면 완성이다.

다음 게시글은 CI/CD를 Git Action으로 구현하는 글을 작성하도록 하겠다.