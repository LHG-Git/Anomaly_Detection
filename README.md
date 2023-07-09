<div align="center">
  <h1>📝 개인 프로젝트 : BattleGround_Anomaly_Detection<br><br>
  🕹 배틀그라운드 비인가 프로그램 탐지 프로젝트</h1>
</div>

<h4> 💭 Language : Python <br><br>
     📝 Library : Pandas, Numpy, Matplotlib, QGIS, Scikit-learn, scipy <br><br>
     🛠  Tool : Google Colab <br><br>
     📅 진행기간 : 2023.03.23 ~ 2023.06.07</h4>
     
### 👨‍👦‍👦 팀원소개
<table>
<tbody>
  <tr>
    <td align="left"><img src="" width="20px;" alt=""/><br /><b>팀원 : 이희구</b></a><br /></td>
   <tr/>
</tbody>
</table>
<br>
<h3 align="center"><img src="https://github.com/LHG-Git/project/assets/100845169/8855bb22-daeb-43ad-a724-b53b02d265c3" height = 600px></h3>


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
<br>

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


# 🔎 데이터 수집
|데이터셋|출처|
|------|:------:|
|해양환경측정망 데이터|관할해역해양정보 공동활용시스템|
|국가 해양생태계 데이터|해양환경공단|
|갯끈풀 위치 데이터|네이처링|
|바다 격자 정보(격자 3단계)shp 데이터|바다누리 해양정보 플랫폼|
|갯벌 현황도 shp데이터|해양수산부 연안 포털|

<br><br>

# 🔎 가설 설정
* winPlacePerc = 1은 우승을 의미한다.

* 보통 핵을 쓰는 유저가 게임에서 우승한다고 생각했기 때문에 우승 유저 가운데 aim_point가 이상치인 경우를 비인가 프로그램 사용 유저라고 정의

* 킬수가 전부 헤드샷 킬수인 경우 '헤드샷 킬수 / 실제 킬수'로 정의하였다.

* 배그는 총기 반동이 심하기 때문에 헤드샷이 쉽지만은 않다.
  
* 보통 'AWM', 'K98' 등 저격총을 이용해서 헤드샷 킬이 잘 나오긴 하지만, 그것마저 흔한 킬이 아니라고 생각했기 때문에, 위의 비율을 봤을때 비정상적으로 헤드샷 킬이 크다면 에임핵을 의심할 수 있다.
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

* 무기획득에 대한 평균 검정을 한 결과 p값이 0.05보다 작아 통계적으로 유의한 차이가 있음<br>

* 가설대로 핵 유저의 무기획득의 평균 수가 적음을 확인함<br>

* 따라서 핵 유저는 파밍을 많이 하지 않는다.

<br><br>

## 3) 정상 유저와 핵 유저의 killStreaks 수 (짧은 시간 내에 동시 처치)
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/3dc60fe6-2d62-4227-89cf-1bdb40aa46fa></h3>

* 핵 유저는 동시 처치가 거의 1에 수렴<br>

* 동시 처치에 대한 평균검정을 한 결과 핵유저가 정상 유저에 비해 killSteaks가 적음<br>

* 핵유저는 짧은 시간안에 여러 명을 죽일 수 없음<br>

<br><br>

## 4) 정상과 핵 유저의 revives시도
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/554cb30d-758e-407a-b219-ba6bb9ca91d8></h3>

* 핵 유저의 소생 횟수가 정상 유저보다 더 적음<br>

<br><br>

## 5) 상관관계 분석
<h3 align="center"><img src= https://github.com/LHG-Git/BattleGround_Anomaly_Detection/assets/100845169/23403d7d-6459-43aa-81dd-d28ece21145e></h3>

* 'killStreaks'와 'DBNOs'와의 상관계수 값이 정상 유저가 핵 유저보다 높음<br>

* 어뷰징유저는 헤드샷이 대부분이기 때문<br>

