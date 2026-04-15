---
artifact: Line_DB
layer: APP
data_types:
  - 사용자 ID
  - 디스플레이 이름
  - 채팅 상태
  - 로그인 계정 정보
action_types:
  - 메시지 송수신
  - 사용자 로그인
os_version: Windows 8 (Store 앱)
path: "%LocalAppData%\\Packages\\[NAVER 패키지명]\\LocalState\\CacheWp\\"
attck:
sources:
  - lee_2015
updated: 2026-04-15
---

## 개요
Windows 8 Store 앱 버전 LINE(네이버)의 SQLite 캐시 데이터베이스. 사용자·채팅 목록 정보를 포함하며 연락처 관계 분석에 활용된다.

## 추출 방법
- 경로: `%LocalAppData%\Packages\NAVER.LINEwin8_...\LocalState\CacheWp\` 내 DB 파일
- SQLite 분석 도구: DB Browser for SQLite, Autopsy

## 분석 포인트

### 주요 테이블 구조
| 테이블 | 주요 컬럼 | 내용 |
|--------|-----------|------|
| Chatl | MID, Mid_Type, Display_Name, Status, Last_Modified | 채팅 상대방 정보 |
| Loginl | User_ID | 로그인 계정 정보 |

### 주목할 포인트
- `MID`: LINE 사용자 고유 식별자
- `Mid_Type`: 사용자/그룹/공식 계정 구분
- `Status`: 현재 상태 (온라인/오프라인 등)
- Viber DB 대비 메시지 본문 미포함 — 연락처 관계 분석에 집중

## Findings
- Chatl 테이블에서 연락처 목록·디스플레이 이름 추출 가능 (→ [[papers/lee_2015]])
- Loginl 테이블에서 로그인 User_ID 확인 (→ [[papers/lee_2015]])
- Viber DB와 병합하여 R igraph 소셜 네트워크 관계도 생성 가능 (→ [[papers/lee_2015]])

## 관련 페이지
[[artifacts/Viber_DB]] · [[techniques/Win8_IM_App_Forensics]]
