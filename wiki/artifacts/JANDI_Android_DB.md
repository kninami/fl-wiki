---
artifact: JANDI_Android_DB
layer: APP
data_types:
  - 계정 정보 (이름, UUID, 활성화 시각)
  - 메시지 본문 및 타입
  - 파일·투표·할일 정보
  - 패스코드 (평문)
action_types:
  - 메시지 송수신
  - 파일 공유
  - 투표
  - 할일 작성
os_version: Android 9+
path: "/data/data/com.tosslab.jandi.app/databases/jandi-v2.db"
attck:
sources:
  - shin_2021
updated: 2026-04-15
---

## 개요
JANDI Android 앱의 SQLite 데이터베이스. Secure Delete 옵션 비활성화 시 삭제된 메시지 레코드가 잔류하여 복원 가능. 패스코드 평문 저장 취약점 존재.

## 추출 방법
- 경로: `/data/data/com.tosslab.jandi.app/databases/jandi-v2.db`
- 루팅 기기: adb pull 직접 접근
- 비루팅: 물리적 추출 또는 에이전트 기반 백업

## 분석 포인트

### 주요 테이블
| 테이블 | 주요 컬럼 | 내용 |
|--------|-----------|------|
| accounts | activatedAt, createdAt, name, loggedAt, uuid | 계정 기본 정보 |
| account_emails | — | 이메일 주소 |
| account_teams | — | 소속 팀 정보 |
| message_link | eventType, fromEntity, messageType, time, toEntity | 메시지 메타데이터 |
| message_text | isLiked, isStarred, writerId | 메시지 속성 |
| message_text_content | body | **실제 메시지 본문** |

### 메시지 타입 (messageType)
`TEXT` / `FILE` / `COMMENT` / `POLL` / `TODO` / `STICKER`

### 삭제 메시지 복원
- **Secure Delete OFF 조건**: 삭제 메시지 레코드 DB 잔류
- 복원 판단: `message_link` 레코드 수 > `message_text_content` 레코드 수 → 불일치분이 삭제 메시지
- 이미지: `image_manager_disk_cache`에 확장자 없이 저장 → PNG/JPG로 변경 후 열람

## 공격 기법 (해당 시)
- **패스코드 평문 저장 취약점**: `accounts` 테이블에서 패스코드 값(0→11) 직접 획득 가능

## Findings
- Secure Delete OFF 시 삭제 메시지 DB 잔류 확인; message_link vs message_text_content 수 비교로 복원 (→ [[papers/shin_2021]])
- 패스코드 평문 저장 취약점 발견 (→ [[papers/shin_2021]])

## 관련 페이지
[[artifacts/NAVER_WORKS_Android_DB]] · [[artifacts/JANDI_LevelDB]] · [[techniques/JANDI_NAVER_WORKS_Android_Forensics]] · [[techniques/JANDI_Artifact_Analysis]]
