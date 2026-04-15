---
paper: palmbach_2020
title: "Artifacts for Detecting Timestamp Manipulation in NTFS on Windows and Their Reliability (2020)"
full_title: "Artifacts for Detecting Timestamp Manipulation in NTFS on Windows and Their Reliability"
authors: "Palmbach, D.; Breitinger, F."
year: 2020
venue: DFRWS EU 2020 (FSI Digital Investigation 32)
doi: https://doi.org/10.1016/j.fsidi.2020.300920
tags:
  - Timestomping
  - timestamp manipulation
  - NTFS
  - $LogFile
  - $USNjrnl
  - Prefetch
  - Windows Event Log
  - anti-forensics
method: 실험 (가상머신 + 4개 텍스트 파일 + 3종 타임스탬핑 도구 실험)
limitations: "활성 공격자가 아닌 수동적 공격 가정; Windows 10만 테스트; 분석된 4개 아티팩트 중 어느 것도 active exploitation에 완전히 저항하지 못함"
---

## 한 줄 요약
NTFS에서 타임스탬프 조작을 탐지하기 위해 $LogFile, $USNjrnl, Prefetch, Windows Event Log 4개 아티팩트를 실험적으로 분석하고 신뢰도를 평가.

## 핵심 발견
- **$LogFile**: 타임스탬프 조작 탐지에 가장 신뢰할 수 있는 아티팩트; 기록 삭제 어려움
- **$USNjrnl**: 파일 변경 추적에 유용; 단, 순환 버퍼로 오래된 기록 덮어쓰기
- **Prefetch files**: 프로그램 실행 증거 제공; 타임스탬핑 도구 실행 흔적 남김
- **Windows Event Log**: 부가적 맥락 제공; 타임스탬프 조작 직접 탐지 한계
- SetMACE(v1.0.0.5)는 FNA 타임스탬프를 변경하지 않음 → SI/FN 불일치 탐지 가능
- nTimestomp: 나노초 0 설정 → 쉽게 식별 가능한 패턴
- Timestomp: SMFT 레코드 번호 기반 탐지 가능성

## 직접 분석한 아티팩트
[[artifacts/$LogFile]] · [[artifacts/$USNjrnl]] · [[artifacts/Prefetch]] · [[artifacts/Windows_Event_Log]]

## 영향받은 wiki 페이지
[[techniques/Timestamp_Manipulation_Detection]]
