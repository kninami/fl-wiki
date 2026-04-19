---
paper: schneider_2020
title: "Unifying Metadata-Based Storage Reconstruction and Carving with LAYR (2020)"
full_title: "Unifying Metadata-Based Storage Reconstruction and Carving with LAYR"
authors: "Schneider, J.; Baier, H.; Breitinger, F."
year: 2020
venue: DFRWS USA 2020
doi: —
tags:
  - file carving
  - metadata
  - storage reconstruction
  - formal model
  - LAYR
  - disk forensics
method: 시스템 구축
limitations: "이론 모델 + 프로토타입 구현; 대규모 실세계 이미지 검증 미수행"
---

## 한 줄 요약
메타데이터 기반 스토리지 재구성과 파일 카빙을 통합하는 형식 모델 LAYR 및 동명 C++ 도구 제안.

## 핵심 발견
- 기존 스토리지 재구성(Ext3, FAT)과 파일 카빙은 별개 연구 영역 → 단일 형식 모델로 통합 가능
- 연산자 4종 정의: Sequential (>>), Parallel (||), Union (+), Intersection (*), Minus (-)
- 휴리스틱 3종 구현: DOS, Ext3, Carving
- LAYR 도구: C++로 구현, 기존 도구 대비 재구성 정확도·유연성 향상
- 공식 모델 기반이므로 새 파일시스템 추가 시 휴리스틱만 작성하면 확장 가능

## 직접 분석한 아티팩트
없음 (이론·프레임워크 논문 — 파일 카빙 및 메타데이터 재구성 방법론 자체가 주제)

## 영향받은 wiki 페이지
[[techniques/Storage_Reconstruction_Formal_Model]]
