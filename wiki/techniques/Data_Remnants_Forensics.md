---
technique: Data Remnants Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Microsoft365_DRF
  - $LogFile
  - $USNjrnl
related_crime:
  - 증거인멸 수사
  - 기업 내부 유출 수사
  - 데이터 위변조
sources:
  - joun_2023
updated: 2026-04-15
---

## 개요
파일이 완전히 삭제된 후에도 할당 영역에 남는 데이터 잔존물(DRF: Data Remnants File)을 체계적으로 식별·분석하는 방법론. 기존 비할당 영역 데이터 복구와 달리 할당된 앱 로그·캐시에서 삭제 파일 흔적 추적.

## 4단계 프레임워크 (DRFD → DRFI → DRFA → DRFE)

**1단계: DRFD (Data Remnants File Dataset 생성)**
- 분석 범위 정의 (OS, 앱, 파일 유형)
- VM 환경 구성 → 사용자 행위 수행 → 스냅샷
- 차별 분석으로 변경 파일 식별

**2단계: DRFI (DRF 식별)**
- 차별 분석: 수정 시간 기반 변경 파일 필터링
- 데이터 전처리: 압축 해제, 역직렬화, 복호화 (open-source 도구)
- 키워드 검색: 파일명을 키워드로 DRF 탐색
- DRF 매칭 → SDRF/UDRF 분류

**3단계: DRFA (DRF 분석)**
- SDRF: 기존 연구 참조로 내용 확인
- UDRF: 파일명 패턴 → 저장 구조 → 파일 포맷 → 목적·내용 수동 분석
- DRF 데이터베이스 구축 및 업데이트

**4단계: DRFE (DRF 추출 및 비교)**
- 타깃 디스크에서 DRF 추출
- potential files (DRF 기반) vs. existing files (디스크) 비교
- missing files = 삭제된 파일 목록

## 탐지 방법 (Investigator)
- OfficeFileCache-C4 분석으로 M365 삭제 파일 경로 확인
- Teams 로그(LevelDB)로 삭제된 공유 파일 이력 추적
- NTFS 아티팩트($LogFile, $USNjrnl)와 병행 분석
- Autopsy, FTK, bulk_extractor로 DRF 전처리

## 한계 & 주의사항
- UDRF는 수동 분석 필요 → 시간 소요
- 앱 버전 업데이트로 DRF 구조 변경 가능
- 차별 분석은 파일시스템 작업에 의한 노이즈 발생 (OS 백그라운드 프로세스)
- 본 연구는 M365 v2304, Windows 11 기준 → 다른 버전 적용 시 검증 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/joun_2023]] | 4단계 DRF 프레임워크; OfficeFileCache-C4 핵심 UDRF 식별; M365 케이스스터디 |

## 관련 페이지
[[artifacts/Microsoft365_DRF]] · [[artifacts/$LogFile]] · [[artifacts/$USNjrnl]]
