---
artifact: Windows.edb
layer: OS
data_types:
  - ThumbnailcacheID
  - 원본 파일 전체 경로
  - 파일 크기·확장자·타입
  - 카메라 제조사·모델
  - 파일 생성·접근 타임스탬프
  - 시스템 카테고리 메타데이터
action_types:
  - 파일 인덱싱 (Windows Search)
  - 썸네일 생성 이력 연계
os_version: Windows Vista+
path: "C:\\ProgramData\\Microsoft\\Search\\Data\\Applications\\Windows\\Windows.edb"
attck:
sources:
  - quick_2014
updated: 2026-05-05
---

## 개요
Windows Search 서비스가 관리하는 Extensible Storage Engine(ESE) 데이터베이스. 파일 인덱싱 목적으로 생성되며, 일부 파일에 대해 ThumbnailcacheID를 포함한 풍부한 메타데이터를 저장한다. [[artifacts/Thumbcache]] 분석 시 ThumbnailcacheID → 원본 파일 경로 역추적에 핵심적으로 활용된다.

**관련 파일:**
- `MSS.log` — Windows.edb 복구용 트랜잭션 로그 (같은 디렉터리)
- `ModernPhoto.edb` (Win8) — 동일 경로; JPEG 썸네일 이미지 포함

## 추출 방법
- **경로**: `C:\ProgramData\Microsoft\Search\Data\Applications\Windows\Windows.edb`
- ESE 형식이므로 직접 열기 불가; 복구 후 CSV 변환 필요
- **복구 명령어** (MSS.log 있는 경우): `esentutl -r MSS -d`
- **복구 명령어** (MSS.log 없는 경우): `esentutl /p windows.edb`
- 복구 후 도구로 CSV 추출:
  - **EseDbViewer** (Woanware): Windows.edb → CSV
  - **WDSCarve**: Windows.edb → CSV (Win8 미지원); 언할로케이티드 공간 스캔 가능
- **Win8 테이블명**: `SystemIndex_PropertyStore` (Win7: `SystemIndex_0A`)

## 분석 포인트

### ThumbnailcacheID 교차 분석
Thumbcache에서 추출한 ThumbnailcacheID를 Windows.edb CSV에서 검색하면 해당 썸네일의 원본 파일 정보 획득:
- 원본 전체 경로 (파일이 삭제된 후에도 가능)
- 카메라 제조사·모델 (EXIF 메타데이터가 인덱싱된 경우)
- 파일 생성·수정·접근 시간
- 파일 크기, 해상도, 타입

### 삭제된 항목 복구
- Windows.edb에서 삭제된 항목도 WDSCarve로 언할로케이티드 스페이스 스캔 시 복구 가능
- pagefile.sys, hiberfil.sys 등 다른 파일도 스캔 대상에 포함

### Win8 ModernPhoto.edb
- Win8 전용; 동일 경로에 위치
- JPEG 썸네일 이미지 자체를 포함 (Thumbcache 파일과 별개)
- Woanware EseDbViewer로 열람 가능

## Findings
- **ThumbnailcacheID → 원본 파일 경로·카메라 정보 역추적 가능** — 파일 삭제 후에도 (→ [[quick_2014]])
- **Win8에서 테이블명 변경**: SystemIndex_0A → SystemIndex_PropertyStore (→ [[quick_2014]])
- **WDSCarve로 삭제된 ThumbnailcacheID 항목 복구 가능** — 언할로케이티드 스캔 (→ [[quick_2014]])
- **Win8 미지원**: WDSCarve는 Win8 edb 파싱 불가 (→ [[quick_2014]])

## 관련 페이지
[[artifacts/Thumbcache]] · [[techniques/Thumbcache_Forensic_Analysis]]
