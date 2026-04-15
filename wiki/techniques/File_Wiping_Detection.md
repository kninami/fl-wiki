---
technique: File_Wiping_Detection
perspective: Investigator
topic: Anti-Forensics Detection
attck: T1485
related_artifacts:
  - AmCache
  - ShimCache
  - IconCache
  - Jumplist
  - UserAssist
  - Prefetch
  - SRUM
related_crime:
  - 증거인멸
  - 반포렌식
sources:
  - joo_2022
updated: 2026-04-15
---

## 개요
파일 와이핑 도구 실행 후 Windows 아티팩트에 남는 실행 흔적을 분석하여 와이핑 행위를 탐지하는 기법. 와이핑 도구 자체도 Windows 아티팩트에 흔적을 남기기 때문에 역설적으로 반포렌식 행위 증거가 된다.

## 메커니즘
파일 와이핑 도구(Glary Utilities, Eraser, Wise Care 365 등)를 실행하면 다음 아티팩트에 흔적이 기록된다:
- **AmCache**: 실행 파일 경로·SHA-1 해시·최초 실행 시각
- **ShimCache(AppCompatCache)**: 실행 파일 경로·수정 시각
- **IconCache.db**: 와이핑 도구 아이콘 캐시
- **Jumplist**: 최근 실행 파일 목록
- **UserAssist**: GUI 프로그램 실행 횟수
- **SRUM**: 앱별 CPU·네트워크 사용 이력

## 탐지 방법
### 아티팩트별 흔적 잔류 특성 (joo_2022 기준)
| 아티팩트 | 흔적 잔류율 | 비고 |
|----------|-------------|------|
| ShimCache(AppCompatCache) | 높음 | 도구 5종 전체에서 흔적 확인 |
| IconCache.db | 높음 | 캐시 초기화 안 됨 |
| AmCache | 중간 | 일부 도구에서 확인 |
| SRUM | 중간 | 실행 시간·리소스 사용 기록 |
| Open&Save MRU / LNK | 낮음 | 흔적 잔류율 최저 |

### 분석 도구
- X-Ways Forensic, REGA, Registry Explorer — ShimCache/AmCache 파싱
- JumpList Explorer — Jumplist 분석
- WinPrefetchView — Prefetch 분석
- UserAssistView — UserAssist 레지스트리 파싱
- SRUM_DUMP — SRUDB.dat 파싱

## 한계 & 주의사항
- 와이핑 도구가 자신의 흔적도 지울 경우 탐지 어려움 (일부 고급 도구)
- 도구 버전에 따라 흔적 잔류 여부 다름
- Windows 10 환경 한정 검증 — 다른 OS 버전에서 차이 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/joo_2022]] | 5개 와이핑 도구 × 13개 아티팩트 매트릭스 분석; ShimCache·IconCache.db 흔적 잔류율 최고 |

## 관련 페이지
[[artifacts/AmCache]] · [[artifacts/ShimCache]] · [[artifacts/IconCache]] · [[artifacts/Jumplist]] · [[artifacts/UserAssist]] · [[artifacts/Prefetch]] · [[artifacts/SRUM]]
