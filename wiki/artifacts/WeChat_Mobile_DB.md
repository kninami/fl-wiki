---
artifact: WeChat_Mobile_DB
layer: APP
data_types:
  - 메시지 내용 및 타입
  - 통화 기록
  - 위치 정보
  - Wi-Fi 연결 기록
  - 앱 설치 정보
action_types:
  - 메시지 송수신
  - 파일 공유
  - 위치 공유
  - 음성/영상 통화
os_version: Android (WeChat Mobile v7.0.10+)
path: "/data/data/com.tencent.mm/"
attck:
sources:
  - park_2020
updated: 2026-04-15
---

## 개요
WeChat Android 앱의 로컬 SQLite 데이터베이스. A/B 그룹에 따라 다른 AES-256-CBC 키 도출 방식을 사용. 통화·위치·Wi-Fi 등 PC 버전보다 풍부한 정보 포함.

## 추출 방법
- **주요 경로**: `/data/data/com.tencent.mm/` (루팅 기기 또는 ADB backup)
- **미디어 경로**: `/sdcard/tencent/MicroMsg/Wechat/`
- **암호화 키 도출**:
  - A-group: `Left7(MD5(UIN || IMEI))`
  - B-group: `Left7(MD5(UIN || IMEI || Account || WeChat_ID))`
  - UIN: WeChat 내부 숫자 ID; IMEI: 기기 고유 식별자

## 분석 포인트

### 주요 DB
| DB | 내용 |
|----|------|
| MicroMsg.db | Session, ChatRoom, MSG 테이블 |
| FTSMSG0.db | 전문 검색 인덱스 |
| MSG0.db | 메시지 상세 정보 |

### Mobile 단독 획득 가능 정보
- 통화 기록 (음성/영상)
- 타임캡슐(Moments) 정보
- Wi-Fi 연결 이력
- 앱 설치 정보

### A/B 그룹 구분
- B-group은 키 도출에 Account·WeChat_ID 추가 필요 → 더 복잡한 키 획득 절차

## Findings
- A/B-group 키 도출 공식 확인; Mobile은 통화·위치·Wi-Fi 등 PC 미수록 정보 포함 (→ [[papers/park_2020]])
- B-group: `Left7(MD5(UIN||IMEI||Account||WeChat_ID))` (→ [[papers/park_2020]])

## 관련 페이지
[[artifacts/WeChat_PC_DB]] · [[techniques/WeChat_Artifact_Analysis]]
