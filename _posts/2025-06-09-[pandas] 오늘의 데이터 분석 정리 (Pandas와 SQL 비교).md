---

title: "[pandas] 오늘의 데이터 분석 정리 (Pandas와 SQL 비교)" 
date: 2025-06-09-00:35:00 +0900
categories: [data-analysis, pandas] 
tags: [pandas, dataframe, csv, excel, 데이터분석, python] 
pin: true

---
# 🧠 오늘의 데이터 분석 정리

## ✅ 1. 데이터 분석의 기본 단계

```text
질 → 이 → 정 → 질 → 탐 → 그 → 말
질문 → 데이터 이해 → 정리 → 질문 구체화 → 탐색 → 그래프 → 인사이트 정리
```
----------

## ✅ 2. 자주 쓰는 Pandas 문법

### 🔹 파생변수 만들기

```python
df["total_mpg"] = (df["cty"] + df["hwy"]) / 2
```

### 🔹 조건 필터링 (where 절)

```python
df[df["cyl"] == 4]
df.query("cyl == 4 and hwy > 30")
```

### 🔹 빈도표 & 시각화

```python
df["fl"].value_counts().plot(kind="bar")
plt.xticks(rotation=45)
```

### 🔹 그룹화 + 평균 + 정렬

```python
df.groupby("manufacturer")["total_mpg"].mean().sort_values(ascending=False)
```

### 🔹 목록 필터링 (isin)

```python
target = ["toyota", "audi"]
df[df["manufacturer"].isin(target)]
```
----------
## ✅ 3. Pandas vs SQL 비교

| 항목           | SQL                                      | Pandas (Python)                           |
|----------------|-------------------------------------------|--------------------------------------------|
| 목적           | 데이터베이스에서 데이터 추출             | 추출된 데이터를 가공·분석                 |
| 문법 스타일    | 선언형 (무엇을 할지 말함)                | 절차형 + 선언형 (어떻게 할지도 말함)     |
| 데이터 위치    | 데이터베이스 내부                        | 메모리(RAM) 내부                          |
| 데이터 구조    | 테이블                                   | DataFrame (판다스 객체)                   |
| 조건 필터링    | `WHERE col = 'x'`                        | `df[df["col"] == "x"]`                    |
| 목록 필터      | `IN ('a', 'b')`                          | `df[df["col"].isin(['a', 'b'])]`          |
| 정렬           | `ORDER BY col DESC`                      | `df.sort_values("col", ascending=False)`  |
| 그룹화         | `GROUP BY col`                           | `df.groupby("col")`                       |
| 평균           | `AVG(col)`                               | `df["col"].mean()`                        |
| 파생변수       | `SELECT col1 + col2 AS new_col`          | `df["new_col"] = df["col1"] + df["col2"]` |
| 중첩 조건      | `AND / OR`                               | `& / |` (괄호 필수!)                      |
| 시각화         | ❌ 불가능 (BI 도구 필요)                 | ✅ `matplotlib`, `seaborn` 가능           |

----------

## ✅ 4. 메서드 체이닝 예시

```python
(
    df.query("cyl == 4")
      .groupby("manufacturer")["hwy"]
      .mean()
      .sort_values(ascending=False)
      .head(5)
)
```
> ⛓️ 깔끔하게 분석 흐름을 이어주는 메서드 체이닝!

