---
id: INQUIRY_DELETE_001
title: 문의 삭제
context: inquiry
api: DELETE /api/v1/inquiries/{inquiryId}
---

# INQUIRY_DELETE_001. 문의 삭제

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)
- 문의를 작성한 회원 본인만 삭제할 수 있다.

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| inquiryId | Path | Long | 필수, 삭제할 문의 ID |

## [4] 처리 로직

1. 문의 ID로 문의(Inquiry)를 조회한다.
2. 요청한 회원이 문의의 작성자인지 확인한다.
   - 문의가 존재하는지 숨기지 않으며, 작성자가 아니면 403으로 응답한다.
3. 문의가 `WAITING`(답변 대기 중) 상태인지 확인한다.
   - `ANSWERED`(답변 완료) 상태의 문의는 삭제할 수 없다.
4. 문의를 삭제한다.
   - 삭제한 문의는 목록·상세 조회에서 조회되지 않는다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `문의 삭제에 성공하였습니다.`
- 응답 데이터 없음 (`data: null`)

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 문의 없음 (이미 삭제된 문의 포함) | 404 Not Found | `INQUIRY-1001` |
| 요청한 회원이 문의 작성자가 아님 | 403 Forbidden | `INQUIRY-1002` |
| 문의가 `WAITING` 상태가 아님 | 409 Conflict | `INQUIRY-1003` |

## [7] 관련 문서

- [INQUIRY_DETAIL_001](INQUIRY_DETAIL_001.md) — 문의 상세 조회
- [INQUIRY_LIST_001](INQUIRY_LIST_001.md) — 문의 목록 조회
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 문의, 문의 상태
