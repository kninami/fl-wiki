---
paper: quick_2014
title: "Forensic Analysis of Windows Thumbcache files (2014)"
full_title: "Forensic Analysis of Windows Thumbcache files"
authors: "Quick, D.; Tassone, C.; Choo, K-K. R.; University of South Australia"
year: 2014
venue: AMCIS 2014 (20th Americas Conference on Information Systems)
doi:
tags:
  - Thumbcache
  - Thumbnails
  - Windows.edb
  - ThumbnailcacheID
  - Insider Threat
method: 실험 (VM 기반 Vista/W7/W8 테스트 + 실제 법집행 사례 적용)
limitations: "Vista·W7·W8 대상; 2014년 기준 소프트웨어 비교; W8 thumbcache 파싱 도구 미성숙"
---

## 한 줄 요약
Vista/W7/W8의 Thumbcache 파일 포렌식 분석 방법론 및 Windows.edb 연계 분석 소프트웨어 프로토타입(VTAS) 제안.

## 핵심 발견
- Thumbcache 엔트리는 파일 삭제 후에도 잔류 → Windows.edb와 교차 분석 시 원본 파일 경로·카메라 정보 복원 가능
- ThumbnailcacheID = Volume GUID + NTFS FILEID + 확장자 + 파일 최종수정시간(DOS GMT) 해시 — **역산 불가**
- 썸네일은 사용자가 파일을 실제로 보지 않아도 생성됨 (UNC 경로 접근, 복사·붙여넣기만으로도 thumbs.db 생성) → "guilty knowledge" 추론은 반박 가능
- Win7: thumbcache_idx.db에서 FILETIME 제거됨 (Vista와 차이)
- Win8: thumbcache_16·48·WIDE 추가, ModernPhoto.edb 내 JPEG 썸네일 포함
- 현재 단일 도구로 thumbcache + Windows.edb 통합 분석 불가 → 복수 도구 필요
- 파일 매직 `CMMM` 으로 언할로케이티드 스페이스 카빙 가능 (일반 삭제·와이핑 후에도 잔존 확인)

## 직접 분석한 아티팩트
[[artifacts/Thumbcache]] · [[artifacts/Windows_edb]]

## 영향받은 wiki 페이지
[[techniques/Thumbcache_Forensic_Analysis]] · [[artifacts/IconCache]]
