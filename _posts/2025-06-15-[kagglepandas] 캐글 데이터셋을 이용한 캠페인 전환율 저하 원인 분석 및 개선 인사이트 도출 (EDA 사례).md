---

title: "[kaggle/pandas] 캐글 데이터셋을 이용한 캠페인 전환율 저하 원인 분석 및 개선 인사이트 도출 (EDA 사례)"
date: 2025-06-15 02:40:00 +0900
categories: [Data Analysis, Performance Marketing]
tags: [전환율 분석, EDA, 마케팅 공부, 데이터분석 연습,kaggle,pandas]
pin: true

---

# 🎯 캠페인 전환율 분석
*Predict Conversion in Digital Marketing 데이터셋 EDA 실습*

---

## 🤔 프로젝트 시작 이유

데이터 분석을 통해 마케팅의 실제 문제를 해결하는 퍼포먼스 마케터가 되고 싶어서  
**전환율과 사용자 행동**을 직접 분석해보고자 이 프로젝트를 기획했습니다.  
그 첫걸음으로, 캐글의 디지털 마케팅 캠페인 데이터셋을 활용해  
“성과가 낮은 캠페인의 원인을 데이터로 탐색하고,  
실질적인 개선 방향을 제안하는 과정”을 직접 경험하고자 했습니다.

---

## 👨‍💻 분석 목표

주니어 마케터로서, 실제로 지난 마케팅 캠페인의 낮은 성과를 데이터로 분석해야 하는 **실무 상황**을 가정하고 연습해보았습니다.

- **데이터를 활용해 낮은 성과의 구체적인 원인을 직접 파악해보고**
- **발견한 문제점을 바탕으로 개선 아이디어까지 생각해보기**
- 실습 과정에서 마케팅 데이터 분석에 필요한 **주요 도메인 지식**과 **분석 흐름**,  
  그리고 **적절한 시각화 도구**를 익히는 데 중점을 두었습니다.

> 단순히 숫자만 보는 것이 아니라,  
> 실제 실무에서 “문제 진단 → 개선 제안”까지 스스로 해보는 경험을 목표로 했습니다.

---

## 📋 분석 흐름

1. 성과가 낮은(전환율이 낮은) 캠페인/채널 찾기  
2. 그 캠페인/채널의 다른 지표(CTR, CPA, 체류시간 등)와 비교  
3. 세그먼트별(나이, 성별, 소득 등) 전환율 차이 확인  
4. 사용자 행동(체류시간, 방문 페이지수, 소셜/이메일 반응 등) 비교  
5. 데이터에서 발견한 문제점 정리 & 개선 아이디어 고민

---
## 🗂️ 데이터셋 컬럼 상세 설명

이번 분석에 사용한 데이터셋에는  
고객의 기본 정보부터 실제 광고 집행 결과, 사이트 내 행동, 캠페인 결과까지  
실무에서 실제로 많이 쓰이는 다양한 컬럼들이 포함되어 있음

| 컬럼명                | 설명 (실무적 의미/예시) |
|-----------------------|---------------------------|
| **CustomerID**        | 고객별로 부여되는 고유 번호 (마치 주민등록번호처럼 유니크함) |
| **Age**               | 고객의 나이 (예: 25세) |
| **Gender**            | 고객 성별 (Male=남성, Female=여성) |
| **Income**            | 고객 소득 (연간 소득, 단위: 원/달러 등) |
| **CampaignChannel**   | 광고 유입 채널 (예: Social Media, Email, PPC, Referral 등) |
| **CampaignType**      | 캠페인 목적 (Awareness=인지도, Retention=재구매, Conversion=전환유도) |
| **AdSpend**           | 광고비 (해당 고객 유치에 실제로 쓴 광고 비용) |
| **ClickThroughRate**  | 클릭률(CTR). 광고가 노출됐을 때 실제로 클릭된 비율 (0~1, 실무에선 % 변환) |
| **ConversionRate**    | 전환율(CVR). 광고를 클릭한 뒤 실제로 목표 행동(구매/회원가입 등)을 한 비율 |
| **WebsiteVisits**     | 광고를 통해 유입된 사이트 방문 수 |
| **PagesPerVisit**     | 방문 1회당 평균 페이지 열람 개수 (페이지를 많이 보면, 관심이 많다는 신호!) |
| **TimeOnSite**        | 방문 1회당 평균 체류 시간 (분/초 단위) |
| **SocialShares**      | SNS 공유 횟수 (해당 캠페인을 소셜에서 몇 번 공유했는지) |
| **EmailOpens**        | 이메일 오픈 수 (광고성 이메일을 실제로 연 횟수) |
| **EmailClicks**       | 이메일 내 링크 클릭 수 (오픈 이후 실제로 반응한 횟수) |
| **PreviousPurchases** | 과거 구매 경험 수 (충성 고객/신규 고객 등 구분 가능) |
| **LoyaltyPoints**     | 누적 로열티 포인트 (멤버십 등 충성도 프로그램에서 쌓이는 점수) |
| **AdvertisingPlatform** | 광고가 집행된 플랫폼 (예: Facebook, Google, Kakao 등) |
| **AdvertisingTool**   | 광고 관리에 사용된 마케팅 툴 (예: ToolConfid, 자동화 솔루션 등) |
| **Conversion**        | 실제 전환 여부 (1=전환 발생, 0=미전환, 목표: 구매/회원가입 등) |

