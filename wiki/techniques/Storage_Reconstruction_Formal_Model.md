---
technique: Storage_Reconstruction_Formal_Model
perspective: Investigator
topic: Extraction/Analysis
attck: —
related_artifacts: []
related_crime:
  - 디스크 포렌식
  - 삭제 파일 복구
sources:
  - schneider_2020
updated: 2026-04-20
---

## 개요
메타데이터 기반 스토리지 재구성과 파일 카빙을 단일 형식 모델로 통합하는 LAYR 프레임워크. 연산자 조합으로 다양한 파일시스템 복구 전략을 표현·실행한다.

## 메커니즘
- 기존 스토리지 재구성(FAT, Ext3)과 파일 카빙은 분리된 기법 → LAYR이 통합
- **연산자 5종**: Sequential (>>), Parallel (||), Union (+), Intersection (*), Minus (-)
- **휴리스틱 3종**: DOS, Ext3, Carving
- 각 파일시스템에 맞는 휴리스틱 작성 → 연산자 조합 → 복구 파이프라인 구성
- C++ 구현체(LAYR 도구) 제공

## 탐지 방법 (Investigator)
1. 대상 디스크 이미지 수집
2. 파일시스템 유형 판별 → 해당 휴리스틱 선택
3. LAYR 연산자로 복구 파이프라인 정의
4. 실행 → 메타데이터 재구성 + 카빙 결과 통합 출력
5. 새 파일시스템: 휴리스틱만 추가 작성하면 기존 파이프라인 재활용 가능

## 한계 & 주의사항
- 프로토타입 수준 — 대규모 실세계 이미지 검증 미수행
- 암호화 볼륨, 일부 독점 파일시스템 미지원 가능
- 연산자 파이프라인 설계에 전문 지식 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[schneider_2020]] | 형식 모델로 재구성+카빙 통합; 연산자 5종·휴리스틱 3종 정의; C++ LAYR 도구 구현 |

## 관련 페이지
[[papers/schneider_2020]]
