---
technique: ReFS_Deleted_Data_Recovery
perspective: Investigator
topic: File Recovery
related_artifacts:
  - ReFS_Metadata_Structures
related_crime:
  - 증거인멸
  - 데이터 복구
sources:
  - kim_2018
updated: 2026-04-15
---

## 개요
ReFS(Resilient File System)의 메타데이터 구조와 Write-at-Write 특성을 활용하여 삭제된 파일·디렉토리를 복원하는 두 가지 기법. 기존 포렌식 도구(Encase, R-Studio, Autopsy)가 ReFS 복구를 지원하지 않는 공백을 메운 연구.

## 메커니즘

### ReFS 고유 특성
- **메타데이터 복사본 관리**: Super block → Check point(primary/secondary) → Object/Container table 계층구조
- **Write-at-Write**: 메타데이터 갱신 시 동일 위치 덮어쓰기 않고 새 위치에 기록 → 이전 상태 잔존

### 방법 1 — 메타데이터 기반 복구
1. Root Directory 내 삭제 엔트리 스캔 (플래그·오프셋 변화 탐지)
2. Object ID로 데이터 블록 위치 추적
3. LCN(Logical Cluster Number) 정보가 유지된 경우 직접 데이터 복원

### 방법 2 — Write-at-Write 기반 복구
1. 현재 메타데이터 외 이전 메타데이터 블록 탐색
2. 삭제 전 상태의 메타데이터 블록 발견 → 파일 엔트리 복원
3. 완전 삭제(LCN 정보 소거) 시에도 메타데이터 기반 경로 추적 가능

## 삭제 유형별 특성
| 삭제 유형 | Sequence number | 플래그 | LCN 정보 |
|-----------|----------------|--------|----------|
| 휴지통 삭제 | 변화 | 변경 | 유지 |
| 완전 삭제 | — | 변화 | 일부 소거 |

## 한계 & 주의사항
- ReFS v1.2 / v3.3 대상 검증 — 버전 의존성 존재
- ReFS는 부팅 파티션으로 사용 불가 (시스템 드라이브 적용 한계)
- 기존 상용 도구 지원 미비 — 전용 도구 또는 수동 분석 필요
- Write-at-Write 특성은 빈번한 메타데이터 갱신 시 이전 블록 덮어쓰일 수 있음

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kim_2018]] | ReFS v1.2·v3.3 구조 역분석; 메타데이터 기반·Write-at-Write 기반 두 가지 복구 방법 제안 |

## 관련 페이지
[[artifacts/ReFS_Metadata_Structures]] · [[artifacts/ReFS_Logfile]] · [[artifacts/ReFS_Change_Journal]] · [[techniques/ReFS_Journaling_Forensics]]
