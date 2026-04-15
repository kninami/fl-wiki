---
technique: Slack_Discord_IM_Forensics
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - Slack_Android_DB
  - Slack_PC_Artifacts
  - Discord_Android_DB
  - Discord_PC_Artifacts
related_crime:
  - 통신 기록 확보
  - 내부자 위협
  - 사이버범죄 수사
sources:
  - shin_2020
updated: 2026-04-15
---

## 개요
Slack과 Discord의 Android/PC 환경 아티팩트를 시나리오 기반으로 분석하여 대화·파일·사용자 정보 복구 가능성을 검증하는 기법.

## 메커니즘

### Slack Android DB 구조
- **DB 경로**: `/data/data/com.Slack/databases/`
- 주요 테이블: `files`, `message_threads`, `message`, `messaging_channel_counts`, `messaging_channels`, `users`
- SQLite 직접 파싱으로 메시지·파일 목록·사용자 정보 복원

### Slack PC 아티팩트 구조
- **경로**: `%AppData%\Roaming\Slack\`
- 구성: Cache, IndexedDB, leveldb, logs, storage
- 대화 내용: LevelDB에 저장 — leveldb 파서로 추출
- Cache: 공유된 이미지·파일 캐시

### Discord Android DB 구조
- **DB 경로**: `/data/data/com.discord/databases/`
- 주요 테이블: `STORE_CHANNELS`(채널 정보), `STORE_MESSAGE_CACHE`(메시지 캐시), `STORE_GUILDS`(서버 정보), `STORE_USERS`(사용자 정보)
- `shared_prefs`: 인증 토큰 저장

### Discord PC 아티팩트 구조
- **주요 경로**:
  - `%LocalAppData%\Discord` — 앱 설치·캐시
  - `%AppData%\Roaming\Discord` — 사용자 데이터
- Cache 폴더에 공유 파일 캐시

## 복구 가능 정보
- 채널 메시지 내용 및 타임스탬프
- 파일 공유 이력
- 사용자 ID·닉네임·프로필
- Discord Android: shared_prefs에서 인증 토큰 획득 → API 접근 가능

## 한계 & 주의사항
- 특정 버전 기준 — 앱 업데이트 시 DB 스키마 변경 가능
- Android/PC 환경 한정 (iOS 미포함)
- Discord PC는 SQLite 아닌 JSON/LevelDB 기반 — 별도 파서 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/shin_2020]] | Slack/Discord Android SQLite + PC LevelDB/Cache 구조 분석; 시나리오 기반 복구 가능성 검증 |

## 관련 페이지
[[artifacts/Slack_Android_DB]] · [[artifacts/Slack_PC_Artifacts]] · [[artifacts/Discord_Android_DB]] · [[artifacts/Discord_PC_Artifacts]] · [[techniques/Teams_Differential_Forensics]]
