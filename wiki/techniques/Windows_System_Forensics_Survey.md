---
technique: Windows_System_Forensics_Survey
perspective: Investigator
topic: Survey / Artifact Classification
related_artifacts:
  - $LogFile
  - $USNjrnl
  - Windows_Event_Log
  - Prefetch
related_crime:
  - 디지털 포렌식 일반
sources:
  - jeon_2016
updated: 2026-04-15
---

## 개요
Windows 시스템 포렌식의 전반적인 아티팩트 분류 체계와 조사 절차를 정리한 서베이. 라이브 포렌식·디스크 포렌식·파일시스템·레지스트리·앱 아티팩트를 체계적으로 분류하고 PFP 포렌식 플랫폼을 소개.

## 메커니즘
Windows 포렌식 아티팩트를 레이어별로 분류:
- **FS 아티팩트**: $MFT, $LogFile, $UsnJrnl
- **OS 아티팩트**: 이벤트 로그(Security.evtx, System.evtx), Prefetch, Superfetch, 바로가기, 점프 목록, 휴지통, Windows 검색 DB, 썸네일
- **레지스트리**: 설치 시간, USB 연결, UserAssist, MRU, SAM 계정, 자동실행

## 아티팩트 경로 참조 (Table 3)
| 아티팩트 | 경로 |
|----------|------|
| $MFT | `C:\$MFT` |
| $LogFile | `C:\$LogFile` |
| $UsnJrnl | `C:\$Extend\$UsnJrnl:$J` |
| Security.evtx | `%SystemRoot%\System32\winevt\Logs\Security.evtx` |
| Prefetch | `C:\Windows\Prefetch\*.pf` |

## 한계 & 주의사항
- Windows XP~10 시대 기준 서베이 — 최신 OS에서는 아티팩트 경로·구조 변경 가능
- 개별 아티팩트 심층 분석 미포함 (개요·분류 수준)

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/jeon_2016]] | Windows 포렌식 아티팩트 체계적 분류(Table 2·3); PFP 포렌식 플랫폼 소개 |

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/$USNjrnl]] · [[artifacts/Windows_Event_Log]] · [[artifacts/Prefetch]]
