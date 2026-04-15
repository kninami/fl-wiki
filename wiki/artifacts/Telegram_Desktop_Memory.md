---
artifact: Telegram Desktop Memory
layer: APP
data_types:
  - 연락처 (전화번호, 사용자명)
  - 대화 내용 (1:1, 그룹, 채널)
  - 세션 정보
  - 암호화 메시지 (복호화 상태)
action_types:
  - 메시지 송수신
  - 연락처 조회
  - 채널 구독
os_version: Windows (Vista+, Telegram Desktop 기준)
path: "프로세스 가상 주소 공간 (Telegram.exe)"
attck: —
sources:
  - fernandez_2022
  - uroz_2026
updated: 2026-04-15
---

## 개요
Windows에서 실행 중인 Telegram Desktop 프로세스의 메모리에 잔존하는 포렌식 아티팩트. 앱이 차단·종료된 상태에서도 메모리에 연락처, 대화 내용, 사용자 정보가 남아 있어 수사에 활용 가능. Telegram은 end-to-end 암호화를 제공하지만, 앱 실행 중에는 데이터가 복호화된 상태로 메모리에 존재.

## 추출 방법
- **도구**: Windows Memory Extractor (프로세스별 메모리 덤프) + IM Artifact Finder (Python, GPLv3)
- Windows Task Manager / Process Hacker로 Telegram.exe PID 확인 후 메모리 덤프
- IM Artifact Finder가 덤프에서 아티팩트 추출 → JSON 보고서 생성
- HeapList(Volatility 3) 플러그인으로 힙 엔트리 직접 순회 가능 (→ uroz_2026)

## 분석 포인트
- **연락처**: UserData 클래스에서 전화번호, 사용자명, 표시 이름 추출
- **대화 내용**: History 클래스 계층 분석; 1:1, 그룹, 채널 구분 가능
- **비밀 대화(Secret Chat)**: end-to-end 암호화이나 MTProto 키 교환 알고리즘 데이터 포함
- 앱이 활성 상태일 때 덤프해야 가장 많은 데이터 추출 가능

## 공격 기법 (해당 시)
- **범죄 증거 획득 우회**: 앱 차단 후에도 메모리에 잔존 → 영장 집행 후 앱 비활성화해도 증거 확보 가능
- 대화 내용 삭제 후에도 메모리에서 복구 가능 (프로세스 종료 전까지)

## Findings
- 앱 차단·암호화 상태에서도 프로세스 메모리에서 연락처·대화 내용 추출 성공 (→ [[papers/fernandez_2022]])
- IM Artifact Finder는 다른 IM 앱(WhatsApp, Skype, Signal 등)으로 확장 가능한 플러그인 아키텍처 (→ [[papers/fernandez_2022]])
- Telegram Desktop 케이스스터디로 NT Heap 분석 기법 실용성 검증 (→ [[papers/uroz_2026]])

## 관련 페이지
[[artifacts/Windows_NT_Heap]] · [[techniques/IM_Memory_Forensics]] · [[techniques/Memory_Heap_Forensics]]
