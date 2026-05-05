# Forensic Memex Wiki Index
_Last updated: 2026-05-03 | Artifacts: 19 | Techniques: 29 | Papers: 30_

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
| [[artifacts/Chrome_OS_Browser_History]] | APP | ChromeOS Forensics | hyde_2019 |
| [[artifacts/Chrome_OS_Local_Data]] | APP | ChromeOS Forensics | hyde_2019 |
| [[artifacts/Android_Auto_App_Data]] | APP | Android Auto Forensics | hickman_2019 |
| [[artifacts/Google_Assistant_Session_Data]] | APP | Android Auto Forensics | hickman_2019 |
| [[artifacts/ICS_PLC_Network_Traffic]] | APP | ICS Control Logic Forensics | qasim_2020 |
| [[artifacts/macOS_Page_Queues_Memory]] | OS | macOS Memory Page Queue Forensics | case_2020 |
| [[artifacts/Xiaomi_MiHome_App_Data]] | APP | IoT Ecosystem Forensics Xiaomi | dragonas_2021 |
| [[artifacts/Android_System_Log]] | OS | Android Log Evidence Extraction | cheng_2021 |

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
| [[techniques/Windows_ASEP_Persistence]] | Both | T1547 | uroz_2019 |
| [[techniques/ChromeOS_Forensics]] | Investigator | — | hyde_2019 |
| [[techniques/Android_Auto_Forensics]] | Investigator | — | hickman_2019 |
| [[techniques/ICS_Control_Logic_Forensics]] | Investigator | — | qasim_2020 |
| [[techniques/macOS_Memory_Page_Queue_Forensics]] | Investigator | — | case_2020 |
| [[techniques/Filesystem_Metadata_Carving]] | Investigator | — | nordvik_2020 |
| [[techniques/Remote_Live_Forensics_GRR]] | Investigator | — | brown_2020 |
| [[techniques/Storage_Reconstruction_Formal_Model]] | Investigator | — | schneider_2020 |
| [[techniques/ICS_Behavioral_Attack_Forensics]] | Investigator | — | neshenko_2021 |
| [[techniques/IoT_Ecosystem_Forensics_Xiaomi]] | Investigator | — | dragonas_2021 |
| [[techniques/Android_Log_Evidence_Extraction]] | Investigator | — | cheng_2021 |
| [[techniques/Smartphone_File_Triage_ML]] | Investigator | — | serhal_2021 |
| [[techniques/Ransomware_Bitcoin_Detection_TDA]] | Investigator | — | akcora_2021 |
| [[techniques/Explainable_DFAI]] | Both | — | solanke_2022 |
| [[techniques/SOLVE_IT_Knowledge_Base]] | Both | — | hargreaves_2025 |

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
| [[papers/uroz_2019]] | 2019 | 실험 | Windows ASEP 분류 + 메모리 포렌식 탐지 가능 여부 |
| [[papers/hyde_2019]] | 2019 | 실험 | Chrome OS / Chromebook 포렌식 아티팩트 |
| [[papers/murphey_2007]] | 2007 | 실험 | Windows NT5 이벤트 로그 자동 복구 |
| [[papers/dubois_2019]] | 2019 | 시연 | 학교 환경 사이버 위협 벡터 + 커뮤니티 치안 대응 |
| [[papers/hickman_2019]] | 2019 | 실험 | Android Auto 연결 기기 포렌식 아티팩트 |
| [[papers/qasim_2020]] | 2020 | 실험 | ICS 제어 로직 주입 공격 네트워크 포렌식 (Reditus) |
| [[papers/case_2020]] | 2020 | 실험 | macOS 페이지 큐 메모리 분석 + Volatility 플러그인 |
| [[papers/nordvik_2020]] | 2020 | 실험 | 타임스탬프 기반 범용 파일시스템 메타데이터 카빙 (Best Paper) |
| [[papers/brown_2020]] | 2020 | 시스템 구축 | GRR Rapid Response + Graylog GELF 통합 플러그인 |
| [[papers/schneider_2020]] | 2020 | 시스템 구축 | 메타데이터 재구성+파일 카빙 통합 형식 모델 LAYR |
| [[papers/neshenko_2021]] | 2021 | 실험 | BiGAN+RNN+CNN으로 ICS 수처리 시설 공격 탐지·귀인 |
| [[papers/dragonas_2021]] | 2021 | 실험 | Xiaomi Mi Home Android 앱 IoT 생태계 포렌식 |
| [[papers/cheng_2021]] | 2021 | 실험 | APK 정적 분석 기반 Android 로그 PII 자동 추출 (LogExtractor) |
| [[papers/serhal_2021]] | 2021 | 실험 | 파일 메타데이터 ML 기반 스마트폰 파일 트리아지 |
| [[papers/akcora_2021]] | 2021 | 실험 | TDA 기반 Bitcoin 랜섬웨어 주소 탐지 |
| [[papers/solanke_2022]] | 2022 | 이론 | 설명 가능한 디지털 포렌식 AI(xDFAI) 프레임워크 |
| [[papers/hargreaves_2025]] | 2025 | 시스템 구축 | MITRE ATT&CK 기반 디지털 포렌식 기법 지식 베이스 SOLVE-IT |
