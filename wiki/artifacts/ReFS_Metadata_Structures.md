---
artifact: ReFS_Metadata_Structures
layer: FS
data_types:
  - Boot Record
  - Super block
  - Check point
  - Object table (B+tree)
  - Container Table
  - Directory tree entries
  - Volume Label
action_types:
  - 파일 생성
  - 파일 삭제
  - 파일 수정
  - 메타데이터 갱신
os_version: Windows Server 2012+ / Windows 8.1+
path: "볼륨 내 메타데이터 블록 (오프셋 기반 직접 접근)"
attck:
sources:
  - kim_2018
  - lee_2021
updated: 2026-04-15
---

## 개요
ReFS(Resilient File System)의 내부 메타데이터 구조 전체. B+tree 기반 Object table, 계층적 Check point 구조, Write-at-Write 특성으로 이전 메타데이터 블록이 잔존하여 삭제 파일 복구에 활용된다.

## 추출 방법
- 디스크 이미지 직접 분석 (FTK Imager, EnCase)
- 오프셋 기반 수동 분석 필요 (기존 도구 지원 미비)
- ReFS v1.2: 0x1E번 블록 = Super block

## 분석 포인트

### 계층 구조
```
Boot Record
  └─ Super block (0x1E번 블록)
       └─ Check point (primary / secondary)
            └─ Container Table
                 └─ Object table (B+tree)
                      ├─ Volume Label
                      ├─ UpCase
                      ├─ Allocator
                      ├─ Directory tree
                      └─ SDS Data
```

### 메타데이터 블록 헤더
- **v1.2**: 48B 헤더 — Signature, Checksum, VCN, LSN
- **v3.3**: 80B 헤더 — 추가 필드 포함

### 삭제 탐지 포인트
- **휴지통 삭제**: Sequence number 변화 + 파일 엔트리 플래그·오프셋 변경 + LCN 정보 유지
- **완전 삭제**: 메타데이터 플래그 변화 + LCN 정보 일부 소거

## Findings
- ReFS v1.2·v3.3 메타데이터 구조 역분석; 두 가지 삭제 복구 방법 제안 (→ [[papers/kim_2018]])
- ReFS의 Change Journal은 기본 비활성화; File Reference Number 필드 32bit로 확장 (→ [[papers/lee_2021]])
- 기존 도구(Encase v8.05, R-studio v5.4, Autopsy v4.9)는 ReFS 1.2까지만 지원, 복구 미지원 (→ [[papers/kim_2018]])

## 관련 페이지
[[artifacts/ReFS_Logfile]] · [[artifacts/ReFS_Change_Journal]] · [[techniques/ReFS_Deleted_Data_Recovery]] · [[techniques/ReFS_Journaling_Forensics]]
