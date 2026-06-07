---
technique: iOS_Backup_Forensics
perspective: Investigator
topic: Artifact Analysis
attck: —
related_artifacts:
  - iOS Backup Manifest.db
  - iOS Backup Info.plist
  - iOS Backup Manifest.plist
  - iOS Backup Status.plist
related_crime:
  - 모바일 기기 포렌식
  - 증거 수집
  - 디지털 범죄 수사
sources:
  - kimyonghyun_2026
updated: 2026-06-07
---

## 개요
macOS Finder 또는 iTunes를 통해 생성된 iOS 백업 데이터를 포렌식적으로 분석하는 방법론. 백업 구성요소인 Manifest.db(SQLite3), Info.plist·Manifest.plist·Status.plist(plist)의 구조를 이해하고, 각 파일에서 기기 식별정보·앱 목록·파일 메타데이터·암호화 여부·백업 완전성을 단계적으로 추출한다.

## 분석 절차

### 1단계 — 백업 폴더 식별 및 초기 판단
- 경로: `~/Library/Application Support/MobileSync/Backup/[UDID-HASH]/`
- `Status.plist` → BackupState·IsFullBackup 확인 (완료 여부, 풀/증분 백업 구분)
- `Manifest.plist` → IsEncrypted 확인 (암호화 백업이면 복호화 절차 선행 필요)
- **`Info.plist` → 암호화 백업 여부와 무관하게 평문 접근 가능**. Product Version, Serial Number, Phone Number 등 기기 식별정보를 항상 최우선 확보 — 암호화 백업의 첫 진입점

### 2단계 — Manifest.db 분석
- SQLite 뷰어로 Files 테이블 조회 → 백업된 모든 파일의 도메인·원본 경로 목록 확보
- `SELECT domain, relativePath, flags FROM Files` → 기기 내 파일 체계 파악
- fileID(SHA-1)를 계산하여 00~ff 디렉토리에서 해당 콘텐츠 파일 직접 접근
  - 해시: `SHA1(domain + '-' + relativePath)`
  - 파일 위치: `[해시 앞 2글자]/[해시 전문]`
- file BLOB (NSKeyedArchiver) 파싱 → LastModified·Birth·Size·ProtectionClass 등 메타데이터 추출
- **암호화 백업 주의**: Manifest.db 자체도 별도 키로 암호화되어 있으므로, 복호화 체인(백업암호 → Manifest.plist BackupKeyBag → Manifest.db 키) 없이 접근 불가

### 2.5단계 — Manifest.db 손상·누락 시 대체 분석
Manifest.db가 손상되거나 누락된 경우에도 아래 방법으로 부분 복구 가능:

1. **SHA-1 Pre-compute 경로 식별**: fileID가 `SHA1(domain + '-' + relativePath)`로 생성되는 점을 역이용. 알려진 주요 경로(예: `HomeDomain-Library/SMS/sms.db` → `3d0d7e5fb2ce288813306e4d4636395e047a3d28`)의 해시를 사전 계산하여 00~ff 디렉토리 내 파일명과 대조, 어떤 앱의 어떤 파일인지 식별
2. **내용 기반 파일 종류 추정**: 파일 매직 바이트, 내부 구조가 알려진 포맷(SQLite, plist, SQLite WAL 등)은 내용 분석으로 파일 종류 추정 가능
3. **비암호화 백업**: 00~ff 디렉토리 파일 내용 직접 열람 가능. Manifest.db 없어도 콘텐츠 접근에는 제한 없음
4. **암호화 백업**: fileID 생성 규칙은 동일하게 적용되므로 경로 존재 여부 일부 추정 가능. 단, 복호화 키는 Manifest.db file 필드에 저장되므로 파일 내용 복호화는 현실적으로 불가

### 3단계 — plist 파일 교차 분석
- **Info.plist** (XML): 기기 식별정보, 설치 앱, 백업 시각
- **Manifest.plist** (bplist): 암호화 여부, Lockdown 정보, 앱·Extension 전체 목록
- **Status.plist** (bplist): 백업 완전성, 증분 여부, 세션 UUID
- 세 파일의 Date 필드로 백업 타임라인 재구성 (시작 → 스냅샷 → 완료)

