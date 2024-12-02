---
title: 3차 아이템 시뮬레이터
date: 2024-11-16 00:00:00
categories: [캠프, 프로젝트]
tags:
  [
    캠프
  ]
---

#### Todo
---

1. 데이터베이스 모델링  
    1. 아이템 테이블
      - 아이템 생성 API를 통해 생성된 아이템 정보는 아이템 테이블에 저장
    2. 계정 테이블
      - 아이디 : 영어 소문자 + 숫자 조합
      - 비밀번호 : 해싱된 값을 저장, 최소 6자 이상
    3. 캐릭터 테이블
      - 하나의 계정은 여러개의 캐릭터를 보유할 수 있다
      - Health,power,money
    4. 캐릭터-인벤토리 테이블
      - 캐릭터가 보유는 하고있으나 장착하고 있지 않은 아이템 정보
    5. 캐릭터-아이템 테이블
      - 이 테이블엔 실제로 캐릭터가 장착한 아이템 정보들이 존재

2. API 구현 (필수)
    1. 회원가입 API
    2. 로그인 API
    3. 캐릭터 생성 API
    4. 캐릭터 삭제 API
    5. 캐릭터 상세 조회 API
    6. 아이템 생성 API
    7. 아이템 수정 API
    8. 아이템 목록 API
    9. 아이템 상세 조회 API

3. API 구현 (도전)
    1. 아이템 구입 API
    2. 아이템 판매 API
    3. 캐릭터가 보유한 인벤토리 내 아이템 목록 조회 API
    4. 캐릭터가 장착한 아이템 목록 조회 API
    5. 아이템 장착 API
    6. 아이템 탈착 API 
    7. 게임 머니를 버는 API

#### 사용 패키지
---

```bash
yarn init -y
yarn add express
yarn add jsonwebtoken
yarn add cookie-parser

yarn add prisma @prisma/client
npx prisma init

yarn add ysql2

yarn add winston
yarn add bcrypt

#yarn add express-session
#yarn add express-mysql-session
```

