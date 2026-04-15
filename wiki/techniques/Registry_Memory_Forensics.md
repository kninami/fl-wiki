---
technique: Registry Memory Forensics
perspective: Investigator
topic: Extraction
attck: —
related_artifacts:
  - Windows Registry (In-Memory)
related_crime:
  - 멀웨어 분석
  - APT 수사
  - 사이버범죄 수사
sources:
  - dolangavitt_2008
updated: 2026-04-15
---

## 개요
물리 메모리 덤프에서 Windows 레지스트리(디스크 캐시 + Volatile Keys)를 추출하는 기법. 디스크 포렌식만으로는 접근 불가한 런타임 상태 정보 및 스텔스 레지스트리 조작 탐지에 활용.

## 메커니즘
메모리의 레지스트리 구조:
- 디스크 하이브 포맷 그대로 캐시; cell index → 가상 주소 변환만 다름
- `_CM_KEY_NODE`의 SubKeyCounts: stable(배열[0]) + volatile(배열[1]) 분리
- Configuration Manager가 KCB(Key Control Block) 해시 테이블로 열린 키 관리
- Volatile Keys: 부팅 시 생성, 재부팅 시 소멸, 디스크에 저장 안 됨

## 탐지 방법 (Investigator)
**Volatility 플러그인:**
1. 메모리 이미지에서 `_HIVE_STORAGE` 구조 탐색
2. 각 하이브의 cell index 0×20에서 루트 키 시작
3. 디스크 레지스트리 파서(Samba regfio, Perl Parse::Win32Registry 등)와 동일 로직으로 순회
4. Volatile 배열 처리 → volatile keys 추출

**주요 수집 대상:**
- `HKLM\HARDWARE` — 현재 물리 하드웨어 목록
- `HKLM\SYSTEM\...\hivelist` — 마운트된 하이브
- `HKCU\Volatile Environment` — 사용자 세션 환경 변수
- `HKLM\SOFTWARE\...\RunOnce` — 공격자 지속성 키 (Volatile 기록 여부 확인)

**스텔스 조작 탐지:**
- 메모리 레지스트리 값 vs. 디스크 하이브 값 비교
- 불일치 발견 시 → 메모리 레벨 조작 의심
- KCB 해시 테이블 분석으로 현재 열린 키 확인

## 한계 & 주의사항
- RAM 덤프 수집 시점의 상태만 반영; 이후 변경 사항 미포함
- 메모리 압박 시 레지스트리 페이지가 디스크로 스왑 → 불완전 추출 가능
- 하이브 위치 탐색에 휴리스틱 사용 → 일부 하이브 누락 가능
- Windows XP SP2 기준 개발; 최신 버전에서 구조 변경 가능

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/dolangavitt_2008]] | 메모리에서 디스크 대비 평균 631 키/1231 값 추가 발견; 커널 조작 디스크로 탐지 불가 |

## 관련 페이지
[[artifacts/Windows_Registry_Memory]] · [[artifacts/Windows_NT_Heap]] · [[techniques/Memory_Heap_Forensics]]
