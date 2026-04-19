---
technique: Android Auto Forensics
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - Android Auto App Data
  - Google Assistant Session Data
related_crime:
  - 차량 이동 경로 추적
  - 내비게이션 이력 수집
  - 음성 명령 복구
sources:
  - hickman_2019
updated: 2026-04-19
---

## 개요
Android Auto가 연결된 Android 기기에서 차량 연결 이력, 마지막 위치, Google Assistant 음성 세션을 포렌식으로 복구하는 방법론. 루팅 기기의 물리 추출을 전제로 함.

## 메커니즘
Android Auto는 차량의 인포테인먼트 시스템에 앱을 투영(projection)하는 방식으로 동작. 음성 명령은 Google Assistant(`com.google.android.googlequicksearchbox`)가 처리하며, 위치·연결 정보는 Android Auto 앱(`com.google.android.projection.gearhead`)의 `shared_prefs`에 저장됨.

## 수집 절차

### 1. 기기 획득
- 루팅 기기(TWRP + Magisk 등): Cellebrite UFED 4PC 등으로 물리 추출
- `/data/data` 접근 가능한 전체 파일시스템 이미지 확보

### 2. Android Auto 앱 데이터 분석
- `shared_prefs/location_manager.xml` → 차량명, 마지막 연결 해제 위도·경도, 시각
- `shared_prefs/common_user_settings.xml` → 연결 차량 Bluetooth MAC
- `shared_prefs/app_state_shared_preferences.xml` → 마지막 앱 실행 시각 (pref_lastrun, Unix Epoch)
- DCode 등으로 Unix Epoch Time 변환

### 3. Google Assistant 세션 복구
1. `/data/data/com.google.android.googlequicksearchbox/app_session/` 의 `.binarypb` 파일 목록 확인
2. 각 파일의 MAC time + 수사 노트로 Android Auto 사용 세션 파일 특정
3. `"car_assistant"` 문자열 검색으로 Android Auto 경유 세션 확인
4. mp3 카빙: `0xFFF344C4`("yoda") sync header 첫 위치 ~ 마지막 위치 구간 추출 → `.mp3`로 저장
5. VLC 등으로 재생 → Google Assistant 응답 음성 확인
6. 파일 내 사용자 발화 텍스트 문자열도 ASCII로 잔존

## 탐지 방법 (Investigator)
- `location_manager.xml`의 위도·경도 → Google Maps로 피의자 차량 위치 확인
- Bluetooth MAC 주소 → 특정 차량과 기기 간 연결 관계 법정 입증
- mp3 복구 음성 → 피의자 음성 쿼리 내용 (목적지, 검색어 등)

## 한계 & 주의사항
- 루팅 기기에서만 `/data/data` 완전 접근 가능; 비루팅 시 제한적
- Android Auto는 직접 Maps·메시지 이력을 저장하지 않음 — Maps 앱은 별도 분석 필요
- `.binarypb`에 Android Auto 외 Google Assistant 세션 혼재 → `car_assistant` 마커 필터 필수

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/hickman_2019]] | location_manager.xml 위치 복구; .binarypb mp3 카빙; car_assistant 마커 식별 |

## 관련 페이지
[[artifacts/Android_Auto_App_Data]] · [[artifacts/Google_Assistant_Session_Data]]
