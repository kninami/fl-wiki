# Forensic Memex Wiki Log

## [2026-04-20] ingest | raw/new 배치 4/14 (15개 완료)
- **진행 상황**: raw/new 중 15개 ingest 완료
- **완료**: schneider_2020, neshenko_2021, dragonas_2021, cheng_2021, serhal_2021, akcora_2021, solanke_2022
- **다음**: 배치 5 — 이후 알파벳 순 파일부터

### schneider_2020
- 생성: papers/schneider_2020.md
- 신규 생성: techniques/Storage_Reconstruction_Formal_Model.md
- 아티팩트 직접 분석 없음 (형식 모델 논문)
- 모순 없음

### neshenko_2021
- 생성: papers/neshenko_2021.md
- 신규 생성: techniques/ICS_Behavioral_Attack_Forensics.md
- 아티팩트 직접 분석 없음 (ICS 물리 센서 시계열 데이터 분석)
- 모순 없음

### dragonas_2021
- 생성: papers/dragonas_2021.md
- 신규 생성: artifacts/Xiaomi_MiHome_App_Data.md
- 신규 생성: techniques/IoT_Ecosystem_Forensics_Xiaomi.md
- 모순 없음

### cheng_2021
- 생성: papers/cheng_2021.md
- 신규 생성: artifacts/Android_System_Log.md
- 신규 생성: techniques/Android_Log_Evidence_Extraction.md
- 모순 없음

### serhal_2021
- 생성: papers/serhal_2021.md
- 신규 생성: techniques/Smartphone_File_Triage_ML.md
- 아티팩트 직접 분석 없음 (파일 메타데이터 ML 피처로 사용)
- 모순 없음

### akcora_2021
- 생성: papers/akcora_2021.md
- 신규 생성: techniques/Ransomware_Bitcoin_Detection_TDA.md
- 아티팩트 직접 분석 없음 (Bitcoin 블록체인 거래 데이터)
- 모순 없음

### solanke_2022
- 생성: papers/solanke_2022.md
- 신규 생성: techniques/Explainable_DFAI.md
- 아티팩트 직접 분석 없음 (이론·포지션 논문)
- 모순 없음

### 참고사항
- schneider_2020: LAYR 도구, FAU 소속 — nordvik_2020(DFRWS 2020 Best Paper)과 동일 학회 수록
- dragonas_2021: 루팅 기기(Magisk) 환경 전제 — 비루팅 시 내부 스토리지 접근 불가

## [2026-04-19] ingest | raw/new 배치 3/14 (8개 완료)
- **진행 상황**: raw/new 중 8개 ingest 완료
- **완료**: uroz_2019, hyde_2019, dubois_2019, hickman_2019, qasim_2020, case_2020, nordvik_2020, brown_2020
- **다음**: `2020_USA_pres-memory_forensics_of_android_volatile_memory.pdf` 부터

### case_2020
- 생성: papers/case_2020.md
- 신규 생성: artifacts/macOS_Page_Queues_Memory.md
- 신규 생성: techniques/macOS_Memory_Page_Queue_Forensics.md
- 모순 없음

### nordvik_2020
- 생성: papers/nordvik_2020.md
- 신규 생성: techniques/Filesystem_Metadata_Carving.md
- artifact 페이지 미생성 (NTFS MFT/Ext4 inode는 기존 wiki에 없으나, 이 논문의 핵심 기여는 카빙 기법 자체임)
- 모순 없음

### brown_2020
- 생성: papers/brown_2020.md
- 신규 생성: techniques/Remote_Live_Forensics_GRR.md
- 아티팩트 직접 분석 없음 (GRR 출력 데이터 활용한 시스템 구축 논문)
- 모순 없음

### 참고사항
- nordvik_2020: DFRWS 2020 Best Paper Award 수상

## [2026-04-19] ingest | raw/new 배치 2/14 (5개 완료)
- **진행 상황**: raw/new 총 40개 파일 중 5개 ingest 완료 (중복 1개 제외 시 38개 중 5개)
- **완료**: uroz_2019, hyde_2019, dubois_2019, hickman_2019, qasim_2020
- **다음**: `2020_USA_paper-fuzzing_closed_source_javascript_engines_with_coverage_guided_feedback.pdf` 부터

### dubois_2019
- 생성: papers/dubois_2019.md
- 아티팩트 직접 분석 없음 (학교 보안 인식 발표)
- 신규 artifact·technique 페이지 없음
- 모순 없음

### hickman_2019
- 생성: papers/hickman_2019.md
- 신규 생성: artifacts/Android_Auto_App_Data.md
- 신규 생성: artifacts/Google_Assistant_Session_Data.md
- 신규 생성: techniques/Android_Auto_Forensics.md
- 모순 없음

### qasim_2020
- 생성: papers/qasim_2020.md
- 신규 생성: artifacts/ICS_PLC_Network_Traffic.md
- 신규 생성: techniques/ICS_Control_Logic_Forensics.md
- 모순 없음

### 참고사항
- dubois_2019는 고등학생 발표 — 포렌식 아티팩트 직접 분석 없어 artifact/technique 페이지 미생성

