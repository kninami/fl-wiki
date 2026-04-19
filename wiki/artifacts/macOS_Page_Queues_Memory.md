---
artifact: macOS Page Queues Memory
layer: OS
data_types:
  - vm_page 구조체 (PFN, object, offset, 큐 상태)
  - 물리 메모리 페이지 (non-present 포함)
  - 페이지 큐 분류 (Wired/Compressed/Free/Throttled/Page Out/Active/Inactive/Speculative/Secluded)
action_types:
  - 프로세스 메모리 접근
  - 페이지 교체 (eviction/swap)
  - 페이지 압축 (Compressed 큐)
os_version: macOS 10.9 (Mavericks) ~ 10.15 (Catalina)
path: "물리 메모리 내 vm_page_buckets 해시 테이블 (커널 심볼 기반 위치)"
attck: —
sources:
  - case_2020
updated: 2026-04-19
---

## 개요
macOS 가상 메모리 서브시스템이 물리 페이지를 관리하는 구조. vm_page_buckets 해시 테이블이 모든 물리 페이지를 vm_page 구조체로 추적. 하드웨어 페이지 테이블에서 non-present로 표시된 페이지도 실제 RAM에 존재하며 포렌식적으로 접근 가능.

## 구조

### vm_page_buckets 해시 테이블
- 가상 주소 → vm_page 구조체 매핑
- 각 vm_page: `phys_page`(PFN), `object`, `offset`, `queue state` 포함
- 인덱스 계산: `(object + offset) % num_buckets`

### 페이지 큐 종류
| 큐 | 설명 |
|----|------|
| Wired | 고정 메모리 (커널, 드라이버) |
| Compressed | 압축됨 (별도 압축 저장소) |
| Free | 미사용 (내용 의미 없음) |
| Throttled | I/O 제한 중 |
| Page Out | 스왑 대기 |
| Active | 최근 접근됨 |
| Inactive External/Internal | 비활성 매핑됨/비매핑됨 |
| Inactive Cleaned | 클린 비활성 |
| Speculative | 투기적 프리페치 |
| Secluded | 재사용 예약 |

### 포렌식적 접근 가능 큐
- Active, Inactive(External/Internal/Cleaned), Speculative, Secluded: 유효한 페이지 내용 포함
- Compressed: 압축 처리 필요 — 별도 해제 로직 필요
- Free: 내용 신뢰 불가
- Wired, Throttled, Page Out: 유효하나 특수 처리 필요

## 추출 방법
- 물리 메모리 이미지 획득: VMware 스냅샷, Surge Collect Pro 등
- Volatility 3 + mac_walkqs 플러그인으로 vm_page_buckets 순회
- mac_find_page: 특정 가상 주소 → vm_page 조회

## 분석 포인트
- 기존 Volatility 주소공간(하드웨어 페이지 테이블 기반)은 non-present 페이지 누락
- vm_page_buckets 기반 주소공간 통합 시 stack/heap 페이지 복구율 대폭 향상
- 프로세스 실행파일 복구: 749 → 1366 (1893 프로세스 대상, 샘플9-10 가장 극적 개선)

## Findings
- vm_page_buckets 순회로 non-present 페이지 포함 시 stack/heap 복구 최대 40% 향상 (샘플1: 8248→11565 페이지) (→ [[papers/case_2020]])
- 프로세스 실행파일 복구: 1893개 프로세스 중 749→1366개; 샘플9 12→238, 샘플10 4→259 (→ [[papers/case_2020]])
- unusual, error, busy, absent, fictitious, restart 상태 페이지는 건너뜀 (→ [[papers/case_2020]])

## 관련 페이지
[[techniques/macOS_Memory_Page_Queue_Forensics]]
