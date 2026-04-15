# Forensic Memex Wiki Index
_Last updated: 2026-04-15 | Artifacts: 36 | Techniques: 29 | Papers: 29_

## Artifacts

| 페이지 | Layer | 주요 기법 | Sources |
|--------|-------|-----------|---------|
| [[artifacts/Windows_NT_Heap]] | OS | Memory Heap Forensics, IM Memory Forensics | uroz_2026, fernandez_2022 |
| [[artifacts/Telegram_Desktop_Memory]] | APP | IM Memory Forensics | fernandez_2022, uroz_2026 |
| [[artifacts/$LogFile]] | FS | Timestamp Manipulation Detection, ReFS Journaling Forensics | palmbach_2020, lee_2021 |
| [[artifacts/$USNjrnl]] | FS | Timestamp Manipulation Detection, ReFS Journaling Forensics, WSA Forensics | palmbach_2020, lee_2021, kim_2022a |
| [[artifacts/Prefetch]] | OS | Timestamp Manipulation Detection, File Wiping Detection, WSA Forensics | palmbach_2020, joo_2022, kim_2017, kim_2022a |
| [[artifacts/Windows_Event_Log]] | OS | EventLog Automated Recovery, Timestamp Manipulation Detection | palmbach_2020, murphey_2007 |
| [[artifacts/Windows_Diagnostics_EventTranscript]] | OS | Windows Diagnostics Forensics | park_2022 |
| [[artifacts/Windows_Registry_Memory]] | OS | Registry Memory Forensics, Automated User Analysis | dolangavitt_2008, kim_2017 |
| [[artifacts/ReFS_Logfile]] | FS | ReFS Journaling Forensics | lee_2021 |
| [[artifacts/ReFS_Change_Journal]] | FS | ReFS Journaling Forensics | lee_2021 |
| [[artifacts/ReFS_Metadata_Structures]] | FS | ReFS Deleted Data Recovery | kim_2018, lee_2021 |
| [[artifacts/Microsoft365_DRF]] | APP | Data Remnants Forensics | joun_2023 |
| [[artifacts/hiberfil_sys]] | OS | Hibernation Forensics | bang_2022 |
| [[artifacts/AmCache]] | OS | File Wiping Detection, WSA Forensics | joo_2022, kim_2022a |
| [[artifacts/ShimCache]] | OS | File Wiping Detection | joo_2022 |
| [[artifacts/IconCache]] | OS | File Wiping Detection | joo_2022 |
| [[artifacts/Jumplist]] | OS | File Wiping Detection | joo_2022 |
| [[artifacts/UserAssist]] | OS | File Wiping Detection, WSA Forensics | joo_2022, kim_2022a |
| [[artifacts/SRUM]] | OS | File Wiping Detection, WSA Forensics | joo_2022, kim_2022a |
| [[artifacts/WSA_userdata_vhdx]] | APP | WSA Forensics | kim_2022a |
| [[artifacts/Microsoft_Teams_Windows]] | APP | Teams Differential Forensics | kim_2021 |
| [[artifacts/Microsoft_Teams_Android]] | APP | Teams Differential Forensics | kim_2021 |
| [[artifacts/Cryptocurrency_Wallet_Files]] | APP | Cryptocurrency Wallet Forensics | park_2022b |
| [[artifacts/Slack_Android_DB]] | APP | Slack Discord IM Forensics | shin_2020 |
| [[artifacts/Slack_PC_Artifacts]] | APP | Slack Discord IM Forensics | shin_2020 |
| [[artifacts/Discord_Android_DB]] | APP | Slack Discord IM Forensics | shin_2020 |
| [[artifacts/Discord_PC_Artifacts]] | APP | Slack Discord IM Forensics | shin_2020 |
| [[artifacts/JANDI_Android_DB]] | APP | JANDI NAVER WORKS Android Forensics, JANDI Artifact Analysis | shin_2021, we_2024 |
| [[artifacts/NAVER_WORKS_Android_DB]] | APP | JANDI NAVER WORKS Android Forensics | shin_2021 |
| [[artifacts/Wire_Cookies_DB]] | APP | Wire Credential Acquisition | shin_2021b |
| [[artifacts/Wire_IndexedDB]] | APP | Wire Credential Acquisition | shin_2021b |
| [[artifacts/JANDI_LevelDB]] | APP | JANDI Artifact Analysis | we_2024 |
| [[artifacts/Viber_DB]] | APP | Win8 IM App Forensics | lee_2015 |
| [[artifacts/Line_DB]] | APP | Win8 IM App Forensics | lee_2015 |
| [[artifacts/WeChat_PC_DB]] | APP | WeChat Artifact Analysis | park_2020 |
| [[artifacts/WeChat_Mobile_DB]] | APP | WeChat Artifact Analysis | park_2020 |

