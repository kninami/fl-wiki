---
artifact: Thumbcache
layer: OS
data_types:
  - 썸네일 이미지 (JPG/BMP/PNG)
  - ThumbnailcacheID
  - 파일 경로 참조 (thumbs.db)
  - 타임스탬프 (Vista: thumbcache_idx에 FILETIME; Win7 이후 제거)
action_types:
  - 파일 탐색기 접근 (폴더 썸네일 뷰)
  - UNC 경로 접근
  - 파일 복사·붙여넣기
  - 파일 삭제 후 잔류
os_version: Windows Vista+
path: "%LocalAppData%\\Microsoft\\Windows\\Explorer\\thumbcache_*.db"
attck:
sources:
  - quick_2014
  - joo_2022
updated: 2026-05-05
---

## 개요
Windows Explorer가 이미지·파일을 썸네일 뷰로 표시할 때 생성하는 중앙 집중식 썸네일 캐시. Vista에서 기존 thumbs.db(폴더별 분산 방식)를 대체하여 도입됐으며, OS 버전마다 파일 구성이 다르다.

**OS별 파일 구성:**

| OS | 파일 | 위치 |
|----|------|------|
| 95B–2003 | thumbs.db | 그림 파일과 같은 폴더 |
| Vista | thumbcache_32/96/256/1024.db, thumbcache_idx.db | AppData\Local\Microsoft\Windows\Explorer\ |
| Win7 | thumbcache_32/96/256/1024.db, thumbcache_idx.db | 동일 (idx에서 FILETIME 제거됨) |
| Win8+ | thumbcache_16/32/48/96/256/1024/WIDE.db, thumbcache_idx.db | 동일 |

- **thumbcache_idx.db**: 인덱스 파일. ThumbnailcacheID와 각 썸네일 파일 내 위치(offset) 매핑
- **Vista**: idx에 Windows FILETIME 포함 → **Win7 이후 제거됨**
- **thumbs.db**: Vista·Win7에서도 네트워크(UNC) 경로 접근 시 해당 폴더에 생성됨

## 추출 방법
- **경로**: `%LocalAppData%\Microsoft\Windows\Explorer\thumbcache_*.db`
- **논리 보존**: thumbcache*.db, thumbcache_idx.db, iconcache*.db, windows.edb, MSS.log, $MFT 수집
- **도구 비교** (quick_2014 기준):

| 도구 | ThumbnailcacheID 추출 | 기존 파일 매칭 | Windows.edb 연계 | Win8 지원 |
|------|----------------------|--------------|-----------------|----------|
| EnCase 6 | Vista만 | Vista만 | ✗ | ✗ |
| EnCase 7 | ✓ | ✗ | ✗ | ✓ |
| X-Ways 17 | ✓ | ✗ | ✓ | ✗ |
| FTK 1.81.6/5.1.1.4 | ✗ (카빙만) | ✗ | ✗ | ✗ |
| ThumbnailExpert 2.8 | ✓ | ✗ | ✗ | 일부 |
| Thumbcache-viewer | ✓ | ✓ (Map File Paths) | ✗ | ✓ |
| VTAS (프로토타입) | ✓ | ✗ | ✓ | ✓ |

## 분석 포인트

### ThumbnailcacheID 해시 구조
ThumbnailcacheID = f(Volume GUID, NTFS FILEID, 확장자, 파일 최종수정시간 DOS GMT)
- 해시 과정에서 값이 "blended and mangled" → **역산 불가** (원본 경로를 ID에서 복원할 수 없음)
- [[artifacts/Windows_edb]]의 SystemIndex_0A 테이블에서 ThumbnailcacheID → 원본 경로·메타데이터 매핑 가능

### 썸네일 생성 조건 (주의)
사용자가 파일을 **직접 보지 않아도** 썸네일이 생성될 수 있다:
- UNC 경로(`\\127.0.0.1\c$`)로 폴더 접근 시 thumbs.db 생성
- UNC 위치에 파일 복사·붙여넣기만으로도 thumbs.db 엔트리 생성
- → "썸네일 존재 = 사용자가 이미지를 봄"이라는 guilty knowledge 추론은 **반박 가능**

### 파일 삭제 후 잔류
파일을 삭제해도 썸네일 엔트리는 LRU에 의해 재사용되기 전까지 캐시에 잔류. 원본 파일 없이 썸네일만으로 이미지 내용 입증 가능 (실제 법정 사례 다수 — quick_2014).

## 공격 기법 / 증거인멸 시도
- 파일 와이핑 도구로 원본 이미지 삭제 후에도 thumbcache + Windows.edb 잔류 확인 (quick_2014 실제 수사 적용)
- 삭제된 Windows.edb 항목도 WDSCarve로 언할로케이티드 스페이스에서 복구 가능

### File Wiping 도구별 ThumbCache 잔류 흔적 수 (joo_2022, Windows 10 NTFS)

| 도구 | 잔류 흔적 수 |
|------|------------|
| Glary Utilities | 3 |
| Wise Care 365 | 6 |
| Eraser | 3 |
| Ashampoo WinOptimizer | **120** |
| File Shredder | 1 |

- **시그니처**: 썸네일 Data Checksum 값 → File wiping 도구의 실행 파일 데이터 식별
- Ashampoo WinOptimizer만 유독 120개로 압도적 — 해당 도구가 실행 중 다수의 파일을 탐색기로 열람하는 동작 때문으로 추정
- 5종 모든 도구에서 최소 1개 이상 흔적 잔류 — ThumbCache는 File wiping 탐지에 유효한 아티팩트
- 분석 도구: ThumbCache Viewer v1.0.3.6

## Findings
- **ThumbnailcacheID는 일방향 해시** — 역산 불가, Windows.edb 교차 분석 필요 (→ [[quick_2014]])
- **Vista thumbcache_idx에는 FILETIME 포함; Win7 이후 제거** (→ [[quick_2014]])
- **UNC 경로 접근·복붙만으로 thumbs.db 생성됨** — guilty knowledge 논리 반박 근거 (→ [[quick_2014]])
- **Win8: thumbcache_16·48·WIDE 추가, ModernPhoto.edb에 JPEG 썸네일 포함** (→ [[quick_2014]])
- **파일 매직 `CMMM`** — 언할로케이티드 스페이스 카빙 기준점 (→ [[quick_2014]])
- **단일 도구로 thumbcache + Windows.edb 통합 분석 불가** — 복수 도구 조합 필요 (→ [[quick_2014]])
- **File wiping 5종 도구 모두에서 흔적 잔류 확인** — 시그니처: 썸네일 Data Checksum 값 (→ [[joo_2022]])
- **Ashampoo WinOptimizer 실행 시 120개로 압도적 잔류** — 다른 도구는 1~6개 수준 (→ [[joo_2022]])

## 관련 페이지
[[artifacts/Windows_edb]] · [[artifacts/IconCache]] · [[techniques/Thumbcache_Forensic_Analysis]] · [[techniques/File_Wiping_Detection]]
