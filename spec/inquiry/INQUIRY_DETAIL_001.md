---
id: INQUIRY_DETAIL_001
title: 문의 상세 조회
context: inquiry
api: GET /api/v1/inquiries/{inquiryId}
---

# INQUIRY_DETAIL_001. 문의 상세 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)
- 문의를 작성한 회원 본인만 조회할 수 있다.

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| inquiryId | Path | Long | 필수, 조회할 문의 ID |

## [4] 처리 로직

1. 문의 ID로 문의(Inquiry)를 조회한다.
2. 요청한 회원이 문의의 작성자인지 확인한다.
   - 문의가 존재하는지 숨기지 않으며, 작성자가 아니면 403으로 응답한다.
3. 문의 상태와 관계없이 문의의 상세 내용을 응답한다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `문의 상세 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| id | Long | 문의 ID |
| memberId | Long | 작성자 회원 ID |
| contactEmail | String | 연락용 이메일 |
| status | String | 문의 상태. `WAITING`(답변 대기 중), `ANSWERED`(답변 완료) |
| title | String | 문의 제목 |
| content | String | 문의 내용 |
| createdAt | LocalDateTime | 작성 시각 |
| updatedAt | LocalDateTime | 수정 시각 |
| answeredAt | LocalDateTime | 답변 시각(첫 답변 발송 시각). `WAITING` 상태이면 `null` |

- 답변 내용은 연락용 이메일로 발송되므로 응답에 포함하지 않는다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 문의 없음 | 404 Not Found | `INQUIRY-1001` |
| 요청한 회원이 문의 작성자가 아님 | 403 Forbidden | `INQUIRY-1002` |

## [7] 관련 문서

- [INQUIRY_SUBMIT_001](INQUIRY_SUBMIT_001.md) — 문의 작성
- [INQUIRY_UPDATE_001](INQUIRY_UPDATE_001.md) — 문의 수정
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 문의, 답변, 문의 상태
