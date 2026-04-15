---
artifact: Microsoft 365 Data Remnants (DRF)
layer: APP
data_types:
  - OfficeFileCache (삭제된 파일 메타데이터·경로)
  - TapCache (자동완성 캐시)
  - MS Teams 로그 (대화, 채팅 이력)
  - OneDrive 메타데이터 (index.dat)
  - SafeDelete 로그
  - MS Outlook 데이터베이스
action_types:
  - 문서 생성/수정/삭제
  - 파일 공유 (OneDrive/Teams)
  - 이메일 송수신
  - 자동 저장·동기화
os_version: Windows 11 (Microsoft 365 v2304)
path: "%UserProfile%\\AppData\\Local\\Microsoft\\Office\\ (OfficeFileCache-C4 등)"
attck: —
sources:
  - joun_2023
updated: 2026-04-15
---

## 개요
Microsoft 365(Word, Excel, PowerPoint, Teams, OneDrive, Outlook 등)가 Windows에서 생성하는 데이터 잔존물(DRF). 파일이 완전히 삭제된 후에도 할당 영역에 경로, 타임스탬프, 콘텐츠 메타데이터가 남아 증거 인멸 수사에 활용 가능.

## 주요 DRF 목록
| DRF 이름 | 앱 | 포맷 | 유형 |
|---------|-----|------|------|
| OfficeFileCache-C4 | MS Office | Unknown | UDRF (핵심) |
| TapCache | MS Office | JSON | SDRF |
| AggrMU-REC | MS Office | JSON | SDRF |
| index.dat | OneDrive | TEXT | SDRF |
| SafeDelete.db | OneDrive | SQLite | SDRF |
| MSTeams-\*.log | MS Teams | LevelDB | SDRF |
| Mail database | Outlook | OST | SDRF |

- **SDRF**: 기존 연구에서 구조·내용 분석 완료
- **UDRF**: 미분석 → 파일명 패턴 + 저장 구조 수동 분석 필요

## 추출 방법
- **차별 분석(Differential Analysis)**: 사용자 행위 전후 파일시스템 스냅샷 비교 (수정 시간 기반 필터링)
- 키워드 검색: 삭제된 파일명을 키워드로 DRF 검색
- Garfinkel의 bulk_extractor, 상용 포렌식 도구(Autopsy, FTK 등)로 전처리·파싱

## 분석 포인트
- OfficeFileCache-C4(`{13807903...}.C4`): 삭제된 파일의 경로·파일명·메타데이터 포함 핵심 UDRF
- 파일명 변수(`[variable]`): 실제 값은 컨텍스트에 따라 다양 → 패턴 파악 필요
- SDRF는 파일명만으로 DRF 존재 확인 가능; UDRF는 구조 분석 추가 필요
- 삭제 확인을 위한 DRF 비교: ①potential files(DRF에서 추출) vs ②existing files(스토리지) → missing files = 삭제된 파일

## Findings
- OfficeFileCache-C4가 삭제된 M365 파일의 경로·메타데이터를 포함하는 핵심 UDRF로 식별 (→ [[papers/joun_2023]])
- 파일 완전 삭제 후에도 할당 영역 DRF로 파일 존재 여부·경로 역추적 가능 (→ [[papers/joun_2023]])
- 기존 $MFT·$LogFile·$USNjrnl 등 NTFS 아티팩트와 함께 APP 레이어 DRF 교차 분석 권장 (→ [[papers/joun_2023]])

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/$USNjrnl]] · [[techniques/Data_Remnants_Forensics]]
