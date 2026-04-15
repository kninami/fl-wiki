---
technique: Wire_Credential_Acquisition
perspective: Investigator
topic: Credential Forensics / IM Forensics
related_artifacts:
  - Wire_Cookies_DB
  - Wire_IndexedDB
related_crime:
  - 범죄 조직 통신 수사
  - 인증 정보 획득
sources:
  - shin_2021b
updated: 2026-04-15
---

## 개요
Wire Windows 앱의 Cookies SQLite DB에서 인증 토큰을 획득하고 IndexedDB LevelDB에서 10종의 대화 유형(일반 메시지, 파일, 자동삭제, 음성 등)을 복원하는 기법. 삭제된 메시지도 로그 파일에 잔류함을 확인.

## 메커니즘

### 인증 토큰 획득
- **Cookies DB 경로**: `%AppData%\Wire\Cookies` (SQLite)
- **주요 컬럼**: creation_utc, host, name, value(access_token/user token), expires_utc, last_access_utc
- **토큰 쿠키 구조**:
  - `t` 필드: 'a'=access 토큰 / 'u'=user 토큰
  - `l` 필드: 's'=세션(비영구) / 'persistent'
  - `a` 필드: UUID || c[8bytes] (access 데이터)
  - `u` 필드: UUID || r[4bytes] (user 데이터)

### Device ID 핑거프린트
- 타 기기 로그인 없이 Device ID로 재인증 가능
- 세션 복구에 활용 가능

### 대화 데이터 복원
- **IndexedDB 경로**: `%AppData%\Wire\IndexedDB\https_app.wire.com_0.indexeddb.leveldb\`
- **파일 패턴**: `{0-9}{6}.log`
- **10종 대화 유형**:
  | 유형 | 설명 |
  |------|------|
  | message-add | 일반 메시지 |
  | asset-add | 파일/미디어 공유 |
  | delete-everywhere | 메시지 삭제 |
  | voice-channel-activate/deactivate | 음성 통화 |
  | one2one-creation | 1:1 대화 생성 |
  | group-creation / rename | 그룹 생성/이름 변경 |
  | knock/ping | 알림 메시지 |
  | verification_state | 보안 검증 상태 |
  | unable-to-decrypt | 복호화 실패 기록 |

### 삭제 메시지 추적
- 자동삭제: `ephemeral_expires` 필드로 만료 시각 확인
- 수동삭제: `conversation.delete-everywhere` 로그 잔류 → **삭제 행위 자체 증거화**

### 로그 파일 활용
- `%AppData%\Wire\logs\electron.log`: 계정 정보(name, userID) 포함

## 한계 & 주의사항
- Wire v3.21.3932 Windows 10 Pro 단일 환경 검증
- 인증 토큰은 세션 중에만 유효 (만료 후 재발급 필요)
- 그룹 채팅의 경우 E2E 암호화로 복호화 불가 사례 존재 (`unable-to-decrypt` 레코드)

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/shin_2021b]] | Cookies DB 토큰 구조 분석; 10종 대화 유형 체계화; 삭제 메시지 로그 잔류 확인; Device ID 핑거프린트 재인증 가능성 |

## 관련 페이지
[[artifacts/Wire_Cookies_DB]] · [[artifacts/Wire_IndexedDB]] · [[techniques/JANDI_Artifact_Analysis]] · [[techniques/Slack_Discord_IM_Forensics]]
