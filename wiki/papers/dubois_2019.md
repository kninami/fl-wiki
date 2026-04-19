---
paper: dubois_2019
title: "School Cyber Risk & Challenges for Community Oriented Policing (2019)"
full_title: "School Cyber Risk & Challenges for Community Oriented Policing, Crime Prevention, and Investigations"
authors: "Dubois, Nicholas"
year: 2019
venue: DFRWS USA 2019
doi: —
tags:
  - school security
  - physical attacks
  - social engineering
  - IoT
  - awareness
method: 실증 시연 (고등학생 발표)
limitations: "단일 학교 환경, 실험적 재현 없음, 포렌식 아티팩트 분석 없음"
---

## 한 줄 요약
고등학생 발표자가 학교 환경에서 실제 가능한 물리·원격·기기 접근 공격 벡터를 시연·정리하고 커뮤니티 치안 관점의 대응책을 제안.

## 핵심 발견

### 물리적 공격
- **3D 프린팅 키 복제**: 학교 3D 프린터 + Autodesk Inventor로 마스터키 복제
- **RFID 카드 복제**: Keysy 도구로 학교 출입 카드 클론

### 원격 공격
- **WiFi DoS**: Linux/WiFi Pineapple으로 네트워크 서비스 거부
- **캡티브 포털 클론**: Wireshark로 HTTP 자격증명 수집
- **IoT/CCTV**: Hikvision 카메라 취약점 악용
- **미패치 OS**: Windows XP MS08-67 취약점 + 키로거 배포
- **프린터 익스플로잇**: PRET(Printer Exploitation Toolkit) 활용
- **표준화 시험(MCAS)**: 온라인 시험 환경 취약점

### 기기 접근 공격 (가장 흔한 유형)
- **Bash Bunny / USB Rubber Ducky**: USB 연결 즉시 백도어·키로거 설치
- MAC 주소 모니터링으로 무단 기기 탐지

### 예방 권고
- 관리자 전용 별도 WiFi 망 분리
- OS·소프트웨어 최신 패치 유지
- 학생 접근 가능 컴퓨터에 패스워드 저장 금지
- 교사 보안 인식 교육 강화

## 직접 분석한 아티팩트
없음 (보안 인식 발표, 포렌식 아티팩트 직접 분석 없음)

## 영향받은 wiki 페이지
없음 (신규 artifact·technique 페이지 생성 대상 없음)
