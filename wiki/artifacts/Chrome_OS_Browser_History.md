---
artifact: Chrome OS Browser History
layer: APP
data_types:
  - 브라우저 방문 URL
  - 방문 횟수 및 타임스탬프
  - 다운로드 기록
  - 탭/세션 정보
  - 브라우저 캐시
action_types:
  - 웹 브라우징
  - 파일 다운로드
  - 탭 복원
os_version: Chrome OS (Chromium OS)
path: |
  \home\.shadow\{GUID}\mount\user\History
  \home\chronos\user\History
  \home\chronos\u-{GUID}\History
  \home\user\{GUID}\History
attck: —
sources:
  - hyde_2019
updated: 2026-04-19
---

## 개요
Chrome OS에서 Chrome 브라우저 히스토리는 SQLite DB 파일로 저장되며, 4개의 경로에 동일 파일이 존재한다(같은 타임스탬프, 같은 파일 오프셋). 다운로드, 탭, 세션 정보도 동일한 패턴으로 4경로에 분산 저장된다.

## 추출 방법
- **획득**: developer mode 활성화 후 live acquisition (Chromium OS VM은 직접 이미지); Chromebook 실기기는 Esc/Refresh/Power → Ctrl-D로 developer mode 진입 시 사용자 데이터 초기화 주의
- **파싱**: Chrome browser 파서(Hindsight 등)로 SQLite DB 직접 파싱; Chrome OS path 지원은 Hindsight에 추가됨
- **경로 패턴**: `.shadow/{GUID}/mount/user/` = `chronos/user/` = `chronos/u-{GUID}/` = `user/{GUID}/` (동일 파일 4벌)

## 분석 포인트
- **History DB** (`visits`, `urls` 테이블): URL, 방문 횟수, last_visit_time (WebKit epoch)
- **Downloads** (`downloads`, `downloads_url_chains` 테이블): target_path, start_time, received/total bytes; source URL 체인 추적 가능
- **Current Tabs / Last Tabs / Current Sessions / Last Sessions**: 탭 복원 및 마지막 세션 재구성
- **Browser Cache**: 개별 파일로 저장, GUID별 디렉터리; HTTP 헤더 포함 (URL, 날짜, content-type 등)
- **Avatar Image**: `Accounts\Avatar Images\{email}.png` — 로그인 이메일 식별에 활용
- **Extensions**: `Extensions\{ext-GUID}\` 하위 `manifest.json`으로 앱 정보 확인; GUID를 Chrome Web Store URL에 대입해 앱 식별 가능
- **Sync App Settings**: 확장 프로그램 설정 `.ldb`에 패스워드 등 민감정보 포함 가능
- **Shell Usage** (`.bash_history`): crosh shell에서 `shell` 명령 실행 후 bash 사용 이력

## 공격 기법 (해당 시)
- **히든 폴더**: `Downloads/dontlookhere/.ProgramData/thumbs/`에 base64 파일명으로 이미지 은닉; 파일 앞에 패스워드 문자열 삽입 (PNG/JPG 헤더 앞)
- Sync App Settings에 저장된 패스워드로 `.ProgramData` 히든 파일 암호화

## Findings
- 브라우저 히스토리·캐시·다운로드·탭·세션은 모두 Chromium 직접 이미지로 수집 가능; Google Takeout에서는 Cache·Downloads·Hidden Folder·Shell usage 미포함 (→ [[papers/hyde_2019]])
- 4개 경로 동일 파일 — 어느 경로에서 수집해도 동일한 증거 (→ [[papers/hyde_2019]])
- Offline Storage(GCache\v1\files)의 파일명은 변경되나 원본 파일명·GUID는 `GCache\v1\meta` .ldb에 보존 (→ [[papers/hyde_2019]])

## 관련 페이지
[[artifacts/Chrome_OS_Local_Data]] · [[techniques/ChromeOS_Forensics]]
