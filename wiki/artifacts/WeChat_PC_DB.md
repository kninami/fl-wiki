---
artifact: WeChat_PC_DB
layer: APP
data_types:
  - 메시지 내용 및 타입
  - 대화 세션 정보
  - 그룹 채팅방 정보
  - 전문 검색 인덱스
action_types:
  - 메시지 송수신
  - 파일 공유
  - 위치 공유
  - 통화
os_version: Windows (WeChat PC v2.8.0+)
path: "%USERPROFILE%\\Documents\\WeChat Files\\[wxid]\\FileStorage\\Msg\\Multi\\"
attck:
sources:
  - park_2020
updated: 2026-04-15
---

## 개요
WeChat PC 앱의 로컬 메시지 데이터베이스. AES-256-CBC로 암호화되어 있으며 복호화 키는 로그인 시 메모리에만 존재. 복호화 후 세션·메시지·그룹 채팅방 정보 복원 가능.

## 추출 방법
- 경로: `%USERPROFILE%\Documents\WeChat Files\[wxid]\FileStorage\Msg\Multi\`
- **암호화**: AES-256-CBC (키 = 256-bit hex 문자열)
- **키 획득**: 라이브 메모리 덤프 → 키 문자열 검색 (로그인 상태 필요)
- 복호화 후 SQLite 도구로 파싱

## 분석 포인트

### 주요 DB 및 테이블
| DB | 테이블 | 내용 |
|----|--------|------|
| MicroMsg.db | Session | 대화 목록·최신 메시지 |
| MicroMsg.db | ChatRoom | 그룹 채팅방 정보 |
| MicroMsg.db | MSG | 메시지 기본 정보 |
| MSG0.db | — | 메시지 상세 내용 |
| FTSMSG0.db | — | 전문 검색(Full-Text Search) 인덱스 |

### 메시지 타입 코드
| 코드 | 유형 |
|------|------|
| 1 | 텍스트 |
| 3 | 사진 |
| 48 | 위치 |
| 49 | 파일 |
| 50 | 통화 |
| -1879048186 | 실시간 위치 공유 |
| 10000 | 실시간 위치 종료 |

### PC 단독 획득 가능 정보
- 프로필 이미지
- 그룹 채팅방명
- 최신 메시지 내용

## Findings
- AES-256-CBC 키는 메모리에만 존재 → 라이브 수집 또는 메모리 덤프 필수 (→ [[papers/park_2020]])
- FTSMSG0.db 전문 검색 인덱스에서 키워드 검색 가능 (→ [[papers/park_2020]])

## 관련 페이지
[[artifacts/WeChat_Mobile_DB]] · [[techniques/WeChat_Artifact_Analysis]]
