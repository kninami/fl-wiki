---
artifact: ReFS Logfile
layer: FS
data_types:
  - Redo Records
  - 파일시스템 연산 트랜잭션
  - Logfile Entry (Control/Data)
  - Log Record Header
action_types:
  - 파일 생성
  - 파일 수정
  - 파일 삭제
  - 디렉토리 변경
  - 메타데이터 변경
os_version: Windows 8+ (ReFS v1.1+)
path: "ReFS 볼륨 내부 (Logfile Information Table로 위치 확인)"
attck: —
sources:
  - lee_2021
updated: 2026-04-15
---

## 개요
ReFS(Resilient File System)의 트랜잭션 로그 파일. NTFS $LogFile과 유사한 역할이지만 구조가 완전히 다름. **Redo 연산만 기록** (NTFS는 redo+undo). ReFS의 AOW(Allocate On Write) 트랜잭션 모델 덕분에 undo 연산이 불필요.

## 구조
- Logfile Entry = Entry Header + Log Record Header + Redo Records
- Entry Header: Entry UUID, CURRENT_ML_LSN, PREVIOUS_ML_LSN, Entry Header Size
- Log Record Header: Checksum, data size, Type(0x01=Control, 0x02=Data area)
- Redo Record: Redo Record Header + Data Offset Array + Transaction Data
- Redo Record에 28종 파일시스템 연산 opcode 포함
- Control area(순환 버퍼)와 Data area가 별도로 배치 (NTFS $LogFile은 연속 배치)
- 시그니처: "MLog" (4 bytes)

## 추출 방법
- **도구**: ARIN(Awesome ReFS Investigation Tool) — 오픈소스 (→ lee_2021)
- Logfile Information Table에서 Logfile 위치·크기 확인
- Object ID Table → Parent Child Table로 전체 경로 재구성

## 분석 포인트
- Redo Record의 opcode와 transaction data로 어떤 파일에 무슨 연산이 발생했는지 파악
- LSN(Log Sequence Number) 기반 순서 재구성
- NTFS $LogFile 파서와 직접 호환 불가 → ReFS 전용 도구 필요

## Findings
- NTFS $LogFile과 완전히 다른 구조 규명: MLog 시그니처, redo-only, Control/Data 분리 배치 (→ [[papers/lee_2021]])
- 28종 파일시스템 연산 opcode 식별로 상세 행위 재구성 가능 (→ [[papers/lee_2021]])
- 커널 역공학(ReFS.sys + resutil.exe)으로 내부 구조 최초 공개 (→ [[papers/lee_2021]])

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/ReFS_Change_Journal]] · [[techniques/ReFS_Journaling_Forensics]]
