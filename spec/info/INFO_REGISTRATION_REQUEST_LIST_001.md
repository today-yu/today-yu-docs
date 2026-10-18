---
id: INFO_REGISTRATION_REQUEST_LIST_001
title: 정보 등록 요청 목록 조회
context: info
api: GET /api/v1/infos/registration-requests
---

# INFO_REGISTRATION_REQUEST_LIST_001. 정보 등록 요청 목록 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| category | Query | String | 선택, `ACADEMIC`, `STUDY`, `CAREER`, `SCHOLARSHIP`, `FACILITY`, `WELFARE`, `FOOD_AND_CAFE` 중 하나. 없으면 전체 카테고리 조회 |
| sortType | Query | String | 필수, `LATEST`(최신순), `OLDEST`(오래된 순) 중 하나 |
| cursorRequestedAt | Query | LocalDateTime | 선택, 이전 응답의 `nextCursor.requestedAt` |
| cursorId | Query | Long | 선택, 이전 응답의 `nextCursor.id` |
| size | Query | Integer | 선택, 1 이상 50 이하. 없으면 10 |

- `cursorRequestedAt`과 `cursorId`는 둘 다 있거나 둘 다 없어야 한다. 둘 다 없으면 첫 페이지를 조회한다.

## [4] 처리 로직

1. 요청한 회원 본인이 작성한 정보 등록 요청만 대상으로 한다.
   - 카테고리가 있으면 **정보 등록 요청의 카테고리**가 일치하는 요청만 조회한다.
   - 요청 상태(`PENDING`, `ACCEPTED`, `REJECTED`)와 관계없이 모두 조회한다.
2. 정렬 기준에 따라 정보 등록 요청을 정렬한다. 요청 시각(`requestedAt`)이 같으면 정보 등록 요청 ID로 순서를 정한다.
   - `LATEST`: 요청 시각 내림차순, 정보 등록 요청 ID 내림차순
   - `OLDEST`: 요청 시각 오름차순, 정보 등록 요청 ID 오름차순
3. 커서가 있으면 커서 이후의 정보 등록 요청만 조회한다.
   - `LATEST`: 요청 시각이 커서보다 이전이거나, 같으면서 정보 등록 요청 ID가 커서보다 작은 요청
   - `OLDEST`: 요청 시각이 커서보다 이후이거나, 같으면서 정보 등록 요청 ID가 커서보다 큰 요청
4. 다음 페이지 존재 여부를 판단하기 위해 `size + 1`개를 조회한다.
   - 조회 결과가 `size`보다 많으면 다음 페이지가 있는 것으로 보고, `size`개만 응답한다.
5. 다음 페이지가 있으면 마지막 정보 등록 요청의 요청 시각과 ID로 다음 커서를 만든다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `정보 등록 요청 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| contents | Array | 정보 등록 요청 목록 (정렬 기준 순서, 없으면 빈 배열) |
| contents[].id | Long | 정보 등록 요청 ID |
| contents[].category | String | 정보 등록 요청의 카테고리. `ACADEMIC`(학사), `STUDY`(학업), `CAREER`(취업), `SCHOLARSHIP`(장학), `FACILITY`(시설), `WELFARE`(복지), `FOOD_AND_CAFE`(맛집·카페) |
| contents[].title | String | 정보 등록 요청 제목 |
| contents[].status | String | 요청 상태. `PENDING`(검토 중), `ACCEPTED`(반영 완료), `REJECTED`(반려) |
| contents[].requestedAt | LocalDateTime | 요청 시각 |
| hasNext | boolean | 다음 페이지 존재 여부 |
| nextCursor | Object | 다음 페이지 조회용 커서. 다음 페이지가 없으면 `null` |
| nextCursor.requestedAt | LocalDateTime | 다음 요청의 `cursorRequestedAt`으로 사용 |
| nextCursor.id | Long | 다음 요청의 `cursorId`로 사용 |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| `sortType` 누락 | 422 Unprocessable Content | `COMMON-0002` |
| `category`, `sortType`에 허용되지 않은 값 | 422 Unprocessable Content | `COMMON-0002` |
| `cursorRequestedAt`, `cursorId` 중 하나만 존재 | 422 Unprocessable Content | `COMMON-0002` |
| `size`가 1 미만 또는 50 초과 | 422 Unprocessable Content | `COMMON-0002` |

## [7] 관련 문서

- [INFO_REGISTRATION_REQUEST_DETAIL_001](INFO_REGISTRATION_REQUEST_DETAIL_001.md) — 정보 등록 요청 상세 조회
- [INFO_REGISTRATION_REQUEST_SUBMIT_001](INFO_REGISTRATION_REQUEST_SUBMIT_001.md) — 정보 등록 요청 제출
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 정보 등록 요청, 요청 상태
