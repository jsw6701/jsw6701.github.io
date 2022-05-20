---
title: "[Door lock] 아두이노 RC522로 도어락 제어하기(1)"
excerpt: "Arduino RC522 Door lock control"

categories:
  - Embedded
tags:
  - [Embedded]

permalink: /embedded/door lock/

toc: true
toc_sticky: true
 
date: 2022-05-21
last_modified_at: 2022-05-21
---

# 아두이노 RC522를 이용한 RFID 기능으로 실제 도어락 제어실험

### 계획

아두이노로 RFID 기능을 구현하기 위해서는 RC522라는 부품이 필요하다.

부품에 포함된 카드와 열쇠고리형 카드를 이용하여 원래 RFID 제어할 수 있는데 심심해서 검색하던 와중에 학생증같은 카드로도 RFID 제어할 수 있다는 소식을 접했다.

바로 다음날 한 번 구현해보고 싶은 것이 생겨서 첫차타고 바로 연구실로 향했다.

계획은 이렇다 MESS 랩실에는 현재 태블릿에 게더타운을 모티브로 한 안드로이드 어플을 제작하여 현재 랩실에 누가 있는지 어떤 상태인지 나타내는 어플이 문앞에 항상 켜져있다.

근데 이 어플이 생각보다 손이 안가서...좀 아쉬웠는데 각자의 고유 카드로 도어락에 대기만 하면 캐릭터가 알아서 자리를 찾아가기도 하며 문도 열리는 걸 하면 좋겠다 싶었다.

### 과정

나중에 기억을 더듬으며 글을 쓸거지만 예전에 라즈베리파이를 이용한 스마트금고를 제작한 적이 있었는데 결국엔 실제 도어락 말고 서보모터를 이용하여 구현했지만 도어락을 뜯어서 한 번 제어한 적이 있었다.

그 기억을 되살려서 MESS 랩실에 있는 도어락을 먼저 뜯어봤다.

교수님은 우리에게 항상 우리 랩실은 분해를 두려워하면 안된다고 하셔서 정말 두려움 없이 다 뜯고 부수고 해봤다.

먼저 뜯어보니까 문 밖에 있는 키패드와 연결되어 있는 선을 제거하고 나사를 제거하니 아래의 모습이 나왔다.

![도어락1](https://jsw6701.github.io/assets/images/posts_img/doorlock1.jpg)

사진에 있는 화살표가 가리키고 있는 네모는 이 도어락의 열고 닫히는 스위치의 반대편이다. 다시 말하자면 저기를 통해 도어락을 열고 닫음을 관리할 수 있다.

참고로 현재 다른곳에 붙어있는 흰색 글루건 같은 것을 떼낸 상태고 원래는 붙어있었다.

스위치를 보면 평범한 아두이노에서 쓰는 택트 스위치와 흡사하게 생겼다.

대각선 방향으로 GND 신호를 주는 곳을 결정해주고 pinMode 이용하여 통제해주면 된다.

```
#include <SPI.h> 
#include <MFRC522.h> 
#define RST_PIN 9 
#define SS_PIN 10 

MFRC522 mfrc522(SS_PIN, RST_PIN);
void setup() { 
  Serial.begin(9600); 
  while (!Serial); 
  SPI.begin(); 
  mfrc522.PCD_Init(); 
  mfrc522.PCD_DumpVersionToSerial(); 
  Serial.println(F("Scan PICC to see UID, SAK, type, and data blocks..."));
} 
void loop() { 
  if ( ! mfrc522.PICC_IsNewCardPresent()) { 
    return; 
  } 
  if ( ! mfrc522.PICC_ReadCardSerial()) { 
    return; 
  }
  mfrc522.PICC_DumpToSerial(&(mfrc522.uid)); 
}
```
위에 보이는 코드로 이제 랩실 인원들의 학생증을 가져다대면 UID, 나머지 정보들이 막 나오는데 사실 지금 필요한 것은 UID뿐 UID 하나하나 아래의 코드에 넣어주면 된다.

```
#include <SPI.h>
#include <MFRC522.h>  
 
#define RST_PIN   9
#define SS_PIN    10
#define switch 2
MFRC522 rc522(SS_PIN, RST_PIN);

int sg90 = 6;
int i=0;

void setup(){
  Serial.begin(9600);
  SPI.begin();
  rc522.PCD_Init();
  pinMode(switch, OUTPUT);
}

void loop(){ 

  if ( !rc522.PICC_IsNewCardPresent() || !rc522.PICC_ReadCardSerial() ) { 
    //카드 또는 ID 가 읽히지 않으면 return을 통해 다시 시작하게 됩니다.
    delay(500);
    return;
  }
  
  Serial.print("Card UID:");
  
  for (byte i = 0; i < 4; i++) {
    Serial.print(rc522.uid.uidByte[i]);
    Serial.print(" ");
  }
  Serial.println(" ");
  
  // 여기에 위에서 뽑은 UID를 각 부분에 맞게 넣어주면 된다.
  if(rc522.uid.uidByte[0]==0x00 && rc522.uid.uidByte[1]==0x00 && rc522.uid.uidByte[2]==0x00 
    && rc522.uid.uidByte[3]==0x00) {
    
    Serial.println("<< 00 학생증 !!! >>  Registered card...");
    delay(500);
  }
  else{
    Serial.println("<< WARNING !!! >>  This card is not registered");
    delay(500);
  }

  delay(100);
}
```

쉽게 말하면 저기 주석에 써놨지만 0x00 이라고 되어있는 부분에 0x 뒤에 두자리만 바꿔주면 UID 맞게 넣어줄 수 있다.

이제 여기까지 했다면 납땜을 조금해주면 충분히 이미 학생증으로 RFID 사용하여 도어락을 열어줄 수 있게 되었을 것이다.

하지만 나는 실제로 사용을 목적으로 구현중이기 때문에 문의 안쪽과 바깥쪽의 선의 연결과 여러가지를 신경써줘야 한다.

그 과정은 다음 글에 하는 것이 좋겠다.
