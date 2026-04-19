---
technique: Windows ASEP Persistence
perspective: Both
topic: Persistence / Detection
attck: T1547
related_artifacts:
  - Windows Registry (In-Memory)
related_crime:
  - 악성코드 지속성
  - 인시던트 리스폰스
sources:
  - uroz_2019
updated: 2026-04-19
---

## 개요
Windows Auto-Start Extensibility Points(ASEP)는 명시적 사용자 상호작용 없이 프로그램을 자동 실행시키는 OS·애플리케이션 확장 지점. 악성코드가 지속성 확보에 남용. 메모리 포렌식으로 탐지 가능 여부가 ASEP 유형에 따라 다름.

## 메커니즘 — 4범주

### 1. System Persistence Mechanisms
| ASEP | 특징 |
|------|------|
| Run keys (HKLM) | 시스템 시작 시 실행; 메모리 포렌식 탐지 가능 |
| Run keys (HKCU) | 사용자 로그인 시 실행; 메모리 포렌식 탐지 가능 |
| Startup folder (%ALLUSERSPROFILE%) | 시작 시 실행; **메모리 포렌식 탐지 불가** (파일 카빙 필요) |
| Startup folder (%APPDATA%) | 사용자 로그인 시 실행; **메모리 포렌식 탐지 불가** |
| Scheduled tasks | 조건 충족 시 실행; **메모리 포렌식 탐지 불가** |
| Services | 시스템 부팅 시 실행; 메모리 포렌식 탐지 가능 |

### 2. Program Loader Abuse
| ASEP | 특징 |
|------|------|
| Image File Execution Options | 디버거로 프로그램 실행; 메모리 탐지 가능 |
| Extension hijacking (HKLM) | 파일 확장자 연결 프로그램 교체; 탐지 가능 |
| Extension hijacking (HKCU) | 사용자 레벨; 탐지 가능 |
| Shortcut manipulation | 기존 바로가기 조작; **메모리 탐지 불가** |
| COM hijacking (HKLM/HKCU) | COM 컴포넌트 교체; 탐지 가능 |
| Shim databases | 실행 전 패치 적용; 탐지 가능 |

### 3. Application Abuse
| ASEP | 특징 |
|------|------|
| Trojanized system binaries | 시스템 바이너리 패치; **실행 중 프로세스 분석 필요** |
| Office add-ins | DLL로 구현; **실행 중 프로세스 분석 필요** |
| Browser helper objects (BHO) | IE 플러그인 DLL; 메모리 탐지 가능 |

### 4. System Behavior Abuse
| ASEP | 특징 |
|------|------|
| Winlogon | 로그인 시 실행; 메모리 탐지 가능 |
| DLL hijacking | DLL 검색 순서 남용; **실행 중 프로세스 분석 필요** |
| AppInit DLLs | user32.dll 로드 앱 전체에 주입; 메모리 탐지 가능 |
| Active Setup (HKLM/HKCU) | 사용자 로그인 시 실행; 탐지 가능 |

## 실행 순서 (실험적 확인)
1. Winlogon (Userinit)
2. Winlogon (Shell)
3. Run keys (HKLM/RunOnceEx)
4. Run keys (HKCU/RunOnceEx)
5. Run keys (HKLM/RunOnce)
6. Active Setup (HKLM)
7. Active Setup (HKCU)
8. Startup folder (%ALLUSERSPROFILE%)
9. Startup folder (%APPDATA%)
10. Run keys (HKCU/Run)
11. Run keys (HKLM/Run)
12. Run keys (HKCU/RunOnce)

## 탐지 방법 (Investigator)
- **레지스트리 기반 ASEP**: Volatility Winesap 플러그인으로 메모리 덤프에서 탐지
  - REG_BINARY/REG_NONE 값에서 PE 헤더 탐지
  - REG_SZ 등에서 suspicious path 또는 shell command 탐지 (rundll32.exe, ShellExecute_RunDLL 등)
- **파일시스템 기반 ASEP**: 파일 카빙 또는 디스크 포렌식 병행 필요
- **실행 중 프로세스 기반**: 기존 전통적 메모리 포렌식 분석 필요

## 한계 & 주의사항
- 메모리 페이징 발생 시 일부 레지스트리 ASEP 탐지 불가
- Startup folder·Scheduled task·Shortcut은 메모리 포렌식 단독으로 탐지 불가
- 실 환경은 VM보다 volatile 데이터가 적어 탐지 범위 축소 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/uroz_2019]] | 4범주 분류, 메모리 탐지 가능 여부 실험 확인, Winesap 플러그인 개발 |

## 관련 페이지
[[artifacts/Windows_Registry_Memory]] · [[techniques/Registry_Memory_Forensics]]
