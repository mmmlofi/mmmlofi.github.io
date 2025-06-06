---
title: "[Colab/Git] fatal not a git repository 에러 해결 (경로 이동 주의!)"
date: 2025-06-06 23:39:00 +0900
categories: [Colab,Git]
tags: [colab,git,repository,오류,fatal]  # TAG names should always be lowercase
pin : true
---
## 😵 문제 상황
> Colab에서 GitHub 저장소랑 연동하고 분명 테스트도 했는데 오늘 깃 푸쉬하려니까 안됨
> fatal: not a git repository (or any of the parent directories): .git
> 흠 전엔 됐는데 이번엔 왜 안됐지?
> 1. 첨에 마운트 안함
> 2. 엥 근데 마운트 하고 경로 이동 (!cd) 했는데도 안됨 ㅡㅡ!!

## 🧠 원인 분석
Colab에서는 내가 생각하는 `!cd` 명령어가 **진짜 이동이 아니였음!**
- `!cd`는 셀 하나만 이동하고 **다음 셀에서는 `기존 디렉터리`로 리셋**
- 그래서 git 명령어를 `기존 디렉터리`에서 실행하게 되고,
- 그 결과 `.git` 폴더가 없어서 **Git 저장소가 아니라고 나오는 것**

## ✅ 해결 방법: Colab에선 반드시 `%cd`를 써라!
```python
from google.colab import drive
drive.mount('/content/drive')  # 드라이브 마운트

%cd /content/drive/내_깃_저장소_경로  # ✅ %cd로 제대로 이동 

!ls -a  # .git 폴더 있는지 확인
!git status  # 정상 작동 확인 ```

오늘도 하나 배우고 갑니다.
