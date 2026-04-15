---
paper: grajeda_2018
title: "Experience Constructing the Artifact Genome Project (AGP) (2018)"
full_title: "Experience constructing the Artifact Genome Project (AGP): Managing the domain's knowledge one artifact at a time"
authors: "Grajeda, C.; Sanchez, L.; Baggili, I.; Clark, D.; Breitinger, F."
year: 2018
venue: DFRWS 2018 USA (Digital Investigation 26)
doi: https://doi.org/10.1016/j.diin.2018.04.021
tags:
  - artifact knowledge management
  - digital forensics repository
  - AGP
  - CuFA
  - CyBOX
  - community forensics
method: 시스템 구축·운영 경험 보고
limitations: "커뮤니티 참여에 의존; 아티팩트 심사(vetting) 과정에 시간 소요"
---

## 한 줄 요약
디지털 포렌식 아티팩트 지식을 체계적으로 수집·큐레이션하는 커뮤니티 플랫폼 AGP(Artifact Genome Project) 구축 경험 공유.

## 핵심 발견
- **CuFA(Curated Forensic Artifact)**: AGP의 아티팩트 단위 모델; 이름·설명·위치유형·MD5/SHA1·발견시각·연관아티팩트 포함
- **CyBOX 스키마** 기반: File, Process, Win Registry, Archive, Network Socket 등 표준 객체 유형
- AGP 아키텍처: PostgreSQL + Python Django + Ubuntu 서버
- 아티팩트 태깅: 테이블·셀·XML·텍스트 수준까지 세분화 가능
- 50개 앱 이상 아티팩트 등록; 법집행·학계·민간 다양한 기여자
- 사용자·관리자 심사 이중 구조로 데이터 무결성 보장

## 직접 분석한 아티팩트
없음 (플랫폼 논문)

## 영향받은 wiki 페이지
[[techniques/Artifact_Knowledge_Management]]
