---
artifact: NAVER_WORKS_Android_DB
layer: APP
data_types:
  - 채팅 메시지 (다양한 타입)
  - 파일 공유 기록
  - 사용자 정보
  - 캘린더·일정 정보
action_types:
  - 메시지 송수신
  - 파일 공유
  - 일정 등록
os_version: Android 9+
path: "/data/data/com.gworks.oneapp.naverworks/databases/"
attck:
sources:
  - shin_2021
updated: 2026-04-15
---

## 개요
NAVER WORKS Android 앱의 SQLite 데이터베이스. `status='DELETED'` 레코드로 삭제 메시지를 직접 식별 가능하며 캘린더·메시지 DB가 분리되어 있다.

## 추출 방법
- 경로: `/data/data/com.gworks.oneapp.naverworks/databases/`
- DB 파일명: `[MultiAccountPrefix]_cal.db`, `[MultiAccountPrefix]_message.db`
- 루팅 기기: adb pull; 비루팅: 물리적 추출

## 분석 포인트

### cal.db 구조
| 테이블 | 내용 |
|--------|------|
| calendar | 캘린더 목록 |
| event | 일정 기본 정보 |
| eventDetail | 일정 상세 정보 |

### message.db 구조
| 테이블 | 주요 컬럼 | 내용 |
|--------|-----------|------|
| chat_channels | — | 채팅 채널 목록 |
| chat_message | type, status, content | 메시지 본문 및 상태 |
| chat_files | — | 공유 파일 정보 |
| chat_users | — | 사용자 정보 |

### chat_message 타입 코드
1, 2, 4, 5, 7, 11, 16, 18, 25, 27, 29, 37, 39, 97, 98, 101, 106, 113, 114

### 삭제 메시지 식별
- `chat_message.status = 'DELETED'` 레코드로 직접 확인 가능
- JANDI와 달리 삭제 상태가 필드값으로 명시됨

## Findings
- status='DELETED' 레코드로 삭제 메시지 직접 식별 가능 (→ [[papers/shin_2021]])
- JANDI와 달리 암호화 미적용 — 직접 SQLite 파싱 가능 (→ [[papers/shin_2021]])

## 관련 페이지
[[artifacts/JANDI_Android_DB]] · [[techniques/JANDI_NAVER_WORKS_Android_Forensics]]
