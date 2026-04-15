---
artifact: AmCache
layer: OS
data_types:
  - 실행 파일 경로
  - SHA-1 해시
  - 파일 크기
  - 최초 실행 시각
  - 설치 정보
action_types:
  - 프로그램 실행
  - 프로그램 설치
  - 드라이버 로드
os_version: Windows 8+
path: "C:\\Windows\\AppCompat\\Programs\\Amcache.hve"
attck:
sources:
  - joo_2022
  - kim_2022a
updated: 2026-04-15
---

## 개요
Windows 8부터 도입된 응용프로그램 호환성 데이터베이스 레지스트리 하이브. 실행된 프로그램의 경로·해시·타임스탬프를 자동 기록하여 실행 이력 추적에 활용된다.

## 추출 방법
- 경로: `C:\Windows\AppCompat\Programs\Amcache.hve`
- 오프라인: 레지스트리 하이브 파일 직접 추출
- 도구: Registry Explorer, AmcacheParser(EricZimmerman), RegRipper

## 분석 포인트
- **InventoryApplication**: 설치된 앱 정보 (이름, 버전, 게시자, 설치 경로)
- **InventoryApplicationFile**: 실행 파일 메타데이터 (경로, SHA-1, 크기, 최초 실행 시각)
- **InventoryApplicationShortcut**: 바로가기 관련 정보
- SHA-1 해시 → VirusTotal 연동으로 악성코드 여부 확인 가능
- 삭제된 프로그램도 일정 기간 레코드 잔류 가능

## Findings
- File Wiping 도구 5종 실행 후 AmCache에 실행 흔적 잔류 확인 (→ [[joo_2022]])
- WSA 앱 설치·실행 이력이 AmCache에 기록됨을 확인 (→ [[kim_2022a]])

## 관련 페이지
[[artifacts/ShimCache]] · [[artifacts/UserAssist]] · [[artifacts/Prefetch]] · [[techniques/File_Wiping_Detection]] · [[techniques/WSA_Forensics]]
