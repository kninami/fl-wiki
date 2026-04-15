---
technique: Automated_User_Analysis
perspective: Investigator
topic: User Behavior Analysis
related_artifacts:
  - Prefetch
  - Windows_Registry_Memory
  - Windows_Event_Log
related_crime:
  - 사이버범죄 수사
  - 사용자 행위 분석
sources:
  - kim_2017
updated: 2026-04-15
---

## 개요
Windows 아티팩트(Prefetch, 레지스트리, 브라우저 기록, MFT)를 자동 수집·분석하고 NLP 키워드 추출 및 소셜 네트워크 분석으로 사용자 행위 패턴을 시각화하는 기법.

## 메커니즘
1. **데이터 수집 자동화**: Prefetch, Registry 주요 키, Chrome Login Data/History, MFT 플래그, Chrome Sync 일괄 추출
2. **NLP 처리**: Mecab 형태소 분석기로 웹 접속 기록 키워드 추출 및 분류
3. **소셜 네트워크 분석**: 키워드·연락처 간 관계 그래프 생성 → 핵심 인물·관심사 시각적 식별
4. **다수 아티팩트 상관 분석**: 단일 아티팩트 한계를 보완하는 크로스-아티팩트 검증

## 분석 대상 아티팩트
- **Prefetch**: 실행 프로그램 목록·시각
- **Registry**: 설치 프로그램, UserAssist(GUI 실행 이력), MRU(최근 접근 파일·폴더)
- **Chrome History**: URL 방문 기록, 검색어
- **MFT 플래그**: 파일 생성·수정·삭제 이벤트

## 한계 & 주의사항
- Chrome 브라우저 한정 — Firefox·Edge 대상 분석 미포함
- Mecab NLP 한국어 처리 정확도 의존
- 온라인 연동(Chrome Sync) 필요 — 오프라인 환경에서는 일부 기능 제한

## 논문별 근거
| 논문 | 핵심 발견 |
|------|-----------|
| [[papers/kim_2017]] | 다수 아티팩트 자동 수집 + NLP + 소셜 네트워크 분석 파이프라인 제안 |

## 관련 페이지
[[artifacts/Prefetch]] · [[artifacts/Windows_Registry_Memory]] · [[artifacts/Windows_Event_Log]]
