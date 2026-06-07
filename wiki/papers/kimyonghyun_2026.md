---
paper: kimyonghyun_2026
title: "iOS 백업 데이터 – Manifest.db 구조 분석, plist 텍스트 및 바이너리 형식 분석 (2026)"
full_title: "iOS 백업 데이터 – Manifest.db 구조 분석, plist 텍스트 및 바이너리 형식 분석"
authors: "김용현"
year: 2026
venue: 개인연구
tags:
  - iOS Backup
  - Manifest.db
  - plist
  - Binary plist
  - bplist
  - SQLite
  - Mobile Forensics
method: 실험 분석 (실측 기반 구조 분석)
limitations: "iPhone 6s/iOS 15.8.7 단일 디바이스, 비암호화 백업만 분석"
notes: "별도 Q&A 세션에서 암호화 백업 동작, Manifest.db 손상 복구, 파일명 변경 이슈 등 보완 정보 확인됨"
---

## 한 줄 요약
iOS 백업의 핵심 구성요소인 Manifest.db(SQLite3)의 테이블 스키마와 file BLOB 메타데이터 구조, Info.plist·Manifest.plist·Status.plist의 텍스트 및 바이너리 plist 형식을 실측 분석.

## 핵심 발견
- **Manifest.db**: Files(백업 파일의 SHA-1 해시, 도메인, 원본 경로, 플래그, NSKeyedArchiver 메타데이터 BLOB) + Properties(빈 key-value) 2개 테이블로 구성
- **file BLOB**: NSKeyedArchiver 직렬화 바이너리로 LastModified, Birth, Size, InodeNumber, Mode, UserID/GroupID, ProtectionClass 등 포함
- **파일 경로 → SHA-1 해시**: `SHA1(domain + '-' + relativePath)` — 해시 앞 2글자로 256개(00~ff) 디렉토리에 분산 저장
- **plist 텍스트(XML)**: Info.plist에 사용. 기기 식별정보(전화번호, IMEI, UDID, 앱 목록 등) 저장
- **plist 바이너리(bplist)**: Manifest.plist·Status.plist·file BLOB 내부에 사용. 헤더(8B) → 오브젝트 테이블 → 오프셋 테이블 → 트레일러(32B) 구조. 상위 nibble로 타입 식별 (0x5=String, 0xD=Dict, 등)
- **bplist 트레일러**: offsetIntSize, objectRefSize, numObjects, topObject, offsetTableOffset 포함
- **bplist 읽기 절차**: Header(bplist00) → Trailer(파일 끝 32B) → Offset Table → Object Table 순으로 단계적 파싱 방법 제시

## 직접 분석한 아티팩트
[[artifacts/iOS_Backup_Manifest_db]] · [[artifacts/iOS_Backup_Info_plist]] · [[artifacts/iOS_Backup_Manifest_plist]] · [[artifacts/iOS_Backup_Status_plist]]

## 영향받은 wiki 페이지
[[techniques/iOS_Backup_Forensics]]
