---
technique: Selective Seizure Evaluation
perspective: Investigator
topic: Analysis
attck: —
related_artifacts:
  - $LogFile
  - Windows Event Log
related_crime:
  - 현장 선별 압수
  - 디지털 증거 수집
sources:
  - kim_2026
updated: 2026-04-15
---

## 개요
현장 라이브 환경에서 관련 디지털 증거만 선별적으로 압수하기 위한 Windows 포렌식 도구 평가 모델. 불필요한 데이터 수집을 최소화하면서 절차적 적법성과 수사 효율성을 보장.

## 메커니즘
3단계 선별 압수 평가 모델:

**1단계: 검색(Search)**
- NTFS $MFT 파싱 및 메타데이터 분석
- 타임스탬프 로그($LogFile, 이벤트 로그) 분석
- Windows 아티팩트(레지스트리, LNK 파일 등) 인식

**2단계: 선택(Selection)**
- 파일 필터링 (확장자, 날짜, 크기, 키워드)
- 암호화 파일 처리 (식별·목록화)
- 파일 미리보기 (내용 확인 후 선별)
- 검색 키워드 매칭

**3단계: 압수(Seizure)**
- 논리 이미지 생성 (파일 레벨 수집)
- 무결성 검증 (해시 생성)
- 보고서 자동 생성

## 탐지 방법 (Investigator)
**도구 선택 기준:**
- 검색 단계: NTFS 파싱 정확도, 타임스탬프 로그 분석 지원 여부
- 선택 단계: 필터 조건의 다양성, 암호화 파일 처리, 미리보기 지원
- 압수 단계: 무결성 검증, 보고서 생성, 증거 수집 표준 준수

**평가 결과 요약 (6종 도구 비교):**
- 단일 도구로 모든 요건 충족 불가 → 상황별 조합 필요
- 검색 단계: NTFS 파싱 도구별 편차 큼
- 압수 단계: 무결성·보고서 요건 충족 도구 소수

## 한계 & 주의사항
- Windows 10 NTFS 환경 한정; macOS/Linux 비적용
- 삭제 데이터 복구·파일 카빙은 평가 범위 제외
- 클라우드·가상화·메시징 플랫폼 아티팩트 제외
- 도구 버전 업데이트로 평가 결과 변동 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kim_2026]] | 3단계 평가 모델 수립; 6종 도구 평가; 단일 도구 한계 확인; ISO/IEC·NIST 기준 검증 |

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/Windows_Event_Log]] · [[techniques/DFIR_Tool_Error_Mitigation]]
