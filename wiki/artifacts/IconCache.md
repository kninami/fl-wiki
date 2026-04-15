---
artifact: IconCache
layer: OS
data_types:
  - 파일 아이콘 이미지
  - 파일 경로 참조
  - 아이콘 해시
action_types:
  - 파일 탐색기 접근
  - 파일 실행
os_version: Windows XP+
path: "%LocalAppData%\\IconCache.db"
attck:
sources:
  - joo_2022
updated: 2026-04-15
---

## 개요
Windows 탐색기가 파일 아이콘을 빠르게 표시하기 위해 캐시하는 파일. 탐색기에서 해당 파일 경로에 접근한 이력을 간접적으로 나타낸다.

## 추출 방법
- 경로 (Windows 10): `%LocalAppData%\IconCache.db`
- 이전 버전: `%LocalAppData%\Microsoft\Windows\Explorer\iconcache_*.db` (다수 파일)
- 도구: ThumbCache Viewer, 16진수 편집기

## 분석 포인트
- 파일 경로 참조가 아이콘 캐시 엔트리에 포함될 수 있어 과거 접근 파일 추적 가능
- 파일 삭제 후에도 아이콘 캐시에 참조 잔류 가능
- `IconCache.db` (메인) 외 `ThumbCache` 파일과 함께 분석 시 효과 증대

## Findings
- **File Wiping 도구 5종 실행 후 IconCache.db에서 흔적 잔류율 2위** (ShimCache 다음) (→ [[joo_2022]])

## 관련 페이지
[[artifacts/ShimCache]] · [[artifacts/AmCache]] · [[techniques/File_Wiping_Detection]]