---

> 실제 마케팅에서는 **인구통계(나이·성별·소득) 정보 + 채널/캠페인 유형 + 행동 데이터(방문, 체류, 클릭, 구매)** 이 3가지를 연결해서  “어떤 사람에게, 어떤 채널·광고·콘텐츠가 효과적인지” 데이터로 분석하고,  타겟/전략/예산 배분을 결정하는 작업을 함


## 1️⃣ 전환율이 낮은 채널 찾기

먼저, 데이터에서 각 캠페인/채널별 전환율을 비교해서  **가장 전환율이 낮은 채널(Referral)**을 찾았습니다.

실행 코드
```
import pandas as pd
# 1. 데이터 불러오기
df = pd.read_csv('/content/digital_marketing_campaign_dataset.csv)

# 2. 광고 채널별로 주요 지표 집계
agg = df.groupby('CampaignChannel').agg(
avg_cvr = ('ConversionRate',  'mean'),  # 전환율 평균
avg_ctr = ('ClickThroughRate',  'mean'),  # 클릭률 평균
total_spent = ('AdSpend',  'sum'),  # 광고비 총합
total_conversion = ('Conversion',  'sum'),  # 전환수 총합
avg_time = ('TimeOnSite',  'mean'),  # 사이트 체류시간 평균
avg_pages = ('PagesPerVisit',  'mean'),  # 페이지뷰 평균
avg_social = ('SocialShares',  'mean'),  # 소셜 공유 평균
avg_email_open = ('EmailOpens',  'mean'),  # 이메일 오픈 평균
avg_loyalty = ('LoyaltyPoints',  'mean')  # 로열티 평균
).reset_index()

# 3. CPA(전환당 비용) 계산 (총 광고비 / 전환수)
agg['avg_cpa'] = agg['total_spent'] / agg['total_conversion']

# 4. 전환율이 가장 낮은(성과 저조) 채널 찾기
low_cvr_row = agg.sort_values('avg_cvr').iloc[0]
print('전환율 가장 낮은 채널:', low_cvr_row['CampaignChannel'])
print(low_cvr_row)

# 5. 저조 채널의 원인 분석을 위해, 해당 채널 데이터만 추출
target_channel = low_cvr_row['CampaignChannel']
df_low = df[df['CampaignChannel'] == target_channel]

# 6. 저조 채널의 나이별 전환율(어떤 연령대가 약한가?)
seg = df_low.groupby('Age').agg(
cvr = ('ConversionRate',  'mean'),
cnt = ('CustomerID',  'count')
).reset_index()
print('\n저조 채널의 연령별 전환율 및 데이터수')
print(seg.head(10))  # 데이터가 많으면 상위 10개만

# 7. 행동지표(체류시간, 페이지뷰 등) 비교: 문제 채널 vs 전체 평균
print('\n행동지표: 문제 채널 vs 전체 평균')
metrics = ['TimeOnSite',  'PagesPerVisit',  'SocialShares',  'EmailOpens',  'LoyaltyPoints']
for m in metrics:
print(f"{m}: 문제 채널 평균 {df_low[m].mean():.2f} / 전체 평균 {df[m].mean():.2f}")

# 8. (선택) 문제 채널의 세그먼트/행동별 시각화도 추가 가능
import matplotlib.pyplot as plt
plt.figure(figsize=(8,4))
plt.plot(seg['Age'], seg['cvr'], marker='o')
plt.title(f'{target_channel} 채널 연령별 전환율')
plt.xlabel('연령')
plt.ylabel('전환율(%)')
plt.grid()
plt.show()
```
결과
```
전환율 가장 낮은 채널: Referral
CampaignChannel           Referral
avg_cvr                   0.103051
avg_ctr                   0.151673
total_spent         8653518.685592
total_conversion              1518
avg_time                  7.651172
avg_pages                  5.54245
avg_social               50.004654
avg_email_open            9.593368
avg_loyalty            2479.101803
avg_cpa                5700.605195
Name: 2, dtype: object

저조 채널의 연령별 전환율 및 데이터수
   Age       cvr  cnt
0   18  0.095999   33
1   19  0.092215   38
2   20  0.081065   26
3   21  0.099605   34
4   22  0.098858   29
5   23  0.097863   41
6   24  0.103947   20
7   25  0.095458   36
8   26  0.092375   28
9   27  0.098926   22

행동지표: 문제 채널 vs 전체 평균
TimeOnSite: 문제 채널 평균 7.65 / 전체 평균 7.73
PagesPerVisit: 문제 채널 평균 5.54 / 전체 평균 5.55
SocialShares: 문제 채널 평균 50.00 / 전체 평균 49.80
EmailOpens: 문제 채널 평균 9.59 / 전체 평균 9.48
LoyaltyPoints: 문제 채널 평균 2479.10 / 전체 평균 2490.27
```

