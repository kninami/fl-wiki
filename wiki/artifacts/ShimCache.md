---
artifact: ShimCache
layer: OS
data_types:
  - 실행 파일 경로
  - 마지막 수정 시각
  - 실행 여부 플래그
action_types:
  - 프로그램 실행
  - 프로그램 호환성 검사
os_version: Windows XP+
path: "HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\AppCompatCache"
attck:
sources:
  - joo_2022
updated: 2026-04-15
---

## 개요
Windows AppCompatCache(ShimCache)는 프로그램 호환성 레이어가 실행 시 기록하는 레지스트리 값. 시스템에서 실행된(또는 실행 시도된) 파일의 경로와 타임스탬프를 추적한다.

## 추출 방법
- 레지스트리 키: `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`
- 값 이름: `AppCompatCache`
- 도구: AppCompatCacheParser(EricZimmerman), RegRipper, ShimCacheParser

## 분석 포인트
- **AppCompatCache**: 파일 경로 + 마지막 수정 시각 저장 (실행 순서는 역순으로 저장)
- **AppCompatFlags**: 호환성 플래그 (Windows 7 이하)
- 실행 여부 플래그: Windows 버전에 따라 실제 실행 여부 판별 가능 여부 다름
- 시스템 종료 시에만 레지스트리에 flush → 마지막 부팅 이후 정보만 완전

## Findings
- **File Wiping 도구 5종 중 ShimCache(AppCompatCache)에서 흔적 잔류율 가장 높음** (→ [[joo_2022]])

## 관련 페이지
[[artifacts/AmCache]] · [[artifacts/UserAssist]] · [[techniques/File_Wiping_Detection]]
