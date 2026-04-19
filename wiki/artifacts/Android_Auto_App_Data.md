---
artifact: Android Auto App Data
layer: APP
data_types:
  - 마지막 연결 해제 위치 (위도·경도)
  - 연결된 차량 Bluetooth MAC 주소
  - 마지막 실행 시각
  - Google Analytics Client ID (UUID)
  - 날씨 카드 URL 캐시
action_types:
  - 차량 연결/해제
  - 위치 기록
  - 앱 실행
os_version: Android 8.1+
path: |
  /data/data/com.google.android.projection.gearhead/
    shared_prefs/location_manager.xml          (위치·차량명·해제 시각)
    shared_prefs/common_user_settings.xml      (차량 Bluetooth MAC)
    shared_prefs/app_state_shared_preferences.xml  (마지막 실행 시각, pref_lastrun)
    databases/CloudCards.db                    (날씨 카드 URL)
    databases/google_analytics_v4_6308.db      (Analytics Client ID, Tracking ID)
    files/gaClientId                           (Google Analytics UUID)
attck: —
sources:
  - hickman_2019
updated: 2026-04-19
---

## 개요
Android Auto 앱(`com.google.android.projection.gearhead`)이 `/data/data`에 남기는 로컬 데이터. 앱 자체의 사용 증거는 제한적이나, `shared_prefs` 하위 XML 파일에 차량 연결 이력과 마지막 위치 정보가 저장됨. 루팅 기기에서 물리 추출 필요.

## 구조

### shared_prefs/location_manager.xml
- 차량 제조사명 (예: "Nissan")
- 마지막 차량 연결 해제 위치: 위도·경도 (실험상 ~3m 이내 정확도)
- 마지막 연결 해제 시각 (Unix Epoch Time)

### shared_prefs/common_user_settings.xml
- 연결된 차량의 Bluetooth MAC 주소 (단일 항목; 다중 차량 여부 미확인)

### shared_prefs/app_state_shared_preferences.xml
- `pref_lastrun`: 앱 마지막 실행 시각 (Unix Epoch Time)

### databases/
- `CloudCards.db`: 홈 화면 날씨 카드용 PNG URL (단일 항목)
- `google_analytics_v4_6308.db`: Google Analytics Client ID + Tracking ID + 앱 버전

### files/gaClientId
- 임의 생성 UUID v4 (기기를 익명 식별하는 Google Analytics 클라이언트 ID)

## 추출 방법
- 루팅 기기: 물리 추출(Cellebrite UFED 4PC 등) 후 `/data/data` 경로 직접 접근
- 비루팅 기기: `/data/data`는 adb backup으로 일부만 접근 가능 (앱 백업 허용 여부 따라 다름)
- 도구: WinHex (파일 뷰어), DCode (Unix Epoch 변환), Cellebrite UFED 4PC

## 분석 포인트
- `location_manager.xml`의 위도·경도로 피의자 차량 마지막 주차 위치 특정 가능
- `common_user_settings.xml`의 Bluetooth MAC으로 특정 차량과 기기 연결 관계 확립
- `pref_lastrun`으로 Android Auto 마지막 사용 시각 특정

## Findings
- `location_manager.xml`에 마지막 차량 연결 해제 위치(~3m 이내 정확도) + 시각 + 차량명 저장 (→ [[papers/hickman_2019]])
- `common_user_settings.xml`에 연결 차량의 Bluetooth MAC 주소 저장 (→ [[papers/hickman_2019]])
- Android Auto 앱 폴더 자체에는 Maps·음성 명령 등 직접 사용 증거 거의 없음 — Google Assistant 앱 폴더를 함께 분석해야 함 (→ [[papers/hickman_2019]])

## 관련 페이지
[[artifacts/Google_Assistant_Session_Data]] · [[techniques/Android_Auto_Forensics]]
