---
title: 2차 [CLI] ROGUELIKE DEFENCE
date: 2024-11-17 00:00:00
categories: [캠프, 프로젝트]
tags:
  [
    캠프
  ]
---

#### **게임 / 전투 기획**
---
1. 플레이어에게는 여러 가지 선택지가 주어진다.
   1. 유닛소환 - '근접, 원거리, 버퍼' 중 선택
   2. 조합 - 유닛을 조합한다 (확률).
      - 근접 유닛 조합 : 같은 등급의 근접 유닛 3마리를 소모하여 상위 등급의 근접 유닛을 1개 생성.
      - 원거리 유닛 조합 : 같은 등급의 원거리 유닛 3마리를 소모하여 상위 등급의 원거리 유닛을 1개 생성.
      - 무작위 조합 : 무작위(근접,원거리)로 같은 등급의 유닛 3마리를 소모하여 랜덤한 종류의 상위 등급 유닛을 1개 생성.  
      - 조합 실패시 1개의 유닛 소모.
   3. 아이템 - 보유하고 있는 아이템을 선택해서 사용한다.
   4. 수리 - 성문의 내구도를 일정량 회복한다 (횟수 제한).
   플레이어는 선택지 중 1가지를 선택하면 해당 턴이 진행된다.
2. 스테이지별 웨이브가 존재하며 웨이브마다 몬스터가 출현.
   - 스테이지당 5 웨이브
   - 3턴당 1웨이브 진행
3. 승리 / 패배
   1. 승리 : 마지막 10 스테이지 까지 생존하여 모든 웨이브의 몬스터 제거.
   2. 패배 : 성의 체력이 '0'이 되면 패배.
4. 스테이지 클리어 시, 성문 체력이 일정수치 회복된다.
5. 난이도와 스테이즈 웨이브에 따라 몬스터가 조금씩 강해진다.
6. 기본 전투형태는 턴제 형식으로 진행된다.
   플레이어의 선택 > 유닛의 행동 > 몬스터 행동
7. 몬스터 처치시 일정 확률로 아이템을 획득한다.

