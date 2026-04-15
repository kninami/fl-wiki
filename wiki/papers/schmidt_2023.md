---
paper: schmidt_2023
title: "Improving Trace Synthesis by Utilizing Computer Vision for User Action Emulation (2023)"
full_title: "Improving trace synthesis by utilizing computer vision for user action emulation"
authors: "Schmidt, L.; Kortmann, S.; Hupperich, T."
year: 2023
venue: DFRWS USA 2023 (FSI Digital Investigation 45)
doi: https://doi.org/10.1016/j.fsidi.2023.301557
tags:
  - trace synthesis
  - synthetic dataset
  - computer vision
  - user emulation
  - forensic dataset
  - QEMU/KVM
method: 실험 (컴퓨터 비전 기반 GUI 자동화 + 차별 분석으로 트레이스 품질 평가)
limitations: "GUI 기반 앱에 적용 가능; 현재 Windows 중심; 프로그램 실행(비GUI) 행위 일부 누락 가능"
---

## 한 줄 요약
컴퓨터 비전(OpenCV 템플릿 매칭)과 HID 주입을 활용해 실제 사용자 행동과 유사한 합성 포렌식 데이터셋을 생성하는 pyautogemu 프레임워크 개발.

## 핵심 발견
- 기존 프레임워크(ForGe, Forensig, ForTrace 등)는 Guest Agent 기반 → 비실제적 트레이스 생성(설치 아티팩트, 비현실적 GUI 흔적)
- 제안 방식: 하이퍼바이저에서 QMP+QEMU/KVM으로 외부 HID 입력 제어 → Guest Agent 불필요 → OS 독립적
- OpenCV 템플릿 매칭으로 GUI 요소 인식, 사람처럼 마우스/키보드 조작
- Plaso(log2timeline)로 트레이스 추출: winreg, lnk, sqlite, winevtx, prefetch 파서 활용
- 실험 결과: 기존 방식보다 더 다양하고 많은 트레이스 생성; 프로그램 실행 외 GUI 상호작용 흔적까지 포함
- pyautogemu: 오픈소스 Python 패키지 (OpenCV + QMP client)

## 직접 분석한 아티팩트
없음 (데이터셋 생성 방법론 논문)

## 영향받은 wiki 페이지
[[techniques/Synthetic_Trace_Generation]]
