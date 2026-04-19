---
technique: Smartphone_File_Triage_ML
perspective: Investigator
topic: Analysis
attck: —
related_artifacts: []
related_crime:
  - 테러 수사
  - 모바일 기기 대량 분석
sources:
  - serhal_2021
updated: 2026-04-20
---

## 개요
Android 기기 내 수백만 개 파일 중 수사 관련 파일만 신속히 선별하기 위해 파일 메타데이터 12개 피처로 ML 분류기를 학습·적용하는 트리아지 기법.

## 메커니즘
- XRY 이미징 후 파일 메타데이터 추출
- 12개 피처 선택: 파일 타입, 크기, Delta Apprehended(압수일~파일 생성일 차이), EXIF 플래그, 해상도, 파일명 길이, 숫자·언더스코어 비율, 경로 깊이 등
- 분류기 후보: NB, KNN, SVM, CART, RF, NN-MLP
- Grid Search CV + 10-fold CV로 하이퍼파라미터 최적화

## 탐지 방법 (Investigator)
1. XRY(또는 동급 도구)로 기기 이미징
2. 전체 파일 메타데이터 추출 (내용 불필요)
3. 학습된 ML 모델에 메타데이터 입력 → 관련/비관련 분류
4. 관련 분류 파일만 상세 검토 → 분석 시간 대폭 단축

## 한계 & 주의사항
- 테러 케이스 데이터셋으로 학습 → 다른 범죄 유형 일반화 미검증
- 파일 내용 미분석 → 메타데이터 조작 시 우회 가능
- 12개 피처는 수사 도메인에 따라 재선택 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[serhal_2021]] | 12개 피처, 6종 분류기 비교 → F1-Score ~0.97; 200만 파일 중 6% 관련 파일 선별 |

## 관련 페이지
[[papers/serhal_2021]]