![Referral 채널의 연령별 전환율](assets/img/referral1.png)

→ 결과: 'Referral' 채널이 전환율, 효율이 가장 낮음

---

## 2️⃣ Referral 채널 성과 자세히 뜯어보기

Referral 채널만 따로 모아서  
CTR, CPA, 체류시간, 페이지수 등 주요 지표를 다시 확인해봤습니다.

실행 코드

```
# Referral 채널 데이터만 추출
df_ref = df[df['CampaignChannel'] == 'Referral']

# 1. 주요 지표 계산 (Referral 채널)
referral_metrics = {
'전환율(평균, %)'  : df_ref['ConversionRate'].mean() * 100,
'클릭률(CTR, %)'  : df_ref['ClickThroughRate'].mean() * 100,
'CPA(전환당 비용)'  : df_ref['AdSpend'].sum() / df_ref['Conversion'].sum(),
'평균 체류시간'  : df_ref['TimeOnSite'].mean(),
'평균 페이지수'  : df_ref['PagesPerVisit'].mean(),
'평균 소셜 공유'  : df_ref['SocialShares'].mean(),
'평균 이메일 오픈'  : df_ref['EmailOpens'].mean(),
'평균 로열티'  : df_ref['LoyaltyPoints'].mean(),
'총 광고비'  : df_ref['AdSpend'].sum(),
'총 전환수'  : df_ref['Conversion'].sum(),
'데이터 건수'  :  len(df_ref)
}
print('[Referral 채널 주요 지표]')
for k,v in referral_metrics.items():
print(f"{k}: {v:.2f}")

# 2. 전체 채널 평균과 비교
metrics = ['ConversionRate',  'ClickThroughRate',  'TimeOnSite',  'PagesPerVisit',  'SocialShares',  'EmailOpens',  'LoyaltyPoints']
print('\n[Referral vs 전체 평균]')
for m in metrics:
print(f"{m}: Referral {df_ref[m].mean():.2f} / 전체 {df[m].mean():.2f}")
print(f"CPA: Referral {referral_metrics['CPA(전환당 비용)']:.2f} / 전체 {df['AdSpend'].sum()/df['Conversion'].sum():.2f}")

```
결과
```
[Referral 채널 주요 지표]
전환율(평균, %): 10.31
클릭률(CTR, %): 15.17
CPA(전환당 비용): 5700.61
평균 체류시간: 7.65
평균 페이지수: 5.54
평균 소셜 공유: 50.00
평균 이메일 오픈: 9.59
평균 로열티: 2479.10
총 광고비: 8653518.69
총 전환수: 1518.00
데이터 건수: 1719.00

[Referral vs 전체 평균]
ConversionRate: Referral 0.10 / 전체 0.10
ClickThroughRate: Referral 0.15 / 전체 0.15
TimeOnSite: Referral 7.65 / 전체 7.73
PagesPerVisit: Referral 5.54 / 전체 5.55
SocialShares: Referral 50.00 / 전체 49.80
EmailOpens: Referral 9.59 / 전체 9.48
LoyaltyPoints: Referral 2479.10 / 전체 2490.27
CPA: Referral 5700.61 / 전체 5705.58
```
→ 전체 평균과는 큰 차이가 없지만,  
동일 예산 대비 성과(전환수, 전환율)가 낮아서 비효율적인 채널임을 알 수 있었음

---

## 3️⃣ 세그먼트별(연령, 성별, 소득) 전환율 분석

-   연령대별, 성별별, 소득 구간별로 전환율이 어떻게 다른지 직접 시각화해봤다.

