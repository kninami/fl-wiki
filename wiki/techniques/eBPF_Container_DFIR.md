---
technique: eBPF-based Container DFIR
perspective: Investigator
topic: Detection
attck: —
related_artifacts: []
related_crime:
  - 클라우드 인프라 침해
  - 랜섬웨어
  - 가상화폐 채굴 멀웨어
  - 공급망 공격
sources:
  - jin_2025
updated: 2026-04-15
---

## 개요
Windows 오케스트레이션 환경(Kubernetes 등)에서 eBPF를 활용해 커널 레벨 이벤트를 실시간 수집하는 DFIR 프레임워크. 단명 컨테이너의 포렌식 공백을 해결하고 NIST SP 800-61 인시던트 대응 라이프사이클에 통합.

## 메커니즘

**수집 이벤트 4종:**
| 유형 | 수집 정보 | IRP/API |
|------|----------|---------|
| 프로세스 | PID, PPID, 실행파일, 커맨드라인, 생성/종료 시각 | PPS_CREATE_NOTIFY_INFO |
| 파일 I/O | 파일명, 경로, 생성/접근/수정 시각, 크기, PID | IRP_MJ 코드 기반 |
| 레지스트리 | 키 경로, 값, 데이터 타입, PID | REG_NOTIFY_CLASS |
| 네트워크 | src/dst IP, 포트, 인터페이스, 프로토콜 | ICMP/TCP/UDP |

**아키텍처:**
- eBPF Shim(ntosebpfext 기반 커널 드라이버) → eBPF 프로그램에 hook point 제공
- Collector(user-space) → eBPF 이벤트 수집·JSON 변환·포워딩
- OpenSearch + 중앙 집중 로그 분석 대시보드

**Pod/Container 매핑:**
- 프로세스 계층 구조(PPID 추적)로 특정 Pod/Container에 이벤트 귀속
- containerd-shim-runhcs-v1.exe 식별로 Windows 컨테이너 프로세스 특정

## 탐지 방법 (Investigator)
**가상화폐 채굴 탐지:**
- 비정상 프로세스(calico-code.exe) 생성 → 부모 PID로 Pod 특정
- 네트워크 이벤트에서 외부 마이닝 풀 연결 탐지

**랜섬웨어 탐지:**
- 다수 파일에 대한 READ→WRITE 패턴 (암호화)
- 파일 확장자 변경 이벤트 + 레지스트리 지속성 키 쓰기

**BSOD 공격 탐지:**
- 커널 드라이버 로딩 이벤트 모니터링
- 비정상 드라이버 로딩 경로 탐지

**4단계 대응 (NIST SP 800-61):**
1. Preparation: eBPF-for-DFIR + DaemonSet 배포
2. Detection & Analysis: 실시간 이벤트 모니터링
3. Containment & Recovery: 감염 Pod 격리, 네트워크 차단
4. Post-Incident: 수집 로그로 공격 체인 재구성

## 한계 & 주의사항
- Windows eBPF는 Linux보다 성숙도 낮음; 커널 드라이버 서명 필요
- Hyper-V 환경에서만 테스트; bare-metal 검증 필요
- 기존 오프라인 포렌식(디스크 이미징) 대체 불가 → 보완적 사용
- 단명 컨테이너의 경우 이미지 삭제 후 일부 아티팩트 손실

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/jin_2025]] | Windows eBPF 기반 실시간 포렌식 프레임워크; 3가지 공격 시나리오 탐지 검증; eBPF-for-DFIR 오픈소스 |

## 관련 페이지
[[techniques/DFIR_Tool_Error_Mitigation]]
