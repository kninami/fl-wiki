---
technique: Windows Event Log Automated Recovery
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Windows_Event_Log
related_crime:
  - 사이버범죄 수사
  - 일반 컴퓨터 포렌식
sources:
  - murphey_2007
updated: 2026-04-15
---

## 개요
손상된 Windows NT5(XP/2003) 이벤트 로그를 자동으로 복구하고, 다중 로그를 단일 패스로 처리해 상관분석하는 방법론. 포렌식 관여 효율성을 높이기 위한 자동화 접근법.

## 메커니즘
NT5 이벤트 로그(.evt) 구조:
- 파일 헤더: signature + 파일 길이로 무결성 판별
- 각 이벤트 레코드: 고정 헤더 + 가변 메시지 데이터
- 최대 크기 도달 시 순환 덮어쓰기 (wrapping)
- 손상 패턴: 헤더 오버라이트, 잘린 레코드, 순환 포인터 불일치

## 탐지 방법 (Investigator)
**Scalped 도구 활용:**
1. .evt 파일 헤더 signature 확인 → 손상 여부 판별
2. 손상된 헤더 자동 재구성 (signature: `0x4c664c65`)
3. 파일 길이와 실제 크기 불일치 시 복구
4. 다중 로그 파일 일괄 처리 → 타임스탬프 기반 이벤트 정렬

**다중 로그 상관분석:**
- 시스템·보안·응용 프로그램 로그 동시 처리
- $MFT, Prefetch 타임스탬프와 교차 검증
- XML 또는 CSV 출력으로 분석 도구 연동

## 한계 & 주의사항
- NT5(.evt) 포맷 전용; Vista+ .evtx 포맷 비적용
- 순환 버퍼 완전 덮어쓰기 시 복구 불가
- 최대 로그 크기: 워크스테이션 512KB, 서버 1MB → 장기 수사에서 기록 손실 가능
- 이벤트 메시지 DLL이 없는 경우 메시지 렌더링 불가

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/murphey_2007]] | NT5 이벤트 로그 자동 복구 방법론; Scalped 오픈소스 도구; 다중 로그 단일 패스 처리 |

## 관련 페이지
[[artifacts/Windows_Event_Log]] · [[techniques/Timestamp_Manipulation_Detection]]
