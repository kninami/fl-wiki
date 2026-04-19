---
technique: Android_Log_Evidence_Extraction
perspective: Investigator
topic: Extraction/Analysis
attck: —
related_artifacts:
  - Android_System_Log
related_crime:
  - 모바일 기기 범죄 수사
  - PII 유출 수사
sources:
  - cheng_2021
updated: 2026-04-20
---

## 개요
Android APK를 정적 분석(String + Taint Analysis)해 앱별 로그 증거 DFA를 자동 생성하고, 수사관이 ADB로 추출한 Android system log에서 개인식별정보(PII)를 자동 추출하는 기법. LogExtractor 도구가 구현체.

## 메커니즘
1. APK 디컴파일 → 코드 추출
2. String Analysis: 로그 출력 구조 파악 (형식 문자열, 태그)
3. Taint Analysis: PII 소스(IMEI, 전화번호 등) → Log sink 경로 추적
4. ALED(App Log Evidence Database): 앱별 Tainted DFA 저장
5. 수사관 제공: 기기 ADB 로그 → DFA 매칭 → PII 값 자동 추출

## 탐지 방법 (Investigator)
- `adb logcat -d`로 로그 버퍼 덤프 → ALED에 매칭
- APK가 없을 경우 구글 플레이 스토어에서 동일 버전 다운로드 후 분석
- DFA 매칭 실패 시 앱 버전 불일치 의심 → 버전별 APK 재분석

## 한계 & 주의사항
- ICC(Inter-Component Communication) 흐름은 정적 분석으로 추적 어려움 → FN 발생
- 암묵적(implicit) 데이터 흐름 미처리
- APK 난독화 시 분석 정확도 저하
- 실시간 로그만 캡처 가능 — 이미 삭제된 과거 로그는 복구 어려움

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[cheng_2021]] | 22,700개 앱의 42.1% Unique ID 흐름이 로그로 유출; 식별 precision 97.7%, recall 79.2%; DFA 정확 시 추출 precision 100% |

## 관련 페이지
[[artifacts/Android_System_Log]] · [[papers/cheng_2021]]
