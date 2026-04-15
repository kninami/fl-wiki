---
artifact: $USNjrnl
layer: FS
data_types:
  - USN Record
  - 파일 변경 이력
  - 파일명
  - 변경 사유 (Reason)
  - 타임스탬프
action_types:
  - 파일 생성
  - 파일 수정
  - 파일 삭제
  - 파일명 변경
  - 타임스탬프 변경
os_version: Windows (NTFS)
path: "C:\\$Extend\\$USNjrnl:$J"
attck: T1070.006
sources:
  - palmbach_2020
  - lee_2021
  - kim_2022a
updated: 2026-04-15
---

## 개요
NTFS Update Sequence Number Journal. 볼륨의 파일·디렉토리 변경 이력을 순환 버퍼에 기록하는 변경 로그. 파일명, 변경 사유, 타임스탬프, File Reference Number를 포함. 기본 활성화 상태.

## 추출 방법
- 경로: C:\\$Extend\\$USNjrnl (NTFS 스트림 $J에 실제 레코드 저장)
- 도구: UsrJrnl2Csv, Autopsy, X-Ways, MFTECmd
- FTK Imager 이미지에서 직접 파싱 가능

## 분석 포인트
- USN 레코드: 파일명, File Reference Number, 변경 사유(Reason flags), 타임스탬프 포함
- $MFT 타임스탬프와 $USNjrnl 레코드 타임스탬프 교차 검증 → 조작 탐지
- 순환 버퍼 특성: 오래된 레코드는 새 레코드에 덮어쓰임 → 보존 기간 제한
- 파일 삭제 후에도 USN 레코드 잔존 → 삭제된 파일 이력 확인 가능

## 공격 기법 (해당 시)
- 대부분의 타임스탬핑 도구가 $USNjrnl까지 조작하지 않음 → 조작 흔적 탐지 용이
- 순환 버퍼 오버플로우를 유도해 증거 덮어쓰기 가능 (대량 파일 생성/삭제)

## Findings
- $USNjrnl은 타임스탬프 변경 흔적을 기록하나 순환 버퍼로 오래된 기록 손실 위험 (→ [[papers/palmbach_2020]])
- ReFS의 Change Journal은 $USNjrnl과 유사하나 USN_RECORD_V3(128bytes) 포맷 사용, 기본 비활성화 (→ [[papers/lee_2021]])
- ReFS Change Journal: File Reference Number 필드가 32bit로 확장 (NTFS는 16bit minor + 32bit) (→ [[papers/lee_2021]])
- WSA 환경에서 userdata.vhdx 파일 변경 이력이 $UsnJrnl에 기록됨 → Android 앱 사용 추적 가능 (→ [[papers/kim_2022a]])

## 관련 페이지
[[artifacts/$LogFile]] · [[artifacts/ReFS_Change_Journal]] · [[techniques/Timestamp_Manipulation_Detection]]
