---
artifact: Windows NT Heap
layer: OS
data_types:
  - Heap Entry
  - Heap Segment
  - LFH Subsegment
  - Allocation metadata
action_types:
  - 메모리 할당
  - 메모리 해제
  - 페이로드 저장
  - C2 은닉
os_version: Windows Vista+
path: "프로세스 가상 주소 공간 (user-space)"
attck: —
sources:
  - uroz_2026
  - fernandez_2022
updated: 2026-04-15
---

## 개요
Windows NT Heap은 Windows 운영체제가 user-space 프로세스의 동적 메모리 관리를 위해 제공하는 힙 구현체. VirtualAlloc API 위에 위치하며, malloc·HeapAlloc 등이 내부적으로 이를 사용. 공격자들이 페이로드, 에이전트 코드, C2 통신 데이터를 힙에 저장하는 경우가 많아 메모리 포렌식의 핵심 대상.

## 구조
- **Backend Layer**: 세그먼트 기반 대형 할당 관리; Heap Entry마다 Size·Flags가 XOR 인코딩(EncodeFlagMask)
- **Frontend Layer (LFH)**: Windows Vista+에서 소형·균일 크기 할당 최적화; Windows 8.1부터 stride·offset이 RtlpLFHKey XOR 인코딩
- 각 프로세스는 최소 1개의 기본 힙 보유; Process Environment Block(PEB)의 ProcessHeaps 배열로 접근

## 추출 방법
- **도구**: Volatility 3 플러그인 HeapList (uroz_2026 공개, https://github.com/reverseame/heaplist)
- **지원 버전**: Windows Vista ~ Windows 11 24H2, x86/x64
- WinDbg 교차 검증으로 정확성 확인
- 메모리 덤프에서 PEB → ProcessHeaps → 각 힙 순회

## 분석 포인트
- 힙 엔트리의 인코딩 해제 후 Size·Flags 확인 → free/busy/uncommitted 구분
- 문자열·구조체 패턴 검색으로 앱 데이터(연락처, 메시지, 자격증명) 추출
- LFH 활성화 여부: FrontEndHeapType == 0x02 확인
- uncommitted 영역 포인터로 미할당 영역 식별

## 공격 기법 (해당 시)
- **메모리 기반 페이로드 은닉**: 힙에 악성코드를 저장해 파일 기반 탐지 우회
- **Use-After-Free, Heap Spraying, Type Confusion**: 힙 레이아웃 조작으로 메모리 취약점 익스플로잇
- 힙 구조 복잡성이 탐지 장벽으로 작용 → 기존 도구는 평면 메모리 영역으로만 처리

## Findings
- HeapList 플러그인으로 Backend·Frontend 힙 엔트리 모두 정확히 추출 가능 (→ [[papers/uroz_2026]])
- Telegram Desktop 케이스스터디: 힙 분석으로 연락처, 대화 내용 등 포렌식 관련 데이터 복구 (→ [[papers/uroz_2026]])
- Telegram 프로세스 메모리(힙 포함)에서 암호화된 채널의 메시지도 복호화된 상태로 존재 (→ [[papers/fernandez_2022]])

## 관련 페이지
[[artifacts/Telegram_Desktop_Memory]] · [[techniques/Memory_Heap_Forensics]] · [[techniques/IM_Memory_Forensics]]
