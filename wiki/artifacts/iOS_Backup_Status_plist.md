---
artifact: iOS Backup Status.plist
layer: APP
data_types:
  - 백업 완료 상태 (BackupState)
  - 파일 스냅샷 상태 (SnapshotState)
  - 전체/증분 백업 여부 (IsFullBackup)
  - 백업 스냅샷 생성 시각 (Date)
  - 백업 세션 UUID
  - Status.plist 포맷 버전
action_types:
  - iOS 기기 백업 진행
  - 풀백업 수행
  - 증분 백업 수행
  - 백업 스냅샷 생성
os_version: macOS (Finder 백업)
path: "~/Library/Application Support/MobileSync/Backup/[UDID-HASH]/Status.plist"
attck: —
sources:
  - kimyonghyun_2026
updated: 2026-06-07
---

## 개요
iOS 백업 진행 상태를 기록하는 Binary plist(bplist) 파일. 백업 완료 여부(BackupState), 스냅샷 완료 여부(SnapshotState), 전체/증분 백업 구분(IsFullBackup), 스냅샷 시각, 세션 UUID를 포함. 포렌식에서 백업의 완전성 및 증분 백업 여부 판단에 활용.

## 구조
Binary plist(bplist) 포맷. 189 bytes의 작은 파일로, 백업 상태에 관한 소수의 키-값 쌍만 포함.

## 추출 방법
- 경로: `~/Library/Application Support/MobileSync/Backup/[HASH]/Status.plist`
- `plutil -p Status.plist` 로 파싱 (macOS 내장)

## 분석 포인트

### 핵심 키
| 키 | plist 타입 | 설명 | 값 예시 | 포렌식 활용 |
|----|-----------|------|---------|------------|
| BackupState | String | 백업 완료 상태 | `"finished"`(정상), `"new"`(미완료) | 미완료 백업은 불완전 데이터 가능성 |
| SnapshotState | String | 파일 스냅샷 단계 완료 여부 | `"finished"` | `"finished"`여야 정합성 보장 |
| IsFullBackup | Boolean | 전체 백업 여부 | `false`(증분), `true`(풀) | 증분 백업은 이전 백업 이후 변경분만 포함 → 누락 파일 존재 가능 |
| Date | Date | 백업 스냅샷 생성 시각 (UTC) | `2026-05-05 06:37:21` | Manifest.plist `Date`(06:37:07) + Info.plist `Last Backup Date`(06:37:22) 사이에 위치 — 백업 절차 단계별 시각 추적 |
| UUID | String | 백업 세션 고유 식별자 (GUID) | `4F2E9BCC-...DA6` | Info.plist의 GUID와 동일 — 세션 연계 |
| Version | String | Status.plist 포맷 버전 | `3.3` | iOS 버전별 구조 변경 가능성 확인 |

### 시간 순서 (kimyonghyun_2026 실측)
1. Manifest.plist `Date`: 06:37:07 (백업 시작 단계)
2. Status.plist `Date`: 06:37:21 (스냅샷 완료)
3. Info.plist `Last Backup Date`: 06:37:22 (백업 완료)

## Findings
- Status.plist는 백업의 완전성(완료/미완료/증분)을 판단하는 핵심 파일 — 미완료·증분 백업은 증거 불완전 가능성 (→ [[papers/kimyonghyun_2026]])
- IsFullBackup=false인 증분 백업은 이전 백업 이후 변경분만 포함 — 전체 증거 수집 시 풀백업 여부 확인 필수 (→ [[papers/kimyonghyun_2026]])
- Status.plist·Manifest.plist·Info.plist의 세 Date 필드가 백업 절차의 단계별 시각을 각각 기록 → 백업 전체 타임라인 재구성 가능 (→ [[papers/kimyonghyun_2026]])

## 관련 페이지
[[artifacts/iOS_Backup_Manifest_db]] · [[artifacts/iOS_Backup_Info_plist]] · [[artifacts/iOS_Backup_Manifest_plist]] · [[techniques/iOS_Backup_Forensics]]
