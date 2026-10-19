---
id: INFO_HELPFUL_MARK_REGISTER_001
title: '도움이 되는 정보에요' 표시
context: info
api: PUT /api/v1/infos/{infoId}/helpful-mark
---

# INFO_HELPFUL_MARK_REGISTER_001. '도움이 되는 정보에요' 표시

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| infoId | Path | Long | 필수, '도움이 되는 정보에요'를 표시할 정보 ID |

## [4] 처리 로직

1. 정보 ID로 정보(Info)가 존재하는지 확인한다.
2. 요청한 회원이 해당 정보에 이미 '도움이 되는 정보에요'를 표시했는지 확인한다.
3. 표시하지 않은 정보이면 '도움이 되는 정보에요'(HelpfulMark)를 저장한다.
   - 이미 표시한 정보이면 새로 저장하지 않고 정상 응답한다. (멱등)

## [5] 정상 출력

`200 OK`

- 응답 메시지: `'도움이 되는 정보에요' 표시에 성공하였습니다.`
- 응답 데이터 없음 (`data: null`)
- 새로 표시한 경우와 이미 표시한 정보인 경우의 응답은 같다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 정보 없음 | 404 Not Found | `INFO-1001` |

## [7] 미결정 사항

- 같은 회원이 같은 정보에 대해 동시에 표시를 요청하면 '도움이 되는 정보에요'가 중복 저장될 수 있다. `(info_id, member_id)` unique 제약과 제약 위반 시 처리는 이후 `GlobalExceptionHandler`에서 일괄 적용할 예정이다.

## [8] 관련 문서

- [INFO_HELPFUL_MARK_CANCEL_001](INFO_HELPFUL_MARK_CANCEL_001.md) — '도움이 되는 정보에요' 해제
- [INFO_DETAIL_001](INFO_DETAIL_001.md) — 정보 상세 조회 (`helpfulMarkCount`, `helpfulMarked`)
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 도움이 되는 정보에요 (회원 한 명이 정보 하나에 한 번만 가능)
