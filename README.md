<div align="center">
  <h1>📝 개인 프로젝트 : BattleGround_Anomaly_Detection<br><br>
  🕹 배틀그라운드 비인가 프로그램 탐지 프로젝트</h1>
</div>

<h4> 💭 Language : Python <br><br>
     📝 Library : Pandas, Numpy, Matplotlib<br><br>
     🛠  Tool : Google Colab <br><br>
     📅 진행기간 : 2022.02.21 ~ 2022.03.06</h4>
     
### 👨‍👦‍👦 팀원소개
<table>
<tbody>
  <tr>
    <td align="left"><img src="" width="20px;" alt=""/><br /><b>팀원 : 이희구</b></a><br /></td>
   <tr/>
</tbody>
</table>
<br>
<h3 align="center"><img src="https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/713237a7-b697-43ef-92fb-04ed1e62caa7" height = 600px></h3>


# 🔊 프로젝트 개요
* 게임에서 비인가 프로그램을 사용하는 유저가 전보다 크게 증가하여 일반 유저들의 게임 플레이에 악영향을 끼치는 현상이 발생<br>

* 요즘 게임 회사들도 비인가 프로그램을 사용하는 유저들을 잡기 위해 많은 노력 시행<br>

* 게임 상에서 비인가 프로그램을 사용하면 다른 일반 유저들이 피해를 입고 게임 유저 이탈을 할 수 있으며, 그로 인해 게임이 서버 종료까지 이를 수 있기 때문에 매우 심각한 문제로 분류<br>

* 게임 회사들은 비인가 프로젝트 사용 유저들을 검거하기 위해 IT부서를 구축하고, 게임 이용 제한 등 강력한 처벌 강화책을 도입

<br><br>

# 💡 분석 기획
* 일반 유저와 핵 유저 분류 분석<br>

* 게임 데이터를 활용하여 분석, EDA, 프로젝트의 전체적인 흐름 및 아키텍처 구상<br>

* isolation Forest를 활용한 이상치 탐지 분석<br><br>
<br><br>

# 🔥 목적 및 필요성
* <strong>게임의 공정성 유지</strong>
> - 공정한 경쟁 환경을 훼손하고, 합법적인 플레이어들의 게임 경험을 해침
> - 비인가 프로그램 탐지는 이러한 플레이어들을 감지하여 제재를 가할 수 있도록 도움을 줌

* <strong>게임 경제의 안정성</strong>
> - 비인가 프로그램을 사용하는 플레이어들은 게임 내 아이템을 불법적으로 얻거나 거래할 수 있음
> - 이는 경제의 불안정성을 초래하고, 합법적인 경제 활동에 영향을 미침
> - 비인가 프로그램 탐지는 게임 경제의 안정성을 유지하고 합법적인 거래를 보호하는 데 도움을 줌

* <strong>보안 및 개인 정보 보호</strong>
> - 일부 비인가 프로그램은 악성 코드를 포함하고 있을 수 있으며, 게임 클라이언트나 서버에 해킹을 시도할 수도 있음
> - 이는 게임 플레이어들의 개인 정보를 위협하고 게임 운영에 대한 보안 취약점을 악용할 수 있음
> - 비인가 프로그램 탐지는 게임 시스템의 보안을 강화하고 플레이어들의 개인 정보를 보호하는 데 도움을 줌

* <strong>게임의 규모 및 성장</strong>
> - 온라인 게임은 많은 플레이어들이 동시에 참여하는 대규모 다중 플레이어 경험을 제공
>  - 비인가 프로그램을 사용하는 플레이어들은 게임 서버에 부하를 주거나 불법적인 활동으로 게임 경험을 해칠 수 있음
>  - 이는 게임 서버의 성능 저하나 플레이어들의 게임 참여 의욕 감소로 이어질 수 있음
>  - 비인가 프로그램 탐지는 게임의 규모와 성장을 지원하고 안정적인 플레이 경험을 제공하는 데 도움을 줌

<br><br>

# 🔎 가설 설정
* winPlacePerc = 1은 우승을 의미<br>

* 킬수가 전부 헤드샷 킬수인 경우 '헤드샷 킬수 / 실제 킬수'로 정의

 <h4>가설 1 : 게임 우승 유저 가운데 aim_point가 이상치인 경우, 비인가 프로그램을 사용하는 유저일 것이다.</h4>
 
