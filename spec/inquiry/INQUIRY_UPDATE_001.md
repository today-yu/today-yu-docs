---
id: INQUIRY_UPDATE_001
title: 문의 수정
context: inquiry
api: PUT /api/v1/inquiries/{inquiryId}
---

# INQUIRY_UPDATE_001. 문의 수정

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)
- 문의를 작성한 회원 본인만 수정할 수 있다.

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| inquiryId | Path | Long | 필수, 수정할 문의 ID |
| contactEmail | Body | String | 필수, 이메일 형식 (대소문자 구분 없음) |
| title | Body | String | 필수, 공백만으로 구성될 수 없음 |
| content | Body | String | 필수, 공백만으로 구성될 수 없음 |

- 연락용 이메일, 제목, 내용을 모두 전달받아 전체를 교체한다. 일부 필드만 수정하는 경우에도 나머지 필드는 기존 값을 그대로 전달해야 한다.

## [4] 처리 로직

1. 문의 ID로 문의(Inquiry)를 조회한다.
2. 요청한 회원이 문의의 작성자인지 확인한다.
   - 문의가 존재하는지 숨기지 않으며, 작성자가 아니면 403으로 응답한다.
3. 문의가 `WAITING`(답변 대기 중) 상태인지 확인한다.
   - `ANSWERED`(답변 완료) 상태의 문의는 수정할 수 없다.
4. 문의의 연락용 이메일, 제목, 내용을 요청 값으로 수정하고, 수정 시각을 현재 시각으로 갱신한다.
   - 작성자, 문의 상태, 작성 시각, 답변 시각은 변경하지 않는다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `문의 수정에 성공하였습니다.`
- 응답 데이터 없음 (`data: null`)

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| `contactEmail`, `title`, `content` 누락 또는 공백 | 422 Unprocessable Content | `COMMON-0002` |
| `contactEmail` 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 요청 본문을 읽을 수 없음 (JSON 형식 오류 등) | 400 Bad Request | `COMMON-0003` |
| 문의 없음 (삭제된 문의 포함) | 404 Not Found | `INQUIRY-1001` |
| 요청한 회원이 문의 작성자가 아님 | 403 Forbidden | `INQUIRY-1002` |
| 문의가 `WAITING` 상태가 아님 | 409 Conflict | `INQUIRY-0001` |

## [7] 관련 문서

- [INQUIRY_DETAIL_001](INQUIRY_DETAIL_001.md) — 문의 상세 조회
- [INQUIRY_DELETE_001](INQUIRY_DELETE_001.md) — 문의 삭제
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 문의, 문의 상태
