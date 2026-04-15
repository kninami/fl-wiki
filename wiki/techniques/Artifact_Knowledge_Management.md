---
technique: Artifact Knowledge Management
perspective: Both
topic: Research Methodology
attck: —
related_artifacts: []
related_crime: []
sources:
  - grajeda_2018
updated: 2026-04-15
---

## 개요
디지털 포렌식 아티팩트 지식을 체계적으로 수집·큐레이션·공유하는 방법론 및 플랫폼 설계 원칙. 개별 수사관의 고립된 지식을 커뮤니티 공유 지식으로 전환.

## 메커니즘

**CuFA(Curated Forensic Artifact) 모델:**
- 필수 필드: 이름, 설명, 주석, MD5/SHA1/MRSHv2, 발견 시각, 등록자, 위치 유형, 장치(제조사·모델·OS)
- CyBOX 객체 상속: File, Process, Win Registry, Network Socket, Archive File 등
- 위치 유형(Location Type): User생성, Application생성, System생성, Download, Network

**아티팩트 분류 체계:**
- 출처 기반: User/Application/System/Download/Network
- 장치 기반: PDA/Mobile/Laptop/Server/External
- 상태: Queued(관리자 검토 대기), Flagged(사용자 업데이트 필요)

**플랫폼 아키텍처:**
- PostgreSQL + Python Django + Ubuntu
- 9가지 파일 유형 태깅 (DB, Text, XML, Log 등 세분화)
- 이중 심사(사용자 + 관리자)로 데이터 무결성 보장

## 활용 방법

**수사관:**
- 키워드/아티팩트 유형/날짜로 아티팩트 검색
- CSV 내보내기로 도구 연동
- 새로운 아티팩트 발견 시 공유

**연구자:**
- 새로운 앱 아티팩트 추가 및 문서화
- 기존 아티팩트 업데이트 (앱 버전 변화 반영)
- 공동 연구 기반 마련

## 한계 & 주의사항
- 커뮤니티 기여에 의존 → 특정 플랫폼/앱 아티팩트 편중
- 심사 지연으로 최신 아티팩트 반영 느릴 수 있음
- 아티팩트 정보의 일반화 가능성 검증 어려움

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/grajeda_2018]] | AGP 플랫폼 구축 경험; CuFA/CyBOX 기반 아티팩트 모델; 50개+ 앱 커버리지 |

## 관련 페이지
[[techniques/DFIR_Tool_Error_Mitigation]] · [[techniques/Synthetic_Trace_Generation]]
