---
paper: neshenko_2021
title: "Behavioral-Based Forensic Investigation of ICS Water Plant Attacks Using GANs (2021)"
full_title: "A Behavioral-Based Forensic Investigation Approach for Analyzing Attacks on Water Plants Using GANs"
authors: "Neshenko, N.; Bou-Harb, E.; Furht, B."
year: 2021
venue: DFRWS USA 2021
doi: —
tags:
  - ICS
  - water plant
  - GAN
  - behavioral forensics
  - anomaly detection
  - attribution
method: 실험
limitations: "단일 수처리 시설 데이터셋; GAN 학습 파라미터 민감도; 실시간 적용 미검증"
---

## 한 줄 요약
BiGAN+RNN+CNN 비지도 앙상블로 수처리 ICS 물리 센서 데이터에서 공격 행위를 탐지·추론·귀인.

## 핵심 발견
- 36개 인시던트 중 32개 탐지 (88.9% 탐지율)
- 행위 지문(Behavior Fingerprinting) → 공격 추론(Attack Inference) → 귀인(Attribution) 3단계 파이프라인
- 귀인에 CART 트리 + Shapley 값 + KernelSHAP 활용 → 판단 근거 설명 가능
- 입력: 물리 센서 속성 데이터 (압력, 유량 등) — 전통적 디스크 아티팩트 미사용
- 레이블 없는 데이터에서 공격 패턴 학습 가능 (비지도)

## 직접 분석한 아티팩트
없음 (ICS 물리 센서 시계열 데이터 기반 — 전통적 디지털 포렌식 아티팩트 없음)

## 영향받은 wiki 페이지
[[techniques/ICS_Behavioral_Attack_Forensics]]
