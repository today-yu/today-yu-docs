---
id: INFO_DETAIL_001
title: 정보 상세 조회
context: info
api: GET /api/v1/infos/{infoId}
---

# INFO_DETAIL_001. 정보 상세 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| infoId | Path | Long | 필수, 조회할 정보 ID |

## [4] 처리 로직

1. 정보 ID로 정보(Info)를 조회한다.
2. 정보의 조회수(ViewCount)를 조회한다.
3. 정보의 '도움이 되는 정보에요'(HelpfulMark) 수와, 요청한 회원이 '도움이 되는 정보에요'를 표시했는지 여부를 조회한다.
4. 정보에 **반영(`ACCEPTED`)된** 정보 등록 요청과 정보 수정 요청을 요청자 닉네임과 함께 조회한다.
   - 검토 중(`PENDING`), 반려(`REJECTED`)된 요청은 포함하지 않는다.
5. 1~4의 조회에 모두 성공하면 정보의 조회수를 1 증가시킨다.
   - 증가분은 Redis에 정보 ID별로 누적한다.
   - Redis 장애로 증가에 실패해도 요청은 실패하지 않으며, 로그만 남긴다.
   - 응답의 조회수(`viewCount`)는 2에서 조회한 값으로, 이번 요청의 증가분은 포함하지 않는다.
6. 4의 두 요청 목록을 하나의 기여 목록으로 합치고, 요청 시각 내림차순(최신순)으로 정렬한다.

## [5] 정상 출력

`200 OK`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| id | Long | 정보 ID |
| category | String | 카테고리. `ACADEMIC`(학사), `STUDY`(학업), `CAREER`(취업), `SCHOLARSHIP`(장학), `FACILITY`(시설), `WELFARE`(복지), `FOOD_AND_CAFE`(맛집·카페) |
| title | String | 제목 |
| content | String | 본문 |
| publishedAt | LocalDateTime | 발행 시각 |
| updatedAt | LocalDateTime | 최종 수정 시각 |
| viewCount | long | 조회수 |
| helpfulMarkCount | long | '도움이 되는 정보에요' 수 |
| helpfulMarked | boolean | 요청한 회원의 '도움이 되는 정보에요' 표시 여부 |
| contributions | Array | 기여 목록 (요청 시각 내림차순, 없으면 빈 배열) |
| contributions[].memberId | Long | 요청자 회원 ID |
| contributions[].nickname | String | 요청자 닉네임 |
| contributions[].type | String | 기여 유형. `REGISTRATION`(정보 등록 요청), `UPDATE`(정보 수정 요청) |
| contributions[].requestId | Long | 정보 등록 요청 또는 정보 수정 요청 ID |
| contributions[].requestedAt | LocalDateTime | 요청 시각 |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 정보 없음 | 404 Not Found | `INFO-1001` |
| 정보는 존재하나 조회수 데이터가 없음 (데이터 정합성 오류) | 500 Internal Server Error | `COMMON-0001` |

## [7] 미결정 사항

- Redis에 누적한 조회수 증가분을 DB의 조회수에 반영하는 로직은 아직 없다. 반영 전까지 응답의 `viewCount`는 증가하지 않는다.

## [8] 관련 문서

- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 정보, 조회수, 기여자, 도움이 되는 정보에요