## Techniques

| 페이지 | Perspective | ATT&CK | Sources |
|--------|-------------|--------|---------|
| [[techniques/Timestamp_Manipulation_Detection]] | Investigator | T1070.006 | palmbach_2020 |
| [[techniques/Memory_Heap_Forensics]] | Investigator | — | uroz_2026, fernandez_2022 |
| [[techniques/IM_Memory_Forensics]] | Investigator | — | fernandez_2022 |
| [[techniques/Windows_Diagnostics_Forensics]] | Investigator | — | park_2022 |
| [[techniques/EventLog_Automated_Recovery]] | Investigator | — | murphey_2007 |
| [[techniques/Registry_Memory_Forensics]] | Investigator | — | dolangavitt_2008 |
| [[techniques/ReFS_Journaling_Forensics]] | Investigator | — | lee_2021 |
| [[techniques/Data_Remnants_Forensics]] | Investigator | — | joun_2023 |
| [[techniques/Synthetic_Trace_Generation]] | Both | — | schmidt_2023 |
| [[techniques/eBPF_Container_DFIR]] | Investigator | — | jin_2025 |
| [[techniques/Selective_Seizure_Evaluation]] | Investigator | — | kim_2026 |
| [[techniques/Artifact_Knowledge_Management]] | Both | — | grajeda_2018 |
| [[techniques/DFIR_Tool_Error_Mitigation]] | Investigator | — | hargreaves_2024 |
| [[techniques/Evidence_Preservation_Evolving]] | Investigator | — | spichiger_2025 |
| [[techniques/Hibernation_Forensics]] | Investigator | — | bang_2022 |
| [[techniques/Windows_System_Forensics_Survey]] | Investigator | — | jeon_2016 |
| [[techniques/File_Wiping_Detection]] | Investigator | T1485 | joo_2022 |
| [[techniques/Automated_User_Analysis]] | Investigator | — | kim_2017 |
| [[techniques/ReFS_Deleted_Data_Recovery]] | Investigator | — | kim_2018 |
| [[techniques/Teams_Differential_Forensics]] | Investigator | — | kim_2021 |
| [[techniques/WSA_Forensics]] | Investigator | — | kim_2022a |
| [[techniques/Win8_IM_App_Forensics]] | Investigator | — | lee_2015 |
| [[techniques/WeChat_Artifact_Analysis]] | Investigator | — | park_2020 |
| [[techniques/Cryptocurrency_Wallet_Forensics]] | Investigator | — | park_2022b |
| [[techniques/Slack_Discord_IM_Forensics]] | Investigator | — | shin_2020 |
| [[techniques/JANDI_NAVER_WORKS_Android_Forensics]] | Investigator | — | shin_2021 |
| [[techniques/Wire_Credential_Acquisition]] | Investigator | — | shin_2021b |
| [[techniques/JANDI_Artifact_Analysis]] | Investigator | — | we_2024 |
| [[techniques/User_Attribution_Framework]] | Investigator | — | yun_2015 |

## Papers

