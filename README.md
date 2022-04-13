
# 게임 배틀그라운드 데이터 EDA를 통한 Top10전략 알아보기
### <배틀그라운드 뉴비, 치킨 못먹어도 Top10은 쉽게 하자!>

## 1.주제선정

![image](https://user-images.githubusercontent.com/80455724/121173056-5531b200-c893-11eb-8fd0-bb8f74b96599.png)

- 지금까지 인터넷이나 유튜브에서 볼 수 있던 배틀그라운드 전략은 대부분은 에임 실력이 좋은 플레이어를 대상으로 1등을 취하기 위한 전략이 많았음. 
- 배틀그라운드 뉴비를 위하여 top10에 안정적으로 들 수 있는 효율적인 생존전략이 궁금했다.
- 이번 EDA프로젝트를 통해 확인하고자 하는 것은 다음과 같다.
    - 1. 플레이어의 생존에 큰 영향을 미치는 핵심 Feature는 무엇인가. 
    - 2. 뉴비 플레이어가 안정적으로 top10에 들기 위해서는 어떻게 플레이해야할까?
        - 1) 솔로, 듀오, 스쿼드 3가지 모드 중 어떤 모드에서 뉴비가 오랫동안 생존하기 좋은가.
        - 2) 안정적으로 top10에 들기 위해 게임당 적에게 줘야할 데미지, 최소 몇 킬을 기록해야 하는가.
        - 3) 게임당 부스트아이템, 회복아이템은 얼마나 써야 하는가.
        - 4) 플레이어의 생존 확률을 높이기 위한 효율적인 이동전략은 무엇인가.

#
### 1-1. 데이터 설명
#### 데이터 수집 : Kaggle 배틀그라운드 데이터셋
- https://www.kaggle.com/leonardokarl/pubg-statisctic

#### 데이터셋
- 데이터셋 : 게임모드별로 Solo, Duo, Squad 3개의 데이터셋을 합쳐서 활용
- 데이터를 합쳐 263694 rows × 51 columns의 데이터셋을 만들었고 결측치는 존재하지 않았음.

<img width="601" alt="스크린샷 2022-04-13 오후 7 20 50" src="https://user-images.githubusercontent.com/80455724/163157693-86350c6d-121a-4e63-aba4-82698d81074c.png">


#### 변수
- 플레이어의 총 조준실력 관련 변수: KillDeathRatio, DamagePg, KillsPg, Kills, HeadshotKillsPg, HeadshotKills, HeadshotKillRatio, Assists, DailyKills, WeeklyKills, RoundMostKills, MaxKillStreaks, LongestKill, DamageDealt
- 생존시간 관련 변수: TimeSurvived, TimeSurvivedPg, LongestTimeSurvived, MostSurvivalTime, AvgSurvivalTime,
- 아이템 사용갯수 관련: HealsPg, Heals, Revives, Boosts	
- 이동거리 관련: MoveDistancePg, WalkDistance, RideDistance, MoveDistance, AvgWalkDistance, AvgRideDistance
- top10 및 우승 관련: Losses, WinRatio, Wins, WinTop10Ratio, Top10s, Top10Ratio, Top10sPg, WinPoints,
- 기타: player_name, RoundsPlayed, Rating, BestRating, RevivesPg, RoadKillsPg, RoadKills, TeamKillsPg, TeamKills, Suicides, VehicleDestroys, Days, DBNOs,GameMode





#
## 2. 데이터 전처리

#### <데이터 전처리 방법>

- 중복되는 컬럼은 삭제(횟수컬럼 drop하고 확률 및 Pg컬럼으로 데이터셋 구성)하였음
<img width="586" alt="스크린샷 2022-04-13 오후 7 43 43" src="https://user-images.githubusercontent.com/80455724/163163920-1babf5ea-cabb-412d-b913-b9ed754777fe.png">

- 플레이한 라운드횟수컬럼로 나누어 게임당 횟수 컬럼 추가
<img width="587" alt="스크린샷 2022-04-13 오후 7 23 20" src="https://user-images.githubusercontent.com/80455724/163159900-0330b284-d89e-42b2-9be2-12cefa0cc909.png">

- EDA의 용의성을 위해 일부 수치형데이터 범주화
<img width="585" alt="스크린샷 2022-04-13 오후 7 23 40" src="https://user-images.githubusercontent.com/80455724/163159938-39f5ab5e-b80f-4a7b-bc19-f48ad511cb8f.png">






#
## 3. EDA

#### <EDA를 통해 살펴볼 것>
1. 플레이어의 생존에 큰 영향을 미치는 핵심 변수는 무엇인가. 
2. 뉴비 플레이어가 안정적으로 top10에 들기 위해서는 어떻게 플레이해야할까?
    - 1) 솔로, 듀오, 스쿼드 3가지 모드 중 어떤 모드에서 뉴비가 오랫동안 생존하기 좋은가.
    - 2) 안정적으로 top10에 들기 위해 게임당 적에게 줘야할 데미지, 최소 몇 킬을 기록해야 하는가.
    - 3) 게임당 부스트아이템, 회복아이템은 얼마나 써야 하는가.
    - 4) 플레이어의 생존 확률을 높이기 위한 효율적인 이동전략은 무엇인가.


