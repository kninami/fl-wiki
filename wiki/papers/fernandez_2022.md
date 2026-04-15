---
paper: fernandez_2022
title: "Extraction and Analysis of Retrievable Memory Artifacts from Windows Telegram Desktop (2022)"
full_title: "Extraction and analysis of retrievable memory artifacts from Windows Telegram Desktop application"
authors: "Fernández-Álvarez, P.; Rodríguez, R.J."
year: 2022
venue: DFRWS EU 2022 (FSI Digital Investigation 40)
doi: https://doi.org/10.1016/j.fsidi.2022.301342
tags:
  - memory forensics
  - Telegram Desktop
  - instant messaging
  - Windows
  - process memory
method: 실험 (3단계 포렌식 분석 방법론: 추출·분석·보고)
limitations: "Windows 플랫폼 한정; 앱 버전 변경 시 재분석 필요; 암호화 해제는 앱 실행 중일 때만 가능"
---

## 한 줄 요약
Windows Telegram Desktop 프로세스 메모리에서 포렌식 아티팩트(연락처, 대화 내용, 사용자 정보)를 추출하는 방법론 및 도구(Windows Memory Extractor + IM Artifact Finder) 개발.

## 핵심 발견
- 앱이 차단·종료된 상태에서도 프로세스 메모리에 대화 내용 잔존
- 암호화된 채널의 메시지도 메모리에서 복호화된 상태로 존재
- Telegram의 UI 표현 클래스 계층 역분석으로 아티팩트 위치 파악
- 연락처(전화번호), 1:1 대화, 그룹 채팅, 채널 내용 모두 추출 가능
- IM Artifact Finder: 임의 IM 앱으로 확장 가능한 플러그인 아키텍처(GPLv3)

## 직접 분석한 아티팩트
[[artifacts/Telegram_Desktop_Memory]] · [[artifacts/Windows_NT_Heap]]

## 영향받은 wiki 페이지
[[techniques/IM_Memory_Forensics]]
