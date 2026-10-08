---
id: MEMBER_VERIFICATION_ISSUE_001
title: 이메일 인증 코드 발급
context: member
api: POST /api/v1/auth/email-verification/issue
---

# MEMBER_VERIFICATION_ISSUE_001. 이메일 인증 코드 발급

## [1] 접근 권한

- 누구나 (인증 불필요)

## [2] 사전 조건

- 없음

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| email | Body | String | 필수, 이메일 형식 |

## [4] 처리 로직

1. 6자리 숫자 인증 코드를 생성한다. (100000 ~ 999999)
2. 해당 이메일로 발급된 인증 코드가 있는지 확인한다.
   - 있으면 재발급으로 처리한다. 기존 발급 시각으로부터 **1분**이 지나지 않았으면 예외가 발생한다.
   - 없으면 새로 발급한다.
3. 인증 코드를 이메일 단위로 저장한다.
   - 유효 시간: 발급 시각부터 **3분**
   - 재발급 시 기존 코드를 덮어쓴다.
4. 인증 코드 이메일을 비동기로 발송한다.
   - 제목: `[오늘의 영대] 이메일 인증 코드`
   - 본문에 인증 코드와 유효 시간(3분)을 표시한다.

## [5] 정상 출력

`200 OK`, 응답 데이터 없음

- 이메일 발송은 비동기이므로, 발송 실패 여부와 관계없이 성공으로 응답한다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| 이메일 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 발급 후 1분 이내 재발급 요청 | 429 Too Many Requests | `MEMBER-0001` |

## [7] 관련 문서

- [MEMBER_VERIFICATION_VERIFY_001](MEMBER_VERIFICATION_VERIFY_001.md), [MEMBER_SIGNUP_001](MEMBER_SIGNUP_001.md)