## [2026-04-19] ingest | raw/new 배치 1/14 (2개 완료)
- **진행 상황**: raw/new 총 40개 파일 중 2개 ingest 완료
- **완료**: uroz_2019, hyde_2019
- **다음**: 2019_USA_pres-school_cyber_risk_challenges.pdf 부터 (중복 파일 1개 제외 시 38개 남음)

### uroz_2019
- 생성: papers/uroz_2019.md
- 신규 생성: techniques/Windows_ASEP_Persistence.md
- 업데이트: artifacts/Windows_Registry_Memory.md (uroz_2019 source 추가, Findings 2개 추가)
- 모순 없음

### hyde_2019
- 생성: papers/hyde_2019.md
- 신규 생성: artifacts/Chrome_OS_Browser_History.md
- 신규 생성: artifacts/Chrome_OS_Local_Data.md
- 신규 생성: techniques/ChromeOS_Forensics.md
- 모순 없음

### 참고사항
- `2019_USA_pres-school_cyber_risk_challenges (1).pdf`는 동일 파일 중복 — 별도 처리 안 함
- uroz_2019는 기존 ingested uroz_2026과 다른 논문 (동일 저자, 다른 주제)

## [2026-04-15] ingest | 전체 초기 구축 (14개 논문)

### uroz_2026
- 생성: papers/uroz_2026.md
- 신규 생성: artifacts/Windows_NT_Heap.md
- 신규 생성: techniques/Memory_Heap_Forensics.md
- 모순 없음

### fernandez_2022
- 생성: papers/fernandez_2022.md
- 신규 생성: artifacts/Telegram_Desktop_Memory.md
- 신규 생성: techniques/IM_Memory_Forensics.md
- 업데이트: artifacts/Windows_NT_Heap.md (fernandez_2022 source 추가, Findings 추가)
- 모순 없음

### palmbach_2020
- 생성: papers/palmbach_2020.md
- 신규 생성: artifacts/$LogFile.md
- 신규 생성: artifacts/$USNjrnl.md
- 신규 생성: artifacts/Prefetch.md
- 신규 생성: artifacts/Windows_Event_Log.md
- 신규 생성: techniques/Timestamp_Manipulation_Detection.md
- 모순 없음

### park_2022
- 생성: papers/park_2022.md
- 신규 생성: artifacts/Windows_Diagnostics_EventTranscript.md
- 신규 생성: techniques/Windows_Diagnostics_Forensics.md
- 모순 없음

### murphey_2007
- 생성: papers/murphey_2007.md
- 업데이트: artifacts/Windows_Event_Log.md (murphey_2007 source 추가, Findings 추가)
- 신규 생성: techniques/EventLog_Automated_Recovery.md
- 비고: raw/ 내 동일 논문 중복 파일 존재 (`2007_USA_paper-automated_windows_event_log_forensics (1).pdf`) — 동일 논문 확인, 별도 처리 안 함

### joun_2023
- 생성: papers/joun_2023.md
- 신규 생성: artifacts/Microsoft365_DRF.md
- 신규 생성: techniques/Data_Remnants_Forensics.md
- 모순 없음

### hargreaves_2024
- 생성: papers/hargreaves_2024.md
- 신규 생성: techniques/DFIR_Tool_Error_Mitigation.md
- 아티팩트 직접 분석 없음 (방법론 논문)
- 모순 없음

### spichiger_2025
- 생성: papers/spichiger_2025.md
- 신규 생성: techniques/Evidence_Preservation_Evolving.md
- 아티팩트 직접 분석 없음 (이론 논문)
- 모순 없음

### lee_2021
- 생성: papers/lee_2021.md
- 신규 생성: artifacts/ReFS_Logfile.md
- 신규 생성: artifacts/ReFS_Change_Journal.md
- 신규 생성: techniques/ReFS_Journaling_Forensics.md
- 업데이트: artifacts/$LogFile.md (lee_2021 source 추가, Findings 추가)
- 업데이트: artifacts/$USNjrnl.md (lee_2021 source 추가, Findings 추가)
- 모순 없음

### grajeda_2018
- 생성: papers/grajeda_2018.md
- 신규 생성: techniques/Artifact_Knowledge_Management.md
- 아티팩트 직접 분석 없음 (플랫폼 논문)
- 모순 없음

### schmidt_2023
- 생성: papers/schmidt_2023.md
- 신규 생성: techniques/Synthetic_Trace_Generation.md
- 아티팩트 직접 분석 없음 (데이터셋 생성 방법론 논문)
- 모순 없음

### jin_2025
- 생성: papers/jin_2025.md
- 신규 생성: techniques/eBPF_Container_DFIR.md
- 수집 대상: 커널 이벤트 스트림 (별도 정적 아티팩트 없음)
- 모순 없음

### kim_2026
- 생성: papers/kim_2026.md
- 신규 생성: techniques/Selective_Seizure_Evaluation.md
- $MFT·Windows Log는 도구 평가 테스트 대상으로 사용 (직접 분석 아님)
- 모순 없음

### dolangavitt_2008
- 생성: papers/dolangavitt_2008.md
- 신규 생성: artifacts/Windows_Registry_Memory.md
- 신규 생성: techniques/Registry_Memory_Forensics.md
- 모순 없음

### 최종 생성 요약
- 생성: papers/ 14개
- 신규 생성: artifacts/ 11개
- 신규 생성: techniques/ 14개
- 업데이트: meta/index.md, meta/log.md
