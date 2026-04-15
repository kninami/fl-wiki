---
technique: JANDI_Artifact_Analysis
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - JANDI_LevelDB
related_crime:
  - 업무 관련 범죄
  - 내부자 위협
  - 영업비밀 유출
sources:
  - we_2024
updated: 2026-04-15
---

## 개요
JANDI Windows 앱의 LevelDB 로컬 스토리지 아티팩트를 분석하고 휘발성 메모리에서 인증 토큰을 추출하여 API를 통해 삭제된 메시지·파일을 복원하는 기법.

## 메커니즘

### 로컬 아티팩트 분석
- **앱 경로**: `C:\Users\[USERNAME]\AppData\Roaming\JANDI`
- **Local Storage (LevelDB)**: `chromium_dump_local_storage.py`로 SQLite 변환
  - `records_view` 테이블: `storage_key`에 팀 도메인 저장
  - 추출 가능 정보: `_jd_team_name`, `_jd_team_id`, `_jd_member_id`
- **Cache 폴더**: 썸네일(jfif/png, 파일명 "640") 저장

### 인증 토큰 추출
1. **winPmem**으로 메모리 덤프 (라이브 수집)
2. "`_jd_access_token=`" 문자열 검색으로 토큰 획득
3. 토큰은 만료 전까지 API 인증에 사용 가능

### API 기반 삭제 콘텐츠 복원 (Fiddler 트래픽 분석)
| API 단계 | 획득 정보 |
|----------|-----------|
| API-1 | teamID |
| API-2 | roomID |
| API-3 | 채팅 내역 |
| API-4 | 파일 목록 |
| API-5 | 다운로드 URL |

### 적용 시나리오
영업비밀 유출 수사: 삭제된 파일·메시지를 API 기반으로 복원하여 유출 경로·내용 확인 성공

## 한계 & 주의사항
- JANDI v1.76 Windows 10 Pro 단일 환경 검증
- API 기반 복원은 인증 토큰 만료 전에만 가능
- `chromium_dump_local_storage.py` 별도 설치 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/we_2024]] | JANDI LevelDB → SQLite 변환 분석; winPmem 메모리 덤프로 토큰 획득; API 5단계 복원 파이프라인 |

## 관련 페이지
[[artifacts/JANDI_LevelDB]] · [[techniques/JANDI_NAVER_WORKS_Android_Forensics]] · [[techniques/Wire_Credential_Acquisition]]
