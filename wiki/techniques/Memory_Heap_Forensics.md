---
technique: Memory Heap Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Windows_NT_Heap
related_crime:
  - 멀웨어 분석
  - C2 통신 추적
  - 메모리 기반 공격 대응
sources:
  - uroz_2026
updated: 2026-04-15
---

## 개요
Windows user-space 프로세스의 NT Heap을 구조적으로 순회해 포렌식 관련 데이터(자격증명, 메시지, 페이로드)를 추출하는 기법. 공격자가 힙에 데이터를 저장하는 경향이 증가해 메모리 포렌식의 핵심 영역으로 부상.

## 메커니즘
NT Heap 순회는 두 레이어를 처리해야 함:

**Backend Layer 순회:**
1. PEB → ProcessHeaps 배열로 모든 힙 접근
2. 각 Heap의 SegmentList 순회
3. Heap Entry마다 EncodeFlagMask 기반 XOR 디코딩 (Size, Flags)
4. uncommitted 영역 포인터 처리
5. busy/free/internally managed 엔트리 분류

**Frontend Layer(LFH) 순회:**
1. FrontEndHeapType == 0x02 확인
2. FrontEndHeap 포인터 → LFH 구조 접근
3. SubSegmentZones 순회
4. Windows 8.1+: RtlpLFHKey XOR로 stride·offset 디코딩
5. BlockCount 도달까지 모든 LFH 엔트리 수집

## 탐지 방법 (Investigator)
- **HeapList 플러그인** (Volatility 3): 모든 프로세스 힙 엔트리 추출 → 특정 문자열·구조체 검색
- WinDbg 교차 검증으로 추출 결과 검증
- 힙 엔트리 크기·플래그 패턴으로 정상/비정상 할당 구분

## 한계 & 주의사항
- User-space heap만 대상; kernel heap 별도 분석 필요
- UWP(Universal Windows Platform) 앱은 Segment Heap 사용 → 별도 처리
- 힙 구조는 Windows 버전에 따라 세부 차이 → 버전별 검증 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/uroz_2026]] | Backend+Frontend 힙 구조 완전 역분석; Vista~Win11 지원 HeapList 플러그인 개발 |
| [[papers/fernandez_2022]] | Telegram Desktop 힙 분석으로 암호화 메시지·연락처 추출 성공 |

## 관련 페이지
[[artifacts/Windows_NT_Heap]] · [[artifacts/Telegram_Desktop_Memory]] · [[techniques/IM_Memory_Forensics]] · [[techniques/Registry_Memory_Forensics]]
