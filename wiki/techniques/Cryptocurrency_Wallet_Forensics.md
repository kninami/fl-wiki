---
technique: Cryptocurrency_Wallet_Forensics
perspective: Investigator
topic: Financial Forensics
related_artifacts:
  - Cryptocurrency_Wallet_Files
related_crime:
  - 가상자산 범죄
  - 자금세탁
  - 랜섬웨어 수사
sources:
  - park_2022b
updated: 2026-04-15
---

## 개요
Windows 10 환경에서 실행되는 암호화폐 지갑 앱의 로컬 저장 파일을 분석하여 복호화 없이 지갑 주소·잔액·트랜잭션 내역을 획득하는 기법.

## 메커니즘

### 주요 지갑 앱별 저장 경로 및 형식
| 지갑 앱 | 저장 경로 | 형식 |
|---------|----------|------|
| Atomic | `%AppData%\Roaming\Atomic\Local Storage\leveldb\*.ldb` | LevelDB |
| Exodus | `%AppData%\Roaming\Exodus\...` | LevelDB/SQLite |
| Electrum | `%AppData%\Roaming\Electrum\wallets\` | JSON |
| Bitcoin Core | `%AppData%\Roaming\Bitcoin\wallet.dat` | BerkeleyDB |
| Coinomi | `%LocalAppData%\Coinomi\...` | SQLite |

### 복호화 없이 획득 가능한 정보 (Table 5)
- 지갑 주소(공개 키)
- 코인 잔액 일부
- 트랜잭션 내역 일부
- 연결 서버 정보

### 파일 시그니처 기반 식별 (Table 6)
- 지갑 파일 고유 시그니처로 이미지 내 지갑 파일 자동 탐지 가능
- 카빙 기법 적용 가능

## 분석 절차
1. 지갑 앱 설치 경로 탐색 (`%AppData%\Roaming`, `%LocalAppData%` 기준)
2. 파일 시그니처로 지갑 파일 식별
3. LevelDB/SQLite 파싱 도구로 키-값 추출
4. 지갑 주소 추출 → 블록체인 익스플로러 조회

## 한계 & 주의사항
- 특정 버전 지갑 앱 10종 한정 — 미분석 앱 다수 존재
- 암호화된 지갑(패스프레이즈 설정 시) 복호화 필요
- 트랜잭션 내역 전체는 블록체인 네트워크 조회 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/park_2022b]] | 10종 지갑 앱 저장 경로·파일 형식 체계화; 복호화 없이 획득 가능 정보 범위 식별; 통합 수사 도구 설계 제안 |

## 관련 페이지
[[artifacts/Cryptocurrency_Wallet_Files]]
