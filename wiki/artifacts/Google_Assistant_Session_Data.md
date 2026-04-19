---
artifact: Google Assistant Session Data
layer: APP
data_types:
  - Google Assistant 음성 세션 (mp3)
  - 사용자 음성 쿼리
  - Google Assistant 응답 음성
  - 세션 시각 (MAC time)
action_types:
  - 음성 명령 발화
  - Google Assistant 호출
  - 위치 검색 / 내비게이션 요청
os_version: Android 8.1+
path: |
  /data/data/com.google.android.googlequicksearchbox/app_session/*.binarypb
attck: —
sources:
  - hickman_2019
updated: 2026-04-19
---

## 개요
Android Auto와 연동된 Google Assistant의 각 세션이 `.binarypb` 파일로 저장됨. 파일 내부에 mp3 오디오 데이터가 내장되어 있으며, 포렌식 카빙으로 Google Assistant 응답 음성 및 사용자 음성 쿼리를 복구 가능.

## 구조
- 파일: `app_session/{숫자}.binarypb` (각 파일 = 하나의 Google Assistant 세션)
- 내용:
  - ASCII 문자열 `"car_assistant"`: Android Auto에서 호출된 세션임을 식별
  - 바이트 `0x5951EF`: car_assistant 마커와 동일 오프셋에 동반 등장
  - mp3 프레임 데이터 (LAME 3.99.5 인코딩, sync header `0xFFF344C4` = "yoda")
  - 사용자 쿼리의 복수 버전/해석 문자열 (scoring용으로 추정)

## 추출 방법
- 루팅 기기에서 물리 추출 후 `/data/data/com.google.android.googlequicksearchbox/app_session/` 접근
- mp3 카빙: 첫 번째 yoda(`0xFFF344C4`) ~ 마지막 yoda 구간을 바이너리 추출 → `.mp3`로 저장 후 VLC 재생
- 세션 시각 특정: 각 `.binarypb` 파일의 MAC time을 수사 노트와 대조

## 분석 포인트
- `"car_assistant"` 문자열 존재 → Android Auto를 통한 Google Assistant 호출 증명
- 카빙된 mp3 파일: 마지막 Google Assistant 응답 음성 복구 (예: "Starbucks is ten minutes from your location by car and light traffic.")
- 파일 내 사용자 발화 문자열: 음성 명령 내용 텍스트로 확인 가능
- MAC time 기반 세션 식별: 수사관이 특정 일시의 내비게이션 요청을 파일과 매핑 가능

## 한계
- Google Assistant의 비-Android Auto 호출 세션 파일도 동일 폴더에 혼재 → `car_assistant` 마커로 Android Auto 세션만 필터링 필요
- 모든 세션이 파일로 저장되는 것은 아닐 수 있음 (파일 보존 정책 미확인)

## Findings
- `.binarypb` 파일에서 LAME mp3 카빙으로 Google Assistant 응답 음성 원문 복구 가능 (→ [[papers/hickman_2019]])
- `"car_assistant"` ASCII 마커로 Android Auto 경유 호출 세션 식별 가능 (→ [[papers/hickman_2019]])
- 파일 MAC time + 수사 노트 대조로 특정 세션의 발화 시각 특정 가능 (→ [[papers/hickman_2019]])
- 사용자 실제 발화 쿼리가 텍스트 문자열로도 파일 내 잔존 (→ [[papers/hickman_2019]])

## 관련 페이지
[[artifacts/Android_Auto_App_Data]] · [[techniques/Android_Auto_Forensics]]
