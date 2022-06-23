---
title: "[Embeded] 라즈베리 파이 클러스터 환경에서의 병렬 프로그래밍 논문 정리"
excerpt: "Raspberry Pi Paper Summary 1"

categories:
  - Embedded
tags:
  - [embedded, 라즈베리파이, "RaspberryPi", 논문, 요약]

permalink: /embedded/PiPaper/

toc: true
toc_sticky: true
 
date: 2022-06-22
last_modified_at: 2022-06-22
---

슈퍼컴퓨터 유지하기 위해 많은 전력이 소비됨

⇒ 부담됨

클라우드 컴퓨팅 서비스, 다양한 분야 컴퓨터 클러스터들 가격 대비 고성능의 컴퓨팅 능력 제공함

그럼에도 불구하고, 개인, 중소기업이 클러스터 보유 용이하지 않음!

<aside>
💡 비용, 공간 측에서 라즈베리 파이 사용은 새로운 방향을 제시

</aside>

![논문정리0](https://jsw6701.github.io/assets/images/posts_img/논문정리0.png)

- 라즈베리 파이 10대를 클러스터로 구성
    - 1대 : 마스터 노드
    - 9대 : 작업 노드
    - ARM 프로세서 512MB RAM 구성


### Message Passing Interface(MPI) 병렬 프로그램 실행

1. 2,500,000까지의 숫자 중에서 소수를 구하는 MPI 프로그램 클러스터 노드 수 2, 4, 8개로 변경하면서 실행 시간 비교
2. 병렬이 순차보다 실행속도 장점 있는지 확인

![논문정리1](https://jsw6701.github.io/assets/images/posts_img/논문정리1.png)

<aside>
💡 순차보다 병렬이 빠르며 노드 수가 많아질수록 더 빨라진다.

</aside>
참고논문 : [https://www.dbpia.co.kr/Journal/articleDetail?nodeId=NODE06545851](https://www.dbpia.co.kr/Journal/articleDetail?nodeId=NODE06545851)

---
그 과정은 다음 글에 하는 것이 좋겠다.
