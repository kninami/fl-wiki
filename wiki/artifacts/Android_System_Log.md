---
artifact: Android_System_Log
layer: OS
data_types:
  - Log message
  - PII (Unique Identifier)
  - App event
  - Timestamp
action_types:
  - 앱 실행
  - 개인정보 접근
  - 네트워크 통신
os_version: Android (ADB 접근 가능 환경)
path: "adb logcat / /data/log/"
sources:
  - cheng_2021
updated: 2026-04-20
---

## 개요
Android 시스템 및 앱이 생성하는 런타임 로그. ADB `logcat` 명령으로 실시간 수집하거나 기기 내 로그 파일로 저장된 것을 추출한다. 앱이 디버그 목적으로 출력한 개인식별정보(PII), 기기 식별자, 네트워크 주소 등을 포함할 수 있다.

## 추출 방법
- **ADB logcat**: `adb logcat -d > device_log.txt` — 현재 로그 버퍼 덤프
- **영구 로그 파일**: `/data/log/` 또는 앱별 로그 디렉토리 (루팅 필요)
- 수집 전 항공기 모드 ON → 진행 중인 통신 로그 변조 방지

## 분석 포인트
- 앱별 태그(TAG)로 필터링하여 특정 앱의 로그만 추출
- Unique Identifier(IMEI, Android ID, 전화번호 등) 패턴 검색
- 로그 타임스탬프로 사용자 행위 타임라인 재구성
- LogExtractor(cheng_2021) 등 도구: APK 정적 분석으로 생성된 DFA를 로그에 매칭 → 자동 PII 추출

## Findings
- 22,700개 앱 분석 결과 Unique Identifier 흐름의 42.1%가 Log sink로 유입 → Android 로그는 주요 PII 증거 소스 (→ [[cheng_2021]])
- LogExtractor: DFA 정확 시 PII 추출 precision 100%; 식별 precision 97.7%, recall 79.2% (→ [[cheng_2021]])
- ICC(Inter-Component Communication) 흐름은 정적 분석으로 추적 어려움 → 7개 FN 발생 (→ [[cheng_2021]])

## 관련 페이지
[[techniques/Android_Log_Evidence_Extraction]] · [[papers/cheng_2021]]
