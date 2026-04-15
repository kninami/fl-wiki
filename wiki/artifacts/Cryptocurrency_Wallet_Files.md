---
artifact: Cryptocurrency_Wallet_Files
layer: APP
data_types:
  - 지갑 주소
  - 잔액 정보
  - 트랜잭션 내역
  - 개인키 (암호화)
  - 계정 메타데이터
action_types:
  - 암호화폐 송수신
  - 지갑 생성·백업
os_version: Windows 10+
path: "%AppData%\\Roaming\\[지갑앱명]"
attck:
sources:
  - park_2022b
updated: 2026-04-15
---

## 개요
Windows에 설치된 암호화폐 지갑 소프트웨어가 생성하는 로컬 파일 아티팩트. 지갑마다 저장 경로·형식이 다르며 LevelDB, SQLite, JSON 등 다양한 형태를 사용한다.

## 추출 방법
지갑별 주요 경로 (Windows 10 기준):
| 지갑 | 경로 |
|------|------|
| Atomic | `%AppData%\Roaming\Atomic\Local Storage\leveldb\*.ldb` |
| Bitcoin Core | `%AppData%\Roaming\Bitcoin\wallet.dat` |
| Exodus | `%AppData%\Roaming\Exodus\` |
| Electrum | `%AppData%\Roaming\Electrum\wallets\` |
| Guarda | `%AppData%\Roaming\Guarda\` |

## 분석 포인트
- **복호화 없이 획득 가능한 정보** (Table 5): 지갑 주소, 일부 트랜잭션 내역, 계정 이름, 잔액 스냅샷
- **파일 시그니처 기반 식별** (Table 6): 지갑 파일 고유 매직 바이트로 종류 식별 가능
- LevelDB `.ldb` 파일은 일반 텍스트 에디터로도 일부 내용 확인 가능
- 개인키는 암호화 저장 → 패스워드 없이 접근 불가 (brute-force 대상)

## 공격 기법 (해당 시)
악성코드를 통한 지갑 파일 탈취 → 원격지로 전송하여 복호화 시도.

## Findings
- 10종 지갑 앱 모두 `%AppData%\Roaming` 하위에 주요 파일 저장 확인 (→ [[park_2022b]])
- 복호화 없이도 지갑 주소·잔액 등 핵심 정보 일부 획득 가능 (→ [[park_2022b]])
- 통합 탐지·추출 도구 부재 → 수동 수집 필요 (→ [[park_2022b]])

## 관련 페이지
[[techniques/Cryptocurrency_Wallet_Forensics]] · [[papers/park_2022b]]
