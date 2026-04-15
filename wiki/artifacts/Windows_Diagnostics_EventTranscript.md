---
artifact: Windows Diagnostics (EventTranscript.db)
layer: OS
data_types:
  - USB 장치 정보 (제조사, 모델, 시리얼)
  - 웹 브라우저 활동 (URL, 탭 생성/닫기)
  - 무선 네트워크 정보 (SSID, BSSID)
  - 디바이스 연결/해제 이벤트
  - OS 버전, 하드웨어 정보
action_types:
  - USB 장치 연결/해제
  - 웹 브라우저 탐색
  - Wi-Fi 연결/해제
  - 시스템 진단 이벤트
os_version: Windows 10+
path: "C:\\ProgramData\\Microsoft\\Diagnosis\\EventTranscript\\EventTranscript.db"
attck: —
sources:
  - park_2022
updated: 2026-04-15
---

## 개요
Windows 10/11에서 Microsoft가 진단 및 문제 해결을 위해 수집하는 SQLite 데이터베이스. `diagtrack.dll`(DiagTrack 서비스)이 기록. 기본적으로 필수 진단 데이터만 수집되나, 선택적 진단 데이터 수집 활성화 시 사용자 행위 정보도 포함. 8개 테이블로 구성.

## 구조
- `events_persisted` 테이블이 핵심 (sid, timestamp, payload(JSON), full_event_name, logging_binary)
- payload 컬럼: 이벤트 데이터를 JSON 형식으로 저장
- 이진 파일명(logging_binaryname): 이벤트를 기록한 프로세스 이름

## 추출 방법
- 경로: `%ProgramData%\Microsoft\Diagnosis\EventTranscript\EventTranscript.db`
- SQLite 브라우저 또는 python sqlite3로 직접 조회
- DiagAnalyzer(Python3): 자동 분석·시각화·HTML 보고서 생성 (→ park_2022)
- 선택적 진단 데이터 활성화 여부에 따라 수집 범위 차이

## 분석 포인트
### USB 연결
- `Microsoft.Windows.Storage.Classpnp.DeviceGuidGenerated` → `Microsoft.Windows.FileSystem.FltMgr.InitInstance` 이벤트 시퀀스
- 제조사, 기기명, 시리얼 번호, 마운트 드라이브명 파악 가능
- 갑작스러운 제거: `Microsoft.Windows.FileSystem.NTFS.SurpriseRemove` 이벤트

### 웹 브라우저 (Edge)
- `Aria.<str>.Microsoft.WebBrowser.HistoryJournal.HJ_BrowserInfo`: 브라우저 시작
- `Aria.<str>.Microsoft.WebBrowser.HistoryJournal.HJ_TabCreated/Closed`: 탭 관리
- `Aria.<str>.Microsoft.WebBrowser.HistoryJournal.HJ_BeforeNavigateExtended`: 방문 URL
- 미방문 사이트: navigationUrl 필드에만 정보 있어 반복 조작 필요

### 무선 네트워크
- `Microsoft.OneCore.NetworkingTriage.GetConnected.WiFiStartConnection`: 연결 시도
- WlanMSM.WirelessScanResult: SSID, BSSID 기록
- BSSID로 Wi-Fi AP 위치 추적 가능 (외부 서비스 활용)

## Findings
- USB 연결/해제 정확한 시각, 장치 식별 정보 기록 → 증거 기기 반출 추적 가능 (→ [[papers/park_2022]])
- Edge 브라우저 탭 생성·URL 방문 기록 → 다른 브라우저 아티팩트 미발견 사이트도 추적 (→ [[papers/park_2022]])
- Wi-Fi AP BSSID 기반 위치 추적 가능 → 행동 반경 파악 (→ [[papers/park_2022]])
- DiagAnalyzer 도구로 3가지 행위 자동 분석·시각화 (→ [[papers/park_2022]])

## 관련 페이지
[[artifacts/Windows_Event_Log]] · [[techniques/Windows_Diagnostics_Forensics]]
