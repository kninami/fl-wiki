---
artifact: Microsoft_Teams_Windows
layer: APP
data_types:
  - 채팅 메시지 (LevelDB)
  - 로컬 스토리지 (팀·채널 정보)
  - 캐시된 미디어
  - 인증 토큰
action_types:
  - 메시지 송수신
  - 파일 공유
  - 화상회의
os_version: Windows 10+
path: "%AppData%\\Roaming\\Microsoft\\Teams"
attck:
sources:
  - kim_2021
updated: 2026-04-15
---

## 개요
Microsoft Teams Windows 앱(Electron 기반)의 로컬 아티팩트. Chromium 엔진 특성으로 IndexedDB, LevelDB, Local Storage 구조를 사용한다.

## 추출 방법
- 경로: `C:\Users\%USERPROFILE%\AppData\Roaming\Microsoft\Teams`
- 하위: `IndexedDB/`, `Local Storage/leveldb/`, `Cache/`, `logs.txt`
- LevelDB 파싱 도구 또는 Chromium 전용 파서 사용

## 분석 포인트
- **IndexedDB**: 채팅 메시지, 파일 목록, 채널 정보 저장
- **Local Storage**: 인증 토큰, 팀·채널 메타데이터
- **Cache**: 파일 썸네일, 프로필 이미지
- **logs.txt**: 앱 실행·오류·네트워크 이벤트

## Findings
- 25개 행위 중 18개(72%) Windows 아티팩트만으로 탐지 가능 (→ [[kim_2021]])
- Android 아티팩트 병합 시 23개(92%)로 탐지율 향상 (→ [[kim_2021]])
- Differential Forensics(행위 전후 diff)로 변화 파일 특정 가능 (→ [[kim_2021]])

## 관련 페이지
[[artifacts/Microsoft_Teams_Android]] · [[techniques/Teams_Differential_Forensics]] · [[papers/kim_2021]]
