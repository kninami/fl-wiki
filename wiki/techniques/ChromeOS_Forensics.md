---
technique: ChromeOS Forensics
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - Chrome OS Browser History
  - Chrome OS Local Data
related_crime:
  - 디지털 증거 수집
  - 사이버범죄 수사
sources:
  - hyde_2019
updated: 2026-04-19
---

## 개요
Chromebook/Chrome OS 기기에서 디지털 증거를 수집·분석하는 방법론. 대부분 데이터가 Google Cloud에 저장되나 로컬에도 유의미한 아티팩트 다수 존재.

## 메커니즘
Chrome OS는 Linux 기반이며, 사용자 데이터는 암호화된 파티션에 저장. 아티팩트는 4개 경로에 중복 저장:
1. `/home/.shadow/{GUID}/mount/user/`
2. `/home/chronos/user/`
3. `/home/chronos/u-{GUID}/`
4. `/home/user/{GUID}/`

## 획득 방법
- **VM 환경**: Chromium OS VM에서 developer mode로 live acquisition
- **실기기**: Esc+Refresh+Power → Ctrl-D로 developer mode 진입 (**사용자 데이터 초기화됨**)
  - 구형 기기: 배터리 칸 내 물리 스위치
- **Google Takeout**: 클라우드 데이터 수집; Cache/Downloads/Hidden Folder/Shell usage 등 미포함

## 주요 수집 아티팩트
| 아티팩트 | Chromium | Takeout | 위치 |
|----------|----------|---------|------|
| Browser History | ✓ | ✓ | {경로}/History (SQLite) |
| Browser Cache | ✓ | ✗ | {경로}/Cache/ |
| Current/Last Tabs | ✓ | ? | {경로}/Current Tabs 등 |
| Current/Last Sessions | ✓ | ? | {경로}/Current Sessions 등 |
| Downloads | ✓ | ✗ | History DB + 파일시스템 |
| Hidden Folder | ✓ | ✗ | Downloads/dontlookhere/ |
| Extensions | ✓ | ✓ | Extensions/{GUID}/ |
| Offline Storage | ✓ | ✗ | GCache/v1/files/ |
| Shell Usage | ✓ | ✗ | .bash_history |
| Avatar | ✓ | ? | Accounts/Avatar Images/ |
| Pictures | ✓* | ✓* | — |
| Task Lists | ✗ | ✓ | — |

## 탐지 방법 (Investigator)

### 브라우저 히스토리
- Chrome browser SQLite 파서(Hindsight 등) 사용; Chrome OS 경로 지원 필요
- visits/urls/downloads/downloads_url_chains 테이블 교차 분석

### 은닉 파일
1. `Downloads/dontlookhere/.ProgramData/thumbs/`에서 base64 파일명 디코딩
2. `Sync App Settings/{ext-GUID}/`의 LevelDB에서 패스워드 추출
3. 파일 앞 패스워드 바이트 제거 후 원본 복원

### 확장 프로그램 식별
1. Extensions/{GUID}/manifest.json 파싱으로 앱 이름·권한 확인
2. GUID를 Chrome Web Store URL에 대입해 앱 검색

### 오프라인 스토리지
1. GCache/v1/files/의 파일 내용 직접 추출
2. GCache/v1/meta/ LevelDB에서 원본 파일명 매핑

## 한계 & 주의사항
- 실기기 developer mode 진입 시 사용자 데이터 초기화 위험
- Chrome OS 스토리지 암호화로 직접 이미징 어려움 (JTAG/Chip-off + 복호화 필요)
- Chromium OS ≠ Chrome OS: Chromium은 Chrome의 보안 기능 일부 미포함

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/hyde_2019]] | Chrome OS 4경로 아티팩트 실험 확인; Takeout vs Chromium 비교; 히든 폴더 분석 |

## 관련 페이지
[[artifacts/Chrome_OS_Browser_History]] · [[artifacts/Chrome_OS_Local_Data]]
