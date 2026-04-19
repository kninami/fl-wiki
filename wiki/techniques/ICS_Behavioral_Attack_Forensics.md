---
technique: ICS_Behavioral_Attack_Forensics
perspective: Investigator
topic: Analysis
attck: —
related_artifacts: []
related_crime:
  - ICS 인프라 공격
  - 산업제어시스템 침해
  - 수처리 시설 공격
sources:
  - neshenko_2021
updated: 2026-04-20
---

## 개요
ICS(산업제어시스템) 물리 센서 시계열 데이터에 BiGAN+RNN+CNN 비지도 앙상블을 적용해 공격 행위를 탐지·추론·귀인하는 포렌식 기법.

## 메커니즘
- **행위 지문(Behavior Fingerprinting)**: 정상 센서 패턴 모델링 → 이탈 탐지 (BiGAN)
- **공격 추론(Attack Inference)**: 이탈 패턴 → 공격 유형 분류 (RNN+CNN)
- **귀인(Attribution)**: CART 트리 + Shapley 값 + KernelSHAP → 판단 근거 설명
- 레이블 없는 데이터에서 학습 가능 (비지도) → 사전 공격 샘플 불필요

## 탐지 방법 (Investigator)
1. ICS 히스토리안 또는 SCADA 로그에서 물리 센서 시계열 추출
2. 정상 운영 기간 데이터로 BiGAN 학습
3. 사고 기간 데이터 입력 → 이탈 구간 식별
4. 공격 추론 모델 적용 → 공격 유형 분류
5. Shapley 값으로 주요 기여 센서 변수 확인 → 공격 경로 역추적

## 한계 & 주의사항
- 단일 수처리 시설 데이터셋 기반 — 다른 ICS 환경 일반화 미검증
- GAN 학습 파라미터 민감도 높음
- 실시간 탐지 파이프라인 미구현 (사후 포렌식 분석 도구)
- 전통적 디스크·네트워크 아티팩트와 연계 없음

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[neshenko_2021]] | 36개 인시던트 중 32개 탐지(88.9%); BiGAN+RNN+CNN 앙상블; Shapley 기반 설명 가능한 귀인 |

## 관련 페이지
[[papers/neshenko_2021]]