> * 이유 : 보통 핵을 쓰는 유저가 정상 유저보다 높은 aim_point 수치로 게임에서 우승을 하기 때문이다.<br>
<br>

 <h4>가설 2 : 게임 내 킬 중에서 헤드샷 킬의 비율이 비정상적으로 높다면 에임핵을 사용하는 유저일 것이다.</h4>
 
> * 이유 : 배틀그라운드 게임은 총기 반동이 심하기 때문에 헤드샷 킬이 쉽지 않다. 보통 'AWM', 'K98' 등 저격총을 이용해서 헤드샷 킬이 잘 나오긴 하지만, 그것마저 흔한 킬이 아니기 때문이다.<br>

<br><br>

# 🔎 데이터 수집
|데이터셋|출처|
|------|:------:|
|해양환경측정망 데이터|관할해역해양정보 공동활용시스템|
|국가 해양생태계 데이터|해양환경공단|
|갯끈풀 위치 데이터|네이처링|
|바다 격자 정보(격자 3단계)shp 데이터|바다누리 해양정보 플랫폼|
|갯벌 현황도 shp데이터|해양수산부 연안 포털|

<br><br>

# 🔎 데이터 전처리
|데이터셋|출처|
|------|:------:|
|불필요한 컬럼 제거|분석에 필요 없는 컬럼 제거|
|'aim_point' 컬럼 생성|헤드샷 킬수 / 실제 킬수로 정의|
|'aim_point' 컬럼의 결측값 처리|결측값을 0으로 대체|

<br><br>


# 📊 EDA (탐색적 데이터 분석)
## 1) 정상 유저와 핵 유저의 플레이 타입
<h3 align="center"><img src= "https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/b533dfa5-a1a5-4f05-8469-2b6491e96991"></h3>

* 정상 유저와 핵 유저의 플레이 타입은 크게 다르지 않다.<br>

* 이것은 플레이 타입에 관계 없이 핵 유저들이 고르게 존재한다는 것을 의미한다.

<br>

## 2) 정상 유저와 핵 유저의 흭득 무기수
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/d2deaa30-1a46-495f-80eb-71850ae5b729></h3>

* <strong>가설 : 핵 유저는 파밍을 많이 하지 않을 것이다. → 오로지 aim에만 의존하기 때문</strong><br>

* 무기획득에 대한 평균 검정을 한 결과 p값이 0.05보다 작아 통계적으로 유의한 차이가 있다.<br>

* 가설대로 핵 유저의 무기획득의 평균 수가 적음을 확인하였다.<br>

* 따라서 핵 유저는 파밍을 많이 하지 않는다.

<br><br>

## 3) 정상 유저와 핵 유저의 killStreaks 수 (짧은 시간 내에 동시 처치)
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/3dc60fe6-2d62-4227-89cf-1bdb40aa46fa></h3>

* 핵 유저는 동시 처치가 거의 1에 수렴한다.<br>

* 동시 처치에 대한 평균검정을 한 결과 핵유저가 정상 유저에 비해 'killSteaks'가 적다.<br>

* 핵 유저는 짧은 시간 안에 여러 명을 죽일 수 없다.<br>

<br><br>

## 4) 정상 유저와 핵 유저의 revives시도
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/554cb30d-758e-407a-b219-ba6bb9ca91d8></h3>

* 핵 유저의 소생 횟수가 정상 유저보다 더 적다.<br>

<br><br>

## 5) 상관관계 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/23403d7d-6459-43aa-81dd-d28ece21145e></h3>

* 'killStreaks'와 'DBNOs'와의 상관계수 값이 정상 유저가 핵 유저보다 높다.<br>

* 핵 유저는 헤드샷이 대부분이기 때문이다.<br>

* 수류탄과 같은 기타무기를 사용하지 않고, 총으로만 상대했을 것이다.<br>

<br><br>

## 6) 정상 유저와 핵 유저의 어시스트 수
* 정상 유저 : 약 1.108997395404042<br>

* 핵 유저   : 약 0.962393983037286<br>

* 핵 유저의 어시스트 수가 더 적다.<br>

<br><br>

## 7) 정상 유저와 핵 유저의 도움 행위 수 (boosts, heals)
* <strong>가설: 무기 획득 수 대비 boosts+heals의 아이템 비율이 더 적을 것이다.</strong><br>

* 정상 유저 : 약 1.4604191866466876<br>

* 핵 유저   : 약 1.2802245955482239<br>

* 핵 유저의 무기 획득 수가 더 낮고, 돕는 비율이 더 적다.<br>

