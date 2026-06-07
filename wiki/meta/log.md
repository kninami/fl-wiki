# Forensic Memex Wiki Log

## [2026-06-07] Q&A 반영 | kimyonghyun_2026
- 업데이트: artifacts/iOS_Backup_Manifest_db.md (Q4 Manifest.db 암호화 — 원문에 없던 추론 정정, Q3 손상 복구, Q9 파일명 변경)
- 업데이트: artifacts/iOS_Backup_Info_plist.md (Q10 암호화 백업 평문)
- 업데이트: techniques/iOS_Backup_Forensics.md (Q10 1단계, Q3 2.5단계 신설, Q11 bplist dedup, Q9 파일명 변경 주의)
- 업데이트: papers/kimyonghyun_2026.md (limitations에 Q&A 보완 노트 추가)
- 출처: 발표스크립트 + 질의응답 txt (raw/experiments/)

## [2026-06-07] ingest | kimyonghyun_2026
- 생성: papers/kimyonghyun_2026.md
- 신규 생성: artifacts/iOS_Backup_Manifest_db.md
- 신규 생성: artifacts/iOS_Backup_Info_plist.md
- 신규 생성: artifacts/iOS_Backup_Manifest_plist.md
- 신규 생성: artifacts/iOS_Backup_Status_plist.md
- 신규 생성: techniques/iOS_Backup_Forensics.md
- 업데이트: meta/index.md, meta/log.md
- 비고: raw/experiments/ 내 위치. iOS 모바일 포렌식 최초 아티팩트. 기존 Windows 중심 wiki와 충돌 없음.

## [2026-06-04] experiment | thumbcache_win11_maxsize_20260604
- 생성: experiments/thumbcache_win11_maxsize_20260604.md
- 업데이트: artifacts/Thumbcache.md (Win11 OS 테이블 행 추가, Findings 3개 추가, 실험 기록 행 추가)
- 업데이트: techniques/Thumbcache_Forensic_Analysis.md (실험 기록 행 추가)
- 결과: confirmed (Win11 DB 파일 체계, 용량 상한, EXIF→저장 DB 분기 신규 확인)
- 출처: 조경숙 개인과제 (학번 2026710464)

## [2026-06-04] experiment | thumbcache_win11_deletion_persistence_20260604
- 생성: experiments/thumbcache_win11_deletion_persistence_20260604.md
- 업데이트: artifacts/Thumbcache.md (Findings 2개 추가, 실험 기록 행 추가)
- 업데이트: techniques/Thumbcache_Forensic_Analysis.md (실험 기록 행 추가)
- 결과: confirmed (파일 삭제 후 잔류 quick_2014 일치; LRU 교체 알고리즘은 FIFO 가능성 있어 불확실)
- 출처: 조경숙 개인과제 (학번 2026710464)

## [2026-06-04] index 보완
- 업데이트: meta/index.md (artifacts/Thumbcache, artifacts/Windows_edb, techniques/Thumbcache_Forensic_Analysis, techniques/Digital_Sex_Crime_Mobile_Artifact_Triage 누락 항목 추가)
- 업데이트: meta/index.md (Experiments 섹션 신설)

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
