---
technique: Thumbcache_Forensic_Analysis
perspective: Investigator
topic: Artifact Analysis
attck:
related_artifacts:
  - Thumbcache
  - Windows_edb
  - IconCache
sources:
  - quick_2014
updated: 2026-05-05
---

## 개요
Windows Thumbcache 파일에서 썸네일 이미지와 ThumbnailcacheID를 추출하고, Windows.edb와 교차 분석하여 원본 파일 경로·카메라 정보 등 추가 메타데이터를 복원하는 분석 기법. McKemmish(1999) IPAP 프레임워크에 맞춰 정의된 운영 방법론 (quick_2014).

## 메커니즘
Thumbcache 파일 엔트리는 ThumbnailcacheID(일방향 해시)와 썸네일 이미지 데이터를 포함. 원본 파일 경로는 해시에서 역산 불가하므로, Windows Search가 별도로 인덱싱한 Windows.edb에서 동일 ThumbnailcacheID를 검색하여 경로를 복원한다.

## 분석 절차 (4단계)

### 1단계 — Identify
- Windows OS가 설치된 기기 식별
- 수집 대상 파일: `thumbcache_*.db`, `thumbs.db`, `iconcache_*.db`, `windows.edb`, `MSS.log`, `$MFT`

### 2단계 — Preserve
- 전체 물리 이미지 (E01/DD) 우선; 불가 시 위 파일들을 논리 컨테이너(L01/AD1/CTR)로 보존

### 3단계 — Analyse (병렬 수행 가능)

**(Step 4)** Thumbcache 파싱 — 개별 썸네일 + ThumbnailcacheID 추출
- EnCase 7, X-Ways, ThumbnailExpert, Thumbcache-viewer 등 사용

**(Step 5)** 언할로케이티드 스페이스 CMMM 카빙
- Thumbcache 파일 시그니처: `43 4D 4D 4D` (`CMMM`)
- 일반 삭제·와이핑 후에도 잔존 가능 (Podhradsky & Streff 2011)
- FTK, X-Ways 카빙 후 ThumbnailExpert로 파싱 가능

**(Step 6)** Windows.edb 분석 — ThumbnailcacheID → 원본 경로·메타데이터 매핑
- `esentutl -r MSS -d` 또는 `esentutl /p windows.edb` 로 복구
- EseDbViewer 또는 WDSCarve로 CSV 추출
- CSV에서 ThumbnailcacheID 검색 → 원본 경로, 파일 크기, 카메라 정보 등 획득
- WDSCarve로 언할로케이티드 스캔 → 삭제된 항목도 복구 가능

**(Step 7)** 포렌식 이미지를 VM으로 부팅 → Thumbcache-viewer의 Map File Paths 기능으로 썸네일-기존 파일 매칭

### 4단계 — Present
보고서: (a) 썸네일 단독 / (b) 기존 파일과 매칭된 썸네일 / (c) Windows.edb 메타데이터 포함 썸네일

## 탐지 포인트
| 상황 | 증거 |
|------|------|
| 파일 삭제 후 | Thumbcache에 썸네일 잔류 (LRU 교체 전) |
| 와이핑 도구 사용 후 | CMMM 카빙 + Windows.edb 항목 잔류 가능 |
| 원본 파일 없음 | Windows.edb에서 원본 경로·카메라 복원 가능 |
| 이미지 파일 소지 부인 | UNC 경로 접근만으로도 thumbs.db 생성 가능 — 단독으로 소지 입증 불충분 |

## 한계 & 주의사항
- ThumbnailcacheID는 역산 불가 — Windows.edb 없이는 원본 경로 불명
- Windows.edb에 모든 썸네일 항목이 등록되지 않음 (일부 파일만 인덱싱)
- **썸네일 존재 ≠ 사용자가 이미지를 봄**: UNC 접근·복붙만으로 생성 가능 → guilty knowledge 추론은 단독 근거로 부족
- Win8 WDSCarve 미지원; Win8 환경에서는 도구 조합 달라짐
- VM 부팅 분석(Step 7) 시 forensic image 변조 방지 조치 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[quick_2014]] | Vista/W7/W8 thumbcache + Windows.edb 통합 분석 방법론 제안, VTAS 프로토타입, 실제 법집행 사례 적용 |

## 실험 기록
| 실험 | 날짜 | 담당자 | 결과 |
|------|------|--------|------|
| [[experiments/thumbcache_win11_maxsize_20260604]] | 2026-06-04 | 조경숙 | confirmed |
| [[experiments/thumbcache_win11_deletion_persistence_20260604]] | 2026-06-04 | 조경숙 | confirmed |

## 관련 페이지
[[artifacts/Thumbcache]] · [[artifacts/Windows_edb]] · [[artifacts/IconCache]] · [[techniques/File_Wiping_Detection]]
