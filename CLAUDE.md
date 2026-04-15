# FL-Wiki — LLM-Powered Persistent Wiki for Digital Forensic Knowledge
# Schema & Workflow Guide

이 파일은 FL-Wiki의 구조, 컨벤션, 워크플로를 정의한다.
Claude Project Instructions에 등록되어 있으며, 모든 작업의 기준이 된다.
사용자와 Claude가 함께 진화시키는 living document다.

---

## 아키텍처 개요

```
FL-Wiki/
├── raw/                  # 원본 소스 (immutable · source of truth)
│   ├── vanini_2025.pdf
│   ├── domingues_2024.pdf
│   └── ...
│
├── wiki/                 # Claude가 생성·관리 (사용자는 읽기만)
│   ├── artifacts/        # 아티팩트 페이지 ($MFT, $USNjrnl ...)
│   ├── techniques/       # 공격·탐지·분석 기법 페이지
│   ├── papers/           # 논문별 요약 페이지
│   └── meta/
│       ├── index.md      # 전체 페이지 목록 + 한 줄 요약
│       └── log.md        # 작업 이력 (ingest/query/lint)
│
└── CLAUDE.md        # 이 파일 (schema)
```

**핵심 원칙:**
- `raw/` 는 변경하지 않는다. Claude는 읽기만 한다.
- `wiki/` 는 Claude가 소유한다. 사용자는 읽고 탐색만 한다.
- Obsidian = IDE / Claude = 프로그래머 / wiki = 코드베이스

---

## 페이지 타입 & 컨벤션

### 1. Artifact 페이지 (`wiki/artifacts/`)

아티팩트 하나 = 페이지 하나. 논문이 쌓일수록 내용이 업데이트된다.

**파일명:** `$MFT.md`, `$USNjrnl.md`, `WebAuthN_EVTX.md`

**YAML frontmatter:**
```yaml
---
artifact: $MFT
layer: FS                         # FS / OS / APP / REG
data_types:
  - Timestamp
  - Metadata
  - File record
action_types:
  - 파일 생성
  - 파일 수정
  - 파일 접근
  - Timestomping
os_version: Windows 10+
path: "C:\\$MFT"
attck: T1070.006                  # 관련 ATT&CK ID (있을 때만)
sources:
  - vanini_2025
  - palmbach_2020
updated: 2026-04-11
---
```

**본문 구조:**
```markdown
## 개요
이 아티팩트가 무엇이고 무엇을 담고 있는지. 불변 스펙.

## 추출 방법
경로, 도구, OS 버전별 차이.

## 분석 포인트
이 아티팩트로 무엇을 알 수 있는가.

## 공격 기법 (해당 시)
어떻게 조작되는가. [[기법 페이지]] 링크.

## Findings
논문별로 발견된 것들. 새 논문 ingest 시 append.
- [[SI_FN_Mismatch]] — Timestomping 탐지 기준 (→ [[vanini_2025]])
- 공격자 대부분 $FILE_NAME 변조 누락 (→ [[vanini_2025]])
- Tamper resistance: 지식 장벽이 요인 (→ [[vanini_2025]])

## 관련 페이지
[[Timestomping]] · [[SI_FN_Mismatch]] · [[Tamper_Resistance_Factors]]
```

---

### 2. Technique 페이지 (`wiki/techniques/`)

공격 기법, 탐지 방법, 분석 기법 하나 = 페이지 하나.

**파일명:** `Timestomping.md`, `SI_FN_Mismatch.md`, `Tamper_Resistance_Factors.md`

**YAML frontmatter:**
```yaml
---
technique: Timestomping
perspective: Attacker             # Attacker / Investigator / Both
topic: Indicator Removal          # ATT&CK 기준 (Attacker) 또는 Detection/Extraction/Analysis
attck: T1070.006
related_artifacts:
  - $MFT
  - $LogFile
related_crime:
  - 증거인멸
  - 사이버범죄 수사
sources:
  - vanini_2025
  - palmbach_2020
updated: 2026-04-11
---
```

