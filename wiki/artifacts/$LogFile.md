---
artifact: $LogFile
layer: FS
data_types:
  - Redo records
  - Undo records
  - Transaction log
  - File system operation history
action_types:
  - 파일 생성
  - 파일 수정
  - 파일 삭제
  - 타임스탬프 변경
  - 메타데이터 변경
os_version: Windows (NTFS 사용 시)
path: "C:\\$LogFile"
attck: T1070.006
sources:
  - palmbach_2020
  - lee_2021
updated: 2026-04-15
---

## 개요
NTFS 파일시스템의 트랜잭션 로그. 파일시스템 연산(생성·수정·삭제·이름변경 등)의 redo·undo 레코드를 순환 버퍼에 기록. 시스템 충돌 복구에 사용되며, 포렌식에서는 타임스탬프 조작 탐지와 과거 파일 활동 재구성에 활용.

## 추출 방법
- 도구: NTFS LogFile Parser, Autopsy, X-Ways
- 경로: C:\\$LogFile (숨김 시스템 파일, 관리자 권한 필요)
- FTK Imager로 디스크 이미지 수집 후 파서 적용

## 분석 포인트
- $LogFile에 기록된 타임스탬프와 $MFT의 $STANDARD_INFORMATION 타임스탬프 비교 → 조작 여부 탐지
- 트랜잭션 ID, LSN(Log Sequence Number)으로 연산 순서 재구성
- 조작 도구(SetMACE, Timestomp 등) 실행 후 $LogFile에 흔적 남음

## 공격 기법 (해당 시)
- 공격자가 $LogFile을 직접 삭제·초기화하려 할 경우 → 그 자체가 이상 징후
- 일반 사용자는 $LogFile 접근 어려움 → 실제 조작 사례 드묾

## Findings
- 4개 아티팩트($LogFile, $USNjrnl, Prefetch, Event Log) 중 타임스탬프 조작 탐지에 가장 신뢰할 수 있는 아티팩트 (→ [[papers/palmbach_2020]])
- SetMACE v1.0.0.5는 FNA 타임스탬프를 변경하지 않아 $LogFile에서 불일치 탐지 가능 (→ [[papers/palmbach_2020]])
- ReFS Logfile은 NTFS $LogFile과 구조적으로 완전히 다름: redo 연산만 기록, MLog 시그니처 사용 (→ [[papers/lee_2021]])

## 관련 페이지
[[artifacts/$USNjrnl]] · [[artifacts/ReFS_Logfile]] · [[techniques/Timestamp_Manipulation_Detection]] · [[techniques/ReFS_Journaling_Forensics]]
