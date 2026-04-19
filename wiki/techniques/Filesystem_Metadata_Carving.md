---
technique: Filesystem Metadata Carving
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - $USNjrnl
  - $LogFile
related_crime:
  - 포맷된 디스크 증거 복구
  - 안티-포렌식 대응
  - 삭제·덮어쓰기 파일 복구
sources:
  - nordvik_2020
updated: 2026-04-19
---

## 개요
포맷되거나 손상된 디스크 이미지에서 파일시스템 메타데이터 엔트리(NTFS MFT 레코드, Ext4 inode 등)를 타임스탬프 패턴을 동적 시그니처로 활용해 카빙하는 범용 기법.

## 메커니즘
기존 파일 카빙은 고정된 마법 바이트(magic bytes)에 의존 → 파일시스템 메타데이터에는 적용 불가. 타임스탬프는 파일마다 고유한 패턴이며, 여러 타임스탬프가 메타데이터 엔트리 내 고정 오프셋에 공존 → "동적 시그니처"로 사용 가능.

**타임스탬프 지원 파일시스템:**
| 파일시스템 | 타임스탬프 수 | 정밀도 |
|------------|--------------|--------|
| NTFS | 4 (MFT $STANDARD_INFORMATION + $FILE_NAME) | 64-bit ns, 1601-01-01 기준 |
| ReFS | 4 | — |
| APFS | 4–5 | — |
| Ext4 | 4–7 | — |
| HFS+, BTRFS, UFS2 | 4 | — |
| ExFAT, FAT, UFS1 | 3 | — |

## 알고리즘 (cPTS)

### 1단계: 제네릭 타임스탬프 스캐너
- 디스크 이미지를 원시(raw) 스캔
- 유효한 날짜 범위 내 타임스탬프 패턴 탐색 (논리적으로 일관된 다중 타임스탬프 공존)
- 복잡도: O(|T|/m × k/m) — m: 섹터 크기, k: 동시 검증 타임스탬프 수

### 2단계: FS별 파서
- 후보 위치를 각 파일시스템 스펙으로 검증
- NTFS: $STANDARD_INFORMATION 오프셋, $FILE_NAME 오프셋, MFT 서명 교차 확인
- Ext4: inode 구조 오프셋, 블록 그룹 일관성 확인

## 탐지 방법 (Investigator)
- EnCase/X-Ways가 복구 실패한 포맷 후 이미지에서 메타데이터 카빙 시도
- NTFS→exFAT 포맷: Precision 0.9939, Recall 1.0 달성 (cPTS)
- Ext4→NTFS 포맷: inode 분류 Precision 1.0; 963개 덮어쓰인 inode 복구
- 타임스탬프만으로 파일 유형과 위치를 부분 추론 가능

## 한계 & 주의사항
- Ext4 inode-파일 귀속(attribution) 정밀도 ~0.29 — 메타데이터는 복구되지만 어느 파일에 속하는지는 불확실
- 암호화 볼륨, 압축 볼륨에서는 타임스탬프 패턴 인식 불가
- 2 GB 소용량 이미지로 검증 — 대형 볼륨 성능 확장성 미검증
- 도구: cPTS (github.com/reviewscientific2020/cPTS)

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/nordvik_2020]] | DFRWS 2020 Best Paper; NTFS Precision 0.9939/Recall 1.0; Ext4 inode 57427 TP 분류; EnCase/X-Ways 대비 압도적 우위 |

## 관련 페이지
[[artifacts/$USNjrnl]] · [[artifacts/$LogFile]]
