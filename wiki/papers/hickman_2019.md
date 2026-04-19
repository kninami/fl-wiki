---
paper: hickman_2019
title: "Ka-Chow!!! Driving Android Auto (2019)"
full_title: "Ka-Chow!!! Driving Android Auto"
authors: "Hickman, Joshua"
year: 2019
venue: DFIR Community Research (2019)
doi: https://doi.org/10.21428/482a8d7a
tags:
  - Android Auto
  - mobile forensics
  - Google Assistant
  - vehicle forensics
  - binarypb
method: 실험 (실차 환경 + 물리 추출)
limitations: "단일 기기(Nexus 5X), 단일 차량(Nissan Rogue 2019), 루팅 필요"
---

## 한 줄 요약
Android Auto 앱이 연결된 Android 기기에서 차량 Bluetooth MAC, 마지막 연결 해제 위치(~3m 정확도), Google Assistant 음성 세션(mp3 내장 .binarypb)을 포렌식으로 복구 가능함을 실험 확인.

## 핵심 발견
- Android Auto 앱 자체(`com.google.android.projection.gearhead`)에는 직접 사용 증거 적음
- `shared_prefs/location_manager.xml`: 마지막 차량 연결 해제 시 위치(위도·경도, ~3m 이내) + 해제 시각 + 차량 제조사명
- `shared_prefs/common_user_settings.xml`: 연결된 차량의 Bluetooth MAC 주소
- `shared_prefs/app_state_shared_preferences.xml`: 마지막 실행 시각(Unix Epoch, "pref_lastrun")
- Google Assistant 세션 파일(`com.google.android.googlequicksearchbox/app_session/*.binarypb`): "car_assistant" 마커 + LAME 인코딩 mp3 데이터 내장 — Google Assistant 응답 음성 및 사용자 음성 쿼리 복구 가능
- .binarypb 파일 MAC time으로 세션 시각 특정 가능

## 직접 분석한 아티팩트
[[artifacts/Android_Auto_App_Data]] · [[artifacts/Google_Assistant_Session_Data]]

## 영향받은 wiki 페이지
[[techniques/Android_Auto_Forensics]]
