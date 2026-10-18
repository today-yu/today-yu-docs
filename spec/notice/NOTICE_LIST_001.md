---
id: NOTICE_LIST_001
title: 공지 목록 조회
context: notice
api: GET /api/v1/notices
---

# NOTICE_LIST_001. 공지 목록 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| cursorId | Query | Long | 선택, 이전 응답의 `nextCursor.id`. 없으면 첫 페이지를 조회 |
| size | Query | Integer | 선택, 1 이상 50 이하. 없으면 10 |

## [4] 처리 로직

1. 공지를 최신 발행 순으로 정렬한다.
   - 공지 ID는 발행 순서대로 증가하므로, 공지 ID 내림차순을 최신 발행 순으로 사용한다.
2. 커서가 있으면 공지 ID가 커서보다 작은 공지만 조회한다.
3. 다음 페이지 존재 여부를 판단하기 위해 `size + 1`개를 조회한다.
   - 조회 결과가 `size`보다 많으면 다음 페이지가 있는 것으로 보고, `size`개만 응답한다.
4. 다음 페이지가 있으면 마지막 공지의 ID로 다음 커서를 만든다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `공지 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| contents | Array | 공지 목록 (최신 발행 순, 없으면 빈 배열) |
| contents[].id | Long | 공지 ID |
| contents[].title | String | 공지 제목 |
| contents[].publishedAt | LocalDateTime | 발행 시각 |
| hasNext | boolean | 다음 페이지 존재 여부 |
| nextCursor | Object | 다음 페이지 조회용 커서. 다음 페이지가 없으면 `null` |
| nextCursor.id | Long | 다음 요청의 `cursorId`로 사용 |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| `size`가 1 미만 또는 50 초과 | 422 Unprocessable Content | `COMMON-0002` |

## [7] 관련 문서

- [NOTICE_DETAIL_001](NOTICE_DETAIL_001.md) — 공지 상세 조회
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 공지
