---
paper: qasim_2020
title: "Control Logic Forensics Framework using Built-in Decompiler of Engineering Software in ICS (2020)"
full_title: "Control Logic Forensics Framework using Built-in Decompiler of Engineering Software in Industrial Control Systems"
authors: "Qasim, Syed Ali; Smith, Jared M.; Ahmed, Irfan"
year: 2020
venue: DFRWS USA 2020 / Forensic Science International: Digital Investigation 33 (2020) 301013
doi: https://doi.org/10.1016/j.fsidi.2020.301013
tags:
  - ICS
  - PLC
  - control logic injection
  - SCADA
  - network forensics
  - Modbus
method: 실험 (Schneider Electric Modicon M221 + SoMachine Basic, 40개 제어 로직 프로그램)
limitations: "SoMachine Basic + Modbus 프로토콜에 특화; 다른 ICS 프로토콜/벤더로 일반화 미검증"
---

## 한 줄 요약
ICS 네트워크 트래픽(PCAP)에서 엔지니어링 소프트웨어의 내장 디컴파일러를 활용해 PLC 제어 로직을 자동 복구하는 Reditus 프레임워크 제안 및 100% 전송 정확도 달성.

## 핵심 발견
- **Reditus**: 가상 PLC가 엔지니어링 소프트웨어와 통신, PCAP에서 제어 로직을 upload 함수로 자동 추출·디컴파일
- 학습 단계: 세션 종속 필드를 다중 PCAP 차분 분석으로 자동 발견 (수동 역공학 불필요)
- 평가: 40개 제어 로직 프로그램, 213 rung, 888 instruction — 100% 매칭/템플릿/전송 정확도
- 핵심 포렌식 아티팩트: ICS 네트워크 트래픽(PCAP) — 제어 로직 바이너리가 네트워크를 통해 전송되므로 캡처된 PCAP이 증거
- 기존 도구(Laddis, Eupheus, Similo) 대비 수동 역공학 없이 완전 자동화 가능
- Stuxnet류 공격에서 네트워크 트래픽만으로 악성 제어 로직 원본 복구 가능

## 직접 분석한 아티팩트
[[artifacts/ICS_PLC_Network_Traffic]]

## 영향받은 wiki 페이지
[[techniques/ICS_Control_Logic_Forensics]]
