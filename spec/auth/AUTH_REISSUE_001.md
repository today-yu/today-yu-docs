---
id: AUTH_REISSUE_001
title: 인증 토큰 재발급
context: auth
api: POST /api/v1/auth/reissue
---

# AUTH_REISSUE_001. 인증 토큰 재발급

## [1] 접근 권한

- 누구나 (Access Token 불필요, Refresh Token으로 검증)

## [2] 사전 조건

- 로그인 시 발급받은 Refresh Token이 만료되지 않음

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| refreshToken | Body | String | 필수 |

## [4] 처리 로직

1. Refresh Token에서 회원 ID, 세션 ID를 추출한다.
2. 저장된 해당 세션(회원 ID + 세션 ID)의 Refresh Token을 조회하여, 요청한 토큰과 일치하는지 확인한다.
3. 회원을 조회한다.
4. **기존과 같은 세션 ID**로 Access Token, Refresh Token을 새로 발급한다.
5. 새 Refresh Token으로 기존 Refresh Token을 덮어쓴다.
   - 따라서 한 번 사용한 Refresh Token은 다시 사용할 수 없다. (Refresh Token Rotation)

### 보안 규칙

- 재발급 과정의 모든 실패는 `AUTH-1004`로 통일한다.
- 프론트엔드는 `AUTH-1004`를 받으면 재발급을 다시 시도하지 않고 재로그인을 유도한다.

## [5] 정상 출력

`200 OK`, `Cache-Control: no-store`

응답 필드는 [AUTH_SIGNIN_001](AUTH_SIGNIN_001.md#5-정상-출력)과 같다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| `refreshToken` 누락 | 422 Unprocessable Content | `COMMON-0002` |
| 서명·형식 오류, 만료, Refresh Token 타입이 아님 | 401 Unauthorized | `AUTH-1004` |
| 저장된 Refresh Token 없음 (로그아웃·만료) | 401 Unauthorized | `AUTH-1004` |
| 저장된 Refresh Token과 불일치 (이미 사용된 토큰) | 401 Unauthorized | `AUTH-1004` |
| 회원이 존재하지 않음 | 401 Unauthorized | `AUTH-1004` |

## [7] 관련 문서

- [AUTH_SIGNIN_001](AUTH_SIGNIN_001.md)
