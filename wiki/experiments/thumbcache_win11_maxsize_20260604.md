---
experiment_id: thumbcache_win11_maxsize_20260604
target_artifacts:
  - Thumbcache
related_techniques:
  - Thumbcache_Forensic_Analysis
tester: 조경숙
date: 2026-06-04
env:
  os: Windows 11
  tools:
    - Thumbcache Viewer
method_summary: "약 6600여 개 이미지(로컬 2000여 개, 외장하드 3300여 개)를 보통아이콘/큰아이콘/매우큰아이콘/미리보기 뷰 모드로 열람 후 thumbcache_*.db 파일 용량·엔트리 수 측정"
result: confirmed
sources_compared:
updated: 2026-06-04
---

## 목적
Windows 11에서 Thumbcache DB 파일 구성, 뷰 모드에 따른 저장 DB, DB별 용량 상한을 실험적으로 파악.

## 환경
- OS: Windows 11
- 도구: Thumbcache Viewer
- 이미지 파일: 약 6600여 개 (로컬 드라이브 2000여 개, 외장하드 3300여 개)
- 경로: `C:\Users\{name}\AppData\Local\Microsoft\Windows\Explorer\`

## 절차
1. 약 6600여 개 이미지를 네 가지 뷰 모드(보통아이콘, 큰아이콘, 매우큰아이콘, 미리보기)로 파일 탐색기에서 순차 열람
2. 열람 후 `thumbcache_*.db` 파일별 용량 기록
3. Thumbcache Viewer로 DB를 열어 썸네일 이미지 가시적 확인 및 엔트리 수 카운트

## 관찰 결과

### Win11 DB 파일 구성
Windows 11: `thumbcache_16/32/48/96/256/768/1280/1920/2560.db`, `thumbcache_idx.db`
- Win8 계열(quick_2014 기준)의 `thumbcache_1024/WIDE.db` 대신 `768/1280/1920/2560.db` 체계로 구성

### 뷰 모드별 저장 DB

| 뷰 모드 | 저장 DB (추정) |
|---------|---------------|
| 보통 아이콘 | _96 |
| 큰 아이콘 | _96 ~ _256 |
| 매우 큰 아이콘 | _256 |
| 미리보기 패널 | _256 ~ _1280 |
| 미리보기 패널 (가장 크게) | _1280 ~ _2560 |

### DB별 최대값 (용량 | 엔트리 수)

| DB 파일 | 최대 용량 | 최대 엔트리 수 |
|---------|----------|--------------|
| thumbcache_16/32 | 1,024 KB (1 MB) | 19 |
| thumbcache_48 | 2,048 KB (2 MB) | 272 |
| thumbcache_96 | 96,256 KB (96.2 MB) | 3,750 |
| thumbcache_256 | 66,560 KB (66.5 MB) | 6,679 |
| thumbcache_1280 | 65,536 KB (65.5 MB) | 360 |
| thumbcache_2560 | 1,637,376 KB (1.63 GB) | 3,146 |

- 최솟값: 1 KB (header + 기본 DB 구조만 있는 상태)
- _2560은 동일 조건에서 3,200개를 초과하지 않음

### EXIF 내장 썸네일에 따른 저장 DB 분기

동일 해상도(3000×2000, 300 dpi)의 이미지라도 촬영 장비에 따라 저장 DB가 다름:
- **전문 카메라 촬영 사진** → `_1280`: 카메라 제조사가 RAW/JPG에 ~1280px 수준의 EXIF 내장 썸네일을 포함 → Windows가 해당 썸네일을 그대로 추출하여 _1280에 저장
- **모바일 촬영 사진** → `_2560`: 모바일 JPG는 EXIF 내장 썸네일이 없거나 매우 작음 → Windows가 원본을 직접 디코딩하여 더 큰 _2560에 저장

## 해석
Win11은 Win8 계열 대비 고해상도 DB(_1280, _1920, _2560)가 추가된 새로운 파일 체계를 사용한다. _2560 기준 최대 엔트리 약 3,200개, 최대 용량 약 1.63 GB. 동일 해상도 이미지도 EXIF 내장 썸네일 유무에 따라 저장 DB 레벨이 달라지므로, 수사 시 _2560에 모바일 촬영 사진이 집중적으로 저장될 수 있음을 고려해야 한다.

## 논문과의 비교
| 논문 | 논문의 주장 | 이번 실험 결과 | 일치 여부 |
|------|-------------|----------------|-----------|
| [[quick_2014]] | Win8+: thumbcache_16/32/48/96/256/1024/WIDE.db 구성 | Win11: thumbcache_16/32/48/96/256/768/1280/1920/2560.db 구성 (1024·WIDE 없음) | ⚠ Win11에서 파일 체계 변경 확인 |

## 관련 페이지
[[artifacts/Thumbcache]] · [[techniques/Thumbcache_Forensic_Analysis]]
