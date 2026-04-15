---
paper: hargreaves_2024
title: "An Abstract Model for Digital Forensic Analysis Tools (2024)"
full_title: "An abstract model for digital forensic analysis tools - A foundation for systematic error mitigation analysis"
authors: "Hargreaves, C.; Nelson, A.; Casey, E."
year: 2024
venue: DFRWS EU 2024 (FSI Digital Investigation 48)
doi: https://doi.org/10.1016/j.fsidi.2023.301679
tags:
  - digital forensics tools
  - abstract model
  - error mitigation
  - validation
  - CASE standard
  - tool testing
method: 방법론 (Autopsy·AXIOM·X-Ways 기능 분석 기반 모델 도출)
limitations: "closed-source 도구는 내부 프로세스 불투명; 모델이 완전하지 않을 수 있음"
---

## 한 줄 요약
디지털 포렌식 도구의 내부 프로세스를 추상화한 모델 제안 → 각 단계에서 발생 가능한 오류와 전파 경로를 체계적으로 분석.

## 핵심 발견
- 포렌식 도구 프로세스를 31개 노드로 추상화 (이미지 파싱, 파티션 식별, 파일시스템 처리, 콘텐츠 카빙, 파일 타입 식별, 해싱, 타임스탬프 추출 등)
- 각 프로세스 단계의 입력·출력·의존성을 명시적으로 정의
- 오류 분류: INCOMP(불완전), INAC-AS(부정확 할당), INAC-COR(부정확 내용), MISINT(오해석), INCOMP-AS 등
- 상위 레이어 오류가 하위 레이어로 전파되는 의존성 체계 규명
- CASE(Cyber-Investigation Analysis Standard Expression) 표준 기반 주석 방법 제시
- 도구 검증 시 프로세스 간 의존성을 고려해야 오류 원인 특정 가능

## 직접 분석한 아티팩트
없음 (방법론 논문)

## 영향받은 wiki 페이지
[[techniques/DFIR_Tool_Error_Mitigation]]
