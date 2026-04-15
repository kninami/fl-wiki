---
technique: WeChat_Artifact_Analysis
perspective: Investigator
topic: IM Forensics
related_artifacts:
  - WeChat_PC_DB
  - WeChat_Mobile_DB
related_crime:
  - 통신 기록 확보
  - 범죄 조직 수사
  - 디지털성범죄 수사
sources:
  - park_2020
updated: 2026-04-15
---

## 개요
WeChat PC/Android 환경의 암호화된 SQLite DB 구조를 역분석하고 복호화 키를 획득하여 메시지·파일·위치 정보를 복원하는 기법. PC는 메모리에서, Mobile은 UIN+IMEI 조합으로 키를 도출.

## 메커니즘

### PC 버전 복호화
- **DB 경로**: `%USERPROFILE%/Document/Wechat Files/[wxid]/FileStorage/Msg/Multi/`
- **암호화**: AES-256-CBC
- **키 위치**: 로그인 시 메모리에만 존재 (256-bit hex 문자열)
- **획득 방법**: 메모리 덤프 후 키 문자열 검색

### Mobile 버전 복호화
- **DB 경로**: `/data/data/com.tencent.mm/` (주요 데이터)
- **A-group 키 도출**: `Left7(MD5(UIN || IMEI))`
- **B-group 키 도출**: `Left7(MD5(UIN || IMEI || Account || WeChat_ID))`
- UIN: WeChat 내부 숫자 ID; IMEI: 기기 고유 식별자

### 주요 DB 및 테이블
| DB | 테이블 | 내용 |
|----|--------|------|
| MicroMsg.db | Session | 대화 목록 |
| MicroMsg.db | ChatRoom | 단체 채팅방 정보 |
| MicroMsg.db | MSG | 메시지 기본 정보 |
| MSG0.db | — | 메시지 상세 내용 |
| FTSMSG0.db | — | 전문 검색 인덱스 |

### 메시지 타입 코드
| 코드 | 유형 |
|------|------|
| 1 | 텍스트 |
| 3 | 사진 |
| 48 | 위치 |
| 49 | 파일 |
| 50 | 통화 |
| -1879048186 | 실시간 위치 |

## PC vs Mobile 차별 정보
- **PC 단독**: 프로필 이미지, 그룹 채팅방명, 최신 메시지
- **Mobile 단독**: 통화 기록, 타임캡슐, Wi-Fi 연결 정보, 앱 설치 정보

## 한계 & 주의사항
- PC DB 복호화에 반드시 메모리 접근 필요 (라이브 시스템 또는 메모리 덤프)
- PC v2.8.0 / Mobile v7.0.10 기준 — 업데이트 시 키 도출 방식 변경 가능
- B-group 계정의 경우 WeChat_ID까지 수집 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/park_2020]] | WeChat PC AES-256-CBC 키 메모리 위치 확인; Mobile A/B-group 키 도출 공식; 메시지 타입 분류 체계 |

## 관련 페이지
[[artifacts/WeChat_PC_DB]] · [[artifacts/WeChat_Mobile_DB]] · [[techniques/Win8_IM_App_Forensics]] · [[techniques/Slack_Discord_IM_Forensics]]
