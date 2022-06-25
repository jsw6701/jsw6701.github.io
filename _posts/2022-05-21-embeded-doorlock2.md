---
title: "[Door lock] 아두이노 RC522로 도어락 제어하기(2)"
excerpt: "Arduino RC522 Door lock control 2"

categories:
  - Embedded
tags:
  - [embedded, 아두이노, "rc522", rfid, 도어락, 제어, 릴레이, 모션, 센서]

permalink: /embedded/door lock2/

toc: true
toc_sticky: true
 
date: 2022-06-24
last_modified_at: 2022-06-24
---

# 아두이노 RC522를 이용한 RFID 기능으로 실제 도어락 제어실험 

### 실험 수행 내용 요약

적외선 PIR 인체감지 모션센서를 이용해 사람이 문 앞에 온 것을 감지하면 I2C LCD에 “Tag Card plz” 출력.

사용자가 RFID에 등록된 카드를 태그하면 1채널 릴레이 모듈을 이용해 도어락의 lock/unlock을 제어하고 I2C LCD에 태그한 날짜와 시간, 등록된 카드의 사용자 이름 이니셜을 출력한다.

만약 미등록된 카드를 태그하면 도어락은 작동하지 않고 I2C LCD에 “Not registered Knock please”를 출력한다.

### 회로도

![도어락회로도.png](https://jsw6701.github.io/assets/images/posts_img/도어락회로도.png)

### 하드웨어 설정

RFID의 RST, SDA, MOSI, MISO, SCK순으로 Arduino의 9~13번 pin에 연결.

RFID의 VCC, GND를 Arduino의 3.3V, GND에 연결.

1채널 릴레이 모듈(1ch, JQC-3FF-S-Z)의 NO, COM을 도어락 switch에 연결.

1채널 릴레이 모듈(1ch, JQC-3FF-S-Z)의 Signal을 Arduino의 8번 pin에 연결.

1채널 릴레이 모듈(1ch, JQC-3FF-S-Z)의 VCC, GND를 Arduino의 5V, GND에 연결.

RTC 모듈의 RST, DAT, CLK순으로 Arduino의 14~16번 pin에 연결.

RTC 모듈의 VCC, GND를 Arduino의 3.3V, GND에 연결.

I2C LCD 모듈의 SDA, SCL을 Arduino의 18, 19번 pin에 연결.

I2C LCD 모듈의 VCC, GND를 Arduino의 5V, GND에 연결.

HC-SR501 모듈의 Signal을 Arduino의 3번 pin에 연결.

HC-SR501 모듈의 VCC, GND를 Arduino의 5V, GND에 연결.

### 중간 과정 사진

![도어락중간과정1.png](https://jsw6701.github.io/assets/images/posts_img/도어락중간과정1.png)

![도어락중간과정2.png](https://jsw6701.github.io/assets/images/posts_img/도어락중간과정2.png)

먼저 도어락 밖의 RFID와 내부의 아두이노와 도어락에 연결하기위해 선을 밖으로 빼주는 작업이 필요했다.

그래서 어느정도 필요한 길을 조금 부셔주기도 하고 왼쪽에 보이는 사진처럼 문 밖의 도어락에 길을 터줬다.

그리고 오른쪽에 보이는 사진처럼 문 안쪽에 선을 빼주고 그 선들을 아두이노에 연결했다.

### 릴레이 모듈

도어락을 제어할 때 중요한 점을 깨달았다.

분명 잘 작동하는 것도 확인했고 문이 열렸다 닫혔다 했던 거 같은데 한 번 하면 삑삑삑 소리도 나고 문제가 자꾸 발생했다.

여기서 알게 된 사실 하나

> 도어락의 개폐는 5V 신호를 주기만 하면 열리는 것이 아닌 HIGH에서 LOW로 갔을 경우에 개폐가 결정되는 것이였다.
>

<aside>
💡 결국 이것을 해결해주기 위해서 릴레이 모듈을 사용하여 해결해주었다.

</aside>

### 전체 모습

![전체모습1](https://jsw6701.github.io/assets/images/posts_img/전체모습1.png)

![전체모습2](https://jsw6701.github.io/assets/images/posts_img/전체모습2.png)

좀 더럽긴 한데 이게 전체 모습이다.

### 카드가 등록되지 않은 사람

[도어락영상1.mp4](https://jsw6701.github.io/assets/images/posts_img/[VAP]도어락영상1.mp4)

### 카드가 등록된 사람

[도어락영상2.mp4](https://jsw6701.github.io/assets/images/posts_img/[VAP]도어락영상2.mp4)

<video width="100%" height="100%" controls="controls">
  <source src="https://jsw6701.github.io/assets/images/posts_img/[VAP]도어락영상2.mp4" type="video/mp4">
</video>