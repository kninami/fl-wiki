---
technique: User_Attribution_Framework
perspective: Investigator
topic: User Attribution
related_artifacts:
  - Windows_Registry_Memory
  - $LogFile
  - UserAssist
  - Jumplist
related_crime:
  - 파일 사용 행위 입증
  - 내부자 위협
  - 지식재산권 침해
sources:
  - yun_2015
updated: 2026-04-15
---

## 개요
디지털 파일의 사용 이력을 5가지 CASE로 분류하고 각 유형별 아티팩트 흔적 조합으로 사용자 인지도(해당 파일 사용 사실 인식 여부)를 추정하는 방법론. 한국 전문법칙 적용 쟁점도 검토.

## 메커니즘

### CASE 분류 체계
| CASE | 설명 | 주요 흔적 |
|------|------|-----------|
| CASE 1 | 해당 기기에서 생성·활용 | $MFT 타임스탬프, LNK, UserAssist |
| CASE 2 | 생성 후 외부로 유출 | 이메일 첨부, 클라우드 업로드 기록 |
| CASE 3 | 외부 유입 후 단순 열람 | LNK 파일, Internet History, MRU |
| CASE 4 | 외부 유입 후 수정·활용 | $MFT 수정 시각, 앱 사용 로그 |
| CASE 5 | 삭제 후 흔적 없음 | (복원 불가 상황) |

### 사용자 인지도 판단 지표 (Table 6)
- **파일사용 관련 아티팩트**: Registry(USB 연결, UserAssist, MRU, ShellBag), LNK 파일(실행 횟수·경로·시각), Internet History/다운로드, 이메일 첨부 기록, 스마트폰 연결 흔적, NTFS $MFT 타임스탬프
- **위치추적 관련 정보**: 외부 서비스 기록, 제3자 보유 정보(통신사·SNS·클라우드)
- 두 정보 교차 분석으로 "인지도 추정" 신뢰도 향상

### 타임스탬프 기반 이동 경로 추적
- 파일 생성·수정·접근 시간 비교 → 기기 간 파일 이동 순서 재구성
- 이동 횟수·경로가 CASE 분류 기준이 됨

## 한계 & 주의사항
- 이론·사례 논문 — 실험 검증 미포함
- 한국 전문법칙(대법원 판례 3건) 기준 논의 → 다른 법체계에서 적용 시 주의
- CASE 5는 복원 불가 — 선제적 증거보존 중요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/yun_2015]] | 파일 사용 이력 5-CASE 분류; 사용자 인지도 판단지표(Table 6); 한국 전문법칙 적용 쟁점 |

## 관련 페이지
[[techniques/Automated_User_Analysis]] · [[artifacts/Windows_Registry_Memory]] · [[artifacts/$LogFile]]
