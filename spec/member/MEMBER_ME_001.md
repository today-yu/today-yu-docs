---
id: MEMBER_ME_001
title: 내 정보 조회
context: member
api: GET /api/v1/members/me
---

# MEMBER_ME_001. 내 정보 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)
- 로그인한 회원 본인의 정보만 조회한다.

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |

- 조회 대상 회원은 Access Token으로 인증된 회원으로 정해지며, 별도의 회원 ID를 입력받지 않는다.

## [4] 처리 로직

1. 인증된 회원의 회원 ID로 회원(Member)을 조회한다.
2. 회원의 회원 ID, 닉네임, 이메일을 응답한다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `내 정보 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| memberId | Long | 회원 ID |
| nickname | String | 닉네임 |
| email | String | 회원 이메일 |

- 마이페이지의 회원 정보 표시와, 문의 작성 시 연락용 이메일 입력란의 기본값으로 사용한다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 회원 없음 | 404 Not Found | `MEMBER-1002` |

- 토큰의 회원이 존재하지 않으면 일반적으로 인증 단계에서 `AUTH-1001`(401)로 응답한다. `MEMBER-1002`는 인증 이후 조회 시점에 회원이 존재하지 않는 경우에만 응답한다.

## [7] 미결정 사항

- 학과: 마이페이지 UI에 표시되지만 회원 도메인과 용어 문서에 정의되어 있지 않다. 정책이 확정되면 응답 필드 추가를 검토한다.
- 활동 통계(등록한 정보 / 수정한 정보 / 스크랩 수): 이 API에 포함하지 않으며, 별도 API로 제공할지 결정이 필요하다.

## [8] 관련 문서

- [MEMBER_SIGNUP_001](MEMBER_SIGNUP_001.md) — 일반 회원가입
- [INQUIRY_SUBMIT_001](../inquiry/INQUIRY_SUBMIT_001.md) — 문의 작성
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 회원, 닉네임
