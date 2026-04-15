---
technique: Teams_Differential_Forensics
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - Microsoft_Teams_Windows
  - Microsoft_Teams_Android
related_crime:
  - 업무 관련 범죄
  - 내부자 위협
  - 증거 확보
sources:
  - kim_2021
updated: 2026-04-15
---

## 개요
Microsoft Teams Windows/Android 환경의 아티팩트를 25개 사용자 행위별로 비교 분석하는 기법. Differential Forensics(행위 전·후 아티팩트 diff)를 적용하여 어떤 파일이 어떤 행위에 의해 변화하는지 체계화. 단일 플랫폼 한계를 복합 사용으로 보완하여 탐지율 92% 달성.

## 메커니즘

### Differential Forensics 절차
1. 기준 이미지(행위 전) 수집
2. 특정 행위(메시지 전송, 파일 공유, 채널 생성 등) 수행
3. 변경 이미지(행위 후) 수집
4. diff 분석 → 행위에 의해 변화한 파일/레코드 특정

### 플랫폼별 아티팩트 위치
- **Windows**: `C:\Users\%USERPROFILE%\AppData\Roaming\Microsoft\Teams`
  - IndexedDB, Local Storage, Cache, leveldb logs
- **Android**: `/data/data/com.microsoft.teams/`
  - `SkypeTeams.db`: User, Message, MessagePropertyAttribute 테이블

## 탐지율 비교 (25개 행위)
| 플랫폼 | 탐지 가능 행위 수 | 탐지율 |
|--------|-----------------|--------|
| Windows 단독 | 18/25 | 72% |
| Android 단독 | 21/25 | 84% |
| 복합 사용 | 23/25 | **92%** |

## 복구 가능 정보
- 삭제된 채팅: Android DB에 일정 기간 잔류
- 파일 공유 이력: Cache 및 IndexedDB 내 메타데이터
- 사용자 정보: mri(Microsoft Resource Identifier), displayName

## 한계 & 주의사항
- Android SM-G970N · Windows 10 특정 환경 검증
- Teams 업데이트에 따른 경로·스키마 변경 가능
- 2/25 행위는 복합 사용에서도 탐지 불가

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kim_2021]] | Differential Forensics 기법 적용; 25개 행위 탐지율; Android DB 삭제 잔류 확인 |

## 관련 페이지
[[artifacts/Microsoft_Teams_Windows]] · [[artifacts/Microsoft_Teams_Android]] · [[techniques/Slack_Discord_IM_Forensics]]
