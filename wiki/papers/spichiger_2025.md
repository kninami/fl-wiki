---
paper: spichiger_2025
title: "Preserving Meaning of Evidence from Evolving Systems (2025)"
full_title: "Preserving meaning of evidence from evolving systems"
authors: "Spichiger, H.; Adelstein, F."
year: 2025
venue: DFRWS EU 2025 (FSI Digital Investigation 52)
doi: https://doi.org/10.1016/j.fsidi.2025.301867
tags:
  - evidence preservation
  - reference data
  - evolving systems
  - data uncertainty
  - distributed systems
  - cloud forensics
method: 이론·개념적 분석
limitations: "실험 검증 없음; 권장 사항 수준"
---

## 한 줄 요약
클라우드·분산 시스템처럼 지속적으로 변화하는 환경에서 디지털 증거의 의미를 보존하기 위한 참조 데이터(Reference Data) 수집·관리 체계 제안.

## 핵심 발견
- **Reference Data**: 트레이스를 분류·해석하기 위한 기준 데이터 (실험·비교 통해 생성)
- 기존 OSAC/SWGDE 프레임워크는 정적 시스템을 가정 → 진화하는 시스템에서는 불충분
- 진화하는 시스템에서 참조 데이터가 오래되면 증거 해석이 잘못될 수 있음
- 예: 클라우드 서비스의 서버사이드 코드 변경 → 앱 로그 의미 변경
- 모바일 앱 잦은 업데이트, 라이브러리 공유 환경에서 참조 데이터 수집이 핵심 과제
- 제안: 참조 데이터를 지속적으로 수집·아카이빙하고, 트레이스-참조 데이터 거리를 추적

## 직접 분석한 아티팩트
없음 (이론 논문)

## 영향받은 wiki 페이지
[[techniques/Evidence_Preservation_Evolving]]
