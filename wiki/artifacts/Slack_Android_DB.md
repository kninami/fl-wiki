---
artifact: Slack_Android_DB
layer: APP
data_types:
  - 채널 메시지
  - 파일 전송 기록
  - 사용자 정보
  - 채널 목록
action_types:
  - 메시지 송수신
  - 파일 공유
  - 채널 참여
os_version: Android
path: "/data/data/com.Slack/databases/"
attck:
sources:
  - shin_2020
updated: 2026-04-15
---

## 개요
Slack Android 앱이 로컬에 저장하는 SQLite 데이터베이스. 채널 메시지·파일·사용자 정보를 오프라인에서도 접근할 수 있도록 캐시 형태로 유지한다.

## 추출 방법
- 루팅 환경: `/data/data/com.Slack/databases/` 직접 접근
- ADB 백업 또는 물리 추출(루팅 필요)
- SQLite Browser, Autopsy 등으로 파싱

## 분석 포인트
| 테이블 | 주요 컬럼 | 포렌식 의미 |
|--------|-----------|------------|
| `message` | timestamp, user_id, channel_id, text | 메시지 내용·시간 |
| `message_threads` | thread_ts, reply_count | 스레드 대화 구조 |
| `files` | name, url_private, created, file_type | 파일 공유 기록 |
| `messaging_channels` | channel_id, name, type | 참여 채널 목록 |
| `messaging_channel_counts` | unread_count | 미확인 메시지 수 |
| `users` | user_id, name, display_name | 대화 상대 식별 |

## Findings
- 시나리오 기반 복원 실험으로 채널 메시지·파일·사용자 정보 재구성 가능 확인 (→ [[shin_2020]])

## 관련 페이지
[[artifacts/Slack_PC_Artifacts]] · [[techniques/Slack_Discord_IM_Forensics]] · [[papers/shin_2020]]
