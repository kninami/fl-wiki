---
technique: Windows Diagnostics Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Windows_Diagnostics_EventTranscript
related_crime:
  - 디지털성범죄
  - 마약 범죄
  - 사이버범죄 수사
  - 기업 내부자 수사
sources:
  - park_2022
updated: 2026-04-15
---

## 개요
Windows 10/11 기본 탑재 진단 서비스(DiagTrack)가 생성하는 EventTranscript.db를 포렌식적으로 분석해 사용자의 USB 사용, 웹 브라우저 활동, 무선 네트워크 접속 이력을 추출하는 기법. 다른 아티팩트가 삭제·조작된 경우에도 보조 증거로 활용 가능.

## 메커니즘
- DiagTrack 서비스(diagtrack.dll)가 Windows 이벤트를 `EventTranscript.db`에 SQLite 형식으로 기록
- 기본 필수 데이터만 수집; 선택적 진단 데이터 활성화 시 사용자 행위 기록 포함
- 이벤트 유형은 JSON payload 내 full_event_name으로 분류

## 탐지 방법 (Investigator)
**USB 분석:**
```sql
SELECT * FROM events_persisted 
WHERE full_event_name LIKE '%Storage.Classpnp%' OR full_event_name LIKE '%NTFS%'
```
- 연결 이벤트 시퀀스 확인으로 장치 식별 정보 추출
- SurpriseRemoval 이벤트 = 갑작스러운 제거

**웹 브라우저 분석 (Edge):**
- HJ_BeforeNavigateExtended에서 방문 URL 추출
- HJ_TabCreated/Closed로 탭 활동 재구성
- 미방문 사이트는 navigationUrl 필드 반복 조작으로 확인

**무선 네트워크 분석:**
- WlanMSM.WirelessScanResult에서 SSID/BSSID 추출
- BSSID → 외부 Wi-Fi 위치 서비스로 좌표 변환 가능

**자동화:** DiagAnalyzer(Python3) 실행 → HTML 보고서 자동 생성

## 한계 & 주의사항
- 선택적 진단 데이터 미활성화 시 사용자 행위 기록 없음 (활성화 여부 먼저 확인)
- Edge 외 다른 브라우저 활동은 기록되지 않음
- Windows 10/11 한정; 서버 OS나 구버전 비적용
- 이벤트 구조는 Windows 업데이트에 따라 변경 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/park_2022]] | USB 장치 정보·브라우저 URL·Wi-Fi BSSID 추출 성공; DiagAnalyzer 도구 개발 |

## 관련 페이지
[[artifacts/Windows_Diagnostics_EventTranscript]] · [[artifacts/Windows_Event_Log]]
