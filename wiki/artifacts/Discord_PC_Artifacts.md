---
artifact: Discord_PC_Artifacts
layer: APP
data_types:
  - 채널 메시지 (LevelDB)
  - 캐시된 미디어
  - 인증 세션 정보
action_types:
  - 메시지 송수신
  - 파일 공유
  - 앱 로그인
os_version: Windows 10+
path: "%AppData%\\Roaming\\discord"
attck:
sources:
  - shin_2020
updated: 2026-04-15
---

## 개요
Discord Windows PC 앱(Electron 기반)의 로컬 아티팩트. Roaming 및 Local 경로에 LevelDB, Cache, 세션 파일이 분산 저장된다.

## 추출 방법
- Roaming 경로: `%AppData%\Roaming\discord` (LevelDB, Local Storage)
- Local 경로: `%LocalAppData%\discord\Cache` (미디어 캐시)
- LevelDB 파싱 도구 또는 Chromium 전용 파서 사용

## 분석 포인트
- **LevelDB**: 채널 메시지, DM 내용 저장
- **Cache**: 이미지·파일 썸네일
- **Local Storage**: 세션 정보, 사용자 설정
- 인증 토큰은 Local Storage leveldb 내에서 추출 가능

## Findings
- 시나리오 실험으로 LevelDB에서 채팅 내용 재구성 가능 확인 (→ [[shin_2020]])

## 관련 페이지
[[artifacts/Discord_Android_DB]] · [[techniques/Slack_Discord_IM_Forensics]] · [[papers/shin_2020]]
