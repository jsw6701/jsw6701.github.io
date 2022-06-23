---
title: "[Embeded] 라즈베리 파이를 이용한 병렬 처리 시스템 구축 논문 정리"
excerpt: "Raspberry Pi Paper Summary 2"

categories:
  - Embedded
tags:
  - [embedded, 라즈베리파이, "RaspberryPi", 논문, 요약]

permalink: /embedded/PiPaper2/

toc: true
toc_sticky: true
 
date: 2022-06-22
last_modified_at: 2022-06-22
---

## 서론

- 목표
    - 라즈베리 파이를 이용하여 컴퓨터 클러스터를 구축
    - 병렬 및 분산 컴퓨팅의 연산능력을 벤치마킹을 통해 확인 ⇒ 라즈베리 파이 적절한지 확인

## 병렬처리 시스템 구성

- 라즈베리 파이 4대 연결
    - 1대 : 마스터 노드
    - 3대 : 슬레이브 노드
    - 성능 : Raspberry Pi 2 Model B

성능 측정 : HPL (High Performance Linpack) 소프트웨어

> Linpack : Linear algebra 수행하는 소프트웨어 라이브러리
>

> HPL : 행렬을 이용한 선형대수 연산함수를 통하여 각 노드의 속도 측정
>

노드가 증가할수록 성능 증가폭이 감소 ⇒ 한계점 도달  Because 암달의 법칙 때문

![논문정리2](https://jsw6701.github.io/assets/images/posts_img/논문정리2.png)

> 암달의 법칙 (Amdal’s law) : 소프트웨어의 병렬화 가능한 부분은 해당 소프트웨어마다 다르고, 개선을 통해서 얻어지는 성능의 향상은 한계가 존재함
>

## 실험

사용한 HPL 버전 : 2.1

수행 작업 : Order가 5040인 행렬에 관한 선형 대수 연산 작업

라즈베리 파이 한 대를 사용하여 HPL 수행과 멀티 노드 한 대씩 늘려 실행 시간 측정

![논문정리3](https://jsw6701.github.io/assets/images/posts_img/논문정리3.png)

비교를 위하여 Intel(R) Core(TM) i5-2410M CPU 2.30GHz 탑재한 랩탑을 같은 조건에서 벤치마킹한 결과 5.044e+00Gflops 의 수행 속도를 보이며, 이는 라즈베리 파이 한대의 30배정도의 해당하는 속도를 보임

<aside>
💡 노드 증가 ⇒ 수행 시간 감소 ⇒ 수행 속도 증가

</aside>

노드 개수 증가에 따른 총 작업 시간의 감소율이 지수함수 꼴을 보이는데 이는 암달의 법칙에 의함이다.

## 결론

저렴하고 리눅스를 포팅하여 사용할 수 있는 장점을 가진 라즈베리 파이는 좋은 방안

다수의 라즈베리 파이 네트워크 연결하고 클러스터 구축하면 위와 같이 MPI 병렬 프로그램을 수행하는 컴퓨팅 구축 가능

클라우드, 통계 등 다양한 병렬 및 분산 프로그래밍을 활용하여 복잡한 알고리즘 해결 활용 가능

참고논문 : [https://www.koreascience.or.kr/article/CFKO201718459284241.pdf](https://www.koreascience.or.kr/article/CFKO201718459284241.pdf)

---
