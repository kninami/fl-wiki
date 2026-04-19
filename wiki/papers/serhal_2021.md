---
paper: serhal_2021
title: "ML-Based File Metadata Analysis for Smart Phone File Triage (2021)"
full_title: "Machine Learning Based Approach to Analyze File Meta Data for Smart Phone File Triage"
authors: "Serhal, R.; Le-Khac, N.-A."
year: 2021
venue: DFRWS EU 2021
doi: —
tags:
  - mobile forensics
  - file triage
  - machine learning
  - Android
  - metadata
method: 실험
limitations: "12개 기기 단일 수사기관 데이터셋; 테러 케이스 특화; 일반화 미검증"
---

## 한 줄 요약
테러 수사 Android 기기 ~200만 파일 중 관련 파일 6%를 빠르게 선별하기 위해 파일 메타데이터 12개 피처로 ML 분류기를 학습·비교.

## 핵심 발견
- NB, KNN, SVM, CART, RF, NN-MLP 6종 비교 → F1-Score ~0.97 수렴
- 선택 피처 12종: 파일 타입, 크기, Delta Apprehended(일수), EXIF 플래그, 해상도, 파일명 길이, 숫자·언더스코어 비율, 경로 깊이 등
- Grid Search CV + 10-fold CV로 과적합 방지
- 파일 내용 분석 없이 메타데이터만으로 고정밀 트리아지 가능
- 도구: XRY 이미징 후 메타데이터 추출

## 직접 분석한 아티팩트
없음 (파일시스템 메타데이터를 ML 피처로 사용 — 특정 아티팩트 포렌식 분석 아님)

## 영향받은 wiki 페이지
[[techniques/Smartphone_File_Triage_ML]]
