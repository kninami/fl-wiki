---
artifact: Windows Event Log
layer: OS
data_types:
  - Event ID
  - 이벤트 소스
  - 타임스탬프
  - 이벤트 메시지
  - 사용자/프로세스 컨텍스트
action_types:
  - 시스템 이벤트
  - 보안 감사
  - 응용 프로그램 오류
  - 로그온/로그오프
  - 프로세스 생성
os_version: Windows (모든 버전; NT5: .evt 포맷, Vista+: .evtx 포맷)
path: "C:\\Windows\\System32\\winevt\\Logs\\*.evtx (Vista+) / C:\\Windows\\System32\\config\\*.evt (NT5)"
attck: T1070.001
sources:
  - palmbach_2020
  - murphey_2007
updated: 2026-04-15
---

## 개요
Windows 운영체제의 핵심 진단 및 감사 로그. 시스템·보안·응용 프로그램 채널로 구분. NT5(XP/2003)는 .evt 바이너리 포맷, Vista 이후는 XML 기반 .evtx 포맷 사용. 포렌식에서 사용자 행동, 시스템 상태, 보안 이벤트 재구성에 필수.

## 추출 방법
- **NT5**: Scalped 도구로 손상된 .evt 파일 복구 (→ murphey_2007)
- **Vista+**: Event Log Explorer, Autopsy, X-Ways, python-evtx
- 이벤트 로그 크기: 워크스테이션 기본 최대 512KB, 서버 최대 1MB
- 최대 크기 도달 시 순환 덮어쓰기 or 아카이브 (정책에 따라)

## 분석 포인트
- NT5 이벤트 로그 헤더: signature + 파일 길이로 무결성 판별
- 다중 로그 상관분석: 동일 타임스탬프 이벤트 교차 검증
- 타임스탬핑 도구 실행 이벤트 기록 가능 (보안 감사 활성화 시)
- DiagAnalyzer 분석 대상인 Windows Diagnostics(EventTranscript.db)와 별도 아티팩트

## Findings
- 손상된 NT5 이벤트 로그 자동 복구 가능; 다중 로그 단일 패스 처리로 효율화 (→ [[papers/murphey_2007]])
- 타임스탬프 조작 탐지에 부가적 맥락 제공; 단독으로 직접 탐지 한계 (→ [[papers/palmbach_2020]])
- $MFT, Prefetch 등 다른 아티팩트와 타임스탬프 교차 검증에 활용 가능 (→ [[papers/murphey_2007]])

## 관련 페이지
[[artifacts/Windows_Diagnostics_EventTranscript]] · [[artifacts/Prefetch]] · [[techniques/EventLog_Automated_Recovery]] · [[techniques/Timestamp_Manipulation_Detection]]
