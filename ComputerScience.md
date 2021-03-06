# OS

 ## 프로세스와 스레드

### 프로세스

프로세스란 실행중인 프로그램, CPU가 할당되는 단위를 의미한다. 프로세스는 대기, 실행 등 여러 상태를 전이하며 수행을 계속하고, 수행 중 컴퓨터에 자원을 요청할 수 있다.

**PCB**(Process Control Block): OS가 프로세스를 제어하는 데 사용하는 단위. 프로세스 ID, 상태정보, 스케줄링 정보 등을 담고 있다.



### 프로세스의 상태

* 생성(New): 사용자의 요청이 커널에 등록되고 프로세스가 생성된 상태. 메모리에 공간이 있다면 준비상태로, 없다면 대기상태로 전이된다.
* 활성(Active): 프로세스가 기억장치를 할당받은 상태
  * 실행(Running): CPU를 할당받아 현재 수행되고 있는 상태
  * 준비(Ready): CPU 할당만을 기다리는 상태
  * 대기(Block): 자원을 요청하고 자원을 할당받길 기다리는 상태. 예를 들면 사용자의 입력을 기다리는 상태. 자원을 할당받으면 다시 준비상태로 들어감.
* 종료(Exit): 더이상 CPU를 요구하지 않고 종료된 상태
* 대기(Suspended): 프로세스가 기억장치를 할당받지 못한 상태



문맥교환(**Context Switching**): 중단된 프로세스의 현재 정보를 저장하고, 새로운 프로세스를 시스템에 적재시키는 것



### 프로세스 스케줄링

여러개의 프로세스가 생성되고 수행되지만 프로세서는 한정적. **한정된 자원을 각각의 프로세스에 어떤 순서로 배분할지 결정**하는 것이 프로세스 스케줄링. 스케줄링 기법을 올바르게 채택하기 위해 시스템의 속성, 프로세스의 속성, 응답시간의 중요성 등을 고려해야 한다.



#### 선점형 스케줄링

프로세스가 CPU를 할당받아 수행되는 도중에, 자신보다 더 높은 우선순위를 가진 프로세스에게 CPU를 양보하는 스케줄링. Context Switching을 위한 오버헤드가 일어날 수 있다.

* Round Robin: **시간 할당량**을 정해놓고, 프로세스가 수행되다가 시간할당량을 초과하면 다음 프로세스에게 CPU를 양보하고 대기열의 맨 뒤로 가서 대기하는 기법. 시간할당량이 너무 크면 FCFS와 다를바가 없어지고, 너무 작으면 Context Switching을 위한 오버헤드가 커질 수 있으므로 적절히 설정하는게 중요하다.
* SRT(Shortest Remaining Time): 프로세스 대기열에 더 새롭고, 더 처리시간이 짧은 프로세스가 들어오면 그 프로세스가 프로세서를 선점하는 기법

#### 비선점형 스케줄링

한번 CPU를 할당받은 프로세스는 수행이 끝날때까지 CPU를 반납하지 않고 유지하는 스케줄링. 기아상태가 발생할 수 있다.

* FCFS(First Come First Served): 대기열에 들어온 순서대로 처리
* SJF(Shortest Job First): 수행시간이 짧은 것부터 처리. 수행시간이 긴 프로세스가 기아상태에 빠질 수 있다.
* 우선순위 알고리즘: 프로세스의 우선순위가 높은 것부터 처리



### 스레드

프로세스보다 작은 실행단위. 하나의 프로세스를 처리하는 속도를 높이기 위해 여러 개의 스레드가 실행될 수 있다. 각각의 스레드는 **독립된 제어 흐름**을 갖지만 자원은 **동일한 프로세스 내에서 공유**한다.



## 데드락(Deadlock)

서로 다른 프로세스가 자원을 점유한 상태로 상대방의 자원을 내놓길 기다리며 무기한 대기에 빠지는 상태. 



### 데드락의 발생조건

다음 네가지 조건이 **모두** 만족될 때 데드락이 발생한다.

* 상호배제(MUTEX): 한 프로세스가 자원을 점유하고 있을 때, 다른 프로세스는 그 자원에 접근할 수 없음
* 점유와 대기: 프로세스가 자원을 점유한 상태로, 다른 자원을 얻을 때까지 대기하는 상태
* 비선점: 한 프로세스가 자원을 점유하고 있을 때, 다른 프로세스는 그 자원을 뺏을 수 없음
* 환형대기: 프로세스와 자원의 대기 상태가 원형을 유지해 맞물린 상태