* 수류탄과 같은 기타무기를 사용하지 않고, 총으로만 상대했을 것<br>

<br><br>






# 📄 Modeling
## 1) 군집화
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/e22674c7-b20e-4298-b8bc-b7b9f2f4583a></h3>

* 최적의 k값 도출을 위해 <strong>실루엣 계수</strong>를 사용<br> 
* 이때 실루엣 계수 평균만을 고려하지 않았고 figure5를 통해 도출된 인사이트를 함께 고려<br>
* 그 결과 cluster별 실루엣 계수 평균이 가장 높지는 않지만, 실루엣 계수의 너비가 비교적 균일한 지점에서 <strong>최적의 k(k=6)값을 도출</strong><br><br>

<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/909e7b1d-0b40-4745-abc5-f3652ef06a84></h3>

* 최적의 K값을 통해 위치별 군집화 결과, figure 5에서 인천 부근의 서해에 위치한 관측치와, 부산 부근의 남해에 위치한 관측치에서 화학적 산소농도 수치가 높게 기록
<br>

## 2) 모델 성능 지표 선정
* 성능 지표의 경우 본 프로젝트의 주제 자체가 ‘해양정보를 활용한 해양오염 예측’이기 때문에, 모델의 설명력을 나타내는 R2값 보다 실제 예측 오차의 크기인 MAE가 본 프로젝트와 맞는 지표라고 생각하여, <strong>MAE값을 기준으로 최종 모델을 선정</strong>
<br>

## 3) 최종 모델 선정
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/da4b5525-0513-4615-a954-9778eadeda55></h3>

* 최종 예측결과 전체 모델에서 훈련세트에 <strong>약간의 과적합 존재</strong><br>
* <strong>CatBoost모델의 MAE값이 가장 준수</strong>
* 해양오염 예측에 사용될 모델을 <strong>CatBoostRegressor로 선정</strong>
<br>

## 4) 하이퍼파라미터 튜닝
* CatBoostRegressor모델의 특성상 하이퍼파라미터 튜닝을 진행하여도 모델 성능 개선에 크게 영향을 미치지 않음
* 오히려 파라미터의 default값으로 모델 예측을 수행하였을 때, 성능이 가장 높게 측정됨
<br>

## 5) K-Fold 교차검증
* 최종 선정된 모델(CatBoostRegressor)의 경우 train과 text의 성능 지표에서 약간의 과적합이 발생
* 과적합을 방지하기 위해 valid data를 생성
* 해당 데이터셋을 이용하여 가장 흔하게 사용되는 <strong>K-Fold 교차검증을 진행</strong>
* <strong>K값은 5로 지정</strong>하였으며, 모델 진행시에 골고루 데이터의 특성을 반영하기 위하여 <strong>shuffle을 진행</strong>
<br>

## 6) 모델평가 및 검증
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/1f3ca8e2-9710-404d-9425-d9a9d3f64cdf></h3>

* <strong>하이퍼파라미터 튜닝 및 K-Fold교차검증을 통하여 모델 성능 최적화를 진행하여 과적합 개선</strong>

* 그 결과 이전의 결과에서 보다 <strong>과적합이 많이 개선</strong>되었음을 확인하였고 <strong>모델의 성능 또한 향상됨</strong>

* 성능 지표의 경우 본 프로젝트의 주제가 ‘해양정보를 활용한 해양오염 예측’ 이기 때문에, 모델의 설명력을 나타내는 R2값 보다 실제 예측 오차의 크기인 MAE가 본 프로젝트와 맞는 지표라고 판단

* <strong>최종 모델링 결과 약 MAE = 0.076으로 실제값과 약 0.076이 차이가 나는 모델 완성</strong>

* 최근 10년동안 국내 연안의 화학적 산소농도가 연평균 1.13~1.82mg/L인 것을 고려했을 때, 꽤 정확도가 높은 모델이라고 설명할 수 있음



