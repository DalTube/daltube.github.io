---
title: 트랜잭션(Transaction)
date: 2024-11-25 00:00:00
categories: [데이터베이스, Transaction]
tags:
  [
    데이터베이스,Transaction
  ]
---

# Transaction
---
작업의 완전성을 보장해주기 위해 사용되는 개념입니다. 특정한 작업을 전부 처리하거나, 전부 실패하게 만들어 데이터의 일관성을 보장해주는 기능.   
작업의 단위를 하나의 쿼리에 종속하는 것이 아닌, 여러개의 작업(쿼리)을 묶어 하나의 작업 단위로 그룹화하여 처리하는 작업.

#### 특징(ACID)
---
데이터베이스 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 특징들을 나열해 놓은 개념

1. **원자성(Atomicity)**
  - 트랜잭션 내에서 실행되는 명령들을 하나의 묶음으로 처리하여, 내부에서 실행된 명령들이 전부 성공하거나, 아니면 모두 실패해야한다는 특징. → 여기서, “원자성”이란, 나눠질 수 없는 단일 작업이라는 것을 의미.
  - 동시에 실행해야하는 여러개의 쿼리를 묶어서 관리.

2. **일관성(Consistency)**
  - 트랜잭션 내부에서 처리되는 데이터의 일관성을 유지해야하는 특징. 만약 작업이 성공할 경우 아무런 문제가 발생하지 않고, 실패하더라도 작업을 진행하던 도중 실패한 상태로 데이터를 방치하지 않는 특징.
  - 트랜잭션 내의 데이터는 일관되어야하며, 에러가 발생하더라도 데이터의 상태가 일관성을 유지해야 한다.

3. **격리성(Isolation)**
  - 트랜잭션이 실행 중인 경우 다른 트랜잭션에 의해 데이터가 변경되는 것을 방지하는 특징. 트랜잭션이 완전히 수행되거나 완전히 수행되지 않은 상태를 외부에서 참조할 수는 있지만, 트랜잭션의 중간 과정이나 중간 결과를 볼 수 없도록 하는 특징.
  - MySQL에서는 사용중인 DB 오브젝트에 락(Lock)을 걸어 격리성을 구현. 여기서 락(Lock)을 건 상태는 DB에 접속한 또다른 클라이언트가 해당 DB 오브젝트를 읽거나, 사용할 수 없도록 방지하여, 데이터 무결성을 보장.
  - 동시성(Concurrency)과 격리 수준(Isolation Level) : 여러 클라이언트가 동시에 하나의 데이터를 사용및 공유 하는 것을 뜻합니다. 동시성은 다수의 사용자가 동일한 시스템을 공유하면서 발생하는 동시 접근 문제를 해결해야 한다.

4. **지속성(Durability)**
  - 랜잭션이 성공적으로 커밋된 후, 해당 트랜잭션에 의해 생성 또는 수정된 데이터가 어떠한 상황에서도 보존되는 특징. 랜잭션이 완료되면 결과는 데이터베이스에 영구적으로 저장되며, 이후 시스템에 어떠한 문제가 생기더라도 데이터는 손상되지 않는다.

#### 사용법(MySQL)
--- 

```sql
-- 트랜잭션을 시작합니다.
START TRANSACTION;

-- 성공시 작업 내역을 DB에 반영합니다.
COMMIT;

-- 실패시 START TRANSACTION이 실행되기 전 상태로 작업 내역을 취소합니다.
ROLLBACK;
```

#### LOCK
--- 
동시성을 제어하기 위해 사용하는 기능입니다. 해당하는 데이터를 점유하여 다른 트랜잭션의 접근을 막아 동시성과 일관성의 균형을 맞추기 위해 사용

#### LOCK 종류
---
1. [**공유 락(Shared Locks) | 읽기 락(READ Locks)**](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-shared-exclusive-locks)
    - 다른 트랜잭션이 데이터를 **읽는 것은 허용**하지만, **수정하는 것을 금지**합니다.
    - **`READ` 전용 락**이라고 불리기도 하며, 해당 락을 사용하는 트랜잭션이 모든 작업을 수행하였다면 공유 락은 해제됩니다.
        ```sql
        # 트랜잭션을 시작합니다.
        START TRANSACTION;

        # SPARTA 테이블을 조회할 때, 해당 데이터들에 공유 락을 설정합니다.
        SELECT * FROM SPARTA LOCK IN SHARE MODE;
        ```
2. [**배타 락(Exclusive Locks) | 쓰기 락(WRITE Locks)**](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-shared-exclusive-locks)
    - 다른 트랜잭션이 데이터를 **읽거나, 수정하는 것을 금지**합니다.
    - **`WRITE` 전용 락**이라고 불리며, 트랜잭션이 해당하는 데이터를 점유한 후 다른 트랜잭션이 해당 데이터에 **접근 할 수 없도록** 만듭니다.
        ```sql
        # 트랜잭션을 시작합니다.
        START TRANSACTION;

        # SPARTA 테이블을 조회할 때, 해당 데이터들에 배타 락을 설정합니다.
        SELECT * FROM SPARTA FOR UPDATE;
        ```

#### 락킹 수준(Locking Level)
---