#### **Play Process**
---
![PlayProcess](https://github.com/user-attachments/assets/79078a64-698d-4e64-b816-71f6adbf7713)

#### **성**
---
1. **속성**
    - hp : 체력
    - repairCnt : 회복 횟수

#### **유닛**
---
1. **종류**  
근접,원거리 유닛은 최대 6개 소환 가능. 버퍼는 최대 1개.
   - 근접 : 범위1, 원거리 유닛보다 공격력 높음
   - 원거리 : 범위3, 근접 유닛보다 공격력 낮음
   - 버퍼 : 아군 전원에게 설정 값 만큼의 '공격력'을 증가 시킨다.
2. **속성**
    - name : 이름
    - type : 유형(근거리,원거리,버퍼)
    - grade : 등급(1등급,2등급,3등급)
    - damage : 공격력
    - isItemBuff : 아이템 버프 효과 여부
    - isUnitBuff : 버프 유닛 효과 여부

#### **몬스터**
---
1. **종류**
    - 근거리, 원거리
2. **속성**
    - name : 이름
    - type : 근접,원거리
    - hp : 체력
    - damage : 공격력
    - displayLoc : 정보 출력을 위한 배열의 위치 값

#### **아이템**
---
1. **종류**
    1. 버프 스톤 : 배치된 모든 유닛에게 n턴간 최소 공격력을 n만큼 증가 시킨다.
    2. 마법의 두루마리 : 몬스터 전체에게 1~n의 데미지를 준다.
    3. 원기옥 : 몬스터 한마리에게 소환된 유닛의 총 공격력의 2배만큼 데미지를 준다.
2. **속성**
    - id : 아이템 아이디
    - name : 아이템 이름
    - desc : 아이템 설명
    - ea : 아이템 소지수량
    - rate : 드랍율

#### **업적**
---
1. **속성**
    - killCount : 몬스터 처치수
    - isLvEasy : 업적 달성 여부
    - isLvNomal : 업적 달성 여부
    - isLvHard : 업적 달성 여부
    - isLvHell : 업적 달성 여부
    - isMeleeMaxGrade : 업적 달성 여부
    - isRangedMaxGrade : 업적 달성 여부
    - isMosterKill01 : 업적 달성 여부
    - isMosterKill02 : 업적 달성 여부
    - isMosterKill03 : 업적 달성 여부
    - isMosterKill04 : 업적 달성 여부
 
#### **작업 진행 기록**
---
- **2024/11/06**
    1. 기획 초안 작성
    2. 스켈레톤 코드 설치 및 분석
    3. 정보 조사. (디자인, 구현방법 및 가능여부 등)
    4. 로비화면 아스키아트 추가
    5. 게임진행 화면 아스키아트 테스트 및 디자인 구상

- **2024/11/07** 
    1. 정보 조사. (디자인, 구현방법 및 가능여부 등)
    2. 프로젝트 디렉토리 구성
    3. Model Class 파일 작업 (Castle, Monster, Item, Unit 등)
    4. Constants 파일 작업 (업적, 아이템)
    5. 기본 선택지 구성

- **2024/11/08**
    1. 유닛 소환 기능 구현
    2. 몬스터 리스폰 기능 구현
    3. 턴 종료시 유닛/몬스터 공격 로직 개발 진행중

- **2024/11/11**
    1. 유닛 조합 기능 구현

- **2024/11/12**
    1. 유닛 조합 버그 수정
    2. 수리 기능 구현
    3. 아이템 사용 기능 구현
    4. 아이템 드랍 기능 구현

- **2024/11/13**
    1. Display Status 영역 구현
    2. DisPlay Map 영역 구현

- **2024/11/14**
    1. DisPlay Map 영역 구현
    2. Display Logs 영역 구현
    3. Display 선택지 영역 구현
    4. 아이템 사용시 몹 처치 처리 로직 추가
    5. 버프 유닛 작업
    6. 조합 로직 버그 수정

- **2024/11/15**
    1. Display 업적 화면 구현
    2. 업적 기능 구현
    3. 난이도 옵션 구현
    4. 난이도에 따른 몬스터 레벨링 적용
    5. 버그 수정

- **20204/11/16**
    1. 테스트 및 버그 수정
    2. 마무리 작업
    
#### **트러블 슈팅 1**
---
1. **배경**  
현재 게임에서 로그 정보를 배열로 관리하고 있다. 자꾸 늘어나면 무한으로 내려가니 일정 이상이 차면 첫번째의 로그를 삭제하고 추가하는 함수를 만들었다.
```javascript
export function logsPush(logs, row) {
   //logs의 10개 이상일 경우 첫 로그 삭제 후 추가
   const maxRow = 10;
   if (logs.length >= maxRow) logs.shift();
   logs.push(row);
}
```
그리고 아래와 같이 해당 함수를 호출하여 로그를 관리했다.
```javascript
logsPush(logs, chalk.white(`[Wave:${stage}] 몬스터 ${spawnCnt} 마리가 등장하였습니다.`));
```

2. **발단**    
로그를 출력하는 부분에서 maxCol사이즈에서 Length만큼 계산해서 가변적으로 출력하는 부분이 있어서 로그 텍스트의 length가 이상한 것을 확인.  
```javascript
console.log(logs[0].length); //37
console.log("[Wave:1] 몬스터 6 마리가 등장하였습니다."); //27
```

3. **전개**  
그래서 확인해보기 위해서 for문으로 돌려보았다.  
```javascript
let b = chalk.white('[Wave:1] 몬스터 6 마리가 등장하였습니다.');
for (let i = 0; i < b.length; i++) {
   console.log(b[i]);
}
```
그랬더니 콘솔 결과창에 아래와 같이 문자열 앞뒤로 공백이 5개씩 있는 것을 확인.  
```javascript
     [Wave:1]몬스터 6 마리가 등장하였습니다.      
```

4. **결말**  
white가 아닌 red, whiteBright 등 다른 컬러로 바꿔서 확인 해봤더니 똑같이 추가로 10 Length가 잡혔다. 그래서 해당 부분을 계산해서 처리 할 수 있는 함수를 별도로 만들어서 해결했다.  
```javascript
const getBlankLength = (col, str) => {
   let res = 0;
   for (let i = 0; i < str.length; i++) {
      if (str[i].charCodeAt(0) <= 0x00007f) {
      } else if (str[i].charCodeAt(0) <= 0x0007ff) {
         // 2
         res++;
      } else if (str[i].charCodeAt(0) <= 0x00ffff) {
         //3
         res++;
      } else {
         //4
         res++;
      }
   }

   //maxcol : 최대 col 값
   //res : 한글
   //10 : chalk 값
   return col - str.length - res + 10;
};
```

#### **트러블 슈팅 2**
---
1. **배경**
- 마무리 작업으로 버그가 있는지 게임을 테스트 해보는 도중에 문제를 발견했다.

2. **발단**
- 같은 유닛이 2번 공격을 하는 것을 확인.
![20241116_225634](https://github.com/user-attachments/assets/5d4d8355-19ec-41b0-8c8f-fdb94634017f)

3. **전개**
- 1개의 유닛은 1번의 공격 밖에 할 수 없기에 공격 패턴의 로직을 확인.
```javascript
if (!isAttack) {
    for (let k = range; k > 0; k--) {
        //대상 찾기(고도화)x
        for (let n = 0; n < locMonsters.length; n++) {
            if (locMonsters[n][k - 1]) {
            locMonsters[n][k - 1].hp -= locUnits[j][i].attack();

            //처치 시 삭제
            if (locMonsters[n][k - 1].hp <= 0) {
                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 를 처치하였습니다.`));

                for (let i = locMonsters[n][k - 1]['displayLoc'] + 1; i < displayMonsters.length; i++) {
                    displayMonsters[i]['displayLoc'] -= 1;
                }

                displayMonsters.splice(locMonsters[n][k - 1]['displayLoc'], 1);
                locMonsters[n][k - 1] = false;

                getItemRate(logs, inventory); //아이템 획득 여부?

                //킬 카운트
                achievement.killCount++;
                checkKillCount(logs, achievement);
            } else {
                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 에게 데미지 ${locUnits[j][i].attack()} 를 주었습니다.`));
            }

            isAttack = true;
            break;
            }
        }
    }
}
```
4. **절정, 결말**
- 공격을 진행 했을 경우 isAttack의 값을 true를 주었지만 2중 for문으로 구성되어 있어서 외부의 for문에 대한 isAttack 처리를 안했던게 원인인 것을 확인하여 해당 부분을 수정하여 문제 되었던 부분을 해결.  
```javascript
//내 앞줄에 몹이 없는 것 확인
if (!isAttack) {
    for (let k = range; k > 0; k--) {
        //대상 찾기(고도화)x
        for (let n = 0; n < locMonsters.length; n++) {
            if (locMonsters[n][k - 1]) {
            locMonsters[n][k - 1].hp -= locUnits[j][i].attack();

            //처치 시 삭제
            if (locMonsters[n][k - 1].hp <= 0) {
                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 를 처치하였습니다.`));

                for (let i = locMonsters[n][k - 1]['displayLoc'] + 1; i < displayMonsters.length; i++) {
                    displayMonsters[i]['displayLoc'] -= 1;
                }

                displayMonsters.splice(locMonsters[n][k - 1]['displayLoc'], 1);
                locMonsters[n][k - 1] = false;

                getItemRate(logs, inventory); //아이템 획득 여부?

                //킬 카운트
                achievement.killCount++;
                checkKillCount(logs, achievement);
            } else {
                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 에게 데미지 ${locUnits[j][i].attack()} 를 주었습니다.`));
            }

            isAttack = true;
            break;
            }
        }

        if (isAttack) break; //추가
    }
}
```


#### **필수 기능**
---
- **단순 행동 패턴 2가지 구현**   
    유닛 소환 / 유닛 조합

- **클래스 문법 활용, 플레이어 스탯 관리**  
    Castle, Unit, Monster, Item, Achievement

- **간단한 전투 로직 구현**  
플레이어 선택 > 유닛 행동(공격/대기) > 몬스터 행동(공격/이동)

- **스테이지 진행에 따른 이벤트 관리**
    1. 스테이지 진행마다 성의 체력 회복.  
    ```javascript
    if (stage > 1 && castle.hp < GameSystem.CASTLE_MAX_HP) {
        let heal = Math.floor(Math.random() * 100 + 1);

        //회복 만피 체크
        if (castle.hp + heal > GameSystem.CASTLE_MAX_HP) {
            castle.hp = GameSystem.CASTLE_MAX_HP;
            heal = GameSystem.CASTLE_MAX_HP - castle.hp;
        } else {
            castle.hp += heal;
        }

        logsPush(logs, chalk.white(`다음 스테이지가 진행되어 성의 체력이 ${heal} 만큼 회복되었습니다.`));
    }
   ```
    2. 몬스터의 공격력 및 체력 상승.  
    ```javascript
    let damage = Math.floor(difficulty * (stage / 2) + (Math.random() * 5 + 1));
    let hp = 10 + Math.floor(difficulty * (stage / 2) + (Math.random() * 20 + 1));
    ````
    


#### **도전 기능** 
---
- **확률 로직 적용**
    1. 몬스트 등장 시 개체 수 및 위치 랜덤 적용.   
    ```javascript
        //몬스터 소환 수 (1~6)
        let spawnCnt = 0;

        if (difficulty === 1 || difficulty === 2) {
            spawnCnt = Math.floor(Math.random() * 6) + 1;
        } else if (difficulty === 3) {
            if (stage <= 5) {
                spawnCnt = Math.floor(Math.random() * 6) + 1; //1~6
            } else {
                spawnCnt = Math.floor(Math.random() * 4) + 3; //3~6
            }
        } else {
            spawnCnt = 6;
        }
   ```
    2. 두루마리 아이템 사용시 몬스터 개별로 랜덤 데미지 적용.  
    ```javascript
        for (let i = 0; i < locMonsters.length; i++) {
            for (let j = 0; j < locMonsters[0].length; j++) {
                if (locMonsters[i][j]) {
                    let damage = Math.floor(Math.random() * 9 + 1);
                    locMonsters[i][j].receveDamage(damage);

                    // 생략
                }
            }
        }
    ```
    3. 유닛 조합 시 성공 확률 적용.  
    ```javascript
        if (Math.floor(Math.random() * 100) < GameSystem.GRADE2_SUCCESS_PER) {
            if (unitGrade1MCnt >= 2 && unitGrade1RCnt >= 2) {
                for (let i = 0; i < 2; i++) {
                    unitType === 1 ? (locUnits[unitGrade1MArr[i]][unitType - 1] = false) : (locUnits[unitGrade1RArr[i]][unitType - 1] = false);
                }

                unitType === 1 ? (locUnits[unitGrade1RArr[unitGrade1RArr.length - 1]][1] = false) : (locUnits[unitGrade1MArr[unitGrade1MArr.length - 1]][0] = false);
            }

            // 생략
        }
    ```
    4. 몬스터 처치 시 아이템 드랍율 적용.  
    ```javascript
        const getItemRate = (logs, inventory) => {
            //개별로 확률이 존재.
            const itemsRates = [Items.ITEM_CODE01_RATE, Items.ITEM_CODE02_RATE, Items.ITEM_CODE03_RATE];

            for (let i = 0; i < itemsRates.length; i++) {
                if (Math.floor(Math.random() * 100 + 1) / 100 <= itemsRates[i]) {
                    logsPush(logs, chalk.blue(inventory[i].getItem()));
                }
            }
        };
    ```
    5. 성 수리 선택 시 5% 확률로 풀피 회복 그 외는 랜덤 회복량 적용.  
    ```javascript
        if (Math.floor(Math.random() * 100 + 1) <= 5) {
            //풀피 회복
            castle.hp = GameSystem.CASTLE_MAX_HP;
            logsPush(logs, chalk.white(`5%의 기적! 성의 체력이 최대치로 회복했습니다.`));
        } else {
            const maxRepairHp = 100;
            let repairHp = Math.floor(Math.random() * maxRepairHp + 20);
            // 생략
        }
    ```
    6. 스테이지 상승 시 랜덤한 회복량 적용.  
        필수 기능 - 스테이지 진행에 따른 이벤트 관리 동일

- **복잡한 행동 패턴 구현**  
    1. 몬스터 행동 : 현재 범위에서 자기 공격 범위 안에 공격 대상이 있으면 공격 하고 없으면 이동.  
    ```javascript
        // 1. 공격 거리 확인
        let sumDamage = 0;
        for (let i = 0; i < locMonsters[0].length; i++) {
            for (let j = 0; j < locMonsters.length; j++) {
                //해당 위치 몬스터 여부 체크
                if (locMonsters[j][i]) {
                    if (i === 0) {
                    castle.hp -= locMonsters[j][i].attack();
                    sumDamage += locMonsters[j][i].attack();
                    } else if (i === 2 && locMonsters[j][i]['type'] === 1) {
                    castle.hp -= locMonsters[j][i].attack();
                    sumDamage += locMonsters[j][i].attack();
                    } else {
                        //내 앞에 몬스터가 있는지 확인
                        if (locMonsters[j][i - 1]) {
                            //있으면 대기?
                        } else {
                            locMonsters[j][i - 1] = locMonsters[j][i];
                            locMonsters[j][i] = false;
                        }
                    }
                }
            }
        }
    ```
    2. 유닛 공격: 공격 시 공격 범위 안에 자신의 위치와 같은 라인에 몬스터를 찾아서 공격. 없으면 다른 위치의 범위 안에 몬스터를 공격.  
    ```javascript
        for (let i = 0; i < 2; i++) {
            //1종류당 6마리 배치
            for (let j = 0; j < 6; j++) {
                if (locUnits[j][i]) {
                    let range = locUnits[j][i].getRange();

                    let isAttack = false; //공격 여부
                    //궁수는 최대 사정거리 부터 공격 그래서 --처리
                    for (let k = range; k > 0; k--) {
                        if (locMonsters[j][k - 1]) {
                            locMonsters[j][k - 1].hp -= locUnits[j][i].attack();

                            //처치 시 삭제
                            if (locMonsters[j][k - 1].hp <= 0) {
                                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[j][k - 1]['name']} 를 처치하였습니다.`));

                                for (let i = locMonsters[j][k - 1]['displayLoc'] + 1; i < displayMonsters.length; i++) {
                                    displayMonsters[i]['displayLoc'] -= 1;
                                }

                                displayMonsters.splice(locMonsters[j][k - 1]['displayLoc'], 1);
                                locMonsters[j][k - 1] = false;

                                getItemRate(logs, inventory); //아이템 획득 여부?

                                //킬 카운트
                                achievement.killCount++;
                                checkKillCount(logs, achievement);
                            } else {
                                logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[j][k - 1]['name']} 에게 데미지 ${locUnits[j][i].attack()} 를 주었습니다.`));
                            }

                            isAttack = true;
                            break;
                        }
                    }

                    //내 앞줄에 몹이 없는 것 확인
                    if (!isAttack) {
                        for (let k = range; k > 0; k--) {
                            //대상 찾기(고도화)x
                            for (let n = 0; n < locMonsters.length; n++) {
                                if (locMonsters[n][k - 1]) {
                                    locMonsters[n][k - 1].hp -= locUnits[j][i].attack();

                                    //처치 시 삭제
                                    if (locMonsters[n][k - 1].hp <= 0) {
                                    logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 를 처치하였습니다.`));

                                    for (let i = locMonsters[n][k - 1]['displayLoc'] + 1; i < displayMonsters.length; i++) {
                                        displayMonsters[i]['displayLoc'] -= 1;
                                    }

                                    displayMonsters.splice(locMonsters[n][k - 1]['displayLoc'], 1);
                                    locMonsters[n][k - 1] = false;

                                    getItemRate(logs, inventory); //아이템 획득 여부?

                                    //킬 카운트
                                    achievement.killCount++;
                                    checkKillCount(logs, achievement);
                                    } else {
                                    logsPush(logs, chalk.white(`${locUnits[j][i]['name']}가 ${locMonsters[n][k - 1]['name']} 에게 데미지 ${locUnits[j][i].attack()} 를 주었습니다.`));
                                    }

                                    isAttack = true;
                                    break;
                                }
                            }

                            if (isAttack) break;
                        }
                    }
                }
            }
        }
    ```
    3. 유닛 조합 : 유닛 조합 시 선택한 조합에 따라 조건에 맞는 유닛을 체크 하고 조건에 맞는 경우 조합 로직 수행. 성공 시 다시 반복하여 체크. 실패 시 조합 종료.    
    ```javascript
        let unitGrade1MCnt = 0; //1등급 밀리
        let unitGrade1RCnt = 0; //1등급 원거리
        let unitGrade2MCnt = 0; //2등급 밀리
        let unitGrade2RCnt = 0; //2등급 원거리

        let unitGrade1MArr;
        let unitGrade1RArr;
        let unitGrade2MArr;
        let unitGrade2RArr;

        do {
            unitGrade1MCnt = 0; //1등급 밀리
            unitGrade1RCnt = 0; //1등급 원거리
            unitGrade2MCnt = 0; //2등급 밀리
            unitGrade2RCnt = 0; //2등급 원거리

            unitGrade1MArr = [];
            unitGrade1RArr = [];
            unitGrade2MArr = [];
            unitGrade2RArr = [];

            for (let i = 0; i < locUnits[0].length - 1; i++) {
                for (let j = 0; j < locUnits.length; j++) {
                    if (locUnits[j][i]) {
                    //등급별 유닛 수 체크
                    if (locUnits[j][i]['grade'] === 1) {
                        if (i === 0) {
                            //근접
                            unitGrade1MCnt++;
                            unitGrade1MArr.push(j);
                        } else if (i === 1) {
                            //원거리
                            unitGrade1RCnt++;
                            unitGrade1RArr.push(j);
                        }
                    } else if (locUnits[j][i]['grade'] === 2) {
                        if (i === 0) {
                            //근접
                            unitGrade2MCnt++;
                            unitGrade2MArr.push(j);
                        } else if (i === 1) {
                            //원거리
                            unitGrade2RCnt++;
                            unitGrade2RArr.push(j);
                        }
                    }
                    }
                }
            }

            if ((unitGrade1MCnt >= 1 && unitGrade1RCnt >= 2) || (unitGrade1MCnt >= 2 && unitGrade1RCnt >= 1) || (unitGrade2MCnt >= 1 && unitGrade2RCnt >= 2) || (unitGrade2MCnt >= 2 && unitGrade2RCnt >= 1)) {
                //등급별 조합 가능 여부
                let isGrade1 = (unitGrade1MCnt >= 1 && unitGrade1RCnt >= 2) || (unitGrade1MCnt >= 2 && unitGrade1RCnt >= 1) ? true : false;
                let isGrade2 = (unitGrade2MCnt >= 1 && unitGrade2RCnt >= 2) || (unitGrade2MCnt >= 2 && unitGrade2RCnt >= 1) ? true : false;

                if (isGrade1) {
                    //성공, 실패 결과 유닛
                    let unitType = Math.floor(Math.random() * 2) + 1; // 0,1

                    // 성공 확률
                    if (Math.floor(Math.random() * 100) < GameSystem.GRADE2_SUCCESS_PER) {
                    if (unitGrade1MCnt >= 2 && unitGrade1RCnt >= 2) {
                        for (let i = 0; i < 2; i++) {
                            unitType === 1 ? (locUnits[unitGrade1MArr[i]][unitType - 1] = false) : (locUnits[unitGrade1RArr[i]][unitType - 1] = false);
                        }

                        unitType === 1 ? (locUnits[unitGrade1RArr[unitGrade1RArr.length - 1]][1] = false) : (locUnits[unitGrade1MArr[unitGrade1MArr.length - 1]][0] = false);

                        createUnit(locUnits, unitType, unitStr, 2);
                    } else {
                        if (unitGrade1MCnt >= 1 && unitGrade1RCnt >= 2) {
                            //근접1 원거리2 소모
                            for (let i = 0; i < 2; i++) {
                                locUnits[unitGrade1RArr[i]][unitType - 1] = false;
                            }

                            locUnits[unitGrade1MArr[unitGrade1MArr.length - 1]][0] = false;

                            //상급 유닛 Create
                            createUnit(locUnits, unitType, unitStr, 2);
                        }

                        if (unitGrade1MCnt >= 2 && unitGrade1RCnt >= 1) {
                            //근접2 원거리1 소모
                            for (let i = 0; i < 2; i++) {
                                locUnits[unitGrade1MArr[i]][unitType - 1] = false;
                            }

                            locUnits[unitGrade1RArr[unitGrade1RArr.length - 1]][1] = false;

                            //상급 유닛 Create
                            createUnit(locUnits, unitType, unitStr, 2);
                        }
                    }
                    logsPush(logs, chalk.blue(`[조합 성공] 중급 ${unitStr[unitType - 1]} 유닛이 소환되었습니다.`));
                    isSuccess = true;
                    } else {
                    //실패
                    unitType === 1 ? (locUnits[unitGrade1MArr[unitGrade1MArr.length - 1]][unitType - 1] = false) : (locUnits[unitGrade1RArr[unitGrade1RArr.length - 1]][unitType - 1] = false);
                    logsPush(logs, chalk.red(`[조합 실패] ${unitStr[unitType - 1]} 유닛이 소모되었습니다.`));
                    break;
                    }
                } else if (isGrade2) {
                    //성공, 실패 결과 유닛
                    let unitType = Math.floor(Math.random() * 2) + 1;

                    if (Math.floor(Math.random() * 100) < GameSystem.GRADE3_SUCCESS_PER) {
                    //성공
                    if (unitGrade2MCnt >= 2 && unitGrade2RCnt >= 2) {
                        for (let i = 0; i < 2; i++) {
                            unitType === 1 ? (locUnits[unitGrade2MArr[i]][unitType - 1] = false) : (locUnits[unitGrade2RArr[i]][unitType - 1] = false);
                        }

                        unitType === 1 ? (locUnits[unitGrade2RArr[unitGrade2RArr.length - 1]][1] = false) : (locUnits[unitGrade2MArr[unitGrade2MArr.length - 1]][0] = false);

                        createUnit(locUnits, unitType, unitStr, 3);
                    } else {
                        if (unitGrade2MCnt >= 1 && unitGrade2RCnt >= 2) {
                            //근접1 원거리2 소모
                            for (let i = 0; i < 2; i++) {
                                locUnits[unitGrade2RArr[i]][unitType - 1] = false;
                            }

                            locUnits[unitGrade2MArr[unitGrade2MArr.length - 1]][0] = false;

                            //상급 유닛 Create
                            createUnit(locUnits, unitType, unitStr, 3);
                        }

                        if (unitGrade2MCnt >= 2 && unitGrade2RCnt >= 1) {
                            //근접2 원거리1 소모
                            for (let i = 0; i < 2; i++) {
                                locUnits[unitGrade2MArr[i]][unitType - 1] = false;
                            }

                            locUnits[unitGrade2RArr[unitGrade2RArr.length - 1]][1] = false;

                            //상급 유닛 Create
                            createUnit(locUnits, unitType, unitStr, 3);
                        }
                    }
                    logsPush(logs, chalk.blue(`[조합 성공] 상급 ${unitStr[unitType - 1]} 유닛이 소환되었습니다.`));
                    isSuccess = true;

                    //업적 체크
                    if (!achievement.isMeleeMaxGrade || !achievement.isRangedMaxGrade) checkAhchiveUnit(logs, achievement, unitType - 1);
                    } else {
                    //실패
                    unitType === 1 ? (locUnits[unitGrade2RArr[unitGrade2RArr.length - 1]][unitType - 1] = false) : (locUnits[unitGrade2RArr[unitGrade2RArr.length - 1]][unitType - 1] = false);
                    logsPush(logs, chalk.red(`[조합 실패] 중급 ${unitStr[unitType - 1]} 유닛이 소모되었습니다.`));
                    break;
                    }
                }
            } else {
                logsPush(logs, chalk.red(`조합 가능한 유닛이 없습니다.`));
                isContinue = true;
                break;
            }
        } while ((unitGrade1MCnt > 0 && unitGrade1RCnt > 0) || (unitGrade2MCnt > 0 && unitGrade2RCnt > 0));

        if (isContinue && !isSuccess) {
            continue;
        } else if (isContinue && isSuccess) {
            break;
        } else {
            break;
        }
    ```

- **새로운 기능 구현** 
    1. 난이도
    2. 업적
    3. 아이템 드랍/사용
    
- 전체적인 코드 흐름 
![PlayProcess](https://github.com/user-attachments/assets/79078a64-698d-4e64-b816-71f6adbf7713)





