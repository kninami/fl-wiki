---
technique: ReFS Journaling Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - ReFS_Logfile
  - ReFS_Change_Journal
  - $LogFile
  - $USNjrnl
related_crime:
  - 파일 은닉·삭제 수사
  - 데이터 위변조 수사
sources:
  - lee_2021
updated: 2026-04-15
---

## 개요
Microsoft의 Resilient File System(ReFS) 저널링 파일(Logfile, Change Journal)에서 파일 생성·수정·삭제 이력을 추출하는 기법. NTFS 포렌식 방법론이 직접 적용되지 않으므로 ReFS 전용 접근법 필요.

## 메커니즘
ReFS는 NTFS와 다른 트랜잭션 모델:
- **AOW(Allocate On Write)**: 업데이트 시 새 페이지 할당; 기존 페이지 유지 → undo 불필요
- 따라서 Logfile에는 **redo 연산만** 기록 (NTFS $LogFile은 redo+undo)
- 저널 단위: key-value 쌍의 행(row) 변경 → Logfile에 트랜잭션으로 기록

## 탐지 방법 (Investigator)

**1단계: ReFS 볼륨 메타데이터 구조 파악**
- Object ID Table → 모든 디렉토리/파일 위치
- Parent Child Table → 전체 경로 재구성
- Logfile Information Table → Logfile 위치·크기

**2단계: ReFS Logfile 분석**
- MLog 시그니처로 파일 식별
- Logfile Entry 순회 → Log Record → Redo Record 파싱
- Opcode 확인 (28종): 파일 생성/삭제/수정/이름변경 등 구분
- LSN 기반 순서 재구성

**3단계: Change Journal 분석 (활성화된 경우)**
- USN_RECORD_V3 파싱 (128 bytes)
- 비활성화 상태 확인 → Logfile로 대체

**도구:** ARIN(Awesome ReFS Investigation Tool) - 오픈소스

## NTFS와의 주요 차이점
| 항목 | ReFS | NTFS |
|------|------|------|
| Logfile 연산 | Redo only | Redo + Undo |
| Change Journal 기본 | 비활성화 | 활성화 |
| USN Record 크기 | 128 bytes (V3) | 60+ bytes (V2) |
| 트랜잭션 모델 | AOW | 전통적 저널링 |

## 한계 & 주의사항
- NTFS 포렌식 도구(LogFile Parser, UsrJrnl2Csv)와 직접 호환 불가
- Change Journal 기본 비활성화 → 증거 없을 수 있음
- ReFS v3.4 기준; 버전별 구조 차이 가능
- 커널 역공학 기반 구조 파악 → 공식 문서 없어 불완전할 수 있음

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/lee_2021]] | ReFS Logfile/Change Journal 구조 최초 공개; ARIN 도구 개발; NTFS 대비 주요 차이 규명 |

## 관련 페이지
[[artifacts/ReFS_Logfile]] · [[artifacts/ReFS_Change_Journal]] · [[artifacts/$LogFile]] · [[artifacts/$USNjrnl]]
