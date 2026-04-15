---
technique: Hibernation_Forensics
perspective: Investigator
topic: Memory Forensics
related_artifacts:
  - hiberfil_sys
related_crime:
  - 데이터 은닉
  - 증거 은닉
sources:
  - bang_2022
updated: 2026-04-15
---

## 개요
Windows 최대 절전(Hibernate) 또는 Fast Startup 기능이 활성화된 시스템에서 생성되는 hiberfil.sys 파일을 분석하여 종료 직전 메모리 상태(프로세스, 열린 파일, 캐시 등)를 복원하는 기법.

## 메커니즘
- hiberfil.sys는 루트 디렉토리에 은닉된 시스템 파일
- **최대 절전(Hibernate)**: 커널 + 사용자 공간 메모리 전체 저장
- **Fast Startup + 종료**: 커널 메모리만 저장 (사용자 세션 제외)
- 헤더(PO_MEMORY_IMAGE) Signature로 두 유형 구분: "HIBR"=최대 절전, "RSTR"=Fast Startup

## 분석 방법
1. **파일 추출**: FTK Imager로 이미지 취득 또는 `C:\hiberfil.sys` 직접 접근
2. **Volatility 3 분석**:
   - `--single-location file:///C:/hiberfil.sys`
   - `pslist`: 종료 직전 실행 프로세스 목록
   - `windows.info`: 시스템 정보(OS 버전, 빌드 시각)
   - `ChromeHistoryView`: 브라우저 내 웹 기록 복원
   - Pool Tags 분석(`FAT: "Fatn"`): 파일 시스템 드라이버 상태 확인
3. **파일 복사 행위 추적**: pslist + 열린 파일 목록 교차 분석

## 한계 & 주의사항
- Fast Startup 환경에서는 사용자 공간 데이터(브라우저, 문서 등) 복원 불가
- 최대 절전 기능이 비활성화된 경우 파일 미존재
- Signature "RSTR"이면 메모리 내용 제한적 — 분석 전 유형 확인 필수

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/bang_2022]] | Fast Startup/최대 절전 환경별 저장 범위 실험; 파일 복사 행위 추적 성공 사례 |

## 관련 페이지
[[artifacts/hiberfil_sys]]
