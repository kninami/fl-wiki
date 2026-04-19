---
artifact: Windows Registry (In-Memory)
layer: OS
data_types:
  - Volatile Keys (메모리 전용)
  - Stable Keys (디스크 동기화)
  - Cached Keys/Values
  - 현재 연결된 하드웨어 목록
  - 마운트된 볼륨 정보
  - 열린 키 핸들
action_types:
  - 레지스트리 키 읽기/쓰기
  - 드라이버 로딩
  - 세션 초기화
os_version: Windows XP+
path: "물리 메모리 (디스크: C:\\Windows\\System32\\config\\*, NTUSER.DAT 등)"
attck: —
sources:
  - dolangavitt_2008
  - uroz_2019
updated: 2026-04-19
---

## 개요
Windows 레지스트리는 물리 메모리에 디스크 하이브와 유사한 구조로 캐시되며, 디스크에 없는 **Volatile Keys**를 추가로 포함. 메모리 포렌식으로만 접근 가능한 런타임 상태 정보(현재 하드웨어, 마운트된 볼륨, 네트워크 연결 등)가 담겨 있어 라이브 포렌식에 중요.

## 구조
- 디스크와 동일한 하이브 포맷; cell index를 가상 주소로 변환하는 방식만 차이
- `_CM_KEY_NODE`: stable(디스크) + volatile(메모리 전용) 두 배열로 서브키 관리
- `_CM_KEY_CONTROL_BLOCK(KCB)`: 열린 키를 빠르게 조회하는 캐시 구조
- Configuration Manager가 해시 테이블(CmpHashTableSize)로 KCB 관리

## 추출 방법
- **도구**: Volatility 플러그인 (dolangavitt_2008 구현); vtypes.py에 데이터 구조 정의
- PEB 없이 `_CM_KEY_NODE` 시그니처 스캔으로 하이브 위치 발견
- cell index 0×20에서 루트 키부터 순회 (디스크 레지스트리 파서와 동일 로직)
- 메모리 이미지에서 직접 실행: 별도 파서 없이 기존 on-disk 도구 재활용 가능

## 분석 포인트
- **Volatile Keys 주요 예시**:
  - `HKLM\HARDWARE`: 현재 연결된 하드웨어 전체 목록
  - `HKLM\SYSTEM\CurrentControlSet\Control\hivelist`: 마운트된 모든 하이브
  - 사용자 환경 변수, 현재 머신 이름, 네트워크 인터페이스
- 디스크 분석 대비 평균 631개 키, 1231개 값 추가 발견 (실험 결과)
- 실제 워크스테이션은 VM보다 volatile 데이터 적음 (메모리 압박으로 페이징)

## 공격 기법 (해당 시)
- **스텔스 레지스트리 조작**: 커널 접근 권한으로 메모리 캐시된 키 수정 → 디스크 포렌식으로 탐지 불가
- 캐시 수정 후 디스크에 동기화하지 않으면 재부팅 시 원상복구 → 지속성 없는 은밀한 조작 가능
- MetaSploit의 `getsystem`, Mimikatz 등이 레지스트리 캐시 조작 활용

## Findings
- 메모리 레지스트리 분석으로 디스크 분석보다 훨씬 많은 키·값 발견 (→ [[papers/dolangavitt_2008]])
- Volatile Keys는 디스크에 없어 라이브 포렌식(RAM 덤프)으로만 수집 가능 (→ [[papers/dolangavitt_2008]])
- 커널 수준 공격으로 메모리 레지스트리 캐시 수정 → 디스크 포렌식으로 탐지 불가; 메모리 포렌식으로만 탐지 (→ [[papers/dolangavitt_2008]])
- 레지스트리 기반 ASEP(Run keys, Services, Winlogon, Active Setup, COM hijacking, Shim DB 등)은 모두 메모리 포렌식으로 탐지 가능 (→ [[papers/uroz_2019]])
- Winesap Volatility 플러그인: REG_BINARY에서 PE 헤더, REG_SZ에서 suspicious path/shell command 탐지 (→ [[papers/uroz_2019]])

## 관련 페이지
[[artifacts/Windows_NT_Heap]] · [[techniques/Registry_Memory_Forensics]] · [[techniques/Windows_ASEP_Persistence]]
