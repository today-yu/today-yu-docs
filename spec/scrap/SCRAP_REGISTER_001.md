---
id: SCRAP_REGISTER_001
title: 스크랩 등록
context: scrap
api: PUT /api/v1/infos/{infoId}/scrap
---

# SCRAP_REGISTER_001. 스크랩 등록

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| infoId | Path | Long | 필수, 스크랩할 정보 ID |

## [4] 처리 로직

1. 정보 ID로 정보(Info)가 존재하는지 확인한다.
2. 요청한 회원이 해당 정보를 이미 스크랩했는지 확인한다.
3. 스크랩하지 않은 정보이면 요청 시각을 스크랩 시각으로 하여 스크랩(Scrap)을 저장한다.
   - 이미 스크랩한 정보이면 새로 저장하지 않고 정상 응답한다. (멱등)

## [5] 정상 출력

`200 OK`

- 응답 메시지: `스크랩에 성공하였습니다.`
- 응답 데이터 없음 (`data: null`)
- 새로 스크랩한 경우와 이미 스크랩한 정보인 경우의 응답은 같다.

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 정보 없음 | 404 Not Found | `INFO-1001` |

## [7] 미결정 사항

- 같은 회원이 같은 정보에 대해 동시에 스크랩을 요청하면 스크랩이 중복 저장될 수 있다. `(info_id, member_id)` unique 제약과 제약 위반 시 처리는 이후 `GlobalExceptionHandler`에서 일괄 적용할 예정이다.

## [8] 관련 문서

- [INFO_DETAIL_001](../info/INFO_DETAIL_001.md) — 정보 상세 조회
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 스크랩 (회원 한 명이 정보 하나를 한 번만 스크랩 가능)
