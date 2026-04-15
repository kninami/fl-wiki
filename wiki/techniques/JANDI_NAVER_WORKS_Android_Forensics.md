---
technique: JANDI_NAVER_WORKS_Android_Forensics
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - JANDI_Android_DB
  - NAVER_WORKS_Android_DB
related_crime:
  - 업무 관련 범죄
  - 내부자 위협
  - 증거 확보
sources:
  - shin_2021
updated: 2026-04-15
---

## 개요
국내 협업 메신저 JANDI·NAVER WORKS의 Android 앱 DB 구조를 분석하고, Secure Delete 옵션 비활성화 시 삭제 메시지·파일을 복원하는 기법. JANDI 패스코드 평문 저장 취약점도 발견.

## 메커니즘

### JANDI Android DB
- **패키지**: `com.tosslab.jandi.app`
- **DB 파일**: `jandi-v2.db`
- 주요 테이블:
  - `account_emails`, `account_teams`, `accounts`(activatedAt, createdAt, name, loggedAt, uuid)
  - `message_link`(eventType, fromEntity, messageType: TEXT/FILE/COMMENT/POLL/TODO/STICKER, time, toEntity)
  - `message_text`(isLiked, isStarred, writerId)
  - `message_text_content`(body — 실제 메시지 본문)

### JANDI 삭제 메시지 복원
- **Secure Delete OFF 환경**: 삭제된 메시지 레코드 DB에 잔류
- 복원 방법: `message_link` 수 vs `message_text_content` 수 비교 → 불일치 항목이 삭제 메시지
- 이미지: `image_manager_disk_cache`에 확장자 없이 저장 → PNG/JPG 확장자 추가로 열람

### JANDI 보안 취약점
- **패스코드 평문 저장** (0→11 값): `accounts` 테이블에서 직접 획득 가능

### NAVER WORKS Android DB
- **패키지**: `com.gworks.oneapp.naverworks`
- 주요 DB:
  - `[prefix]_cal.db`: calendar, event, eventDetail 테이블
  - `[prefix]_message.db`: chat_channels, chat_message(type 1/2/4/5/7/11/16/18/25/27/29/37/39 등), chat_files, chat_users
- **삭제 메시지 식별**: `status='DELETED'` 레코드로 직접 확인

## 한계 & 주의사항
- JANDI v2.28.3 / NAVER WORKS v3.0.1.4, Android 9 한정
- Secure Delete ON 환경에서는 삭제 메시지 복원 불가
- JANDI DB 암호화 없음 → 루팅 기기에서 직접 접근 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/shin_2021]] | JANDI/NAVER WORKS DB 스키마 상세 분석; Secure Delete OFF 시 삭제 메시지 잔류 확인; JANDI 패스코드 평문 저장 취약점 발견 |

## 관련 페이지
[[artifacts/JANDI_Android_DB]] · [[artifacts/NAVER_WORKS_Android_DB]] · [[techniques/JANDI_Artifact_Analysis]] · [[techniques/Slack_Discord_IM_Forensics]]