**본문 구조:**
```markdown
## 개요
이 기법이 무엇인지 한 단락으로.

## 메커니즘
어떻게 동작하는가. 직관적으로 설명.

## 탐지 방법 (Investigator)
어떻게 찾는가. 교차 검증 포인트.

## 한계 & 주의사항
오탐 가능성, 일반화 한계.

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[vanini_2025]] | 참가자 대부분 $FILE_NAME 누락 |
| [[palmbach_2020]] | NTFS timestamp 신뢰도 분석 |

## 관련 페이지
[[artifacts/$MFT]] · [[SI_FN_Mismatch]] · [[Tamper_Resistance_Factors]]
```

---

### 3. Paper 페이지 (`wiki/papers/`)

논문 하나 = 페이지 하나. ingest 시 생성, 이후 수정 없음.

**파일명:** `vanini_2025.md`, `domingues_2024.md`
- 파일명은 `저자성_연도` 형식 유지 — 링크·인용 일관성 보장
- Obsidian Graph View 표시 이름은 `title` 필드 사용 — 키워드 노드와 자연스럽게 섞임

**YAML frontmatter:**
```yaml
---
paper: vanini_2025
title: "Timestamp Tampering Strategies in Live Systems (2025)"
full_title: "Understanding Strategies and Challenges of Timestamp Tampering..."
authors: "Vanini, C.; Gruber, J.; Hargreaves, C.; et al."
year: 2025
venue: DFDS 2025
doi: https://doi.org/10.1145/3712716.3712727
tags:
  - Timestomping
  - live tampering
  - user study
  - NTFS
method: qualitative user study     # 연구 방법론
limitations: "n=10, 단일 시나리오"
---
```

**본문 구조:**
```markdown
## 한 줄 요약
10명 대학원생 대상 live timestamp tampering user study.

## 핵심 발견
- 공격자는 APP → OS → FS 순으로 하향 조작
- $FILE_NAME 변조 누락 경향
- Tamper resistance 6요인 도출

## 직접 분석한 아티팩트
[[artifacts/$MFT]] · [[artifacts/$USNjrnl]] · [[artifacts/$LogFile]]

## 영향받은 wiki 페이지
[[techniques/Timestomping]] · [[techniques/SI_FN_Mismatch]] · [[techniques/Tamper_Resistance_Factors]]
```

---

## 작업 절차 (Operations)

### Ingest — 새 논문 추가

사용자가 할 일: `raw/` 에 PDF 저장 후 Claude에게 아래 명령.

```
/ingest raw/vanini_2025.pdf
```

Claude가 할 일 (순서대로):
1. PDF 읽기 → 핵심 내용 파악
2. `wiki/papers/vanini_2025.md` 생성
3. 직접 분석된 아티팩트 식별 → 각 artifact 페이지 업데이트 또는 신규 생성
4. 새로 등장한 기법 식별 → technique 페이지 업데이트 또는 신규 생성
5. 기존 페이지와 모순·보완 관계 확인 → 해당 페이지에 반영
6. `wiki/meta/index.md` 업데이트
7. `wiki/meta/log.md` 에 ingest 기록 추가

**판단 기준:**
- 논문이 직접 실험·분석한 아티팩트만 artifact 페이지에 추가
- 배경·언급만 된 것은 technique 페이지 관련 항목에만 링크
- 기존 페이지와 모순되면 해당 내용 옆에 `> ⚠ 모순: [[새_논문]]에서 반박` 표시

---

### Query — 질의응답

사용자가 할 일: 자연어로 질문.

```
$USNjrnl 관련 수사 방법을 알려줘
디지털성범죄 압수수색 시 확인해야 할 아티팩트는?
Timestomping 탐지를 우회하는 공격 방법은?
```

Claude가 할 일:
1. `wiki/meta/index.md` 읽기 → 관련 페이지 파악
2. 관련 artifact / technique / paper 페이지 읽기
3. 인용과 함께 답변 합성
4. 좋은 답변은 새 technique 페이지로 저장 (탐색 자체가 wiki에 축적)

---

### Lint — wiki 건강 점검

주기적으로 실행. 사용자가 할 일:

```
/lint
```

