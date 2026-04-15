---
artifact: Wire_IndexedDB
layer: APP
data_types:
  - 메시지 내용 (10종 유형)
  - 파일/미디어 공유 기록
  - 음성 통화 기록
  - 삭제 메시지 기록
  - 그룹 생성/이름 변경 이력
action_types:
  - 메시지 송수신
  - 파일 공유
  - 음성 통화
  - 메시지 삭제
os_version: Windows 10 (Wire v3.21.3932+)
path: "%AppData%\\Wire\\IndexedDB\\https_app.wire.com_0.indexeddb.leveldb\\"
attck:
sources:
  - shin_2021b
updated: 2026-04-15
---

## 개요
Wire Windows 앱의 IndexedDB LevelDB 로그 파일. 10종의 대화 유형이 기록되며 자동삭제·수동삭제 메시지도 로그에 잔류하여 삭제 행위 자체의 포렌식 증거가 된다.

## 추출 방법
- 경로: `%AppData%\Wire\IndexedDB\https_app.wire.com_0.indexeddb.leveldb\`
- 파일 패턴: `{0-9}{6}.log` (6자리 숫자 파일명)
- LevelDB 파서 또는 텍스트 에디터(UTF-8)로 분석

## 분석 포인트

### 10종 대화 유형
| 유형 | 설명 |
|------|------|
| message-add | 일반 텍스트 메시지 |
| asset-add | 파일·이미지·미디어 공유 |
| delete-everywhere | 메시지 수동 삭제 (흔적 잔류) |
| voice-channel-activate | 음성 통화 시작 |
| voice-channel-deactivate | 음성 통화 종료 |
| one2one-creation | 1:1 대화방 생성 |
| group-creation | 그룹 채팅방 생성 |
| rename | 그룹 이름 변경 |
| knock/ping | 알림 메시지 |
| verification_state | 보안 검증 상태 변경 |
| unable-to-decrypt | E2E 복호화 실패 기록 |

### 삭제 메시지 추적
- **자동삭제**: `ephemeral_expires` 필드로 만료 예정 시각 확인
- **수동삭제**: `delete-everywhere` 이벤트 레코드 잔류 → 삭제 행위 자체 증거화

## Findings
- 수동삭제 메시지도 `delete-everywhere` 로그로 추적 가능 (→ [[papers/shin_2021b]])
- E2E 암호화 실패 시 `unable-to-decrypt` 레코드 잔류 (→ [[papers/shin_2021b]])
- 대화 생성·이름 변경 등 메타 이벤트도 모두 로그에 기록됨 (→ [[papers/shin_2021b]])

## 관련 페이지
[[artifacts/Wire_Cookies_DB]] · [[techniques/Wire_Credential_Acquisition]]