### 해결방법

* 예방기법: 데드락의 발생조건 중 하나 이상을 차단하는 방법.
  * 상호배제, 비선점 차단은 잘 사용되지 않는다. 자원이 공유될 때 발생할 수 있는 문제가 더 크기 때문
  * 점유와 대기를 차단하면, 자원의 활용률이 떨어지고 기아상태가 발생할 수 있다.
  * 환형대기: 모든 프로세스가 단방향으로만 자원을 요구하게 하는 방법. 예방기법중에서 제일 좋은 방법
* 회피기법: 프로세스가 자원을 요구할 때, 안전한 상태인지 계산 후 안전할 때 자원을 분배하는 방법
* 발견과 회복: 주기적으로 상태를 검사해 데드락이 발견되면 회복하는 방법. **프로세스를 종료**하거나, **우선순위가 더 높은 프로세스가 자원을 선점**하도록 하는 방법이 쓰인다.



# DB

## 트랜잭션

데이터베이스에서 일어나는 상태 변환 과정의 **논리적 작업 단위**. 하나의 작업을 수행하기 위한 일련의 연산들로 구성됨.



### 트랜잭션의 성질 (ACID)

* 원자성 Atomicity: 
  트랜잭션은 한 덩어리로 처리된다. 연산 중 하나라도 실패하면 전체 트랜잭션이 모두 취소되어야 함.
* 일관성 Consistency: 트랜잭션이 성공하면 언제 어디서나 일관된 상태를 유지함.
* 격리성 Isolation: 한 트랜잭션이 수행되는 도중에 다른 트랜잭션의 연산이 끼어들지 못함. 수행중인 트랜잭션은 종료되기 전까지 다른 트랜잭션에서 참조할 수 없음
* 영속성 Durability: 성공적으로 수행된 트랜잭션의 결과는 시스템이 고장나더라도 영원히 반영되어야 함



### 트랜잭션의 상태

![img](https://doooyeon.github.io/assets/img/post/transaction-status.png)



### 트랜잭션의 격리수준

트랜잭션의 성질 중 격리성(Isolation)에 관한 문제. 처리속도를 높이기 위해 병렬성을 높이는 대신 격리성을 포기하거나, 데이터의 정합성을 지키기 위해 격리성을 높이고 병렬성을 낮춰야 한다. 



**격리성에 관련된 문제**

* Dirty Read: 한 트랜잭션이 A 데이터를 B로 변경한 후, 다른 트랜잭션이 데이터를 읽으면 B라고 읽는다. 그 후 원래 트랜잭션이 변경을 취소하고 데이터를 A로 다시 돌려놓으면 B를 읽던 트랜잭션의 데이터가 꼬이게 되는 것.
* Non-Repeatable Read: 트랜잭션이 데이터를 읽고 있는데, 다른 트랜잭션이 그 데이터를 변경/삭제하는 것
* Phantom Read: 트랜잭션이 데이터를 추가한 뒤, 다른 트랜잭션이 데이터를 읽으면 추가된 데이터까지 읽게 됨. 그 후 원래 트랜잭션이 롤백을 해서 변경을 취소하면 다른 트랜잭션은 없는 데이터를 읽고 있는게 됨.



**격리수준**

1. Read Uncommitted: 커밋하지 않은 데이터도 읽을 수 있음. 모든 격리성 문제가 발생할 수 있음. RDBMS에서는 격리성 수준으로 인정하지도 않는 단계
2. **Read Committed:** 커밋이 완료된 데이터만 읽을 수 있음. Dirty read 문제가 해결. 오라클이 기본으로 채택, 온라인 서비스에서 가장 많이 쓰이는 격리 수준
3. **Repeatable Read**: 한 트랜잭션이 읽은 데이터는 그 트랜잭션이 끝날 때까지 변경/삭제가 불가능함. Non-repeatable read 문제가 해결. MySQL의 InnoDB가 기본으로 채택.
4. Serializable: 가장 높은 격리수준. 모든 격리성 문제를 해결할 수 있지만 동시처리성능은 떨어진다.

# 