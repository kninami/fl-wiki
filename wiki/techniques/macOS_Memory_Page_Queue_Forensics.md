---
technique: macOS Memory Page Queue Forensics
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - macOS Page Queues Memory
related_crime:
  - macOS 시스템 메모리 분석
  - 프로세스 메모리 복구
  - 악성코드 메모리 포렌식
sources:
  - case_2020
updated: 2026-04-19
---

## 개요
macOS 가상 메모리의 vm_page_buckets 해시 테이블을 순회해 하드웨어 페이지 테이블에서 non-present로 표시된 페이지까지 복구하는 Volatility 기반 메모리 포렌식 기법.

## 메커니즘
macOS는 페이지를 하드웨어 페이지 테이블(PTEs)에서 non-present로 표시해도 실제 물리 RAM에 유지함. 기존 Volatility address space는 PTE 기반이므로 이 페이지를 모두 누락. vm_page_buckets(커널 심볼)를 직접 참조하면 모든 페이지와 그 상태(큐)를 확인 가능.

**가상 주소 → 물리 주소 변환 (큐 인식):**
1. 가상 주소 → vm_map_entry 탐색 (vm_map 트리 순회)
2. vm_map_entry에서 vm_object + offset 추출
3. `(object + offset) % num_buckets`로 vm_page_buckets 인덱스 계산
4. 해당 버킷의 vm_page 체인에서 object+offset 일치 항목 탐색
5. vm_page 상태 확인: unusual/error/busy/absent/fictitious/restart → 건너뜀; compressed → 별도 처리; free → 건너뜀
6. `phys_page` × PAGE_SIZE → 물리 오프셋

## Volatility 플러그인

### mac_walkqs
- vm_page_buckets 전체 순회
- 각 vm_page의 PFN, object, offset, 큐 상태 출력
- 메모리 내 모든 페이지 인벤토리 생성

### mac_find_page
- 특정 가상 주소에 대한 vm_page 조회
- macOS Volatility address space 내부 통합용

## 탐지 방법 (Investigator)
- Inactive, Speculative, Secluded 큐의 페이지: 최근 접근되었으나 하드웨어 테이블에서 제거된 것 → 삭제 시도 후 잔존 데이터일 수 있음
- 프로세스 실행파일이 기존 방법으로 누락될 경우 mac_walkqs로 재탐색
- Compressed 큐: 압축 해제 후 유효한 데이터 복구 가능

## 한계 & 주의사항
- macOS 10.9–10.15 검증; 이후 버전(Big Sur+)은 커널 구조 변경 가능성
- 물리 메모리 전체 획득 필요 (부분 덤프 불가)
- 성능 오버헤드 ~30–40% (대형 이미지 기준) — 허용 범위
- Compressed 큐 페이지는 추가 압축 해제 처리 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/case_2020]] | mac_walkqs/mac_find_page 구현; 10개 샘플(Mavericks~Catalina) 검증; 프로세스 실행파일 복구 749→1366 |

## 관련 페이지
[[artifacts/macOS_Page_Queues_Memory]]
