---
id: INFO_HELPFUL_MARK_CANCEL_001
title: '도움이 되는 정보에요' 해제
context: info
api: DELETE /api/v1/infos/{infoId}/helpful-mark
---

# INFO_HELPFUL_MARK_CANCEL_001. '도움이 되는 정보에요' 해제

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| infoId | Path | Long | 필수, '도움이 되는 정보에요'를 해제할 정보 ID |

## [4] 처리 로직

1. 정보 ID로 정보(Info)가 존재하는지 확인한다.
2. 요청한 회원이 해당 정보에 표시한 '도움이 되는 정보에요'(HelpfulMark)를 삭제한다.
   - 표시하지 않은 정보이면 아무것도 하지 않고 정상 응답한다. (멱등)

## [5] 정상 출력

`200 OK`

- 응답 메시지: `'도움이 되는 정보에요' 해제에 성공하였습니다.`
- 응답 데이터 없음 (`data: null`)
- 표시를 해제한 경우와 표시하지 않은 정보인 경우의 응답은 같다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 정보 없음 | 404 Not Found | `INFO-1001` |

## [7] 관련 문서

- [INFO_HELPFUL_MARK_REGISTER_001](INFO_HELPFUL_MARK_REGISTER_001.md) — '도움이 되는 정보에요' 표시
- [INFO_DETAIL_001](INFO_DETAIL_001.md) — 정보 상세 조회 (`helpfulMarkCount`, `helpfulMarked`)
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 도움이 되는 정보에요
