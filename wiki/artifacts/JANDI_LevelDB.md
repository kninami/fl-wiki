---
artifact: JANDI_LevelDB
layer: APP
data_types:
  - 팀 도메인
  - 팀 ID
  - 멤버 ID
  - 썸네일 이미지
action_types:
  - 앱 로그인
  - 메시지 수신
  - 파일 공유
os_version: Windows 10 (JANDI v1.76+)
path: "C:\\Users\\[USERNAME]\\AppData\\Roaming\\JANDI\\Local Storage\\leveldb\\"
attck:
sources:
  - we_2024
updated: 2026-04-15
---

## 개요
JANDI Windows 앱의 Chromium 기반 Local Storage LevelDB. 팀 정보·멤버 ID가 저장되며, 메모리에서 추출한 인증 토큰과 결합하여 API 기반 삭제 메시지·파일 복원에 활용된다.

## 추출 방법
- 경로: `C:\Users\[USERNAME]\AppData\Roaming\JANDI\Local Storage\leveldb\`
- LevelDB → SQLite 변환: `chromium_dump_local_storage.py` 사용
- 변환 후 `records_view` 테이블 쿼리

## 분석 포인트

### records_view 테이블 주요 키-값
| Key | Value 내용 |
|-----|-----------|
| `_jd_team_name` | 소속 팀 이름 |
| `_jd_team_id` | 팀 고유 ID (API 조회에 필요) |
| `_jd_member_id` | 멤버 고유 ID |
| `storage_key` | 팀 도메인 URL |

### Cache 폴더
- 썸네일 이미지: jfif/png 형식, 파일명 "640"
- 공유된 파일의 미리보기 이미지 복원 가능

### 인증 토큰과 연계한 API 복원
1. winPmem 메모리 덤프 → `_jd_access_token=` 검색으로 토큰 획득
2. LevelDB에서 teamID 확인
3. API 순서: teamID → roomID → 채팅 내역 → 파일 목록 → 다운로드 URL

## Findings
- `chromium_dump_local_storage.py`로 LevelDB → SQLite 변환 후 팀 정보 추출 가능 (→ [[papers/we_2024]])
- 메모리 토큰 + LevelDB 팀ID 결합으로 삭제 파일·메시지 API 복원 성공 (→ [[papers/we_2024]])

## 관련 페이지
[[artifacts/JANDI_Android_DB]] · [[techniques/JANDI_Artifact_Analysis]] · [[techniques/JANDI_NAVER_WORKS_Android_Forensics]]
