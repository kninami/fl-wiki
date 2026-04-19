---
technique: Explainable_DFAI
perspective: Both
topic: Analysis
attck: —
related_artifacts: []
related_crime:
  - 디지털 포렌식 AI 법정 증거 채택
  - AI 기반 포렌식 수사
sources:
  - solanke_2022
updated: 2026-04-20
---

## 개요
포렌식 AI(DFAI)의 법정 불신을 완화하기 위해 설명 가능한 포렌식 AI(xDFAI)의 목표·방법론·권고사항을 정의하는 프레임워크.

## 메커니즘
- **xDFAI 목표 7종**: Confidence, Trustworthiness, Discovering Causality, Algorithmic Fairness, Availability, Reproducibility, Informativeness
- **방법론 분류**:
  - Model-Agnostic(사후): 모델 단순화, 피처 중요도, 시각화, 로컬 설명, 텍스트 설명
  - Model-Specific: DNN·CNN·RNN 전용 설명 기법
- 법정에서 AI 판단의 설명 가능성이 증거 채택 요건임을 논증

## 탐지 방법 (Investigator)
- AI 분류 결과와 함께 피처 중요도·국소 설명(LIME, SHAP) 제공 → 전문가 증언 보완
- 모델 재현 가능성(Reproducibility) 확보: 동일 입력 → 동일 출력 보장
- 알고리즘 공정성(Fairness) 감사: 보호 특성 편향 검사

## 한계 & 주의사항
- 실증 평가 없는 이론·포지션 논문 — 구체적 구현 미제공
- xDFAI 기준 달성 여부 평가 방법론 미정의

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[solanke_2022]] | xDFAI 목표 7종 정의; Model-Agnostic vs Model-Specific 분류; 불신 완화 권고사항 7종 |

## 관련 페이지
[[papers/solanke_2022]]
