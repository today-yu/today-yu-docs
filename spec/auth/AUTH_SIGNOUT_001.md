---
id: AUTH_SIGNOUT_001
title: 로그아웃
context: auth
api: POST /api/v1/auth/sign-out
---

# AUTH_SIGNOUT_001. 로그아웃

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |

## [4] 처리 로직

1. Access Token에서 회원 ID, 세션 ID, 토큰 ID(jti), 만료 시각을 추출한다.
2. Access Token의 토큰 ID를 블랙리스트에 등록한다.
   - 보관 기간은 Access Token의 남은 만료 시간과 같다.
   - 이후 해당 Access Token으로 요청하면 인증에 실패한다.
3. 해당 세션(회원 ID + 세션 ID)의 Refresh Token을 삭제한다.
   - 다른 세션(다른 기기)의 로그인 상태에는 영향을 주지 않는다.

## [5] 정상 출력

`200 OK`, 응답 데이터 없음

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |

## [7] 관련 문서

- [AUTH_SIGNIN_001](AUTH_SIGNIN_001.md)
