---
artifact: iOS Backup Manifest.plist
layer: APP
data_types:
  - 암호화 여부 (IsEncrypted)
  - BackupKeyBag (키백 바이너리)
  - 백업 생성 시각 (Date)
  - 기기 잠금 암호 설정 여부 (WasPasscodeSet)
  - Lockdown 인증서 (기기명, 일련번호, UDID, ProductVersion 등)
  - 백업 대상 앱·Extension 목록 (Applications)
  - Manifest·SystemDomains 포맷 버전
action_types:
  - iOS 기기 백업 생성
  - 암호화 백업 설정
  - 백업 앱 선택
os_version: macOS (Finder 백업); iOS 10+
path: "~/Library/Application Support/MobileSync/Backup/[UDID-HASH]/Manifest.plist"
attck: —
sources:
  - kimyonghyun_2026
updated: 2026-06-07
---

## 개요
iOS 백업의 메타데이터를 저장하는 Binary plist(bplist) 파일. 암호화 여부, 암호키(BackupKeyBag), 백업 생성 시각, 기기 잠금 암호 설정 여부, Lockdown 인증서 정보, 그리고 백업 대상에 포함된 모든 앱·Extension 목록을 포함. 바이너리 형식으로 저장되어 `plutil -p` 등 전용 도구로 파싱 필요.

## 구조
Binary plist(bplist) 포맷. 헤더 `bplist00`(8바이트)로 시작하며, 트레일러(32바이트)에서 오브젝트 테이블·오프셋 테이블 위치 파악.

## 추출 방법
- 경로: `~/Library/Application Support/MobileSync/Backup/[HASH]/Manifest.plist`
- **plutil**: `plutil -p Manifest.plist` — 바이너리 plist를 사람이 읽을 수 있는 형태로 출력 (macOS 내장)
- **Hex Editor**: Imhex 등으로 바이너리 직접 분석
- **plistlib**: Python `plistlib.load()` 로 파싱

## 분석 포인트

### 핵심 키
| 키 | plist 타입 | 설명 | 포렌식 활용 |
|----|-----------|------|------------|
| IsEncrypted | Boolean | 암호화 백업 여부 | True면 00~ff 모든 콘텐츠 암호화, 복호화 없이 접근 불가 |
| BackupKeyBag | Data | Keybag 바이너리 (1336 bytes) | 암호화 백업 복호화에 필요; 비암호화 시에도 더미 존재 |
| WasPasscodeSet | Boolean | 백업 당시 기기 잠금 암호 설정 여부 | True면 물리 기기 접근 시 암호 필요 |
| Date | Date | 백업 생성 시각 (UTC) | Info.plist의 Last Backup Date보다 약 15초 빠름 (백업 시작 시점) |
| Lockdown | Dictionary | 기기 Lockdown 인증서 정보 | DeviceName, SerialNumber, ProductType, ProductVersion, BuildVersion, UniqueDeviceID, Accessibility 설정 등 |
| Applications | Dictionary | 백업 대상 앱·Extension 목록 | 시스템 앱 450여 개 + 타사 앱 + Extension. 키=번들ID, 값={CFBundleIdentifier, CFBundleVersion, ContainerContentClass, Path} |
| SystemDomainsVersion | String | 시스템 도메인 백업 포맷 버전 (예: "24.0") | iOS 버전과 연동, 버전별 구조 차이 파악 |

### Lockdown 하위 정보
- DeviceName, SerialNumber, ProductType — Info.plist와 교차 검증
- UniqueDeviceID — 백업 폴더명(HASH)과 관계
- Accessibility 설정 — 사용자 접근성 환경 정보

### Applications 구조
각 앱별 하위 딕셔너리:
- **CFBundleIdentifier**: 번들 ID (com.iwilab.KakaoTalk 등)
- **CFBundleVersion**: 앱 버전
- **ContainerContentClass**: 컨테이너 유형 (DataApplication, AppExtension 등)
- **Path**: 백업 내 경로 (기본값: "")

## Findings
- Manifest.plist는 bplist 형식으로 암호화 여부(IsEncrypted)와 WasPasscodeSet을 포함 — 수사 초기 백업 접근 가능성 판단의 첫 관문 (→ [[papers/kimyonghyun_2026]])
- IsEncrypted=False이지만 BackupKeyBag이 존재하는 경우 — 더미 키백이 포함되며 00~ff 파일은 평문 접근 가능 (→ [[papers/kimyonghyun_2026]])
- Applications 딕셔너리로 시스템 앱 450여 개 + 타사 앱 목록 확보 — 설치 앱 감사 및 Info.plist의 Installed Applications과 교차 검증 (→ [[papers/kimyonghyun_2026]])
- Lockdown 정보로 기기명·일련번호·UDID 등 Info.plist와 중복·보완 정보 획득 (→ [[papers/kimyonghyun_2026]])

## 관련 페이지
[[artifacts/iOS_Backup_Manifest_db]] · [[artifacts/iOS_Backup_Info_plist]] · [[artifacts/iOS_Backup_Status_plist]] · [[techniques/iOS_Backup_Forensics]]
