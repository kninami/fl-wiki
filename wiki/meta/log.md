# Forensic Memex Wiki Log

## [2026-04-15] lint
- 모순: 없음
- 고아 페이지: 없음
- Findings 미비 artifact: 없음
- Sources 미매칭 paper: 없음
- **수정 완료**: technique 페이지 YAML `related_artifacts` 파일명 통일 (공백→언더스코어) — 8개 파일
  - EventLog_Automated_Recovery, IM_Memory_Forensics, Memory_Heap_Forensics, ReFS_Journaling_Forensics, Registry_Memory_Forensics, Selective_Seizure_Evaluation, Timestamp_Manipulation_Detection, Windows_Diagnostics_Forensics
- **수정 완료**: technique 페이지 YAML `related_artifacts` 누락 항목 추가 — 4개 파일
  - User_Attribution_Framework (Windows_Registry_Memory, $LogFile, UserAssist, Jumplist 추가)
  - Windows_System_Forensics_Survey ($LogFile, $USNjrnl, Windows_Event_Log, Prefetch 추가)
  - Automated_User_Analysis (Windows_Event_Log 추가)
  - WSA_Forensics (관련 페이지 본문에 Prefetch 링크 추가)

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

---

## [2026-04-15] ingest | 한국어 논문 15개 (2세션)

> 이전 세션에서 paper 페이지 15개·artifact 페이지 15개가 생성됐으나 technique 페이지·누락 artifact·index/log 미반영 상태였음. 본 세션에서 완성.

### bang_2022
- 생성: papers/bang_2022.md (이전 세션)
- 신규 생성: artifacts/hiberfil_sys.md (이전 세션)
- 신규 생성: techniques/Hibernation_Forensics.md
- 모순 없음

### jeon_2016
- 생성: papers/jeon_2016.md (이전 세션)
- 직접 분석 아티팩트 없음 (서베이 논문)
- 신규 생성: techniques/Windows_System_Forensics_Survey.md
- 모순 없음

### joo_2022
- 생성: papers/joo_2022.md (이전 세션)
- 신규 생성: artifacts/AmCache.md, ShimCache.md, IconCache.md, Jumplist.md, UserAssist.md, SRUM.md (이전 세션; Prefetch 기존 페이지 업데이트)
- 신규 생성: techniques/File_Wiping_Detection.md
- 업데이트: artifacts/Prefetch.md (joo_2022 source·Findings 추가)
- 모순 없음

### kim_2017
- 생성: papers/kim_2017.md (이전 세션)
- 직접 분석: Prefetch, Windows_Registry_Memory (기존 페이지 업데이트)
- 신규 생성: techniques/Automated_User_Analysis.md
- 업데이트: artifacts/Prefetch.md (kim_2017 source·Findings 추가)
- 모순 없음

### kim_2018
- 생성: papers/kim_2018.md (이전 세션)
- 신규 생성: artifacts/ReFS_Metadata_Structures.md
- 신규 생성: techniques/ReFS_Deleted_Data_Recovery.md
- 모순 없음

### kim_2021
- 생성: papers/kim_2021.md (이전 세션)
- 신규 생성: artifacts/Microsoft_Teams_Windows.md, Microsoft_Teams_Android.md (이전 세션)
- 신규 생성: techniques/Teams_Differential_Forensics.md
- 모순 없음

### kim_2022a
- 생성: papers/kim_2022a.md (이전 세션)
- 신규 생성: artifacts/WSA_userdata_vhdx.md, SRUM.md, AmCache.md, UserAssist.md (이전 세션)
- 신규 생성: techniques/WSA_Forensics.md
- 업데이트: artifacts/Prefetch.md, $USNjrnl.md (kim_2022a source·Findings 추가)
- 모순 없음

### lee_2015
- 생성: papers/lee_2015.md (이전 세션)
- 신규 생성: artifacts/Viber_DB.md, Line_DB.md
- 신규 생성: techniques/Win8_IM_App_Forensics.md
- 모순 없음

### park_2020
- 생성: papers/park_2020.md (이전 세션)
- 신규 생성: artifacts/WeChat_PC_DB.md, WeChat_Mobile_DB.md
- 신규 생성: techniques/WeChat_Artifact_Analysis.md
- 모순 없음

### park_2022b
- 생성: papers/park_2022b.md (이전 세션)
- 신규 생성: artifacts/Cryptocurrency_Wallet_Files.md (이전 세션)
- 신규 생성: techniques/Cryptocurrency_Wallet_Forensics.md
- 모순 없음

### shin_2020
- 생성: papers/shin_2020.md (이전 세션)
- 신규 생성: artifacts/Slack_Android_DB.md, Slack_PC_Artifacts.md, Discord_Android_DB.md, Discord_PC_Artifacts.md (이전 세션)
- 신규 생성: techniques/Slack_Discord_IM_Forensics.md
- 모순 없음

### shin_2021
- 생성: papers/shin_2021.md (이전 세션)
- 신규 생성: artifacts/JANDI_Android_DB.md, NAVER_WORKS_Android_DB.md
- 신규 생성: techniques/JANDI_NAVER_WORKS_Android_Forensics.md
- 모순 없음

### shin_2021b
- 생성: papers/shin_2021b.md (이전 세션)
- 신규 생성: artifacts/Wire_Cookies_DB.md, Wire_IndexedDB.md
- 신규 생성: techniques/Wire_Credential_Acquisition.md
- 모순 없음

### we_2024
- 생성: papers/we_2024.md (이전 세션)
- 신규 생성: artifacts/JANDI_LevelDB.md
- 신규 생성: techniques/JANDI_Artifact_Analysis.md
- 모순 없음

### yun_2015
- 생성: papers/yun_2015.md (이전 세션)
- 직접 분석 아티팩트 없음 (이론·사례 논문)
- 신규 생성: techniques/User_Attribution_Framework.md
- 모순 없음

### 최종 생성 요약
- 생성: papers/ 15개 (이전 세션 완료)
- 신규 생성 artifacts/ 25개 (이전 세션 15개 + 본 세션 10개)
- 신규 생성 techniques/ 15개
- 업데이트: artifacts/Prefetch.md, artifacts/$USNjrnl.md
- 업데이트: meta/index.md, meta/log.md
