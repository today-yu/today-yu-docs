---
id: INQUIRY_SUBMIT_001
title: 문의 작성
context: inquiry
api: POST /api/v1/inquiries
---

# INQUIRY_SUBMIT_001. 문의 작성

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| contactEmail | Body | String | 필수, 이메일 형식 (대소문자 구분 없음) |
| title | Body | String | 필수, 공백만으로 구성될 수 없음 |
| content | Body | String | 필수, 공백만으로 구성될 수 없음 |

- `contactEmail`은 답변을 받을 연락용 이메일로, 회원의 가입 이메일과 달라도 된다.

## [4] 처리 로직

1. 요청한 회원을 작성자로, 작성 시각을 현재 시각으로 하여 문의(Inquiry)를 저장한다.
   - 문의 상태는 `WAITING`(답변 대기 중)으로 시작한다.
   - 수정 시각은 작성 시각과 같고, 답변 시각은 비어 있는 상태로 저장한다.

## [5] 정상 출력

`201 Created`

- 응답 메시지: `문의 작성에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| inquiryId | Long | 저장된 문의 ID |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| `contactEmail`, `title`, `content` 누락 또는 공백 | 422 Unprocessable Content | `COMMON-0002` |
| `contactEmail` 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 요청 본문을 읽을 수 없음 (JSON 형식 오류 등) | 400 Bad Request | `COMMON-0003` |

## [7] 관련 문서

- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 문의, 답변, 문의 상태
