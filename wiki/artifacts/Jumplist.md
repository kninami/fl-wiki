---
artifact: Jumplist
layer: OS
data_types:
  - 최근 사용 파일 목록
  - 앱 실행 이력
  - 파일 경로
  - 타임스탬프
action_types:
  - 파일 열기
  - 파일 저장
  - 앱 실행
os_version: Windows 7+
path: "%AppData%\\Roaming\\Microsoft\\Windows\\Recent\\AutomaticDestinations"
attck:
sources:
  - joo_2022
updated: 2026-04-15
---

## 개요
Windows 7부터 도입된 작업표시줄 점프 목록 기능의 아티팩트. 각 앱별로 최근 열었던 파일·경로·작업이 기록된다. 파일 접근 이력 복원에 매우 유용하다.

## 추출 방법
- 자동 생성: `%AppData%\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\` (`*.automaticDestinations-ms`)
- 수동 핀고정: `%AppData%\Roaming\Microsoft\Windows\Recent\CustomDestinations\` (`*.customDestinations-ms`)
- 도구: JumpList Explorer(EricZimmerman), Autopsy, EnCase

## 분석 포인트
- 파일명: 앱 App ID(해시) 기반 → 어느 앱에서 열었는지 식별
- 각 엔트리: 파일 경로, 마지막 접근 시각, Shell Link(LNK) 정보 포함
- 파일이 삭제된 후에도 Jumplist에 경로 잔류 가능

## Findings
- File Wiping 도구 실행 후 Jumplist에도 흔적 잔류 확인 (→ [[joo_2022]])
- Open&Save MRU, LNK보다 잔류율 높음 (→ [[joo_2022]])

## 관련 페이지
[[artifacts/UserAssist]] · [[artifacts/Prefetch]] · [[techniques/File_Wiping_Detection]]
