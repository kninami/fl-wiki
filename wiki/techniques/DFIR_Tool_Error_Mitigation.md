---
technique: DFIR Tool Error Mitigation
perspective: Investigator
topic: Analysis
attck: —
related_artifacts: []
related_crime: []
sources:
  - hargreaves_2024
updated: 2026-04-15
---

## 개요
디지털 포렌식 도구의 각 처리 단계에서 발생 가능한 오류를 체계적으로 식별·추적·완화하기 위한 추상 모델 기반 방법론. 도구 자동화가 심화될수록 검증과 오류 관리가 더 중요해짐.

## 메커니즘

**추상 모델 (31개 프로세스 노드):**
주요 처리 단계와 의존성:
1. 미디어 이미지 파싱 → 2. 디스크 이미지 검증 → 3. 파티션 식별
4. 파일시스템 처리 (라이브/비할당 포함) → 5. 콘텐츠 카빙
6. 파일 타입 식별 → 7. 파일별 처리 (EXIF, DB, OCR 등)
8. 타임스탬프 추출 → 9. 지오로케이션 추출 → 10. 키워드 인덱싱
11. 파일 해싱 → 12. 해시 매칭

**오류 분류 (CASE 기반):**
| 코드 | 의미 |
|------|------|
| INCOMP | 불완전 (데이터 일부 누락) |
| INAC-AS | 부정확한 할당 (잘못된 유형/속성 부여) |
| INAC-COR | 부정확한 내용 (잘못된 값) |
| MISINT | 오해석 (올바른 데이터지만 잘못 분류) |
| INCOMP-AS | 불완전한 할당 |
| NAC-ALT | 부정확한 대안 |

## 탐지 방법 (Investigator)
**도구 검증 시 고려 사항:**
- 파티션 테이블 오류 → 파일시스템 처리까지 오류 전파
- 파일 타입 오인식 → 파일별 처리 전 단계 오류
- 해시 계산 오류 → 해시 매칭 단계에 영향

**검증 방법:**
- 알려진 데이터셋으로 각 단계 출력 검증
- CASE 표준 주석으로 오류 원인 문서화
- 단계 간 의존성 추적으로 근본 원인 특정

## 한계 & 주의사항
- Closed-source 도구는 내부 프로세스 추적 불가
- 모델이 모든 도구 기능을 완전히 커버하지 못할 수 있음
- 오류 완화는 가능하나 완전 제거 불가

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/hargreaves_2024]] | 31개 프로세스 노드 추상 모델; 단계 간 오류 전파 체계 규명; CASE 표준 주석 방법 |

## 관련 페이지
[[techniques/Selective_Seizure_Evaluation]] · [[techniques/Artifact_Knowledge_Management]] · [[techniques/Synthetic_Trace_Generation]]