<br><br>

## 8) 정상 유저와 핵 유저의 처치횟수 등수 분포
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/75c7e2b1-f96c-45c3-a86c-2763f1bdefce></h3>

* 핵 유저(주황), 정상 유저(파랑)의 분포를 봤을 때, 핵 유저는 상위 등수에 포진하고 있지 않지만, 하위 등수에는 포진하는 경우가 아예 없다.<br>

* 정상 유저는 우승했음에도 하위 등수에 있는 경우가 있다. (핵 유저에 비해 실력이 고르게 분포 즉, 운 좋게 우승하는 경우도 빈번히 있다.)<br>

<br><br>

## ✨ Insight
* <strong>핵 유저는 정상 유저보다 파밍을 적게 한다.</strong><br>

* <strong>총(무기) 파밍만 하기 때문에, 수류탄을 잘 사용하지 않는다. 따라서 동시 처지 횟수가 낮다.</strong><br>

* <strong>소생, 회복 등 돕는 횟수가 적다.</strong><br>

* <strong>핵 유저는 처치 횟수 등수가 정상 유저에 비해 더 높은 편에 속한다.</strong><br>

* <strong>정상 유저는 처지 등수가 낮음에도 불구하고 우승을 하는 경우가 종종있다.</strong><br>

<br><br>

# ⭐ 비인가 프로그램 사용자 탐지
## 1) Isolate Forest
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/2f282e5f-6369-46d4-b3cc-fbe222bdd89b></h3>
<br><br>


## 2) 핵 유저 특징 분석
### 2-A) 데미지 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/b699e069-f8ab-41b1-952c-2a1797f48a90></h3>

* 파란색 분포 : 정상 유저<br>

* 빨간색 분포 : 핵 유저<br>

* 정상 유저 보다 핵 유저가 전체적으로 높은 데미지 분포를 가지고 있다.<br>

<br><br>

### 2-B) 헤드샷 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/1ffbaba3-6a49-4c07-b867-4f4e0bcc8851></h3>

* 핵 유저의 헤드샷 비율이 확연히 더 높은 것을 알 수 있다.<br>

<br><br>

### 2-C) 등수 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/103dc6ba-b42b-4fe1-9cfa-8da09575e7ec></h3>

* 핵 유저들의 최대 예상 점수가 0에 많이 몰려있다.<br>

* 높은 등수가 예상되는 것으로 보아, 이전 게임 기록이 좋았음을 예상할 수 있다.<br>

<br><br>

### 2-D) 거리 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/59b98fb1-a522-47bc-88c9-0620dc68e6db></h3>

* 핵 유저들은 걸어서 이동한 거리가 상대적으로 낮게 분포한다.<br>

<br><br>

# 🌟 결론
#### 🍳 EDA를 통한 Insight
* <strong>핵 유저는 정상 유저보다 파밍을 적게 한다.</strong><br>

* <strong>총(무기) 파밍만 하기 때문에, 수류탄을 잘 사용하지 않는다. 따라서 동시 처지 횟수가 낮다.</strong><br>

* <strong>소생, 회복 등 돕는 횟수가 적다.</strong><br>

* <strong>핵 유저는 처치횟수 등수가 정상 유저에 비해 더 높은 편에 속한다.</strong><br>

* <strong>정상 유저는 처지 등수가 낮음에도 불구하고 우승을 하는 경우가 종종있다.</strong><br>

<br>

#### 🍳 IsolateForest를 통한 결론
* <strong>핵 유저들은 짧은 시간 안에 동시처치(Ex: 수류탄과 같은 광역 무기)가 높은 편에 분포한다.</strong><br>

* <strong>핵 유저들은 타유저에 비해 입힌 데미지가 높은 편이다.</strong><br>

* <strong>핵 유저들은 헤드샷 비율이 높다.</strong><br>

* <strong>핵 유저들은 이동 거리가 짧다.</strong><br>

<br><br>

# 📑 회고
* 분산분석시 비모수검정에서 크루스칼왈리스 중위수 검정은 사후검정을 할 수 없다는 것을 알게되었다.

* 두 집단씩 나눠서 Mann Whiteny U-test를 실시해야 한다.

* 등분산성을 만족하지 않는다면 welch ttest를 실시한다.

* 경매장 시세분석, 유저 매크로 분석, 유저 특징 분석, 게임 이탈 요인분석과 같이 더 다양한 게임 데이터로 분석을 해보고 싶다.
