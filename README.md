# FL-Wiki

**디지털 포렌식 논문을 Claude(AI)가 읽고, 팀이 함께 쓰는 지식 wiki**

이미 축적된 wiki를 탐색하거나, 새 논문을 추가해서 지식을 확장할 수 있습니다.
논문 관리는 연구진이 담당합니다. 클론한 사람은 바로 질의부터 시작하면 됩니다.

---

## 시작하기 전에 필요한 것

| 항목 | 설명 | 설치 링크 |
|------|------|-----------|
| **Claude Code** | AI가 wiki를 읽고 답변하는 도구 | [설치 방법](https://docs.anthropic.com/ko/docs/claude-code) |
| **Obsidian** | wiki를 읽고 탐색하는 노트 앱 | [obsidian.md](https://obsidian.md) |
| **Git** | 최신 wiki를 받아오는 도구 | [git-scm.com](https://git-scm.com) |

> Claude Code는 Anthropic 계정과 API 크레딧이 필요합니다.
> 논문 한 편 처리에 약 $0.05–0.15 정도 소요됩니다.

---

## 설치

```bash
# 1. 이 저장소 클론
git clone https://github.com/kninami/fl-wiki.git
cd FL-Wiki

# 2. Obsidian에서 vault 열기
#    Obsidian 실행 → "Open folder as vault" → FL-Wiki 폴더 선택
```

---

## FL-Wiki 시작하는 법

### 1단계: Claude Code 실행

FL-Wiki 폴더에서 터미널을 열고 Claude Code를 시작합니다.

```bash
cd FL-Wiki
claude
```

### 2단계: Claude Code에게 FL-Wiki 알려주기

```
CLAUDE.md 를 보고 FL-Wiki 에 대해 파악해줘
```

### 3단계: 질의하기

Claude Code에 자연어로 질문할 수 있습니다.

```
$USNjrnl로 무엇을 수사할 수 있어?
Timestomping을 탐지하는 방법은?
File wiping 사건에서 확인해야 할 아티팩트 목록을 알려줘
```

Claude가 wiki에 축적된 내용을 바탕으로 출처를 포함한 답변을 제공합니다.
좋은 답변은 자동으로 새 technique 페이지로 저장됩니다.

---

## 논문 추가하는 법

### 1단계: 논문 PDF 저장

분석할 논문 PDF를 `raw/` 폴더에 넣습니다.
파일명은 `저자성_연도.pdf` 형식으로 맞춰주세요.

```
raw/
  vanini_2025.pdf
  palmbach_2020.pdf
```

### 2단계: ingest 요청

```
raw 폴더에 새로 추가한 파일명.pdf 를 ingest 해줘
```

Claude가 자동으로 다음 작업을 수행합니다:
- 논문 전문 분석
- `wiki/papers/` 에 논문 요약 페이지 생성
- 논문이 다룬 아티팩트 페이지 생성 또는 업데이트
- 관련 기법 페이지 생성 또는 업데이트
- 기존 내용과의 모순·보완 관계 반영
- 인덱스 및 작업 로그 업데이트

### 3단계: Obsidian에서 결과 확인

Obsidian으로 FL-Wiki 폴더를 열면 생성된 페이지들을 볼 수 있습니다.
Graph View(좌측 사이드바 네트워크 아이콘)에서 개념 간 연결을 시각적으로 탐색할 수 있습니다.

---

## wiki 건강 점검

주기적으로 아래 명령을 실행하면 wiki의 품질을 점검합니다.

```
wiki를 lint 해줘
```

고아 페이지, 누락된 링크, 논문 간 모순 등을 탐지해서 보고합니다.

---

## 폴더 구조

```
FL-Wiki/
├── raw/              # 원본 논문 PDF (연구진만 관리)
├── wiki/
│   ├── artifacts/    # 아티팩트 페이지 ($MFT, Prefetch ...)
│   ├── techniques/   # 공격·탐지·분석 기법 페이지
│   ├── papers/       # 논문별 요약 페이지
│   └── meta/
│       ├── index.md  # 전체 페이지 목록 (Claude가 질의 시 가장 먼저 읽음)
│       └── log.md    # 모든 작업 이력
└── CLAUDE.md         # wiki 규칙 (Claude에게 주는 지침)
```

`wiki/` 는 Claude가 관리합니다. 직접 편집하지 마세요.

---

## 팀원과 공유하기

```bash
# 새 논문 ingest 후 팀에 공유
git add wiki/
git commit -m "ingest: vanini_2025 — Timestomping user study"
git push

# 팀원이 최신 wiki 받기
git pull
```

---

## 권장 Obsidian 플러그인

Obsidian Settings → Community plugins 에서 설치할 수 있습니다.

| 플러그인 | 용도 |
|----------|------|
| **Dataview** | YAML 기반 동적 테이블·리스트 (index 페이지 자동 렌더링) |
| **Git** | 저장 시 자동 commit/push |
| **Graph View** | 기본 내장 — 개념 연결 시각화 |

---

## 현재 wiki 현황

논문 3편, 아티팩트 14개, 기법 7개가 축적되어 있습니다.
`wiki/meta/index.md` 에서 전체 목록을 확인하세요.

| 분류 | 주요 내용 |
|------|-----------|
| Artifacts | $MFT, $USNjrnl, $LogFile, Prefetch, LNK, AmCache, SRUDB 등 |
| Techniques | Timestomping, SI/FN Mismatch, File Wiping Detection 등 |
| Papers | vanini_2025, palmbach_2020, joo_2022 |
