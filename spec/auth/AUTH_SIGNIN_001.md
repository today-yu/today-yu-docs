---
id: AUTH_SIGNIN_001
title: 일반 로그인
context: auth
api: POST /api/v1/auth/sign-in
---

# AUTH_SIGNIN_001. 일반 로그인

## [1] 접근 권한

- 누구나 (인증 불필요)

## [2] 사전 조건

- 이메일·비밀번호로 가입한 로컬 계정(LocalAccount)이 존재함

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| email | Body | String | 필수, 이메일 형식 |
| password | Body | String | 필수, 8자 이상 20자 이하 |

## [4] 처리 로직

1. 이메일로 회원을 조회한다.
2. 회원의 로컬 계정 비밀번호와 입력한 비밀번호를 비교한다.
3. 새로운 세션 ID(SessionId)를 생성한다.
4. 세션 ID를 클레임으로 포함한 Access Token, Refresh Token을 발급한다.
   - Access Token 만료 기한: 30분
   - Refresh Token 만료 기한: 2주
5. Refresh Token을 회원 ID + 세션 ID 단위로 저장한다. (만료 기한까지 보관)

### 보안 규칙

- 회원 없음 / 로컬 계정 없음 / 비밀번호 불일치를 구분하지 않고 동일한 에러로 응답한다.
- 회원이나 로컬 계정이 없는 경우에도 더미 비밀번호 비교를 수행하여, 응답 시간으로 계정 존재 여부를 추측할 수 없게 한다.

## [5] 정상 출력

`200 OK`, `Cache-Control: no-store`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| tokenType | String | `Bearer` |
| accessToken | String | Access Token |
| accessTokenExpiresAt | Instant | Access Token 만료 시각 |
| refreshToken | String | Refresh Token |
| refreshTokenExpiresAt | Instant | Refresh Token 만료 시각 |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| 이메일·비밀번호 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 회원 없음 / 로컬 계정 없음 / 비밀번호 불일치 | 401 Unauthorized | `AUTH-1003` |

## [7] 관련 문서

- [AUTH_REISSUE_001](AUTH_REISSUE_001.md), [AUTH_SIGNOUT_001](AUTH_SIGNOUT_001.md)
