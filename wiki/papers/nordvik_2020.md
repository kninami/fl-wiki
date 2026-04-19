---
paper: nordvik_2020
title: "Generic Metadata Time Carving (2020)"
full_title: "Generic Metadata Time Carving"
authors: "Nordvik, R.; Porter, K.; Toolan, F.; Axelsson, S.; Franke, K."
year: 2020
venue: DFRWS USA 2020 (Best Paper Award)
doi: —
tags:
  - file carving
  - metadata carving
  - timestamp
  - NTFS
  - Ext4
  - disk forensics
method: 실험
limitations: "2 GB 소용량 이미지 기반 실험; 파일 수 제한 (NTFS 50개, Ext4 25000개); 암호화·압축 볼륨 미검증"
---

## 한 줄 요약
포맷된 디스크 이미지에서 타임스탬프를 동적 시그니처로 활용해 NTFS/Ext4 메타데이터 엔트리를 자동 카빙하는 범용 알고리즘.

## 핵심 발견
- 타임스탬프는 파일별로 고유한 패턴 → 마법 바이트 없이도 메타데이터 위치 특정 가능
- NTFS→exFAT 포맷 후 카빙: Precision 0.9939, Recall 1.0 (162 TP, 1 FP, 0 FN)
- Ext4→NTFS 포맷 후 카빙: inode 분류 Precision 1.0 (57427 TP, 0 FP); 963개 덮어쓰인 inode 복구
- EnCase, X-Ways는 포맷 후 메타데이터 0개 복구; Bulk_Extractor는 NTFS만 복구
- 런타임: NTFS 2GB ~14초, Ext4 2GB ~28초 — 실용적

## 직접 분석한 아티팩트
NTFS MFT 레코드, Ext4 inode (기존 wiki 페이지 없음 — 별도 artifact 페이지 미생성, technique으로 처리)

## 영향받은 wiki 페이지
[[techniques/Filesystem_Metadata_Carving]]
