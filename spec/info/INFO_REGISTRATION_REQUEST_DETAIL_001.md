---
id: INFO_REGISTRATION_REQUEST_DETAIL_001
title: 정보 등록 요청 상세 조회
context: info
api: GET /api/v1/infos/registration-requests/{requestId}
---

# INFO_REGISTRATION_REQUEST_DETAIL_001. 정보 등록 요청 상세 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)
- 정보 등록 요청을 작성한 회원 본인만 조회할 수 있다.

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| requestId | Path | Long | 필수, 조회할 정보 등록 요청 ID |

## [4] 처리 로직

1. 정보 등록 요청 ID로 정보 등록 요청(InfoRegistrationRequest)을 조회한다.
2. 요청한 회원이 정보 등록 요청의 작성자인지 확인한다.
   - 정보 등록 요청이 존재하는지 숨기지 않으며, 작성자가 아니면 403으로 응답한다.
3. 정보 등록 요청에 연결된 정보(Info)를 조회한다.
   - 연결된 정보는 요청이 반영(`ACCEPTED`)되어 새 정보가 발행된 경우에만 존재한다. `PENDING`, `REJECTED` 상태에서는 `info`를 `null`로 응답한다.
   - 연결된 정보 ID가 있지만 정보가 존재하지 않으면 요청은 실패하지 않고 `info`를 `null`로 응답하며, 경고 로그를 남긴다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `정보 등록 요청 상세 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| id | Long | 정보 등록 요청 ID |
| requesterId | Long | 요청자 회원 ID |
| category | String | 정보 등록 요청의 카테고리. `ACADEMIC`(학사), `STUDY`(학업), `CAREER`(취업), `SCHOLARSHIP`(장학), `FACILITY`(시설), `WELFARE`(복지), `FOOD_AND_CAFE`(맛집·카페) |
| title | String | 정보 등록 요청 제목 |
| content | String | 정보 등록 요청 내용 |
| contactEmail | String | 연락용 이메일 |
| requestedAt | LocalDateTime | 요청 시각 |
| status | String | 요청 상태. `PENDING`(검토 중), `ACCEPTED`(반영 완료), `REJECTED`(반려) |
| info | Object | 연결된 정보 요약. 연결된 정보가 없거나 존재하지 않으면 `null` |
| info.id | Long | 정보 ID |
| info.category | String | 카테고리. `ACADEMIC`(학사), `STUDY`(학업), `CAREER`(취업), `SCHOLARSHIP`(장학), `FACILITY`(시설), `WELFARE`(복지), `FOOD_AND_CAFE`(맛집·카페) |
| info.title | String | 정보 제목 |
| info.updatedAt | LocalDateTime | 정보 최종 수정 시각 |
| rejectionReason | String | 반려 사유. `REJECTED` 상태가 아니면 `null` |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 정보 등록 요청 없음 | 404 Not Found | `INFO-1101` |
| 요청한 회원이 정보 등록 요청 작성자가 아님 | 403 Forbidden | `INFO-1102` |

## [7] 미결정 사항

- 반영으로 발행된 정보가 이후 삭제되거나 숨김 처리된 경우의 처리 방식은 정해지지 않았다. 현재는 연결된 정보가 조회되지 않으면 `info: null`로 응답한다.

## [8] 관련 문서

- [INFO_REGISTRATION_REQUEST_LIST_001](INFO_REGISTRATION_REQUEST_LIST_001.md) — 정보 등록 요청 목록 조회
- [INFO_REGISTRATION_REQUEST_SUBMIT_001](INFO_REGISTRATION_REQUEST_SUBMIT_001.md) — 정보 등록 요청 제출
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 정보 등록 요청, 요청 상태, 반영, 반려 사유
