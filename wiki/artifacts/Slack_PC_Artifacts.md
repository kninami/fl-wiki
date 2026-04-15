---
artifact: Slack_PC_Artifacts
layer: APP
data_types:
  - 채널 메시지 (LevelDB)
  - 캐시된 미디어
  - 인증 세션 정보
  - 로컬 스토리지
action_types:
  - 메시지 송수신
  - 파일 공유
  - 앱 로그인
os_version: Windows 10+
path: "%AppData%\\Roaming\\Slack"
attck:
sources:
  - shin_2020
updated: 2026-04-15
---

## 개요
Slack Windows PC 앱이 Electron 기반으로 생성하는 아티팩트 집합. Chromium 엔진 특성상 LevelDB, IndexedDB, Cache 구조를 사용한다.

## 추출 방법
- 경로: `%AppData%\Roaming\Slack`
- 하위 폴더: `Cache/`, `IndexedDB/`, `Local Storage/leveldb/`, `logs/`, `storage/`
- LevelDB는 직접 파싱 도구(leveldb-dump 등) 또는 Chromium 내부 파싱 도구 사용
- `chromium_dump_local_storage.py`로 Local Storage를 SQLite 변환 가능

## 분석 포인트
- **LevelDB**: 채널 메시지·사용자 상태 등 주요 데이터 저장
- **Cache**: 이미지·파일 썸네일 캐시 (MIME 타입으로 식별)
- **logs/**: 앱 실행·오류·로그인 이벤트 기록
- **storage/**: 팀 설정, 알림 설정 등

## Findings
- 시나리오 기반 복원 실험으로 LevelDB에서 채팅 내용 재구성 가능 확인 (→ [[shin_2020]])

## 관련 페이지
[[artifacts/Slack_Android_DB]] · [[techniques/Slack_Discord_IM_Forensics]] · [[papers/shin_2020]]
