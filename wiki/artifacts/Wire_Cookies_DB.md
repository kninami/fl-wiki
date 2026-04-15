---
artifact: Wire_Cookies_DB
layer: APP
data_types:
  - 인증 토큰 (access/user)
  - 토큰 생성·만료 시각
  - 세션 유형
action_types:
  - 사용자 로그인
  - 세션 유지
os_version: Windows 10 (Wire v3.21.3932+)
path: "%AppData%\\Wire\\Cookies"
attck:
sources:
  - shin_2021b
updated: 2026-04-15
---

## 개요
Wire Windows 앱의 Chromium 기반 쿠키 SQLite 데이터베이스. 인증 토큰(access/user)이 저장되어 있어 토큰 추출 시 서버 API 접근 가능. 세션 기간 중 유효.

## 추출 방법
- 경로: `%AppData%\Wire\Cookies` (SQLite)
- SQLite 분석 도구: DB Browser for SQLite, Autopsy

## 분석 포인트

### 주요 컬럼
| 컬럼 | 내용 |
|------|------|
| creation_utc | 쿠키 생성 시각 |
| host | 호스트명 |
| name | 쿠키 이름 |
| value | 토큰 데이터 (JSON 형식) |
| expires_utc | 만료 시각 |
| last_access_utc | 마지막 접근 시각 |

### 토큰 value 필드 구조
| 필드 | 의미 |
|------|------|
| `t` | 토큰 유형: 'a'=access, 'u'=user |
| `l` | 영속성: 's'=세션(비영구), 'persistent'=영구 |
| `a` | access 데이터: `UUID || c[8bytes]` |
| `u` | user 데이터: `UUID || r[4bytes]` |
| `k` | 키 인덱스 |
| `d` | 타임스탬프 |

### Device ID 핑거프린트
- Device ID 확인 시 타 기기 로그인 없이 재인증 가능

## Findings
- Wire 로그인 패킷: password+handle/email 또는 code+phone (HTTP JSON 평문 전송 확인) (→ [[papers/shin_2021b]])
- Device ID 핑거프린트로 세션 재인증 가능성 확인 (→ [[papers/shin_2021b]])
- `%AppData%\Wire\logs\electron.log`에 계정 정보(name, userID) 포함 (→ [[papers/shin_2021b]])

## 관련 페이지
[[artifacts/Wire_IndexedDB]] · [[techniques/Wire_Credential_Acquisition]]