1. [**글로벌 락(Global Locks) | 데이터베이스 락(Database Locks)**](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
    - **데이터베이스의 모든 테이블에 락**을 걸어, 현재 트랜잭션을 제외한 나머지 트랜잭션들이 모든 테이블을 사용할 수 없도록 만듭니다.
    - **가장 높은 수준의 락**을 가지고 있으며, **가장 큰 범위**를 가지고 있습니다.
    - **예시 SQL**
        
        ```sql
        # 글로벌 락을 획득합니다.
        # MySQL 서버에 존재하는 모든 테이블에 락을 겁니다.
        FLUSH TABLES WITH READ LOCK;
        ```
        
2. [**테이블 락(Table Locks)**](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
    - 다른 사용자가 작업중인 **테이블**을 동시에 수정하지 못하도록 합니다.
    - **예시 SQL**
        
        ```sql
        # SPARTA 테이블에 테이블 락을 설정합니다.
        LOCK TABLES SPARTA READ;
        ```
        
3. [**네임드 락(Named Locks)**](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html)
    - 테이블이나 테이블의 행과같은 DB 오브젝트가 아닌, **특정한 문자열**을 **점유**합니다.
    - **예시 SQL**
        
        ```sql
        # sparta_name 문자열을 획득합니다.
        # 만약, 10초 동안 획득 하지 못한다면, NULL을 반환합니다.
        SELECT GET_LOCK('sparta_name', 10);
        ```
        
4. [**메타데이터 락(Metadata Locks)**](https://dev.mysql.com/doc/refman/8.0/en/metadata-locking.html)
    - 다른 사용자가 작업중인 테이블의 **동일한 행 및 동일한 데이터베이스의 객체**를 동시에 수정하지 못하도록 합니다.
    - **예시 SQL**
        
        ```sql
        # 테이블 구조를 변경할 때, MySQL은 내부적으로 메타데이터 락을 설정합니다.
        ALTER TABLE SPARTA ADD COLUMN Age Int;
        ```

#### 교착 상태(Dead Lock)
---
여러 테이블에 락(Lock)을 적용하여, 다른 작업이 처리되지 못하게 점유하고 있는 작업이 있을 때, 다른 작업을 끝나는 것을 무한정 기다리는것
![Dead Lock](https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F88e5dd69-0c9a-4d1f-a027-57830e276d0f%2Fdeadlock.png?table=block&id=b88fae44-1146-4142-b82a-7bf66109591e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1060&userId=&cache=v2)

#### 트랜잭션의 격리 수준 (Isolation Level)
---
여러 트랜잭션이 동시에 처리될 때 다른 트랜잭션에서 변경 및 조회하는 데이터를 읽을 수 있도록 허용하거나 거부하는 것을 결정하기 위해 사용하는 것. (‘데이터의 일관성’과 ‘동시성 처리 성능’ 사이에서 균형을 잡는 것)

**`READ UNCOMMITTED`**

- **커밋 되지 않은 읽기(Uncommitted Read)**를 허용하는 격리 수준입니다.
- 가장 낮은 수준의 격리수준이며, 락을 걸지 않아 동시성이 높지만 일관성이 쉽게 깨질 수 있습니다.

**`READ COMMITTED`**

- **커밋 된 읽기(Committed Read)**만을 허용하고, `SELECT` 문을 실행할 때 **공유락**을 겁니다.
- 다른 트랜잭션이 데이터를 **수정하고 있는 중**에는 **데이터를 읽을 수 없어** 커밋되지 않은 읽기현상이 발생하지 않습니다.

**`REPEATABLE READ`**

- 읽기를 마치더라도 **공유락**을 **풀지 않으며**, 트랜잭션이 **완전히 종료될 때 까지 락을 유지**합니다.
- 공유락이 걸린 상태에서 데이터를 수정하는 것은 불가능하지만, 데이터를 삽입하는 것이 가능해집니다. 그로인해 **팬텀 읽기**가 발생할 수 있는 문제점이 있습니다.

**`SERIALIZABLE`**

- 데이터를 읽는 동안 다른 트랜잭션이 해당 데이터를 읽거나 삽입할 수 없고, 새로운 데이터를 추가하는 것 또한 불가능합니다.
- **가장 높은 수준의 격리 수준**이므로, 동시성이 떨어지는 문제점이 존재합니다.

---
- **커밋되지 않은 읽기(Uncommitted Read)**
    **커밋되지 않은 읽기(Uncommitted Read)**는 다른 트랜잭션에 의해 작업중인 데이터를 읽게 되는 것을 나타냅니다. 만약 커밋되지 않은 읽기가 발생할 경우, **의도치 않은 데이터를 참조**하게 되어 **데이터의 일관성이 깨지게 되는 상황**이 발생하게됩니다.
    
- **팬텀 읽기(Phantom Read)**
    트랜잭션을 수행하던 중 **다른 트랜잭션**에 의해 **삭제된 데이터**를 **팬텀행(Phantom Rows)**이라고 합니다. 여기서, **팬텀행**에 해당하는 데이터를 읽는 것을 **팬텀 읽기(Phantom Read)**라고 부릅니다.