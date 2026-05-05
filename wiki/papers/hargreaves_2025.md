---
paper: hargreaves_2025
title: "SOLVE-IT: A Proposed Digital Forensic Knowledge Base Inspired by MITRE ATT&CK (2025)"
full_title: "SOLVE-IT: A proposed digital forensic knowledge base inspired by MITRE ATT&CK"
authors: "Hargreaves, C.; van Beek, H.; Casey, E."
year: 2025
venue: Forensic Science International: Digital Investigation 52 (2025) 301864 / DFRWS EU 2025
doi: https://doi.org/10.1016/j.fsidi.2025.301864
tags:
  - SOLVE-IT
  - knowledge base
  - digital forensic techniques
  - weaknesses
  - mitigations
  - MITRE ATT&CK
  - quality assurance
  - CASE Ontology
method: 시스템 구축 (knowledge base design + prototype implementation)
limitations: "초기 버전(104 기법); 일부 필드 자유 텍스트; crowdsourcing 품질관리 체계 미비; 기법 분류의 세분화 수준 미정"
---

## 한 줄 요약
MITRE ATT&CK에서 영감받아 디지털 포렌식 기법·취약점·완화책을 체계적으로 색인한 지식 베이스 SOLVE-IT의 설계와 활용 사례 제시.

## 핵심 발견
- 디지털 포렌식에도 MITRE ATT&CK 수준의 구조화된 기법 지식 베이스가 필요하며 실현 가능함을 실증
- **SOLVE-IT 구조:** 104개 기법(T1001–T1106) × 17개 목표(objectives) 분류; 156개 취약점(weaknesses), 108개 완화책(mitigations) 색인
- ATT&CK의 Tactics → SOLVE-IT의 Objectives(조사 목표), Techniques → 포렌식 기법으로 대응 구조 설계
- 기법마다 ID, 설명, 동의어, 하위기법, CASE 출력 엔티티, 예시 데이터셋, 취약점, 참조를 문서화
- 취약점(Weakness)은 INCOMP·INAC-EX·INAC-AS·INAC-ALT·INAC-COR·MISINT 6가지 오류 유형으로 분류
- CASE Ontology 클래스와 매핑하여 기법 결과물을 온톨로지로 표현 가능 (e.g. T1072 → observable:Message)
- 104개 기법 중 AI 적용 가능 51개, 도구 내 구현 5개, 학술 구현 11개를 분류 식별
- GitHub 크라우드소싱 방식으로 외부 연구자 기여 검증 완료 (Timeline generation, Memory imaging 등)
- 기존에 완화책이 없는 취약점 다수 존재 → 향후 연구 방향 식별 도구로 활용 가능

## 직접 분석한 아티팩트
없음 (프레임워크·방법론 논문)

## 영향받은 wiki 페이지
[[techniques/SOLVE_IT_Knowledge_Base]] · [[techniques/DFIR_Tool_Error_Mitigation]]
