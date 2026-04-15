---
artifact: Microsoft_Teams_Android
layer: APP
data_types:
  - 채팅 메시지
  - 사용자 정보
  - 테넌트 정보
  - 대화방 메타데이터
action_types:
  - 메시지 송수신
  - 파일 공유
  - 화상회의
os_version: Android
path: "/data/data/com.microsoft.teams/databases/SkypeTeams.db"
attck:
sources:
  - kim_2021
updated: 2026-04-15
---

## 개요
Microsoft Teams Android 앱의 로컬 SQLite 데이터베이스(SkypeTeams.db). 메시지·사용자·테넌트 정보를 구조화하여 저장한다.

## 추출 방법
- 경로: `/data/data/com.microsoft.teams/databases/SkypeTeams.db`
- 루팅 또는 물리 추출 필요
- SQLite Browser, Autopsy로 파싱

## 분석 포인트
| 테이블 | 주요 컬럼 | 포렌식 의미 |
|--------|-----------|------------|
| `User` | mri, displayName | 사용자 식별 |
| `Message` | messageid, content, compositeTime | 메시지 내용·시각 |
| `MessagePropertyAttribute` | tenantId, conversationId | 테넌트·대화방 식별 |

- 삭제된 채팅도 일정 기간 DB에 잔류 가능 (SQLite WAL 모드 의존)

## Findings
- 25개 행위 중 21개(84%) Android 아티팩트만으로 탐지 가능 (→ [[kim_2021]])
- Windows 아티팩트 병합 시 23개(92%)로 향상 → 교차 플랫폼 분석 유효 (→ [[kim_2021]])

## 관련 페이지
[[artifacts/Microsoft_Teams_Windows]] · [[techniques/Teams_Differential_Forensics]] · [[papers/kim_2021]]
