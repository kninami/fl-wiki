---
artifact: hiberfil_sys
layer: OS
data_types:
  - 커널 메모리 이미지
  - 사용자 공간 메모리
  - 프로세스 목록
  - 열린 파일 정보
  - Pool Tags
action_types:
  - 시스템 최대 절전 진입
  - Fast Startup 종료
os_version: Windows 10+
path: "C:\\hiberfil.sys"
attck:
sources:
  - bang_2022
updated: 2026-04-15
---

## 개요
Windows 최대 절전(Hibernate) 또는 Fast Startup 기능 사용 시 생성되는 메모리 이미지 파일. 종료 직전 시스템 상태(커널 및 일부 사용자 메모리)가 보존되어 디지털 포렌식에서 동적 데이터 복원에 활용된다.

## 추출 방법
- 경로: `C:\hiberfil.sys` (숨김·시스템 파일, 루트 직속)
- FTK Imager, Volatility 3로 직접 파싱 가능
- Volatility 3: `--single-location file:///C:/hiberfil.sys`

## 분석 포인트
- **헤더 구조 (PO_MEMORY_IMAGE)**:
  - Signature: "HIBR"(최대 절전) 또는 "RSTR"(Fast Startup)
  - SystemTime: 최대 절전 진입 시각
  - NumPagesForLoader: 저장된 페이지 수
- **전원 옵션별 저장 범위**:
  - Fast Startup ON + 종료: 커널 메모리만 저장
  - 최대 절전(Hibernate): 커널 + 사용자 공간 전체 저장
- **Volatility 3 플러그인**: `pslist`(프로세스 목록), `windows.info`(시스템 정보), `ChromeHistoryView`, Pool Tags 분석(`FAT: "Fatn"`)

## 공격 기법 (해당 시)
최대 절전 기능을 이용한 메모리 내 민감 정보 보존 — 역설적으로 범죄자의 메모리 행위가 보존될 수 있음.

## Findings
- Signature "HIBR"/"RSTR"로 파일 유효성 및 전원 옵션 유형 판별 가능 (→ [[bang_2022]])
- 종료 직전 파일 복사 행위를 hiberfil.sys 메모리 포렌식으로 추적 성공 (→ [[bang_2022]])
- Fast Startup 활성 시 커널 메모리만 저장되어 사용자 공간 데이터 복원 불가 (→ [[bang_2022]])

## 관련 페이지
[[techniques/Hibernation_Forensics]] · [[papers/bang_2022]]
