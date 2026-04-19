---
artifact: Chrome OS Local Data
layer: APP
data_types:
  - 오프라인 저장 파일
  - 확장 프로그램 데이터
  - 쉘 사용 이력
  - 아바타 이미지
  - 히든 파일
action_types:
  - 파일 다운로드/은닉
  - 확장 프로그램 설치
  - 터미널 명령 실행
os_version: Chrome OS (Chromium OS)
path: |
  \home\chronos\u-{GUID}\GCache\v1\files\  (Offline Storage)
  \home\chronos\u-{GUID}\Extensions\{ext-GUID}\  (Extensions)
  \home\chronos\u-{GUID}\.bash_history  (Shell Usage)
  \home\user\{GUID}\Accounts\Avatar Images\{email}.png
  \home\chronos\u-{GUID}\Downloads\dontlookhere\  (Hidden Folder)
attck: —
sources:
  - hyde_2019
updated: 2026-04-19
---

## 개요
Chrome OS 로컬 스토리지 아티팩트 모음. 오프라인 저장 파일, 확장 프로그램, 쉘 이력, 아바타, 히든 폴더 등 브라우저 히스토리 외의 로컬 아티팩트.

## 추출 방법
- 모두 Chromium 직접 이미지에서 수집 가능; Google Takeout에서는 대부분 미포함
- 4개 경로 패턴 동일 적용 (`.shadow/{GUID}/mount/user/`, `chronos/user/`, `chronos/u-{GUID}/`, `user/{GUID}/`)

## 분석 포인트

### Offline Storage (GCache\v1\files)
- Google Drive 오프라인 파일 캐시; 파일명이 UUID로 변경됨
- 원본 파일명·GUID는 `GCache\v1\meta\` 하위 LevelDB `.ldb`에 보존
- 파일 내용은 원본 그대로 (예: FidoNet Policy Document 확인)

### Extensions (확장 프로그램)
- `Extensions\{ext-GUID}\{버전}\manifest.json`: 앱 이름, 권한, 버전, 업데이트 URL
- ext-GUID를 Chrome Web Store/Google Play URL에 대입해 앱 식별 가능
- `Sync App Settings\{ext-GUID}\`: 확장 앱 설정 LevelDB — 패스워드 등 민감 데이터 포함 가능

### Shell Usage (.bash_history)
- crosh(Chrome OS developer shell)에서 `shell` 명령으로 bash 진입 후의 명령 이력
- 4개 경로 동일하게 존재

### Avatar Image
- `Accounts\Avatar Images\{로그인 이메일}.png`: 로그인한 Google 계정 이메일 식별
- PNG 파일, 4개 경로 존재

### Hidden Folder (dontlookhere)
- `Downloads\dontlookhere\.ProgramData\thumbs\{base64_encoded_filename}`: 은닉 파일
- 파일명: base64 인코딩 (원본 경로/파일명 인코딩)
- 파일 내용: 패스워드 문자열이 파일 앞에 삽입 → PNG/JPG 헤더 손상
- 패스워드는 `Sync App Settings` LevelDB에서 발견 가능

## 공격 기법 (해당 시)
- Hide It Pro 등 확장 앱을 통해 다운로드 폴더에 히든 폴더 생성, 이미지 파일 은닉
- 파일명 base64 인코딩 + 패스워드 prepend로 단순 복구 회피

## Findings
- Chromium 직접 이미지 > Google Takeout: Cache, Downloads, Hidden Folder, Offline Storage, Shell usage 등 Takeout 미포함 (→ [[papers/hyde_2019]])
- Sync App Settings에서 히든 폴더 패스워드 복구 가능 (→ [[papers/hyde_2019]])
- GCache\v1\meta LevelDB로 오프라인 파일 원본 파일명 복원 가능 (→ [[papers/hyde_2019]])

## 관련 페이지
[[artifacts/Chrome_OS_Browser_History]] · [[techniques/ChromeOS_Forensics]]
