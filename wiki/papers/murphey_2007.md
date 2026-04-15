---
paper: murphey_2007
title: "Automated Windows Event Log Forensics (2007)"
full_title: "Automated Windows event log forensics"
authors: "Murphey, R."
year: 2007
venue: DFRWS 2007 USA (Digital Investigation 4S)
doi: https://doi.org/10.1016/j.diin.2007.06.012
tags:
  - Windows event log
  - NT5
  - Windows XP
  - automation
  - log recovery
  - log analysis
method: 실험 (도구 개발 + Windows XP/2003 케이스 검증)
limitations: "NT5(XP/2003) 이벤트 로그 형식 한정; Vista 이후 EVTX 포맷 비적용"
---

## 한 줄 요약
Windows NT5(XP/2003) 이벤트 로그의 손상 자동 복구, 다중 로그 상관분석, 포렌식 워크플로 자동화 방법론 및 Scalped 도구 개발.

## 핵심 발견
- NT5 이벤트 로그 파일 헤더 signature와 파일 길이로 손상 여부 판별
- Scalped: 손상된 NT5 이벤트 로그를 자동 복구하는 오픈소스 도구
- 여러 이벤트 로그를 단일 패스로 수리·복구·상관분석 가능
- $MFT, Prefetch, FTK 등 다른 Windows 아티팩트와 타임스탬프 교차 검증 방법 제시
- 이벤트 로그 크기: 워크스테이션 기본 최대 512KB, 서버 최대 1MB
- 로그 순환(rotation) 시 가장 오래된 항목 덮어쓰기 → 복구 한계

## 직접 분석한 아티팩트
[[artifacts/Windows_Event_Log]]

## 영향받은 wiki 페이지
[[techniques/EventLog_Automated_Recovery]]
