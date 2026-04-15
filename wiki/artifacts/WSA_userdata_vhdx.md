---
artifact: WSA_userdata_vhdx
layer: OS
data_types:
  - Android 앱 데이터 전체
  - 설치된 앱 목록
  - 앱별 DB·캐시
  - 사용 통계 (UsageStats)
action_types:
  - WSA 앱 설치
  - WSA 앱 실행
  - WSA 데이터 생성·수정
os_version: Windows 11
path: "%UserProfile%\\AppData\\Local\\Packages\\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\\LocalCache\\userdata.vhdx"
attck:
sources:
  - kim_2022a
updated: 2026-04-15
---

## 개요
Windows 11의 WSA(Windows Subsystem for Android)가 Android 데이터를 저장하는 가상 디스크 이미지(VHDX). Android의 `/data` 파티션 전체가 이 파일 내에 보존되어 모든 Android 앱 데이터를 포렌식할 수 있다.

## 추출 방법
- 경로: `%UserProfile%\AppData\Local\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalCache\userdata.vhdx`
- VHDX 마운트(Windows 디스크 관리 또는 7-Zip) 후 내부 Android 파티션 분석
- ADB (127.0.0.1:58526): WSA 실행 중 Android 쉘 접속
- SMB 서버: WSA 설정에서 활성화 시 파일 공유 접근 가능
- $UsnJrnl에 userdata.vhdx 변경 이력 기록 → 타임라인 재구성 가능

## 분석 포인트
- VHDX 내부: Android Ext4 파일시스템 → `/data/data/[package]/` 구조
- **UsageStats**: 앱 사용 시간·빈도 통계 (수집 도구 필요)
- **설정 디렉토리** (LocalState): `settings.dat`, Icons, `appcompatdb.json`
- Windows 아티팩트(Prefetch, AmCache, UserAssist, SRUM)와 교차 분석으로 Android 앱 사용 타임라인 구성

## Findings
- userdata.vhdx 내 Android 앱 DB 전체 접근 가능 확인 (→ [[kim_2022a]])
- $UsnJrnl에 userdata.vhdx 변경 이력 기록 → 타임라인 재구성 활용 (→ [[kim_2022a]])
- 커스텀 CLI 수집 도구로 자동화 수집 가능성 제시 (→ [[kim_2022a]])

## 관련 페이지
[[artifacts/$USNjrnl]] · [[artifacts/SRUM]] · [[artifacts/AmCache]] · [[artifacts/UserAssist]] · [[techniques/WSA_Forensics]] · [[papers/kim_2022a]]
