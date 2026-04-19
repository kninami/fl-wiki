---
paper: cheng_2021
title: "LogExtractor: Extracting Digital Evidence from Android Log Messages (2021)"
full_title: "LogExtractor: Extracting Digital Evidence from Android Log Messages via String and Taint Analysis"
authors: "Cheng, L.; Shi, X.; Gong, N.Z.; Guan, L."
year: 2021
venue: DFRWS USA 2021
doi: —
tags:
  - Android
  - log forensics
  - taint analysis
  - string analysis
  - ADB
  - mobile forensics
method: 실험
limitations: "7개 FN(ICC 흐름), 암묵적 흐름 미처리; 문자열 분석 한계; APK 난독화 미처리"
---

## 한 줄 요약
APK 정적 분석(String + Taint Analysis)으로 앱별 로그 증거 DFA를 자동 생성하고, 실 기기 Android 로그에서 개인식별정보를 추출하는 LogExtractor 도구.

## 핵심 발견
- 22,700개 앱 분석: Unique Identifier 흐름의 42.1%가 Log sink로 유입 → 로그가 주요 증거 소스
- DroidBench 65개 앱: 식별 precision 97.7%, recall 79.2%; DFA 정확 시 추출 precision 100%
- 워크플로: APK → 코드 추출 → String & Taint Analysis → ALED(앱 로그 증거 DB) → 기기 로그 매칭
- 기존 도구 대비 수동 규칙 작성 불필요 — 앱별 자동 DFA 생성이 핵심 기여
- 수사관이 ADB로 추출한 Android system log를 입력으로 사용

## 직접 분석한 아티팩트
[[artifacts/Android_System_Log]]

## 영향받은 wiki 페이지
[[techniques/Android_Log_Evidence_Extraction]]
