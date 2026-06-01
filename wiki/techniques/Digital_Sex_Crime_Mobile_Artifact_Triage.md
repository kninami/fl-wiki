---
technique: Digital Sex Crime Mobile Artifact Triage
perspective: Investigator
topic: Evidence Prioritization
related_artifacts:
  - Discord_Android_DB
  - Slack_Android_DB
  - JANDI_Android_DB
  - NAVER_WORKS_Android_DB
  - Microsoft_Teams_Android
  - WeChat_Mobile_DB
related_crime:
  - 디지털성범죄 수사
  - 통신 기록 확보
  - 사용자 귀속 판단
sources:
  - shin_2020
  - shin_2021
  - kim_2021
  - park_2020
  - yun_2015
updated: 2026-05-22
---

## 개요
현재 위키에 정리된 Android 메신저·협업 앱 아티팩트를 기준으로, 디지털성범죄 수사에서 휴대폰에서 우선 확보할 증거를 triage 관점으로 정리한 페이지. 단일 디지털성범죄 전용 논문이 아니라, 통신 기록·파일 공유·삭제 흔적·사용자 귀속 관련 연구를 결합해 수사 체크리스트로 재구성한 것이다.

## 메커니즘
휴대폰 내 메신저·협업 앱 DB는 단순 채팅 본문만이 아니라 다음 층위를 함께 남긴다.

- **대화 내용**: 메시지 본문, 메시지 타입, 전송 시각
- **관계 구조**: 채널·길드·채팅방·테넌트·참여자 목록
- **공유 행위**: 첨부 파일, 이미지 캐시, 파일 메타데이터
- **귀속 정보**: 계정명, UUID, 사용자 ID, 토큰, 기기 식별자
- **은닉 흔적**: 삭제 메시지 상태값, DB 잔류 레코드, WAL 잔존
- **행동 문맥**: 통화, 위치, Wi-Fi, 일정, 앱 설치 정보

디지털성범죄 맥락에서는 위 정보를 교차해 "누가", "누구와", "무엇을", "어떤 파일로", "삭제를 시도했는지"를 재구성하는 데 의미가 있다. 사용자 인지도 판단은 [[techniques/User_Attribution_Framework]]의 귀속 프레임워크를 보조 기준으로 활용할 수 있다.

## 탐지 방법 (Investigator)

### 1. 채팅 본문 DB를 최우선 확보
- [[artifacts/Discord_Android_DB]]: `STORE_MESSAGE_CACHE`, `STORE_CHANNELS`, `STORE_GUILDS`, `STORE_USERS`
- [[artifacts/Slack_Android_DB]]: `message`, `message_threads`, `users`, `messaging_channels`
- [[artifacts/JANDI_Android_DB]]: `message_text_content.body`, `message_link`
- [[artifacts/NAVER_WORKS_Android_DB]]: `chat_message`, `chat_channels`, `chat_users`
- [[artifacts/Microsoft_Teams_Android]]: `Message`, `User`, `MessagePropertyAttribute`
- [[artifacts/WeChat_Mobile_DB]]: `MicroMsg.db`, `MSG0.db`, `FTSMSG0.db`

채팅 본문과 채널/방 메타데이터는 대화 상대, 모집·유인·협박 문맥, 전송 시각, 대화 지속성을 입증하는 중심 증거다.

### 2. 첨부 파일·이미지 캐시를 바로 병행 확보
- [[artifacts/Slack_Android_DB]]: `files` 테이블에서 파일명, 업로드 시각, URL 메타데이터 확인
- [[artifacts/JANDI_Android_DB]]: `image_manager_disk_cache`에서 이미지 잔존 가능
- [[artifacts/NAVER_WORKS_Android_DB]]: `chat_files`에서 공유 파일 기록 확인
- [[artifacts/WeChat_Mobile_DB]]: `/sdcard/tencent/MicroMsg/Wechat/` 경로의 미디어 확보

디지털성범죄에서는 채팅 본문보다 실제 첨부 파일·이미지 잔존물이 더 직접적인 경우가 많으므로, DB와 미디어 저장 경로를 반드시 함께 수집한다.

### 3. 사용자 귀속 정보를 따로 분리 보존
- [[artifacts/JANDI_Android_DB]]: `accounts`, `account_emails`, `uuid`
- [[artifacts/Discord_Android_DB]]: `shared_prefs`의 인증 토큰·계정 설정
- [[artifacts/Microsoft_Teams_Android]]: `mri`, `displayName`, `tenantId`, `conversationId`
- [[artifacts/WeChat_Mobile_DB]]: UIN, IMEI, Account, WeChat_ID 기반 식별

