---
paper: dolangavitt_2008
title: "Forensic Analysis of the Windows Registry in Memory (2008)"
full_title: "Forensic analysis of the Windows registry in memory"
authors: "Dolan-Gavitt, B."
year: 2008
venue: DFRWS 2008 USA (Digital Investigation 5)
doi: https://doi.org/10.1016/j.diin.2008.05.003
tags:
  - Windows Registry
  - memory forensics
  - volatile memory
  - Volatility
  - cached data
  - registry in-memory
method: 실험 (4개 메모리 이미지 비교, Volatility 플러그인 구현)
limitations: "Windows XP SP2 대상 실험; 64-bit 아키텍처 미검증; 하이브 주소 탐색에 휴리스틱 의존"
---

## 한 줄 요약
물리 메모리에서 Windows 레지스트리를 추출하는 기법 제시 — 디스크에 없는 휘발성 키를 포함해 더 많은 레지스트리 데이터 복구 가능.

## 핵심 발견
- 메모리의 레지스트리 구조는 디스크와 동일한 하이브 포맷 사용 (cell index 기반 주소 변환만 다름)
- **Volatile Keys**: 디스크에 저장되지 않고 부팅 시 OS가 자동 생성 (현재 연결된 하드웨어, 마운트된 볼륨, 머신 이름 등)
- 디스크 분석 대비 메모리에서 평균 631개 키, 1231개 값 추가 발견
- **보안 위협**: 커널 접근 권한을 가진 공격자가 캐시된 키를 수정하면 디스크 포렌식으로 탐지 불가 → 메모리 포렌식으로만 탐지 가능
- _CM_KEY_NODE 구조에서 stable(디스크) + volatile(메모리 전용) 두 배열로 서브키 관리
- Volatility 플러그인으로 구현; vtypes.py에 데이터 구조 정의

## 직접 분석한 아티팩트
[[artifacts/Windows_Registry_Memory]]

## 영향받은 wiki 페이지
[[techniques/Registry_Memory_Forensics]]