### 4단계 — Binary plist 파싱 (필요 시 수동 분석)
1. 헤더 `bplist00`(8B) 확인
2. 파일 끝 32B 트레일러에서 offsetIntSize·objectRefSize·numObjects·topObject·offsetTableOffset 읽기
3. offsetTableOffset 부터 offsetIntSize × numObjects 만큼 오프셋 테이블 읽기
4. topObject(offset table index) → object table → marker nibble 해석
   - 상위 nibble: 타입 (0x5=ASCII, 0xD=Dict, 0xA=Array, 0x4=Data, 0x1=Int 등)
   - 하위 nibble: 길이/개수 (≥15면 0x?F + 뒤 Int로 실제 길이)
5. Dict(0xD): 이후 objectRefSize × 2N 바이트(키 N개, 값 N개)로 키-값 매핑

**bplist 저장 공간 최적화**: 동일 데이터가 여러 번 참조될 경우 오브젝트 테이블에 1회만 저장하고, 오프셋 테이블의 여러 항목이 동일 위치를 참조하는 중복 제거(deduplication) 구조 사용. 대용량 plist 파싱 시 동일 객체의 반복 파싱을 피할 수 있음.

## 탐지 포인트
| 분석 목표 | 활용 아티팩트 |
|-----------|-------------|
| 기기 소유자 식별 | Info.plist (Phone Number, Serial Number, IMEI, UDID) — 암호화 백업에서도 평문 |
| 설치 앱 감사 | Info.plist (Installed Applications) + Manifest.plist (Applications) |
| 파일 존재·경로 확인 | Manifest.db Files 테이블 (domain, relativePath) |
| Manifest.db 없을 때 경로 추정 | SHA-1 pre-compute (주요 경로 해시 사전 계산 후 00~ff 대조) |
| 파일 타임스탬프 | Manifest.db file BLOB (Birth, LastModified, LastStatusChange) |
| 백업 완전성 | Status.plist (BackupState, IsFullBackup) |
| 암호화 접근 가능성 | Manifest.plist (IsEncrypted, BackupKeyBag) |
| 백업 시각 확정 | Info.plist·Manifest.plist·Status.plist Date 교차 비교 |
| 파일명 변경 / 증분 판단 | Manifest.db fileID(SHA-1) — 파일명 변경 시 새 fileID로 신규 처리 확인 |

## 한계 & 주의사항
- 암호화 백업(IsEncrypted=True) 시 00~ff 콘텐츠 파일은 AES256 CBC로 암호화 — BackupKeyBag·기기 비밀번호로 복호화 필요
- 증분 백업(IsFullBackup=False)은 이전 백업 이후 변경분만 포함 → 전체 증거 수집에 불완전
- **파일명 변경 시 주의**: fileID = SHA1(domain + '-' + relativePath)이므로, 파일 내용이 동일해도 파일명만 변경되면 새 fileID가 생성되어 증분 백업에서 기존 파일이 아닌 신규 파일로 처리됨 → 삭제 후 재생성 파일과 구분 어려움
- 단일 iOS 버전(15.8.7)·단일 디바이스(iPhone 6s) 실측 — 다른 버전에서 구조 차이 가능 (→ kimyonghyun_2026)
- macOS 기반 분석 — Windows iTunes 백업도 유사하나 경로 차이 존재

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kimyonghyun_2026]] | Manifest.db Files·Properties 테이블 스키마 실측, file BLOB 메타데이터 분석, bplist 단계별 파싱 방법 제시 |

## 관련 페이지
[[artifacts/iOS_Backup_Manifest_db]] · [[artifacts/iOS_Backup_Info_plist]] · [[artifacts/iOS_Backup_Manifest_plist]] · [[artifacts/iOS_Backup_Status_plist]]
