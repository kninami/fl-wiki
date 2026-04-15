---
technique: Win8_IM_App_Forensics
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - Viber_DB
  - Line_DB
related_crime:
  - 통신 기록 확보
  - 디지털성범죄 수사
sources:
  - lee_2015
updated: 2026-04-15
---

## 개요
Windows 8 Style UI 앱(Modern/Metro 앱) 형태로 배포된 메신저(Viber, Line)의 SQLite DB 아티팩트를 분석하고 R igraph로 통화·메시지 관계망을 시각화하는 기법.

## 메커니즘

### Windows 8 앱 데이터 경로
- `%LocalAppData%\Packages\[패키지명]\LocalState` 또는 유사 경로
- 기본 경로: `AppData\Local\Packages`

### Viber DB 구조 (`viber.db`)
- **Events 테이블**: timestamp, direction(발신/수신), type, 위치정보(lat/lon), 연락처 번호
- **Messages 테이블**: status, body(메시지 본문), payload_path(미디어 파일 경로)
- **Contact 테이블**: 연락처 정보

### Line DB 구조 (CacheWp 폴더 내)
- **Chatl 테이블**: MID(사용자 ID), Mid_Type, Display_Name, Status, Last_Modified
- **Loginl 테이블**: User_ID(로그인 계정 정보)

### 소셜 네트워크 분석
- R igraph 패키지로 발신/수신 관계 그래프 생성
- 통화·메시지 빈도 기반 핵심 연락처 시각적 식별
- 관계망 분석으로 공범 관계 파악 가능

## 한계 & 주의사항
- Windows 8 Style UI 앱 한정 — Win32 앱 버전과 경로 상이
- Viber/Line 구버전 기준 — 현재 버전과 DB 스키마 다를 수 있음
- R igraph 분석에 데이터 전처리 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/lee_2015]] | Viber/Line DB 스키마 분석; R igraph 소셜 네트워크 관계도 시각화 |

## 관련 페이지
[[artifacts/Viber_DB]] · [[artifacts/Line_DB]] · [[techniques/Slack_Discord_IM_Forensics]] · [[techniques/WeChat_Artifact_Analysis]]
