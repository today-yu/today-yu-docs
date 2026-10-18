---
id: INFO_REGISTRATION_REQUEST_SUBMIT_001
title: 정보 등록 요청 제출
context: info
api: POST /api/v1/infos/registration-requests
---

# INFO_REGISTRATION_REQUEST_SUBMIT_001. 정보 등록 요청 제출

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| category | Body | String | 필수, `ACADEMIC`, `STUDY`, `CAREER`, `SCHOLARSHIP`, `FACILITY`, `WELFARE`, `FOOD_AND_CAFE` 중 하나 |
| title | Body | String | 필수, 공백만으로 구성될 수 없음 |
| content | Body | String | 필수, 공백만으로 구성될 수 없음 |
| contactEmail | Body | String | 필수, 이메일 형식 (대소문자 구분 없음) |

## [4] 처리 로직

1. 요청한 회원을 요청자로, 요청 시각을 현재 시각으로 하여 정보 등록 요청(InfoRegistrationRequest)을 저장한다.
   - 요청 상태는 `PENDING`(검토 중)으로 시작한다.
   - 연결된 정보와 반려 사유는 비어 있는 상태로 저장한다.

## [5] 정상 출력

`201 Created`

- 응답 메시지: `정보 등록 요청 제출에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| infoRegistrationRequestId | Long | 저장된 정보 등록 요청 ID |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| `category` 누락 | 422 Unprocessable Content | `COMMON-0002` |
| `title`, `content`, `contactEmail` 누락 또는 공백 | 422 Unprocessable Content | `COMMON-0002` |
| `contactEmail` 형식 오류 | 422 Unprocessable Content | `COMMON-0002` |
| 요청 본문을 읽을 수 없음 (JSON 형식 오류, `category`에 허용되지 않은 값 등) | 400 Bad Request | `COMMON-0003` |

## [7] 미결정 사항

- `title`의 최대 길이 제한이 정해지지 않았다. 현재 입력 검증은 없고, DB 컬럼은 기본 길이(`varchar(255)`)이므로 이를 넘으면 저장 시 서버 에러가 발생할 수 있다.

## [8] 관련 문서

- [INFO_REGISTRATION_REQUEST_LIST_001](INFO_REGISTRATION_REQUEST_LIST_001.md) — 정보 등록 요청 목록 조회
- [INFO_REGISTRATION_REQUEST_DETAIL_001](INFO_REGISTRATION_REQUEST_DETAIL_001.md) — 정보 등록 요청 상세 조회
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 정보 등록 요청, 요청 상태