| 페이지 | 연도 | 방법론 | 핵심 주제 |
|--------|------|--------|-----------|
| [[papers/uroz_2026]] | 2026 | 실험 | Windows NT Heap 구조 역분석 + Volatility 3 플러그인 |
| [[papers/kim_2026]] | 2026 | 실험 | 선별 압수 포렌식 도구 평가 모델 |
| [[papers/spichiger_2025]] | 2025 | 이론 | 진화하는 시스템에서 증거 의미 보존 |
| [[papers/jin_2025]] | 2025 | 실험 | Windows eBPF 기반 컨테이너 실시간 DFIR |
| [[papers/we_2024]] | 2024 | 실험 | JANDI Windows 앱 LevelDB 아티팩트 + API 복원 |
| [[papers/hargreaves_2024]] | 2024 | 방법론 | 포렌식 도구 추상 모델 + 오류 완화 |
| [[papers/joun_2023]] | 2023 | 실험 | 데이터 잔존물(DRF) 프레임워크 + M365 케이스 |
| [[papers/schmidt_2023]] | 2023 | 실험 | 컴퓨터 비전 기반 합성 포렌식 데이터셋 생성 |
| [[papers/kim_2022a]] | 2022 | 실험 | Windows 11 WSA 아티팩트 수집 및 분석 |
| [[papers/bang_2022]] | 2022 | 실험 | Windows 10 hiberfil.sys 구조 분석 + Volatility 3 활용 |
| [[papers/joo_2022]] | 2022 | 실험 | 파일 와이핑 도구 실행 흔적 — 13개 아티팩트 비교 |
| [[papers/park_2022b]] | 2022 | 실험 | Windows 암호화폐 지갑 앱 10종 아티팩트 분석 |
| [[papers/fernandez_2022]] | 2022 | 실험 | Telegram Desktop 메모리 아티팩트 추출 |
| [[papers/park_2022]] | 2022 | 실험 | Windows Diagnostics(EventTranscript.db) 사용자 행위 분석 |
| [[papers/kim_2021]] | 2021 | 실험 | Microsoft Teams Windows/Android Differential Forensics |
| [[papers/shin_2021]] | 2021 | 실험 | JANDI·NAVER WORKS Android DB + 삭제 메시지 복원 |
| [[papers/shin_2021b]] | 2021 | 실험 | Wire Windows 인증 토큰 획득 + 대화 복원 |
| [[papers/lee_2021]] | 2021 | 실험 | ReFS 저널링 파일 구조 역분석 + ARIN 도구 |
| [[papers/park_2020]] | 2020 | 실험 | WeChat PC/Android 암호화 DB 역분석 |
| [[papers/shin_2020]] | 2020 | 실험 | Slack·Discord Android/PC 아티팩트 분석 |
| [[papers/palmbach_2020]] | 2020 | 실험 | NTFS 타임스탬프 조작 탐지 아티팩트 신뢰도 평가 |
| [[papers/kim_2018]] | 2018 | 실험 | ReFS v1.2·v3.3 메타데이터 구조 + 삭제 데이터 복구 |
| [[papers/grajeda_2018]] | 2018 | 시스템 구축 | AGP 아티팩트 지식 공유 플랫폼 |
| [[papers/kim_2017]] | 2017 | 실험 | Windows 아티팩트 자동화 사용자 행위 분석 + NLP |
| [[papers/lee_2015]] | 2015 | 실험 | Windows 8 Viber·Line DB + R 소셜 네트워크 분석 |
| [[papers/yun_2015]] | 2015 | 이론 | 디지털 파일 사용자 추정 5-CASE 방법론 |
| [[papers/jeon_2016]] | 2016 | 서베이 | Windows 시스템 포렌식 아티팩트 체계적 분류 |
| [[papers/dolangavitt_2008]] | 2008 | 실험 | 물리 메모리 내 Windows 레지스트리 포렌식 |
| [[papers/murphey_2007]] | 2007 | 실험 | Windows NT5 이벤트 로그 자동 복구 |