Claude가 점검할 항목:
- 페이지 간 모순 (최신 논문이 기존 주장을 반박하는 경우)
- 인바운드 링크 없는 고아 페이지
- sources 목록에는 있지만 paper 페이지가 없는 논문
- artifact 페이지에 Findings 섹션이 비어있는 경우
- technique 페이지에 관련 artifact 링크가 누락된 경우
- ATT&CK ID가 있는 technique 중 공식 ATT&CK 매핑이 안 된 것

---

## index.md 구조

`wiki/meta/index.md` 는 Claude가 query 시 가장 먼저 읽는 파일.

```markdown
# Forensic Memex Wiki Index
_Last updated: 2026-04-11 | Artifacts: 13 | Techniques: 8 | Papers: 3_

## Artifacts
| 페이지 | Layer | 주요 기법 | Sources |
|--------|-------|-----------|---------|
| [[artifacts/$MFT]] | FS | Timestomping, SI/FN Mismatch | vanini_2025, palmbach_2020 |
| [[artifacts/$USNjrnl]] | FS | Change log 분석 | vanini_2025 |
| [[artifacts/WebAuthN_EVTX]] | OS | FIDO2 패스키 추적 | domingues_2024 |

## Techniques
| 페이지 | Perspective | ATT&CK | Sources |
|--------|-------------|--------|---------|
| [[techniques/Timestomping]] | Attacker | T1070.006 | vanini_2025, palmbach_2020 |
| [[techniques/SI_FN_Mismatch]] | Investigator | — | vanini_2025 |

## Papers
| 페이지 | 연도 | 방법론 | 핵심 주제 |
|--------|------|--------|-----------|
| [[papers/vanini_2025]] | 2025 | User study | Timestomping live tampering |
| [[papers/domingues_2024]] | 2024 | 실험 | FIDO2 passkey artifacts |
```

---

## log.md 구조

```markdown
# Forensic Memex Wiki Log

## [2026-04-11] ingest | vanini_2025
- 생성: papers/vanini_2025.md
- 업데이트: artifacts/$MFT.md, artifacts/$USNjrnl.md, artifacts/$LogFile.md
- 신규 생성: techniques/Timestomping.md, techniques/Tamper_Resistance_Factors.md
- 모순 없음

## [2026-04-11] ingest | domingues_2024
- 생성: papers/domingues_2024.md
- 신규 생성: artifacts/WebAuthN_EVTX.md, artifacts/FIDO_LinkedDevices_Registry.md
- 업데이트: meta/index.md

## [2026-04-11] query | "$USNjrnl 수사 방법"
- 참조 페이지: artifacts/$USNjrnl.md, techniques/Timestomping.md
- 답변 wiki 저장: techniques/USNjrnl_Investigation.md
```

---

## 팀 협업 (Git)

wiki 폴더는 Git repo로 관리한다.

```bash
# 초기 설정
git init
git remote add origin https://github.com/skku-forensics/surf-wiki

# 새 논문 ingest 후
git add wiki/
git commit -m "ingest: vanini_2025 — Timestomping user study"
git push
```

팀원은 `git pull` 후 Obsidian에서 열면 된다.
각자 다른 논문을 ingest하고 push — wiki가 자연스럽게 공동 축적.

---

## Obsidian 권장 플러그인

| 플러그인 | 용도 |
|----------|------|
| Dataview | YAML frontmatter 기반 동적 테이블·리스트 |
| Copilot | wiki 전체를 컨텍스트로 LLM 질의 |
| Git | 자동 commit/push 설정 |
| Marp | wiki에서 바로 슬라이드 생성 |
| Graph View | 기본 내장 — 개념 연결 시각화 |

---

## 절대 하지 말 것

- `raw/` 파일 수정 또는 삭제
- 논문이 직접 분석하지 않은 아티팩트를 artifact 페이지로 생성
- 논문이 말하지 않은 내용을 Findings에 추가 (추론·해석 금지)
- ATT&CK ID 추측 입력
- 기존 Findings를 덮어쓰기 — 항상 append
- index.md / log.md 업데이트 생략