## 1) 플레이어의 생존에 큰 영향을 미치는 핵심 Feature는 무엇인가.
Top10에 들 확률 컬럼과 상관계수가 높은 컬럼을 추려보았다.

<img width="681" alt="스크린샷 2022-04-13 오후 6 27 14" src="https://user-images.githubusercontent.com/80455724/163159985-805683f5-ba00-4560-9cad-334d68daff70.png">

#### 타켓변수: Top10ratio
#### 활용할 변수
- 이동거리관련 컬럼 : WalkDistancePg, RideDistancePg, MoveDistancePg
- 아이템 사용관련 컬럼 : HealsPg, BoostsPg
- 조준실력(에임) 관련 컬럼 : DamagePg, KillsPg, KillDeathRatio
- 생존시간컬럼 : TimeSurvivedPg
#### 상기의 컬럼들을 살펴보았다.


## 2) 뉴비 플레이어가 안정적으로 top10에 들기 위한 생존전략은 무엇인가.

### 2-1) 솔로, 듀오, 스쿼드 3가지 모드 중 어떤 모드에서 뉴비가 오랫동안 생존하기 좋은가.

<img width="397" alt="스크린샷 2022-04-13 오후 7 30 04" src="https://user-images.githubusercontent.com/80455724/163160043-cb7aec45-7481-465a-8d23-58b64ffe48d1.png">

- (k/d, 데미지, 킬수 등) 총 쏘는 실력과 관련된 컬럼은 squad -> duo -> solo 일수록 더 높은 상관계수를 보인다. => 솔로모드에서는 top10에 들기 위해서는 플레이어 개인의 에임실력이 중요하다.
- 그 외(아이템 사용 및 이동관련)의 컬럼은 solo -> duo -> squad 일수록 더 높은 상관관계를 보인다. => 팀원이 많은 모드일수록, 아이템의 사용방법과 이동방식과 같은 플레이어의 운영능력이 top10에 들 확률을 좌우한다.
- 
####  뉴비가 Top10에 쉽게 들기 위해서는 '스쿼드'모드를 플레이하는 것이 가장 유리하다!




### 2-2) 안정적으로 top10에 들기 위해 게임당 적에게 줘야할 데미지, 최소 몇 킬을 기록해야 하는가.

<img width="1009" alt="스크린샷 2022-04-13 오후 7 29 42" src="https://user-images.githubusercontent.com/80455724/163160451-14582a6e-e806-43f8-a0d5-79313a298c54.png">

- 스쿼드모드 기준, 한판당 3.9킬, 평균 데미지의 양을 323이상 기록하면 Top10 진입확률을 극대화할 수 있다.




### 2-3) 게임당 부스트아이템, 회복아이템은 얼마나 써야 하는가.

- 스쿼드모드 기준, 회복아이템은 약 2.8개 이상, 부스트아이템은 약 2개 이상 사용하는 것이 이상적.




### 2-4) 플레이어의 생존 확률을 높이기 위한 효율적인 이동전략은 무엇인가.

<img width="1024" alt="스크린샷 2022-04-13 오후 7 30 49" src="https://user-images.githubusercontent.com/80455724/163161434-5f9e097c-4987-435f-81c7-199bc4b4fc5d.png">

### 생존률을 극대화하기 위한 효율적인 이동전략은 무엇인가?
- top10에 들 확률로 나눈 범주 간 RideDistance가 확연한 차이를 보이는 것을 감안했을 때, 탈것을 적극적으로 이용하는 것이 top10에 들어갈 매우 효율적인 전략이 될 것.
#### 탈것을 이용하여 3500m이상 이동한다면 생존률을 비약적으로 높일 수 있다.




#
## 최종 결론 

#### Top10에 들 확률을 높이기 위해서는 어떻게 플레이 해야할까?

1) 뉴비에게 어떤 모드가 쉬울까?
- 팀원이 많은 모드일수록 에임실력보다는 아이템의 활용과 이동전략이 생존에 중요한 영향을 미친다.
- 따라서 뉴비는 스쿼드모드를 플레이어해야 Top10을 쉽게 할 수 있다.

2) 게임당 적을 몇명 kill할 수 있어야 하는가.(스쿼드 기준 top10확률 50~75%)
- Kill/Death : 약 2.0 이상
- 게임당 Damage : 231 (한명을 죽이기 위해 필요한 데미지: 100)

3) 게임당 아이템을 얼마나 필요한가.(스쿼드 기준 top10확률 75%이상)
- 회복아이템 : 약 3개 이상 (2.8)
- 부스트아이템 : 약 2개 이상 (2.3)

4) 생존률을 극대화 하기 위해서는 어떤 이동전략을 취해야 하는가.(스쿼드 기준)
- k/d에 따라 구분한 그룹 간 Ride Distance의 차이가 작음에도, Ride Distance가 길면 top10확률이 비약적으로 높아짐을 확인함.
- 걸어서 이동하는 것보다 탈 것을 적극적으로 활용하는 것이 생존률을 극대화할 수 있음.
- 이동거리 : 많으면 많을수록 좋다. 약 5000M이상
- (걸어서 이동: 약 2000m 이상, 차타고 이동: 약 3000m 이상)
