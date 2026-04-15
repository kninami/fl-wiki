---
technique: Instant Messaging Memory Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Telegram_Desktop_Memory
  - Windows_NT_Heap
related_crime:
  - 디지털성범죄
  - 마약 범죄
  - 사기·협박
  - 테러 수사
sources:
  - fernandez_2022
updated: 2026-04-15
---

## 개요
Windows IM(Instant Messaging) 애플리케이션의 프로세스 메모리에서 포렌식 아티팩트를 추출하는 기법. IM 앱은 데이터를 암호화된 채널로 전송하거나 암호화 DB에 저장하지만, 실행 중에는 프로세스 메모리에 복호화된 상태로 데이터가 존재.

## 메커니즘
3단계 포렌식 분석 방법론:
1. **Extraction Phase**: Windows Memory Extractor로 IM 프로세스 메모리 덤프
2. **Analysis Phase**: IM Artifact Finder로 덤프에서 아티팩트 추출 (앱별 플러그인)
3. **Reporting Phase**: JSON/HTML 형식 보고서 생성

IM Artifact Finder 아키텍처:
- 각 IM 앱마다 Factory 패턴으로 분석기 클래스 구현
- Telegram 플러그인: UML 클래스 다이어그램 역분석으로 아티팩트 위치 파악
- UserData(연락처), History(대화), ChatData(채팅방) 클래스 분석

## 탐지 방법 (Investigator)
- 영장 집행 시 앱이 실행 중인 상태에서 즉시 메모리 덤프
- 앱 차단·종료 전에 덤프 → 더 많은 데이터 확보
- VirtualProtect API로 메모리 보호 여부 확인 후 덤프
- HeapList(Volatility 3)로 힙 엔트리 직접 순회 가능

## 한계 & 주의사항
- 앱이 완전히 종료된 후에도 일정 시간 메모리에 잔존하나 점차 덮어쓰임
- 앱 버전 업데이트 시 내부 클래스 구조 변경 → 플러그인 재개발 필요
- iOS/Android 모바일 버전은 별도 분석 필요 (→ fernandez_2022 Windows 한정)
- Signal처럼 메모리 보호 강화 앱은 추출 어려울 수 있음

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/fernandez_2022]] | Telegram Desktop 암호화 채널 메시지·연락처 메모리 추출 성공; 다른 IM 앱 확장 가능 아키텍처 |

## 관련 페이지
[[artifacts/Telegram_Desktop_Memory]] · [[artifacts/Windows_NT_Heap]] · [[techniques/Memory_Heap_Forensics]]
