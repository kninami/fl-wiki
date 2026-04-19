---
artifact: ICS PLC Network Traffic
layer: APP
data_types:
  - PLC 제어 로직 바이너리 (업로드/다운로드 패킷)
  - 세션 ID / Transaction ID
  - 메모리 주소 및 바이트 크기
  - Modbus 요청·응답 메시지
action_types:
  - 제어 로직 다운로드 (공학용 소프트웨어 → PLC)
  - 제어 로직 업로드 (PLC → 공학용 소프트웨어)
os_version: ICS 환경 (Schneider Electric Modicon M221 등)
path: "네트워크 캡처 파일 (PCAP), Modbus TCP 포트 502"
attck: —
sources:
  - qasim_2020
updated: 2026-04-19
---

## 개요
ICS(산업제어시스템) 환경에서 엔지니어링 소프트웨어와 PLC 간 네트워크 트래픽(PCAP). 제어 로직이 바이너리 청크로 전송되며, 캡처된 PCAP에 악성 제어 로직 주입 공격의 포렌식 증거가 포함됨. Reditus 프레임워크를 통해 PCAP에서 제어 로직 소스코드 자동 복구 가능.

## 구조 (Modbus 기준)
- **다운로드 요청** (공학용 소프트웨어 → PLC): 제어 로직 바이너리 청크 + PLC 메모리 주소 + Modbus Function Code
- **업로드 요청** (공학용 소프트웨어 → PLC): 특정 메모리 주소의 제어 로직 청크 읽기 요청
- **업로드 응답** (PLC → 공학용 소프트웨어): 해당 주소의 제어 로직 바이너리
- 청크 최대 크기: 236바이트; 다운로드와 업로드 시 동일 메모리 주소·바이트 수 사용

## 수집 방법
- 엔지니어링 네트워크 구간(Control LAN)에서 패킷 캡처 (예: Wireshark/tcpdump)
- PCAP 파일로 저장 후 Reditus 등 분석 도구에 입력

## 분석 포인트 (Reditus 프레임워크)
1. **학습 단계**: 동일 제어 로직의 서로 다른 세션 PCAP 2쌍을 비교 → 세션 종속 필드(Transaction ID 등) 자동 식별
2. **업로드 템플릿 생성**: 다운로드·업로드 PCAP 대조로 제어 로직 위치·정적/동적 필드 학습
3. **테스트 단계**: 가상 PLC가 공학용 소프트웨어의 업로드 요청에 응답 → 제어 로직 자동 디컴파일
- 결과물: IEC 61131-3 호환 고수준 소스코드 (Instruction List 또는 Ladder Logic)

## Findings
- Reditus: PCAP에서 수동 역공학 없이 40개 제어 로직 프로그램 100% 정확도로 복구 (213 rung, 888 instruction) (→ [[papers/qasim_2020]])
- 다운로드·업로드 패킷이 동일 메모리 주소·바이트 수를 사용 → 다운로드 PCAP만으로 업로드 응답 템플릿 생성 가능 (→ [[papers/qasim_2020]])
- `"car_assistant"` 마커와 유사하게, Redbus는 `0x0080` 주소부터 다운로드·`0xd4fe` 주소부터 업로드라는 고정 패턴으로 제어 로직 위치 특정 (→ [[papers/qasim_2020]])

## 관련 페이지
[[techniques/ICS_Control_Logic_Forensics]]
