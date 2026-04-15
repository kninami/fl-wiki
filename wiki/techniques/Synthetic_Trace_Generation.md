---
technique: Synthetic Forensic Trace Generation
perspective: Both
topic: Research Methodology
attck: —
related_artifacts: []
related_crime: []
sources:
  - schmidt_2023
updated: 2026-04-15
---

## 개요
실제 포렌식 훈련·도구 검증·알고리즘 개발을 위한 합성 데이터셋을 생성하는 기법. 실제 증거 공유의 법적·윤리적 제약을 극복하고, 현실적인 사용자 행위 흔적을 인공적으로 생성.

## 메커니즘
컴퓨터 비전 기반 GUI 자동화 접근법:

**기존 방식의 한계:**
- Guest Agent 사용 → 에이전트 설치 아티팩트 오염
- GUI 상호작용 흔적 누락 (마우스 클릭, 키보드 입력)
- 운영체제 의존적

**pyautogemu 방식:**
1. QEMU/KVM 하이퍼바이저에서 QMP 프로토콜로 가상머신 제어
2. OpenCV 템플릿 매칭으로 GUI 요소 인식 (버튼, 아이콘 등)
3. HID(Human Interface Device) 주입으로 마우스/키보드 에뮬레이션
4. Guest Agent 불필요 → 하이퍼바이저 외부에서 제어
5. 트레이스 추출: Plaso(log2timeline)로 winreg, lnk, sqlite, winevtx, prefetch 파서 적용
6. 차별 분석으로 생성된 트레이스 품질 평가

## 활용 방법

**포렌식 연구자:**
- 훈련 데이터셋 생성 (ML 모델 학습용)
- 도구 검증 데이터셋 구축 (CFReDS 대체)
- 반복 가능한 실험 환경 확보

**포렌식 교육자:**
- 실습용 시나리오 데이터셋 자동 생성
- 다양한 OS/앱 조합 데이터셋

## 한계 & 주의사항
- 기술적 현실성(technical realism)에 집중; 사회학적 현실성은 별도
- 프로그램 실행 중 비-GUI 상호작용 일부 누락 가능
- 현재 Windows 중심; macOS/Linux 확장 필요
- 생성된 트레이스가 실제 데이터와 동등한지 지속 검증 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/schmidt_2023]] | 컴퓨터 비전 기반 GUI 자동화로 기존 방식보다 많고 다양한 트레이스 생성; pyautogemu 오픈소스 |

## 관련 페이지
[[techniques/DFIR_Tool_Error_Mitigation]] · [[techniques/Artifact_Knowledge_Management]]
