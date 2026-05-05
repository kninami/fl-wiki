---
technique: SOLVE-IT Knowledge Base
perspective: Both
topic: Research Methodology
attck: —
related_artifacts: []
related_crime:
  - 디지털 증거 무결성 검증
  - 수사 품질 보증
sources:
  - hargreaves_2025
updated: 2026-05-03
---

## 개요
MITRE ATT&CK에서 영감을 받아 디지털 포렌식 기법·취약점·완화책을 체계적으로 색인·공유하는 지식 베이스(SOLVE-IT). 수사 과정에서 발생 가능한 오류를 사전에 식별하고 완화책을 적용함으로써 증거의 신뢰성과 수사 품질을 높이는 것을 목표로 한다.

## 메커니즘

**SOLVE-IT 구조 (ATT&CK 매핑):**
| ATT&CK 개념 | SOLVE-IT 대응 |
|-------------|---------------|
| Tactics (목표) | Objectives (17개 조사 목표) |
| Techniques | Digital Forensic Techniques (104개) |
| Sub-techniques | Sub-techniques (일부 기법에 포함) |

**기법(Technique) 필드:**
- ID (T1001–), 이름, 설명, 동의어, 상세, 하위기법
- 예시 (데이터셋, 도구, 실제 사례)
- 취약점(Weaknesses), CASE 출력 엔티티, 참조

**취약점(Weakness) 분류:**
| 코드 | 의미 |
|------|------|
| INCOMP | 불완전 — 정보 일부 누락 |
| INAC-EX | 부정확:존재 — 존재하지 않는 항목을 존재로 처리 |
| INAC-AS | 부정확:연결 — 잘못된 속성·유형 부여 |
| INAC-ALT | 부정확:대안 — 다른 유효한 결과를 무시 |
| INAC-COR | 부정확:내용 — 잘못된 값 |
| MISINT | 오해석 — 올바른 데이터를 잘못 분류 |

**완화책(Mitigation) 필드:**
- ID (M1001–), 이름, 상세 설명, 연관 기법(선택), 참조

## 활용 방법 (Investigator)

**품질 보증 (QA) 워크시트 생성:**
- `generate_case_evaluation.py` 스크립트로 수사에 사용된 기법 ID 입력 → 해당 기법의 취약점·완화책 목록 자동 생성
- 조사 중 각 완화책 적용 여부를 체크하여 미적용 항목 식별

**오류 중심 데이터셋 설계:**
- 특정 기법의 취약점 목록을 참고하여 테스트 데이터셋에 포함해야 할 엣지 케이스 도출
- e.g. T1072(Chat app examination) → W1085~W1102 18개 취약점 → 이를 검증하는 데이터셋 구성

**CASE Ontology 연동:**
- 기법 출력 결과를 CASE 클래스로 표현 가능 (e.g. T1072 → observable:Message, observable:SMSMessage)
- 온톨로지 갭 식별 — SOLVE-IT에 존재하나 CASE에 없는 클래스 발굴

**AI 활용 가이드:**
- 104개 기법을 AI 적용 가능 여부로 분류 (51개 Non-AI, 26개 적용 가능, 11개 학술 구현, 5개 도구 내 구현)
- 특정 기법에서 AI가 해결 가능한 취약점 사전 식별

## 한계 & 주의사항
- 104개 기법은 초기 버전 — 실제 디지털 포렌식 기법 전체를 커버하지 못함
- 기법 분류(목표별)가 발표·교육 편의성에 따른 것으로, 특정 프레임워크에 종속되지 않음
- 일부 취약점에 완화책이 없음 — 이는 연구 공백을 나타냄
- Crowdsourcing 품질관리 체계 아직 미성숙

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/hargreaves_2025]] | SOLVE-IT 설계·구현; 104 기법/156 취약점/108 완화책 색인; QA·AI 활용·CASE 연동 사례 실증 |

## 관련 페이지
[[techniques/DFIR_Tool_Error_Mitigation]] · [[techniques/Artifact_Knowledge_Management]] · [[techniques/Selective_Seizure_Evaluation]]
