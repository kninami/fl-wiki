---
artifact: Xiaomi_MiHome_App_Data
layer: APP
data_types:
  - Account info
  - Device inventory
  - Automation scene
  - Timestamp
  - Configuration log
action_types:
  - 스마트홈 기기 사용
  - 자동화 씬 실행
  - 기기 공유
os_version: Android (루팅 환경, Magisk)
path: "/data/data/com.xiaomi.smarthome/"
sources:
  - dragonas_2021
updated: 2026-04-20
---

## 개요
Xiaomi Mi Home(스마트홈) Android 앱의 내부 데이터. 연결된 ZigBee IoT 기기(모션·도어 센서, 허브, 무선 스위치)의 계정 정보, 기기 구성, 자동화 씬, 사용 로그를 포함한다.

## 추출 방법
- **루팅 필수**: Magisk 기반 루팅 기기에서 ADB로 내부 스토리지 접근
- **항공기 모드 ON**: 추출 전 항공기 모드 활성화 → 클라우드 동기화로 인한 데이터 변경 방지
- **캐시 초기화 금지**: 앱 캐시 삭제 없이 전체 앱 데이터 디렉토리 복사

## 분석 포인트

| 파일 경로 | 포함 데이터 |
|-----------|-------------|
| `databases/miio.db` | 계정 이메일, 닉네임, 전화번호, 사용자 ID, 기기 공유 타임스탬프 |
| `shared_prefs/home_roomv_manager_sp_.xml` | 스마트홈 구조(방 목록, 기기 배치) |
| `shared_prefs/MD5(UserID)scene_list_cache.xml` | 자동화 씬 설정 + 트리거 타임스탬프 |
| `files/plugin/install/rn/.../model_list.json` | 연결된 IoT 기기 모델 정보 |
| `files/data/lumi.XXXXXXX/data/config.xml` | 기기별 설정 및 이벤트 로그 |

## Findings
- `miio.db`에서 계정 소유자 신원(이메일·닉네임·전화)과 기기 공유 이력 복원 가능 (→ [[dragonas_2021]])
- 씬 타임스탬프로 자동화 기기 동작 시간대 재구성 → 피해자 생활 패턴·부재 시간 추론 가능 (→ [[dragonas_2021]])
- ZigBee 기기 이벤트는 클라우드에만 저장될 수 있음 → 루팅+로컬 추출만으로는 이벤트 로그 누락 가능 (→ [[dragonas_2021]])

## 관련 페이지
[[techniques/IoT_Ecosystem_Forensics_Xiaomi]] · [[papers/dragonas_2021]]
