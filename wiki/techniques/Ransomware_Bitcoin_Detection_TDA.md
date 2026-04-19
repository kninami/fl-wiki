---
technique: Ransomware_Bitcoin_Detection_TDA
perspective: Investigator
topic: Analysis
attck: —
related_artifacts: []
related_crime:
  - 랜섬웨어
  - 암호화폐 추적
  - 사이버 금융범죄
sources:
  - akcora_2021
updated: 2026-04-20
---

## 개요
Bitcoin 블록체인에서 24시간 시간 창 단위로 위상 데이터 분석(TDA) 기반 6개 피처를 추출하여 랜섬웨어 주소를 식별하는 기법.

## 메커니즘
- Bitcoin 거래 그래프를 24시간 단위 슬라이딩 윈도우로 분할
- 각 주소에 대해 6개 위상 피처 계산:
  - **Income**: 총 수신 코인
  - **Neighbors**: 수신 트랜잭션 수
  - **Weight**: starter 트랜잭션에서 오는 비율
  - **Length**: 비-starter 체인의 최장 길이
  - **Count**: 경로로 연결된 starter 트랜잭션 수
  - **Loop**: 여러 경로를 가진 starter 트랜잭션 수
- Weight·Loop 피처가 랜섬웨어 행위 지문으로 가장 유효

## 탐지 방법 (Investigator)
1. 조사 대상 Bitcoin 주소의 거래 그래프 수집 (blockchain explorer API)
2. 24시간 윈도우별 6개 피처 계산
3. 사전 학습 모델에 입력 → 랜섬웨어 패밀리 분류
4. 귀인된 패밀리 기반 C2·지갑 주소 추가 추적

## 한계 & 주의사항
- 믹서(Tumbler) 서비스 사용 시 추적 회피 가능
- 27개 랜섬웨어 패밀리 한정 — 신규 패밀리 미검증
- 실시간 탐지 파이프라인 미구현 (배치 분석)
- 전통적 디스크·메모리 아티팩트와 연계 미지원

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[akcora_2021]] | 24,486개 주소, 27개 패밀리 대상 TDA 기반 분류; Weight·Loop 피처 효과적 |

## 관련 페이지
[[papers/akcora_2021]]
