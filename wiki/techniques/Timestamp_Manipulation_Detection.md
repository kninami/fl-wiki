---
technique: Timestamp Manipulation Detection
perspective: Investigator
topic: Detection
attck: T1070.006
related_artifacts:
  - $LogFile
  - $USNjrnl
  - Prefetch
  - Windows_Event_Log
related_crime:
  - 증거인멸
  - 알리바이 조작
  - 사이버범죄 수사
sources:
  - palmbach_2020
updated: 2026-04-15
---

## 개요
NTFS 타임스탬프 조작(Timestomping)을 탐지하기 위해 $LogFile, $USNjrnl, Prefetch, Windows Event Log를 교차 검증하는 방법론. 각 아티팩트의 신뢰도와 한계를 이해하고 조합해야 효과적인 탐지 가능.

## 메커니즘 (공격자 관점)
타임스탬핑 도구들은 $MFT의 $STANDARD_INFORMATION 속성 타임스탬프를 수정:
- **SetMACE**: Windows API 사용; FNA($FILE_NAME) 타임스탬프는 수정하지 않음
- **nTimestomp**: 나노초 부분을 0으로 설정; 64-bit 전체 값 수정
- **Timestomp**: SMFT 레코드 번호 기반 탐지 가능성; 구버전 SetMACE 기반

일반적인 Windows 파일 연산에서는 $STANDARD_INFORMATION과 $FILE_NAME 타임스탬프가 일치 → 불일치 = 조작 징후

## 탐지 방법

### 1. $LogFile 분석 (가장 신뢰)
- 타임스탬프 변경 트랜잭션 레코드 확인
- 타임스탬핑 도구 실행 후 $LogFile에 기록 남음
- 도구: NTFS LogFile Parser

### 2. $USNjrnl 분석
- 파일 변경 레코드에서 타임스탬프 변경 이벤트 확인
- Reason 플래그에서 `DATA_EXTEND`, `CLOSE`, `SECURITY_CHANGE` 등 확인
- 한계: 순환 버퍼로 오래된 기록 덮어쓰기

### 3. Prefetch 분석
- 타임스탬핑 도구(SetMACE.exe, nTimestomp.exe 등)의 .pf 파일 존재 여부
- 도구 실행 시각과 타임스탬프 변경 시각 비교

### 4. Windows Event Log 분석
- 보안 감사 활성화 시 도구 실행 이벤트 기록 가능
- 단독으로 직접 탐지는 한계 → 보조 수단

## 한계 & 주의사항
- 4개 아티팩트 모두 active exploitation에 완전히 저항하지 못함 (→ palmbach_2020)
- 순환 버퍼로 인한 기록 손실: 오래된 조작은 $USNjrnl에서 사라질 수 있음
- Windows 10 실험 결과이므로 다른 버전에서 동작 차이 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/palmbach_2020]] | $LogFile이 가장 신뢰, 4개 아티팩트 모두 active exploitation에 완전 저항 불가 |

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/$USNjrnl]] · [[artifacts/Prefetch]] · [[artifacts/Windows_Event_Log]]
