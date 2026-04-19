---
paper: akcora_2021
title: "Topological Data Analysis for Ransomware Detection on Bitcoin Blockchain (2021)"
full_title: "Topological Data Analysis for Ransomware Detection on the Bitcoin Blockchain"
authors: "Akcora, C.G.; Gel, Y.R.; Kantarcioglu, M."
year: 2021
venue: DFRWS USA 2021
doi: —
tags:
  - ransomware
  - Bitcoin
  - blockchain forensics
  - topological data analysis
  - financial forensics
method: 실험
limitations: "27개 랜섬웨어 패밀리 한정; 실시간 탐지 미구현; 믹서·텀블러 우회 미검증"
---

## 한 줄 요약
Bitcoin 네트워크의 24시간 시간 창에서 위상 데이터 분석(TDA) 기반 6개 피처로 27개 랜섬웨어 패밀리의 주소를 식별.

## 핵심 발견
- 데이터셋: 24,486개 주소, 27개 랜섬웨어 패밀리 (Montreal + Princeton + Padua 데이터 통합)
- 피처 6종: Income, Neighbors, Weight, Length, Count, Loop
- Weight·Loop 피처가 랜섬웨어 행위 특성 가장 잘 반영
- 24시간 시간 창 단위 분석 → 랜섬웨어 지불 타이밍 패턴 포착
- 기존 그래프 분석 대비 TDA의 위상적 특성이 랜섬웨어 식별에 유효

## 직접 분석한 아티팩트
없음 (Bitcoin 블록체인 거래 데이터 분석 — 전통적 디스크·메모리 아티팩트 없음)

## 영향받은 wiki 페이지
[[techniques/Ransomware_Bitcoin_Detection_TDA]]
