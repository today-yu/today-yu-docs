---
id: MEMBER_VERIFICATION_VERIFY_001
title: 이메일 인증 코드 검증
context: member
api: POST /api/v1/auth/email-verification/verify
---

# MEMBER_VERIFICATION_VERIFY_001. 이메일 인증 코드 검증

## [1] 접근 권한

- 누구나 (인증 불필요)

## [2] 사전 조건

- [MEMBER_VERIFICATION_ISSUE_001](MEMBER_VERIFICATION_ISSUE_001.md)로 인증 코드를 발급받음

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| email | Body | String | 필수, 이메일 형식 |
| code | Body | String | 필수 |

## [4] 처리 로직

1. 해당 이메일로 발급된 인증 코드를 조회한다.
2. 입력한 코드와 저장된 코드가 일치하는지 확인한다.
3. 일치하면 저장된 인증 코드를 삭제한다.

## [5] 정상 출력

`200 OK`, 응답 데이터 없음

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| 이메일 형식 오류, 코드 누락 | 422 Unprocessable Content | `COMMON-0002` |
| 발급된 인증 코드 없음 (미발급, 3분 경과로 만료) | 400 Bad Request | `MEMBER-0002` |
| 인증 코드 불일치 | 400 Bad Request | `MEMBER-0002` |


## [7] 관련 문서

- [MEMBER_VERIFICATION_ISSUE_001](MEMBER_VERIFICATION_ISSUE_001.md), [MEMBER_SIGNUP_001](MEMBER_SIGNUP_001.md)