```
# 연령 구간화 (10대, 20대, 30대, 40대, 50대, 60대+)
bins = [10, 19, 29, 39, 49, 59, 100]
labels = ['10대', '20대', '30대', '40대', '50대', '60대+']
df_ref['AgeGroup'] = pd.cut(df_ref['Age'], bins=bins, labels=labels, right=True)

# 연령대+성별별로 전환율 평균 구하기
age_gender = df_ref.groupby(['AgeGroup', 'Gender']).agg(
    avg_cvr = ('ConversionRate', 'mean'),
    cnt = ('CustomerID', 'count')
).reset_index()

# 시각화 (막대그래프)
import seaborn as sns
plt.figure(figsize=(10,5))
sns.barplot(data=age_gender, x='AgeGroup', y='avg_cvr', hue='Gender')
plt.title('Referral 채널: 연령대/성별별 전환율(%)')
plt.xlabel('연령대')
plt.ylabel('전환율(%)')
plt.ylim(0, age_gender['avg_cvr'].max()*1.2)
plt.legend(title='성별')
plt.show()
```
결과

![enter image description here](assets/img/referral2.png)

```

df_ref['IncomeGroup'] = pd.qcut(df_ref['Income'], 4, labels=['하위25%', '중하위', '중상위', '상위25%'])
income_gender = df_ref.groupby(['IncomeGroup', 'Gender']).agg(
    avg_cvr = ('ConversionRate', 'mean'),
    cnt = ('CustomerID', 'count')
).reset_index()

plt.figure(figsize=(8,4))
sns.barplot(data=income_gender, x='IncomeGroup', y='avg_cvr', hue='Gender')
plt.title('Referral 채널: 소득구간/성별별 전환율(%)')
plt.xlabel('소득구간')
plt.ylabel('전환율(%)')
plt.ylim(0, income_gender['avg_cvr'].max()*1.2)
plt.legend(title='성별')
plt.show()
```
결과

![enter image description here](assets/img/referral3.png)


**→ 인사이트 :**
-   "30대 여성", "상위 소득" 세그먼트에서 전환율이 가장 높았고,
-   "10대/저소득 남성"에서는 전환율이 낮음 → 이 그룹이 개선 타깃이 될 수 있음

---

## 4️⃣ 행동지표 분석 (체류시간, 페이지수 등)

누가, 어떤 행동을 할 때 전환이 잘 일어나는지 보기 위해  
연령대, 성별별로 체류시간, 페이지수와 전환율을 같이 살펴봤다.


```
#연령대×성별별 체류시간 평균
age_gender_behav = df_ref.groupby(['AgeGroup', 'Gender']).agg(
    avg_time = ('TimeOnSite', 'mean'),
    avg_pages = ('PagesPerVisit', 'mean'),
    avg_social = ('SocialShares', 'mean'),
    avg_email = ('EmailOpens', 'mean'),
    avg_cvr = ('ConversionRate', 'mean'),
    cnt = ('CustomerID', 'count')
).reset_index()
```

```
or gender in age_gender_behav['Gender'].unique():
    tmp = age_gender_behav[age_gender_behav['Gender'] == gender]
    fig, ax1 = plt.subplots(figsize=(8,4))
    ax2 = ax1.twinx()
    ax1.bar(tmp['AgeGroup'], tmp['avg_time'], color='skyblue', alpha=0.7, label='평균 체류시간')
    ax2.plot(tmp['AgeGroup'], tmp['avg_cvr']*100, color='tomato', marker='o', label='전환율(%)')
    ax1.set_ylabel('평균 체류시간')
    ax2.set_ylabel('전환율(%)')
    plt.title(f'Referral: {gender} - 연령대별 체류시간 & 전환율')
    ax1.legend(loc='upper left')
    ax2.legend(loc='upper right')
    plt.show()
```
결과

![enter image description here](assets/img/referral4.png)

**→ 생각 해 볼 수 있는 인사이트**
-   40대 여성은 체류시간, 페이지수 모두 높고 전환율도 높음
-   10대 남성은 체류시간, 페이지수, 전환율 모두 낮음
-   행동지표는 좋은데 전환율만 낮은 그룹은 결제, CTA, 폼 등 마지막 단계에서 막힐 수도 있다

## ✏️ 실습 소감

처음으로 실제 데이터로 “성과 저조의 원인→개선 아이디어”까지 연결하는 마케팅 분석을 해봤다.  
수치만 보는 게 아니라, **왜 이런 현상이 나오는지, 실제로 어떻게 바꿔야 할지**  
스스로 고민해볼 수 있어서, 퍼포먼스 마케터로 한 발 더 가까워졌다는 느낌!

앞으로도 계속 실제 데이터로 실습을 쌓아보고,  
더 다양한 변수와 상황에서 “원인-인사이트-실행” 분석력을 키워보고 싶다. 🚀
