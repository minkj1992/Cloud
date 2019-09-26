# Gaming on Google Cloud
> [Link](https://events.withgoogle.com/gaming-on-google-cloud-4/?fbclid=IwAR0fqw3qgxc85Fra8sO1lGgMYBYo_PTn_7wpVYehWfjzfFET109_UAv28hY)

## 1.Amy Krishnamohan
Cross platform : 콘솔, 모바일 .. 
Game As A Service : Dynamic Gaming 언제나 유저가 들어올 수 있는
Competition : 이제 게임은 미디어 매체들과 경쟁한다. netflex, game
    - 컨텐츠에 대한 경쟁

Realtime, Availability(끊기지 않음)

 
MySql -> 샤딩 -> NoSql - > Spanner(확장성, 일관성)
Hadoop, Apache open source

- open source에 대한 애정
redis, elastic, mongoDB, 쿠버네틱스
(파트너 서비스)오픈소스를 만드는 업체들과 협력하여, 구글 클라우드안에서 사용가능하도록 한다 (내년 초)



1. in-memory(Redis)
2. Non relational
    - firebase
        - 모바일 게임에 굉장히 적합 ( vs MongoDB)
        - off-line sync(모바일에서 캐싱을 했다가 sync함)
    - bigTable
        - latency가 굉장히 낮음
        - 데이터가 굉장히 크다면, 유리
3. cloud spanner
    - 일관성, 확장성을 둘다 가지고 있는 혁신 DB
    - global한 consistency를 지키도록 하는 DB


## 2. `Cloud SQl`

`cloud pub/sub` -> `쿠버네틱스` -> `sql Server cloud`

fail over를 대비하여 동시에 2곳에 backup 시킨다.


## 3. Cloud Memorystore
latency는 중요하다 그러므로 caching layer가 중요(Redis, db와 app사이에 caching) 

## 4. Database migration(`GCP`)

### 단계
1. assessment
    - risk 체크
    - posql -> oracle? ,,
2. Env setup
    - query conversion
3. migarte
4. check

### GCP
- `migVisor`, `striim`
- Data Migration Service(DMS)

### Homo DB migration  vs ## Hetro 

### migvisior, striim, DMS(google)
Check해서 위험한 스키마 찾아줌.
H12020 plan - 구글 plan

## 5. 최유정, GCP-Cloud Native DB
RDB는 scale out(수평확장) 불리. scale-up하는 방식밖에 없다.
그래서 sharding을 해서 db를 쪼개기 시작한다.(관리부터 엄청 고통스럽다, app도 쪼개야하고 db도 함께 쪼개야한다.)

수평확장에 용의한 No-SQL 최적화됨. self-managed하기 매우 어렵다네

- game db case
    - gaming leaderboard
    - profile metadata(사용자 정보)
    - game state(Big table, game log 트랜잭션)
        - Big query를 통한 분석
        - Big query ML

`cache - RDB - Big Table(log)`

- What is Spanner? 2017

> `Relational semantics + Horizontal scale`

    - `ACID, transactions, SQL + SLA, Fully manages and Scalable`
    - NoSql의 수평확장성과 SQL의 Relational을 가져옴
    - gmail, storage
    - 서울도 곧 spanner가 가능해진다. region
> Building Blocks in Spanner (어떻게 가능하지?)


- 1. google network infra
- 2. TrueTime
    - 원자시계(시간 보장 in worldwide의 )
    - 분산시스템(정확한 시계가 있고 없고는 엄청난 차이, )
    - `strong global consistency`: 어디서든 쓸수있고(엄청난 장점), across region
    - 멀티마스터 (특정 read after write critical 보장해준다 spanner가)
        - `eventual consistency` 다른 이벤트에 대하여 참조할 수 없지 -> exponential backoff
        - 특정 instance안에서만 해야한다는 한계점

- 3. Optimized Software Stack
    - Custom paxos
    - fully managed(resharding 같은 것들이 사용자의 개입없이 알아서)


- consistent secondary indexs
    - Nosql에서는 eventual consistance만 가능해서 retrial
    - spanner는 이게 가능하다.
    - 이게 얼마나 편한지 아냐? 

- Interleave구조를 제공하면서 parent-child db값 빠르게 가져올 수 있게 하였다.(c.f composite key??)


## 6. What is Cloud Firestore?
- firebase + 기존 DB 팀(이번 년에 launch)
- serverless DB
    - 사용자 수에 상관없이 같은 code를 가질 수 있다.
- hyper connected, off-line, sync (airplane, mountain, ..)
    - reconnect sync 한다.

- DATA MODEL (COllection - DOCUMENT - DATA)
    - DATAstore mode: 
    - Native mode: 
        - newer feature
        - realtime sync logic


## 7. What is Cloud Bigtable

> 장점

- seamless cluster resizing
    - Dynamically add/remove cluster nodes without restating

- 자동 rebalancing & resizing


## 8. 질문 

- sql -> spanner
어느정도의 변화가 적용되야 할까?

- cloud native 적용을 한다면, cloud에서도 가능하고 on-frame에서도 가능하게 제공이 될지?
ㅇㅇ, 앞으로 하려고 할것이다.

- NoSql 2nd index in firestore
not support

- spanner multi region
    - RO RW region이 구별되어있고, reroute(구글이 알아서)
    - type별로 가능(미국, 유럽)

- spanner의 특징이 transaction cell단위는 가능한데, low level보다 cell이 더 작은 단위


## 9. Ark Order(회사,D&C) on Google Cloud
DB과 구글 기술이 어떤식으로 적용이 되고 있는지.

- 구글 클라우드 장점, AWS Cloud, Naver Cloud
    - 1. Google Cloud
        - 한국 region이 없다. Tokyo region사용
    - 2. 성능적으로 다 비슷, VM google이 비교적 저렴
        - 사용하고 싶은 만큼 core, mem을 수정가능해서 vm 비용 저렴
    - 3. 리소스 용량이 상당한데 (3GB) -> CDN이 고려대상
        - 구글이 가장 매력적인 CDN
    - 4. 직관적인 interface, 따로 머신을 두지 않아도 서버 monitor
    - 5. AWS는 VPC를 별도의 작업을 해야했지만 구글은 아니여서 편했다.

- 개선 포인트, 단점
    - 웹상으로 구동이 되는데 클라우드가.. 크롬이 무겁고 구글 console로 돌아올때 응답없음.
    - DB관점 개선: 개발자 측면에서 parse DB의 option(database flag)처리하는게 필요



## 10. 용어정리
- Down_time
- SLA?
- Big Query(ware House)
- IAM/ encryption
- Legacy game 
- REST / gRPC
- .net ecosystem
- windows on gke
- failover, 무장애 
- HBase
- ROI
- POC scenario
- workload
- Dedicated Game servers
- SLA: 멀티 region SLA(5,9)
- DDL statements
- conquerency control
- JDBC Driver
- Compliance
- zonal instances
- 클러스터 - node
- access pattern
- onFrame
- cloud CDN
- single VPC
