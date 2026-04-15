---
technique: WSA_Forensics
perspective: Investigator
topic: Mobile/Virtualization Forensics
related_artifacts:
  - WSA_userdata_vhdx
  - SRUM
  - AmCache
  - UserAssist
  - $USNjrnl
  - Prefetch
related_crime:
  - 모바일 앱 증거 수집
  - 앱 사용 이력 추적
sources:
  - kim_2022a
updated: 2026-04-15
---

## 개요
Windows 11의 Windows Subsystem for Android(WSA)에서 생성되는 아티팩트를 체계적으로 수집·분석하는 기법. Android 앱 사용 행위를 Windows 측 아티팩트와 Android 측 아티팩트 양쪽에서 복원.

## 메커니즘

### 아티팩트 레이어
**Android 측 (WSA 내부)**:
- `userdata.vhdx`: Android 데이터 전체 저장 (`%UserProfile%\AppData\Local\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalCache`)
- 접근 방법: ADB(127.0.0.1:58526), SMB 서버, UsageStats

**Windows 측**:
| 아티팩트 | 기록 정보 |
|----------|-----------|
| LNK | WSA 앱 실행 바로가기 (`%AppData%\Roaming\Microsoft\Windows\Start Menu\Programs`) |
| Prefetch | WSACLIENT.EXE 등 WSA 관련 프로세스 실행 이력 |
| $UsnJrnl | userdata.vhdx 파일 변경 추적 |
| UserAssist | GUI 앱 실행 횟수 |
| SRUM | wsaclient.exe 앱 리소스 사용 이력 |
| AmCache | WSA 앱 설치·실행 해시·경로 |

## 수집 절차
1. WSA 설정 확인: `LocalState/settings.dat`, `appcompatdb.json`
2. ADB 연결: `adb connect 127.0.0.1:58526`
3. Android 데이터 추출: ADB pull 또는 SMB 마운트
4. userdata.vhdx 마운트 후 Android 파티션 분석
5. Windows 측 아티팩트 별도 수집 (AmCache, SRUM, $UsnJrnl 등)

## 한계 & 주의사항
- Windows 11 + WSA 특정 버전 한정 — 버전 업에 따른 경로 변경 가능
- ADB 연결 필요 — WSA 실행 중이어야 가능
- userdata.vhdx 분석에는 Android 포렌식 도구 추가 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kim_2022a]] | WSA 아티팩트 체계 분석; ADB/SMB 수집 방법; 커스텀 CLI 수집 도구 개발·공개 |

## 관련 페이지
[[artifacts/WSA_userdata_vhdx]] · [[artifacts/SRUM]] · [[artifacts/AmCache]] · [[artifacts/UserAssist]] · [[artifacts/$USNjrnl]] · [[artifacts/Prefetch]]
