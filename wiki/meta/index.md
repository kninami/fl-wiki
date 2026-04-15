# Forensic Memex Wiki Index
_Last updated: 2026-04-15 | Artifacts: 11 | Techniques: 14 | Papers: 14_

## Artifacts

| 페이지 | Layer | 주요 기법 | Sources |
|--------|-------|-----------|---------|
| [[artifacts/Windows_NT_Heap]] | OS | Memory Heap Forensics, IM Memory Forensics | uroz_2026, fernandez_2022 |
| [[artifacts/Telegram_Desktop_Memory]] | APP | IM Memory Forensics | fernandez_2022, uroz_2026 |
| [[artifacts/$LogFile]] | FS | Timestamp Manipulation Detection, ReFS Journaling Forensics | palmbach_2020, lee_2021 |
| [[artifacts/$USNjrnl]] | FS | Timestamp Manipulation Detection, ReFS Journaling Forensics | palmbach_2020, lee_2021 |
| [[artifacts/Prefetch]] | OS | Timestamp Manipulation Detection | palmbach_2020 |
| [[artifacts/Windows_Event_Log]] | OS | EventLog Automated Recovery, Timestamp Manipulation Detection | palmbach_2020, murphey_2007 |
| [[artifacts/Windows_Diagnostics_EventTranscript]] | OS | Windows Diagnostics Forensics | park_2022 |
| [[artifacts/Windows_Registry_Memory]] | OS | Registry Memory Forensics | dolangavitt_2008 |
| [[artifacts/ReFS_Logfile]] | FS | ReFS Journaling Forensics | lee_2021 |
| [[artifacts/ReFS_Change_Journal]] | FS | ReFS Journaling Forensics | lee_2021 |
| [[artifacts/Microsoft365_DRF]] | APP | Data Remnants Forensics | joun_2023 |

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

## Papers

| 페이지 | 연도 | 방법론 | 핵심 주제 |
|--------|------|--------|-----------|
| [[papers/uroz_2026]] | 2026 | 실험 | Windows NT Heap 구조 역분석 + Volatility 3 플러그인 |
| [[papers/kim_2026]] | 2026 | 실험 | 선별 압수 포렌식 도구 평가 모델 |
| [[papers/spichiger_2025]] | 2025 | 이론 | 진화하는 시스템에서 증거 의미 보존 |
| [[papers/jin_2025]] | 2025 | 실험 | Windows eBPF 기반 컨테이너 실시간 DFIR |
| [[papers/hargreaves_2024]] | 2024 | 방법론 | 포렌식 도구 추상 모델 + 오류 완화 |
| [[papers/joun_2023]] | 2023 | 실험 | 데이터 잔존물(DRF) 프레임워크 + M365 케이스 |
| [[papers/schmidt_2023]] | 2023 | 실험 | 컴퓨터 비전 기반 합성 포렌식 데이터셋 생성 |
| [[papers/fernandez_2022]] | 2022 | 실험 | Telegram Desktop 메모리 아티팩트 추출 |
| [[papers/park_2022]] | 2022 | 실험 | Windows Diagnostics(EventTranscript.db) 사용자 행위 분석 |
| [[papers/lee_2021]] | 2021 | 실험 | ReFS 저널링 파일 구조 역분석 + ARIN 도구 |
| [[papers/palmbach_2020]] | 2020 | 실험 | NTFS 타임스탬프 조작 탐지 아티팩트 신뢰도 평가 |
| [[papers/grajeda_2018]] | 2018 | 시스템 구축 | AGP 아티팩트 지식 공유 플랫폼 |
| [[papers/dolangavitt_2008]] | 2008 | 실험 | 물리 메모리 내 Windows 레지스트리 포렌식 |
| [[papers/murphey_2007]] | 2007 | 실험 | Windows NT5 이벤트 로그 자동 복구 |
