---
artifact: UserAssist
layer: OS
data_types:
  - 프로그램 실행 횟수
  - 마지막 실행 시각
  - 실행 파일 경로 (ROT13 인코딩)
action_types:
  - GUI 프로그램 실행
os_version: Windows XP+
path: "HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\UserAssist"
attck:
sources:
  - joo_2022
  - kim_2022a
updated: 2026-04-15
---

## 개요
Windows Explorer가 GUI 프로그램 실행 시 자동으로 기록하는 레지스트리 키. 프로그램 경로를 ROT13으로 인코딩하여 실행 횟수·마지막 실행 시각을 저장한다.

## 추출 방법
- 레지스트리 키: `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`
- GUID는 OS 버전별로 상이 (실행 프로그램용 GUID: `{CEBFF5CD-...}` 등)
- 값 이름은 ROT13 인코딩 → 디코딩 후 실행 파일 경로 확인
- 도구: UserAssistView(Nirsoft), Registry Explorer

## 분석 포인트
- 실행 횟수: 동일 프로그램이 몇 번 실행되었는지
- 마지막 실행 시각: FILETIME 형식
- ROT13 디코딩: 문자 회전으로 경로 복원
- 삭제된 프로그램도 레코드 잔류 가능

## Findings
- File Wiping 도구 5종 실행 흔적이 UserAssist에 기록됨 확인 (→ [[joo_2022]])
- WSA 앱 실행 이력이 UserAssist에 기록됨 확인 (→ [[kim_2022a]])

## 관련 페이지
[[artifacts/AmCache]] · [[artifacts/ShimCache]] · [[artifacts/Prefetch]] · [[techniques/File_Wiping_Detection]] · [[techniques/WSA_Forensics]]
