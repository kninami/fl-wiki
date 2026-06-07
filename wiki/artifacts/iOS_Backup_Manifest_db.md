---
artifact: iOS Backup Manifest.db
layer: APP
data_types:
  - SHA-1 해시 (fileID)
  - 도메인 식별자 (domain)
  - 원본 파일 경로 (relativePath)
  - 파일 플래그 (flags: 1=File/2=Directory/4=Symlink)
  - NSKeyedArchiver 메타데이터 BLOB (file)
  - Key-Value 저장 (Properties 테이블)
action_types:
  - iOS 기기 백업
  - 파일 메타데이터 색인
  - 암호화 백업 시 키 정보 저장
os_version: macOS (MobileSync 백업용); iOS 10+
path: "~/Library/Application Support/MobileSync/Backup/[UDID-HASH]/Manifest.db"
attck: —
sources:
  - kimyonghyun_2026
updated: 2026-06-07
---

## 개요
iOS 백업에서 모든 백업 대상 파일의 메타데이터를 색인하는 SQLite3 데이터베이스. iOS 10부터 도입. Files 테이블(백업 파일의 해시·도메인·경로·메타데이터)과 Properties 테이블(key-value)로 구성. 비암호화 백업에서 각 파일 콘텐츠 접근의 관문 역할.

## 구조

### Files 테이블
```sql
CREATE TABLE Files (
 fileID TEXT PRIMARY KEY,   -- SHA-1 해시 (파일명, 00~ff 디렉토리 내)
 domain TEXT,               -- 백업 도메인 (AppDomain-com.xxx, HomeDomain 등)
 relativePath TEXT,         -- 기기 내 원본 파일 경로
 flags INTEGER,             -- 1=File, 2=Directory, 4=Symlink (상호배타적)
 file BLOB                  -- NSKeyedArchiver 직렬화 메타데이터
);
CREATE INDEX FilesDomainIdx ON Files(domain);
CREATE INDEX FilesRelativePathIdx ON Files(relativePath);
CREATE INDEX FilesFlagsIdx ON Files(flags);
```

**fileID 해시 생성**: `SHA1(domain + '-' + relativePath)` — 해시 앞 2글자로 256개(00~ff) 디렉토리에 분산 저장

**file BLOB 내부 메타데이터** (NSKeyedArchiver 바이너리 plist):
- **LastModified** — 마지막 수정 시각
- **LastStatusChange** — ctime (메타데이터 변경 시각)
- **Birth** — 파일 생성 시각
- **Size** — 파일 크기 (바이트)
- **InodeNumber** — inode 번호
- **Mode** — 파일 권한 (Unix mode)
- **UserID / GroupID** — 소유자/그룹
- **Flags** — 파일 플래그
- **ProtectionClass** — iOS 데이터 보호 등급 (NSFileProtectionComplete 등)
- **RelativePath** — 원본 경로
- **암호화 키** — 암호화 백업 시 각 파일의 래핑된 암호키 포함 (Manifest.db가 있어야 복호화 가능)

### Properties 테이블
```sql
CREATE TABLE Properties (
 key TEXT PRIMARY KEY,
 value BLOB
);
```
Dictionary와 같은 key-value 정보 저장용으로 추정. 테스트 시점에서는 빈 상태.

## 추출 방법
- 경로: `~/Library/Application Support/MobileSync/Backup/[HASH]/Manifest.db`
- 도구: SQLite 뷰어 (TablePro 등), sqlite3 CLI
- file BLOB은 NSKeyedArchiver 파싱 필요 (plutil, Imhex Hex Editor)

## 분석 포인트
- Files 테이블로 백업된 모든 파일의 원본 경로·도메인 역추적 가능 → 기기 내 앱·파일 존재 확인
- file BLOB에서 파일 생성·수정·메타데이터 변경 시각 확보 → 타임라인 재구성
- ProtectionClass 값으로 파일별 iOS 데이터 보호 수준 확인
- 비암호화 백업: Files 테이블은 평문 접근 가능 → 도메인·경로 기반 수사 가능
- 암호화 백업: Manifest.db 파일 자체도 암호화됨 → 복호화 키 체인(백업암호 → KeyBag → Manifest.db 키) 없이 접근 불가
- fileID(SHA-1)로 00~ff 디렉토리에서 해당 콘텐츠 파일 직접 탐색 가능

## Findings
- Manifest.db는 iOS 10부터 도입된 SQLite3 포맷으로 Files·Properties 2개 테이블 구성 (→ [[papers/kimyonghyun_2026]])
- Files 테이블의 file 컬럼은 NSKeyedArchiver 직렬화 bplist로 LastModified·Birth·Size·ProtectionClass 등 10종 이상의 메타데이터 포함 (→ [[papers/kimyonghyun_2026]])
- fileID = SHA1(domain + '-' + relativePath)로 생성되어 256개 디렉토리에 분산 저장 (→ [[papers/kimyonghyun_2026]])
- 암호화 백업 시 Manifest.db 파일 자체도 별도 키로 암호화됨 — 전체 파일 복호화 체인(백업암호 → BackupKeyBag → Manifest.db 키 → 개별 파일 키) 필요 (→ [[papers/kimyonghyun_2026]] Q&A Q4)
- Manifest.db가 손상·누락되어도 fileID 생성 규칙(SHA1(domain + '-' + relativePath))을 역이용해 주요 경로의 해시를 사전 계산·대조함으로써 파일 식별 가능. 예: `3d0d7e5fb2ce...` = HomeDomain-Library/SMS/sms.db (→ [[papers/kimyonghyun_2026]] Q&A Q3)
- Manifest.db 누락 시에도 비암호화 백업이면 00~ff 파일 내용 직접 확인 가능하며, 파일 매직 바이트·내부 구조로 파일 종류 추정 가능 (→ [[papers/kimyonghyun_2026]] Q&A Q3)
- fileID는 domain + '-' + relativePath 기반이므로 파일명 변경 시 relativePath가 달라져 새 fileID 생성 → 증분 백업에서 기존 파일이 아닌 신규 파일로 처리됨 (→ [[papers/kimyonghyun_2026]] Q&A Q9)

## 관련 페이지
[[artifacts/iOS_Backup_Info_plist]] · [[artifacts/iOS_Backup_Manifest_plist]] · [[artifacts/iOS_Backup_Status_plist]] · [[techniques/iOS_Backup_Forensics]]
