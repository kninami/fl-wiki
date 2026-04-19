---
technique: IoT_Ecosystem_Forensics_Xiaomi
perspective: Investigator
topic: Extraction/Analysis
attck: —
related_artifacts:
  - Xiaomi_MiHome_App_Data
related_crime:
  - 스마트홈 관련 범죄
  - 스토킹·가정폭력
  - 침입·절도
sources:
  - dragonas_2021
updated: 2026-04-20
---

## 개요
Xiaomi Mi Home 앱 기반 스마트홈 IoT 생태계(ZigBee 기기)의 포렌식 아티팩트를 Android 기기에서 추출·분석하는 기법.

## 메커니즘
Xiaomi 스마트홈은 Android Mi Home 앱이 ZigBee 허브를 통해 모션 센서, 도어 센서, 스위치 등 기기를 제어한다. 앱 내부 데이터(`miio.db`, `shared_prefs`, `config.xml`)에 계정 정보, 기기 구성, 자동화 씬, 타임스탬프가 저장된다.

## 탐지 방법 (Investigator)
1. **기기 보존**: 압수 즉시 항공기 모드 ON → 클라우드 동기화 차단
2. **루팅 확인**: Magisk 등 루팅 상태 확인; 미루팅 시 앱 내부 스토리지 접근 불가
3. **ADB 추출**: `/data/data/com.xiaomi.smarthome/` 전체 복사
4. **DB 분석**: `miio.db` SQLite → 계정 소유자 신원 확인
5. **XML 파싱**: `home_roomv_manager_sp_.xml` → 기기 배치·방 구조 재구성
6. **씬 타임스탬프**: `scene_list_cache.xml` → 자동화 동작 시간대 분석
7. **클라우드 병행**: 로컬에 없는 기기 이벤트는 영장을 통한 Xiaomi 클라우드 요청 필요

## 한계 & 주의사항
- 비루팅 기기에서는 내부 스토리지 접근 불가 → 루팅 전제 기법
- ZigBee 기기 이벤트 로그는 클라우드에만 저장될 수 있음
- Xiaomi 앱 버전 업데이트에 따라 DB 스키마 변경 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[dragonas_2021]] | Mi Home 앱 5개 주요 아티팩트 위치 식별; 계정·씬·기기 정보 복원 가능성 확인 |

## 관련 페이지
[[artifacts/Xiaomi_MiHome_App_Data]] · [[papers/dragonas_2021]]
