---
paper: shin_2021b
title: "Windows에서의 Wire 크리덴셜 획득 및 아티팩트 분석 (2021)"
full_title: "Windows에서의 Wire 크리덴셜 획득 및 아티팩트 분석"
authors: "신수민, 김소람, 윤병철, 김종성; 국민대학교"
year: 2021
venue: 정보보호학회논문지 31(1)
method: 실험
limitations: "Wire v3.21.3932 Windows 10 Pro 단일 환경; 인증 토큰은 세션 중 유효"
tags:
  - Wire
  - Messenger
  - Credential Acquisition
  - Cookies SQLite
  - LevelDB
  - IndexedDB
---

## 한 줄 요약
Wire Windows 앱의 Cookies DB에서 인증 토큰 획득 및 IndexedDB LevelDB에서 10종 대화 유형 복원 방법 체계화.

## 핵심 발견
- **Wire 경로**: `%AppData%\Wire`
- **Cookies DB** (SQLite): `creation_utc`, `host`, `name`, `value`(access_token/user token), `expires_utc`, `last_access_utc`
- 쿠키 데이터 구조: String(signature), v(version), k(key-index), d(timestamp), t(type: 'a'=access/'u'=user), l(tag: 's'=세션/'persistent'), a(access data=UUID‖c[8bytes]), u(user data=UUID‖r[4bytes])
- 로그인 패킷(Fiddler): password+handle/email 또는 code+phone — HTTP JSON 전송
- **Device ID 핑거프린트**: 타 기기 로그인 없이 재인증 가능
- **로그 파일** (`%AppData%\Wire\logs\`): electron log에 계정 정보(name, userID) 포함
- **대화 데이터** (`IndexedDB\https_app.wire.com_0.indexeddb.leveldb\{0-9}{6}.log`): 10종 유형 — message-add, asset-add, delete-everywhere, voice-channel-activate/deactivate, one2one-creation, group-creation, rename, knock/ping, verification_state, unable-to-decrypt
- 자동삭제: `ephemeral_expires` 필드 / 수동삭제: `conversation.delete-everywhere` 로그로 추적; **삭제 메시지도 로그 파일에 잔류**

## 직접 분석한 아티팩트
[[artifacts/Wire_Cookies_DB]] · [[artifacts/Wire_IndexedDB]]

## 영향받은 wiki 페이지
[[techniques/Wire_Credential_Acquisition]]
