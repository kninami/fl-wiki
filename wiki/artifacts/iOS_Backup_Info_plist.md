---
artifact: iOS Backup Info.plist
layer: APP
data_types:
  - 기기명 (Device Name, Display Name)
  - 제품명·모델 식별자 (Product Name, Product Type)
  - 일련번호 (Serial Number)
  - IMEI / MEID (이동통신 장비 식별자)
  - ICCID (SIM 카드 식별번호)
  - 전화번호 (Phone Number)
  - UDID (Unique Device Identifier)
  - iOS 빌드 버전 (Build Version, Product Version)
  - 설치 앱 목록 (Installed Applications)
  - 백업 시각 (Last Backup Date)
  - 백업 호스트 macOS 버전
action_types:
  - iOS 기기 백업 생성
  - 기기 식별정보 수집
  - 설치 앱 감사
os_version: macOS (Finder 백업); iOS 전 버전
path: "~/Library/Application Support/MobileSync/Backup/[UDID-HASH]/Info.plist"
attck: —
sources:
  - kimyonghyun_2026
updated: 2026-06-07
---

## 개요
iOS 백업 시 생성되는 XML 텍스트 plist 파일. 백업 대상 기기의 식별정보(기기명, 일련번호, IMEI, 전화번호, UDID 등), 설치된 앱 목록, 백업 시각, 백업 호스트 macOS 버전을 포함. 사람이 읽을 수 있는 평문 XML로 저장되며 포렌식에서 기기 소유자·사용자 식별에 핵심적.

## 구조
XML 1.0 기반, UTF-8 인코딩. `<plist version="1.0">` 상위 태그 아래 `<dict>` 로 키-값 쌍 구성.

## 추출 방법
- 경로: `~/Library/Application Support/MobileSync/Backup/[HASH]/Info.plist`
- 텍스트 편집기로 직접 열람 가능
- `plutil -p Info.plist` 로 macOS에서 가독성 높은 출력

## 분석 포인트

### 기기 식별정보
| 키 | 설명 | 포렌식 활용 |
|----|------|------------|
| Device Name / Display Name | iTunes/Finder 표시 기기명 | 사용자 설정 기기명 |
| Product Name | 제품명 (iPhone 6s 등) | 기기 모델 식별 |
| Product Type | 하드웨어 모델 식별자 (iPhone8,1 등) | 정확한 기기 세대 파악 |
| Serial Number | 기기 일련번호 | Apple 제품 고유 식별 |
| IMEI / MEID | 이동통신 장비 식별자 | 통신사 협조 수사 |
| ICCID | SIM 카드 식별번호 | SIM 카드 추적 |
| Phone Number | 기기 전화번호 (+82 10-...) | 사용자 연락처 식별 |
| Unique Identifier | 기기 UDID (40자 16진수) | 백업 폴더명과 일치 |
| GUID | 백업 세션 GUID | 백업 세션 추적 |
| Product Version / Build Version | iOS 버전 | 환경 재구성 |

### 설치 앱 정보
- **Installed Applications**: 설치된 타사 앱 번들 ID 배열. 앱 사용 여부 신속 파악
- **Applications[번들ID]**: `ApplicationSINF`(서명), `iTunesMetadata`(앱 메타데이터 XML), `PlaceholderIcon`(PNG) 포함
- `iTunesMetadata` 는 XML plist로 앱 구매·버전 정보 포함

### 기타
- **Last Backup Date**: 마지막 백업 완료 시각 (UTC). 여러 백업 간 시간 순서 확정
- **macOS Version / Build Version**: 백업을 수행한 호스트 시스템 정보. 분석 환경 재현에 참고
- **iTunes Files**: iTunes 동기화 설정 (계정 사이닝, 환경설정, 음성메모 설정)

### 암호화 백업에서의 평문 접근성
Info.plist는 **암호화 백업을 수행해도 평문으로 저장**됨. Manifest.db·00~ff 파일들이 암호화되어도 기기 식별정보(전화번호, 일련번호, IMEI, UDID)와 설치 앱 목록은 암호화되지 않은 상태로 획득 가능 → 암호화 백업에서도 최초 분석 시 Info.plist를 가장 먼저 확인할 것.

## Findings
- Info.plist는 평문 XML plist로 기기 식별정보 15종 이상 포함 — 기기 소유자·사용자 추적에 핵심 (→ [[papers/kimyonghyun_2026]])
- Installed Applications 필드로 타사 앱 9종 식별 가능, Applications 딕셔너리로 앱별 서명·메타데이터 접근 (→ [[papers/kimyonghyun_2026]])
- Last Backup Date로 최종 백업 시점 확정, 여러 백업 간 순서 비교에 활용 (→ [[papers/kimyonghyun_2026]])
- 암호화 백업에서도 Info.plist는 평문 저장 — 기기 식별정보(전화번호·일련번호·IMEI·UDID)는 암호화되지 않아 암호화 백업 최초 분석의 핵심 진입점 (→ [[papers/kimyonghyun_2026]] Q&A Q10)

## 관련 페이지
[[artifacts/iOS_Backup_Manifest_db]] · [[artifacts/iOS_Backup_Manifest_plist]] · [[artifacts/iOS_Backup_Status_plist]] · [[techniques/iOS_Backup_Forensics]]
