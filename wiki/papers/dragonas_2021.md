---
paper: dragonas_2021
title: "Forensic Analysis of Xiaomi IoT Ecosystem (2021)"
full_title: "Forensic Analysis of Xiaomi IoT Ecosystem"
authors: "Dragonas, P.; Petrov, T.; Mylonakis, G.; Katos, V."
year: 2021
venue: DFRWS EU 2021
doi: —
tags:
  - IoT forensics
  - Xiaomi
  - MiHome
  - Android
  - ZigBee
  - smart home
method: 실험
limitations: "루팅된 기기 필요(Magisk); Redmi Note 6 Pro 단일 모델; 클라우드 데이터 미수집"
---

## 한 줄 요약
Android Mi Home 앱과 Xiaomi ZigBee IoT 기기(모션·도어 센서, 허브, 스위치)를 연결한 스마트홈 포렌식 아티팩트 식별.

## 핵심 발견
- `miio.db`: 계정 이메일, 닉네임, 전화번호, 사용자 ID, 공유 타임스탬프 포함
- `home_roomv_manager_sp_.xml`: 스마트홈 구조(방 목록, 기기 배치) 복원 가능
- `MD5(UserID)scene_list_cache.xml`: 자동화 씬(scene) 설정 + 타임스탬프 → 사용 패턴 재구성
- `model_list.json`: 연결된 IoT 기기 모델 정보
- `config.xml` (기기별 디렉토리): 기기 설정·로그
- 항공기 모드 ON + ADB 추출 → 캐시 초기화 없이 데이터 보존

## 직접 분석한 아티팩트
[[artifacts/Xiaomi_MiHome_App_Data]]

## 영향받은 wiki 페이지
[[techniques/IoT_Ecosystem_Forensics_Xiaomi]]
