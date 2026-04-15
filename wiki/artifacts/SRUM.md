---
artifact: SRUM
layer: OS
data_types:
  - 앱별 네트워크 사용량
  - 앱별 CPU·메모리 사용량
  - 에너지 소비량
  - 앱 실행 시간·빈도
action_types:
  - 프로그램 실행
  - 네트워크 통신
  - 리소스 사용
os_version: Windows 8+
path: "%SystemRoot%\\System32\\sru\\SRUDB.dat"
attck:
sources:
  - joo_2022
  - kim_2022a
updated: 2026-04-15
---

## 개요
System Resource Usage Monitor(SRUM)는 Windows 8부터 도입된 리소스 사용 추적 데이터베이스(ESE 형식). 앱별 네트워크·CPU·메모리 사용 내역을 60일 이상 보존하여 프로그램 실행 이력 입증에 활용된다.

## 추출 방법
- 경로: `%SystemRoot%\System32\sru\SRUDB.dat`
- ESE(Extensible Storage Engine) 형식 → SRUM_DUMP, SrumECmd(EricZimmerman), Autopsy 파싱
- 라이브 시스템에서는 VSS 복사본 통해 추출 권장

## 분석 포인트
- **{973F5D5C-...} 테이블**: 앱 실행 정보 (exe 경로, 실행 사용자, 사용 시간)
- **{D10CA2FE-...} 테이블**: 네트워크 바이트 송수신 (Bytes Sent/Received)
- 타임스탬프: 1시간 단위 집계 → 세밀한 시각 특정은 어려움
- WSA 앱 사용 시 Windows SRUM에도 wsaclient.exe 등으로 기록됨

## Findings
- File Wiping 도구 5종 실행 흔적이 SRUM에도 기록됨 확인 (→ [[joo_2022]])
- WSA 앱 사용량이 SRUDB.dat에 기록됨 확인 (→ [[kim_2022a]])

## 관련 페이지
[[artifacts/AmCache]] · [[artifacts/Prefetch]] · [[artifacts/WSA_userdata_vhdx]] · [[techniques/File_Wiping_Detection]] · [[techniques/WSA_Forensics]]
