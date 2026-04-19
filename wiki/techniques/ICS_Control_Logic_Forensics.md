---
technique: ICS Control Logic Forensics
perspective: Investigator
topic: Extraction / Analysis
attck: —
related_artifacts:
  - ICS PLC Network Traffic
related_crime:
  - 산업 제어 시스템 공격 수사
  - 제어 로직 주입 공격 분석
  - 사이버물리 공격 포렌식
sources:
  - qasim_2020
updated: 2026-04-19
---

## 개요
ICS 네트워크 트래픽(PCAP)에서 PLC에 주입된 악성 제어 로직을 자동 복구하는 포렌식 방법론. Reditus 프레임워크는 엔지니어링 소프트웨어의 내장 디컴파일러를 활용해 수동 역공학 없이 바이너리 제어 로직을 IEC 61131-3 고수준 소스코드로 변환.

## 메커니즘
ICS 공격(예: Stuxnet)은 엔지니어링 소프트웨어로 PLC에 악성 제어 로직을 다운로드. 이 전송 과정이 네트워크 트래픽(PCAP)으로 캡처되면 포렌식 증거가 됨.

**핵심 관찰**: 엔지니어링 소프트웨어(예: SoMachine Basic)가 다운로드·업로드 시 동일한 메모리 주소와 바이트 수를 사용 → 다운로드 PCAP으로 업로드 응답 메시지 재생성 가능.

## Reditus 프레임워크 절차

### 학습 단계 (사전 준비)
1. 동일 제어 로직의 서로 다른 세션 양성 PCAP 2쌍 준비
2. **세션 종속 필드 발견**: Pairing → Grouping → 차분 분석 → 규칙 추출 (Transaction ID 등 세션마다 변하는 필드 자동 식별)
3. **업로드 템플릿 생성**: 업로드+다운로드 PCAP 쌍에서 Static/Dynamic/Control Logic 필드 구분; LCS(최장공통부분수열)로 제어 로직 위치 특정

### 테스트 단계 (실제 분석)
1. 수사 대상 PCAP(목표 PLC 트래픽) 입력
2. 가상 PLC가 엔지니어링 소프트웨어의 업로드 요청을 수신
3. 데이터베이스에서 가장 유사한 요청 메시지를 찾아 응답 생성 (세션 종속 필드 갱신)
4. 엔지니어링 소프트웨어의 내장 디컴파일러가 응답을 자동 소스코드로 변환
5. 결과: IEC 61131-3 Ladder Logic 또는 Instruction List 소스코드

## 한계 & 주의사항
- SoMachine Basic + Modicon M221 / Modbus 프로토콜에 특화; 다른 ICS 프로토콜(PCCC 등)은 별도 학습 데이터 필요
- 다운로드 PCAP에 일부 메시지 누락 시 업로드 응답 생성 실패 가능 (40개 실험에서 파일당 1개 메시지 누락 관찰)
- 네트워크 트래픽 암호화 시 적용 불가

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/qasim_2020]] | Reditus 100% 정확도 검증 (40개 프로그램, 213 rung, 888 instruction); 완전 자동화 달성 |

## 관련 페이지
[[artifacts/ICS_PLC_Network_Traffic]]
