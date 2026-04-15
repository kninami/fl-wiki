---
artifact: Discord_Android_DB
layer: APP
data_types:
  - 채널 메시지
  - 서버(길드) 목록
  - 사용자 정보
  - 인증 정보
action_types:
  - 메시지 송수신
  - 서버 참여
  - 앱 로그인
os_version: Android
path: "/data/data/com.discord/databases/"
attck:
sources:
  - shin_2020
updated: 2026-04-15
---

## 개요
Discord Android 앱의 로컬 SQLite 캐시 데이터베이스. 메시지·서버·사용자 정보와 `shared_prefs`에 인증 정보가 저장된다.

## 추출 방법
- 루팅 환경: `/data/data/com.discord/databases/` 직접 접근
- `shared_prefs/`: `com.discord_preferences.xml` 등에서 토큰 정보 확인
- ADB 또는 물리 추출(루팅 필요)

## 분석 포인트
| 테이블/파일 | 내용 |
|------------|------|
| `STORE_MESSAGE_CACHE` | 채널별 메시지 캐시 |
| `STORE_CHANNELS` | 참여 채널 목록·메타데이터 |
| `STORE_GUILDS` | 참여 서버(Guild) 목록 |
| `STORE_USERS` | 대화 상대 사용자 정보 |
| `shared_prefs` | 인증 토큰·계정 설정 |

## Findings
- 시나리오 실험으로 서버 채널 메시지·사용자 정보 복원 가능 확인 (→ [[shin_2020]])

## 관련 페이지
[[artifacts/Discord_PC_Artifacts]] · [[techniques/Slack_Discord_IM_Forensics]] · [[papers/shin_2020]]
