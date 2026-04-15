---
artifact: Viber_DB
layer: APP
data_types:
  - 통화 기록 (타임스탬프, 방향, 유형)
  - 메시지 본문
  - 연락처 정보 (위치 포함)
  - 미디어 파일 경로
action_types:
  - 메시지 송수신
  - 통화 발신/수신
  - 미디어 공유
os_version: Windows 8 (Store 앱)
path: "%LocalAppData%\\Packages\\[Viber 패키지명]\\LocalState\\viber.db"
attck:
sources:
  - lee_2015
updated: 2026-04-15
---

## 개요
Windows 8 Store 앱 버전 Viber의 SQLite 데이터베이스. 통화 이력·메시지·연락처 정보를 포함하며 포렌식 분석으로 통신 행위 재구성이 가능하다.

## 추출 방법
- 경로: `%LocalAppData%\Packages\[Viber 패키지명]\LocalState\viber.db`
- SQLite 분석 도구: DB Browser for SQLite, SQLiteExpert, Autopsy

## 분석 포인트

### 주요 테이블 구조
| 테이블 | 주요 컬럼 | 내용 |
|--------|-----------|------|
| Events | timestamp, direction, type, lat, lon, number | 통화·이벤트 기록 (위치정보 포함) |
| Messages | status, body, payload_path | 메시지 본문 및 미디어 경로 |
| ChatInfo | — | 채팅방 정보 |
| UploadFile | — | 업로드 파일 정보 |
| Contact | — | 연락처 정보 |
| Origin Number | — | 발신 번호 정보 |

### 주목할 포인트
- `direction` 컬럼: 발신/수신 구분 → 소셜 네트워크 분석 가능
- `lat`/`lon` 컬럼: 통화 시 위치 정보 포함 → 위치 추적에 활용
- `payload_path`: 미디어 파일 저장 경로 → 별도 파일 분석으로 공유 내용 확인

## Findings
- Events 테이블의 타임스탬프·방향·위치정보로 통화 패턴 분석 가능 (→ [[papers/lee_2015]])
- R igraph로 발신/수신 관계 그래프 생성 → 핵심 연락처 식별 (→ [[papers/lee_2015]])

## 관련 페이지
[[artifacts/Line_DB]] · [[techniques/Win8_IM_App_Forensics]]
