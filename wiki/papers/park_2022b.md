---
paper: park_2022b
title: "Windows 시스템 및 지갑 앱 아티팩트 분석을 통한 암호화폐 수사 지원 기술 연구 (2022)"
full_title: "Windows 시스템 및 지갑 앱 아티팩트 분석을 통한 암호화폐 수사 지원 기술 연구"
authors: "박현재, 김소정, 박정흠; PLAINBIT, 고려대학교"
year: 2022
venue: 디지털포렌식연구
method: 실험
limitations: "특정 버전 지갑 앱 10종 한정, Windows 10 환경"
tags:
  - 암호화폐
  - Cryptocurrency Wallet
  - LevelDB
  - SQLite
  - Windows Forensics
---

## 한 줄 요약
Windows 10에서 10개 암호화폐 지갑 앱의 저장 경로·아티팩트를 체계적으로 분석하고 복호화 없이 획득 가능한 정보 식별.

## 핵심 발견
- 분석 대상 10종: Atomic, Bitcoin Core, Bither, Bitpay, Blockstream Green, Coinomi, Electrum, Exodus, Guarda, Jaxx Liberty
- 주요 저장 경로 (예): Atomic = `%AppData%\Roaming\Atomic\Local Storage\leveldb\*.ldb`
- 복호화 없이 획득 가능한 정보: 지갑 주소, 잔액, 트랜잭션 내역 일부 (Table 5)
- 파일 시그니처로 지갑 파일 식별 가능 (Table 6)
- 통합 수사 도구 설계 제안 (지갑 자동 탐지·정보 추출)

## 직접 분석한 아티팩트
[[artifacts/Cryptocurrency_Wallet_Files]]

## 영향받은 wiki 페이지
[[techniques/Cryptocurrency_Wallet_Forensics]]
