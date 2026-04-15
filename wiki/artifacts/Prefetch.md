---
artifact: Prefetch
layer: OS
data_types:
  - 실행 파일 경로
  - 실행 횟수
  - 마지막 실행 시간 (최대 8회)
  - 로드된 DLL 목록
  - 파일 생성 시각
action_types:
  - 프로그램 실행
  - 시스템 부팅
os_version: Windows XP+ (기본 활성화; 서버 버전은 기본 비활성화)
path: "C:\\Windows\\Prefetch\\*.pf"
attck: —
sources:
  - palmbach_2020
  - joo_2022
  - kim_2017
  - kim_2022a
updated: 2026-04-15
---

## 개요
Windows Prefetch는 프로그램 로딩 속도 향상을 위해 실행된 프로그램의 메타데이터를 캐시하는 메커니즘. 각 실행 파일마다 .pf 파일 생성. 파일명은 `실행파일명-해시값.pf` 형식. 포렌식에서는 프로그램 실행 증거 제공에 핵심적 역할.

## 추출 방법
- 경로: C:\\Windows\\Prefetch\\
- 도구: PECmd, WinPrefetchView, Autopsy, X-Ways
- 파일명의 8자리 해시값은 실행 파일 경로 기반 (드라이브 레터 포함)

## 분석 포인트
- 파일 생성 시각 = 최초 실행 시각; 파일 수정 시각 = 마지막 실행 시각
- 실행 횟수 카운터로 반복 실행 여부 파악
- 최대 8개 마지막 실행 시각 기록 (Windows 8+)
- 로드된 모듈 목록으로 악성코드 행위 분석 가능

## Findings
- 타임스탬핑 도구(SetMACE, nTimestomp, Timestomp) 실행 흔적이 Prefetch 파일로 남음 → 도구 사용 증거 (→ [[papers/palmbach_2020]])
- Prefetch 파일 자체는 조작 도구 실행 후에도 내용 변경 어려움 (→ [[papers/palmbach_2020]])
- Plaso(log2timeline)의 prefetch 파서로 합성 데이터셋 추출 가능 (→ [[papers/schmidt_2023]])
- 파일 와이핑 도구 5종 실행 후 Prefetch에 실행 흔적 잔류 확인 (→ [[papers/joo_2022]])
- Windows 아티팩트 자동 사용자 행위 분석 시 Prefetch를 핵심 수집 대상으로 활용 (→ [[papers/kim_2017]])
- WSA 환경에서 WSACLIENT.EXE 등 WSA 관련 프로세스 실행 이력이 Prefetch에 기록됨 (→ [[papers/kim_2022a]])

## 관련 페이지
[[artifacts/Windows_Event_Log]] · [[techniques/Timestamp_Manipulation_Detection]]
