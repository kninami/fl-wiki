---
paper: brown_2020
title: "Integrating GRR Rapid Response with Graylog Extended Log Format (2020)"
full_title: "Integrating GRR Rapid Response with Graylog Extended Log Format"
authors: "Brown, J.; Pan, Y."
year: 2020
venue: DFRWS USA 2020
doi: —
tags:
  - GRR
  - live forensics
  - GELF
  - Graylog
  - remote forensics
  - enterprise forensics
method: 시스템 구축
limitations: "소규모 랩 환경 검증; 실제 대규모 엔터프라이즈 배포 미검증"
---

## 한 줄 요약
GRR Rapid Response의 결과를 GELF JSON으로 변환해 Graylog로 전송하는 GELFOutputPlugin 구현 — 원격 라이브 포렌식 결과를 중앙 집중형으로 인덱싱·시각화.

## 핵심 발견
- GRR Output Plugin 3개 함수 구현: ProcessResponses, _MakePayload, _SendPayload
- Windows 10 환경: 악성 레지스트리 Run 키(HKCU/.../Run/Malware) — RegistryFinder Hunt → Graylog 시각화로 감염 호스트 식별
- Ubuntu 18.04 환경: bash 역방향 셸 — Netstat Flow → Graylog로 감염 호스트 실시간 추적
- GELF는 syslog의 payload 크기 제한 극복 + JSON 타입 지원 + 압축(GZIP/zlib) 지원

## 직접 분석한 아티팩트
Windows Registry (Run 키), 네트워크 연결 정보 — GRR 출력 데이터로 처리됨 (직접 아티팩트 포렌식 분석 아님)

## 영향받은 wiki 페이지
[[techniques/Remote_Live_Forensics_GRR]]
