---
id: MEMBER_SIGNUP_001
title: 일반 회원가입 (이메일·비밀번호)
context: member
api: POST /api/v1/auth/sign-up
---

# MEMBER_SIGNUP_001. 일반 회원가입

## [1] 접근 권한

- 누구나 (인증 불필요)

## [2] 사전 조건

- 없음 (미결정 사항 참고)

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| email | Body | String | 필수, 이메일 형식 |
| password | Body | String | 필수, 8자 이상 20자 이하 |

## [4] 처리 로직

1. 이미 가입된 이메일인지 확인한다.
2. 비밀번호를 암호화한다.
3. 암호화한 비밀번호로 로컬 계정(LocalAccount)을 생성한다.
4. 닉네임을 자동 생성한다.
   - 형식: `{수식어} 영대인 {번호}` (예: `착한 영대인 12`)
   - 수식어는 미리 정의된 21개 중 무작위로 선택한다.
   - 번호는 수식어별 가입 순번이며 1부터 시작한다.
5. `MEMBER` 역할을 가진 회원을 생성하여 저장한다.

## [5] 정상 출력

`201 Created`, 응답 데이터 없음

- 회원가입 후 자동 로그인하지 않는다. 토큰은 [AUTH_SIGNIN_001](../auth/AUTH_SIGNIN_001.md)로 발급받는다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| 이메일·비밀번호 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 이미 가입된 이메일 | 409 Conflict | `MEMBER-1001` |


## [7] 관련 문서

- [MEMBER_VERIFICATION_ISSUE_001](MEMBER_VERIFICATION_ISSUE_001.md), [MEMBER_VERIFICATION_VERIFY_001](MEMBER_VERIFICATION_VERIFY_001.md)
- 용어: [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) Member 컨텍스트