#### 테이블 설계
---
![item_simulator_erd](https://github.com/user-attachments/assets/55073a52-a4db-4404-a612-6e95617350e8)
  
1. ACCOUNT (계정)
  - ACCOUNT_ID : 테이블 고유ID (PK)
  - USER_ID : 아이디
  - USER_PW : 비밀번호
  - CREATE_DT : 생성 일자
  - UPDATE_DT : 수정 일자

2. CHARACTER (캐릭터)
  - CHARACTER_ID : 테이블 고유ID (PK)
  - ACCOUNT_ID : 계정 테이블 ID (FK)
  - CHARACTER_NAME : 캐릭터 이름
  - HEALTH : HEALTH 스탯
  - POWER : POWER 스탯
  - MONEY : 소지 돈
  - CREATE_DT : 생얼 일자
  - UPDATE_DT : 수정 일자

3. ITEM (아이템)
  - ITEM_CODE : 테이블 고유ID (PK)
  - ITEM_TYPE : 아이템 종류
  - ITEM_NAME : 아이템 이름
  - ITEM_PRICE : 가격
  - HEALTH : HEALTH 스탯
  - POWER : POWER 스탯
  - CREATE_DT : 생성 일자
  - UPDATE_DT : 수정 일자

4. INVENTORY (인벤토리)
  - INVENTORY_ID : 테이블 고유ID (PK)
  - CHARACTER_ID : 캐릭터 테이블 ID (FK)
  - ITEM_CODE : 아이템 테이블 ID (FK)
  - COUNT : 개수
  - CREATE_DT : 생성 일자
  - UPDATE_DT : 수정 일자

5. EQUIPED (장비 장착)
  - EQUIPED_ID : 테이블 고유ID (PK)
  - CHARACTER_ID : 캐릭터 테이블 ID (FK)
  - WEAPON_CODE : 무기의 아이템 코드
  - WEAPON_NAME : 무기의 아이템 이름
  - HEAD_CODE : 머리 부위의 아이템 코드
  - HEAD_NAME : 머리 부위의 아이템 이름
  - BODY_CODE : 몸 부위의 아이템 코드
  - BODY_NAME : 몸 부위의 아이템 이름
  - SHOES_CODE : 신발 부위의 아이템 코드
  - SHOES_NAME : 신발 부위의 아이템 이름
  - ACCESSORY1_CODE : 액세서리1 부위의 아이템 코드
  - ACCESSORY1_NAME : 액세서리1 부위의 아이템 이름
  - ACCESSORY2_CODE : 액세서리2 부위의 아이템 코드
  - ACCESSORY2_NAME : 액세서리2 부위의 아이템 이름

#### API 명세서
---

| 기능 | API URL | Method | request(가져 갈 데이터) | response(서버로부터 받아 올 데이터) |
|---|---|---|---|---|---|
| 회원가입 | /api/account | POST | {"userId":"sparta1","userPw":"sparta1","userPwChk":"sparta1"} | { "accountId": 2,"userId": "sparta1","createDt": "2024-11-28T03:29:33.190Z"} |
| 로그인 | /api/login | POST | { "userId":"sparta1",	"userPw":"sparta1"} | "로그인 성공" |
| 캐릭터 생성 | /api/character | POST | { "characterName":"스파르타1" } | {	"message": "캐릭터 생성 성공" } |
| 캐릭터 삭제 | /api/character/:characterId | DELETE | {} | {	"message": "캐릭터가 삭제되었습니다." } |
| 캐릭터 상세 조회 | /api/character/:characterId | GET | {} | {	"name": "스파르타1","health": 500,"power": 100,"money": 10000} |
| 아이템 생성 | /api/item | POST | { "itemCode": 20, "itemType": 0, "itemName": "만능약", "itemStatus": { "health": 0, "power": 0 }, "itemPrice": 100} | { "message": "아이템을 생성하였습니다." } |
| 아이템 수정 | /api/item/:itemCode | PATCH | {	"itemName": "일반 단검", "itemType": 4,	"itemStatus" : { "health" : 1 , "power" : 5 }} | { "message": "아이템 정보를 변경하였습니다." } |
| 아이템 목록 | /api/item | GET | {} | [ { "itemCode": 1,	"itemType": 0, "itemName": "빨간 포션",	"health": 0, "power": 0, "itemPrice": 10 },	{ "itemCode": 2, "itemType": 0,	"itemName": "노란 포션", "health": 0,  "power": 0, 	"itemPrice": 20	}] |
| 아이템 상세 조회 | /api/item/:itemCode | GET | {} | {	"itemCode": 4, "itemType": 4, "itemName": "일반 단검", "health": 1,	"power": 5,	"itemPrice": 50 } |
| 아이템 구입 | /api/npc/:characterId | POST | {	"itemCode" : 1,	"count" : 1 } | {	"message": "아이템 구매에 성공하셨습니다.",	"money": 7352 } |
| 아이템 판매 | /api/npc/:characterId | DELETE | { "itemCode" : 1 }| { "message": "아이템 판매에 성공하셨습니다.", "money": 7358 }|
| 인벤토리 조회 | /api/character/:characterId/inventory | GET | {} | [ { "itemCode": 1,	"qty": 8,	"item": {	"itemCode": 1, "itemName": "빨간 포션" } },	{	"itemCode": 4, "qty": 2, "item": { "itemCode": 4,	"itemName": "단검" } }] | 
| 장착한 아이템 조회 | /api/equipped/:characterId | GET | {} | [{	"equipped_id": 1,	"character_id": 1, "weapon_code": null,	"weapon_name": null, "head_code": "7", "head_name": "일반 투구", "body_code": null, 	"body_name": null, "shoes_code": null, "shoes_name": null, "accessory_left_code": null,	"accessory_left_name": null, "accessory_right_code": null, "accessory_right_name": null }] |
| 아이템 장착 | /api/equipped/:characterId/puton | PATCH | { "itemCode": 4 } | { "message": "단검을 장착 하셨습니다." } |
| 아이템 탈착 | /api/equipped/:characterId/takeoff | PATCH | { "itemCode": 4 } | { "message": "단검을 탈착 하셨습니다." } |
| 게임 머니 | /api/work/:characterId | PATCH | {} | { "money": 7850 } |