이 정보는 "이 폰의 이 계정이 실제로 해당 대화방과 파일 공유에 관여했는가"를 설명하는 축이다.

### 4. 삭제·은닉 흔적을 별도 분석 트랙으로 운영
- [[artifacts/JANDI_Android_DB]]: Secure Delete OFF 환경에서 `message_link`와 `message_text_content` 불일치분으로 삭제 메시지 추정
- [[artifacts/NAVER_WORKS_Android_DB]]: `chat_message.status = 'DELETED'`
- [[artifacts/Microsoft_Teams_Android]]: 삭제 채팅이 SQLite WAL에 일정 기간 잔류 가능

삭제 메시지 여부는 단순 부가정보가 아니라, 은닉 의도와 사후 행위를 보여주는 핵심 정황이 될 수 있다.

### 5. 행위 문맥을 복원할 수 있는 부가 DB를 챙김
- [[artifacts/WeChat_Mobile_DB]]: 통화 기록, 위치 정보, 실시간 위치, Wi-Fi 연결 기록, 앱 설치 정보
- [[artifacts/NAVER_WORKS_Android_DB]]: `calendar`, `event`, `eventDetail`
- [[artifacts/Discord_Android_DB]] / [[artifacts/Slack_Android_DB]]: 길드·채널·스레드 구조

이 층위는 단순 대화 복원보다 "언제 접촉했고 어떤 맥락에서 활동했는지"를 입체적으로 재구성하는 데 유리하다.

### 6. 같은 계정의 PC 사용 흔적이 있으면 교차 수집
- [[techniques/Teams_Differential_Forensics]]: Android 단독 84%, Windows 병합 92%
- [[techniques/WeChat_Artifact_Analysis]]: Mobile 단독 정보와 PC 단독 정보가 상호보완
- [[techniques/Slack_Discord_IM_Forensics]]: Android DB와 PC 캐시/LevelDB를 함께 보면 복원 범위 확장

질문은 휴대폰 기준이지만, 실제 수사에서는 동일 계정의 PC 클라이언트가 있으면 증거 밀도가 크게 높아진다.

## 한계 & 주의사항
- 현재 위키 근거는 **Android 메신저·협업 앱 중심**이다. 기본 전화, SMS, 갤러리, 브라우저, 클라우드 백업, iOS 시스템 아티팩트는 아직 약하다.
- 다수 아티팩트가 `/data/data/...` 경로에 있으므로 루팅·물리 추출·합법적 파일시스템 획득 수준이 필요하다.
- 앱 버전 의존성이 크다. DB 스키마, 암호화 방식, 삭제 정책은 업데이트에 따라 바뀔 수 있다.
- 이 페이지는 디지털성범죄 시나리오에 대한 **수사 적용형 종합 정리**이며, 직접적인 단일 실험 논문 요약은 아니다.

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/shin_2020]] | Slack/Discord Android DB에서 메시지·파일·사용자 정보 복원 가능; Discord는 `shared_prefs` 인증 정보 확인 |
| [[papers/shin_2021]] | JANDI/NAVER WORKS Android DB 구조 분석; 삭제 메시지 잔류 및 식별 가능; JANDI 패스코드 평문 저장 확인 |
| [[papers/kim_2021]] | Teams Android DB만으로 25개 행위 중 21개 탐지; Windows 병합 시 23개로 증가 |
| [[papers/park_2020]] | WeChat Mobile에서 통화·위치·Wi-Fi 등 풍부한 행위 정보 확보; 키 도출 후 DB 복호화 가능 |
| [[papers/yun_2015]] | 사용자 인지도와 파일 사용 귀속을 별도 아티팩트 조합으로 판단하는 5-CASE 프레임워크 제시 |

## 관련 페이지
[[artifacts/Discord_Android_DB]] · [[artifacts/Slack_Android_DB]] · [[artifacts/JANDI_Android_DB]] · [[artifacts/NAVER_WORKS_Android_DB]] · [[artifacts/Microsoft_Teams_Android]] · [[artifacts/WeChat_Mobile_DB]] · [[techniques/Slack_Discord_IM_Forensics]] · [[techniques/JANDI_NAVER_WORKS_Android_Forensics]] · [[techniques/Teams_Differential_Forensics]] · [[techniques/WeChat_Artifact_Analysis]] · [[techniques/User_Attribution_Framework]]
