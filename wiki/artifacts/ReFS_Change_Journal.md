---
artifact: ReFS Change Journal
layer: FS
data_types:
  - USN_RECORD_V3
  - 파일 변경 이력
  - File Reference Number (32bit)
  - 변경 사유 (Reason)
  - 타임스탬프
action_types:
  - 파일 생성
  - 파일 수정
  - 파일 삭제
  - 파일명 변경
os_version: Windows 8+ (ReFS; 기본 비활성화)
path: "ReFS 볼륨 내 File System Metadata 하위 (fsutil usn createjournal으로 활성화)"
attck: —
sources:
  - lee_2021
updated: 2026-04-15
---

## 개요
ReFS의 파일 변경 이력 로그. NTFS $USNjrnl과 유사한 역할이나 구조적 차이 존재. **기본 비활성화** 상태 — 수사관이 ReFS 볼륨을 만날 때 활성화 여부 확인 필요. fsutil 또는 tsnail 명령으로 활성화.

## 구조 (NTFS $USNjrnl과의 차이)
| 항목 | ReFS Change Journal | NTFS $USNjrnl |
|------|---------------------|----------------|
| 포맷 | USN_RECORD_V3 | USN_RECORD_V2 |
| 레코드 크기 | 128 bytes | 60 bytes+ |
| File Reference Number | 32 bits | Minor(16) + MFT number(32) |
| 기본 활성화 | 비활성화 | 활성화 |

## 추출 방법
- **도구**: ARIN(Awesome ReFS Investigation Tool) (→ lee_2021)
- fsutil usn readjournal 명령으로 내용 확인 (라이브)
- ReFS 볼륨 이미지에서 File System Metadata 경로 탐색

## 분석 포인트
- USN_RECORD_V3 구조: Record Length, Major/Minor, File/Parent Reference Number, USN, Timestamp, Reason, Source Info, Security ID, File Attribute, File Name
- $USNjrnl 분석 방법론 직접 적용 불가 → ReFS 전용 오프셋 구조 파악 필요
- Change Journal이 비활성화 상태면 증거 없음 → Logfile로 대체 분석

## Findings
- ReFS Change Journal은 기본 비활성화 → 실전에서 수사관이 접근 어려울 수 있음 (→ [[papers/lee_2021]])
- USN_RECORD_V3(128bytes) 포맷으로 NTFS USN_RECORD_V2와 구조 다름; $USNjrnl 분석 도구와 직접 호환 불가 (→ [[papers/lee_2021]])
- 비활성화 시 ReFS Logfile이 대안 분석 수단 (→ [[papers/lee_2021]])

## 관련 페이지
[[artifacts/$USNjrnl]] · [[artifacts/ReFS_Logfile]] · [[techniques/ReFS_Journaling_Forensics]]
