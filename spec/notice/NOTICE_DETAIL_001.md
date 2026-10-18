---
id: NOTICE_DETAIL_001
title: 공지 상세 조회
context: notice
api: GET /api/v1/notices/{noticeId}
---

# NOTICE_DETAIL_001. 공지 상세 조회

## [1] 접근 권한

- `MEMBER` 역할을 가진 회원 (이미 로그인된 사용자)

## [2] 사전 조건

- 유효한 Access Token으로 인증된 상태

## [3] 입력

| 이름 | 위치 | 타입 | 제약 |
| --- | --- | --- | --- |
| Authorization | Header | String | 필수, `Bearer {AccessToken}` 형식 |
| noticeId | Path | Long | 필수, 조회할 공지 ID |

## [4] 처리 로직

1. 공지 ID로 공지(Notice)를 조회한다.

## [5] 정상 출력

`200 OK`

- 응답 메시지: `공지 상세 조회에 성공하였습니다.`

| 필드 | 타입 | 설명 |
| --- | --- | --- |
| id | Long | 공지 ID |
| title | String | 공지 제목 |
| content | String | 공지 내용 |
| publishedAt | LocalDateTime | 발행 시각 |
| updatedAt | LocalDateTime | 최종 수정 시각 |

## [6] 예외

| 상황 | 응답 | 에러 코드 |
| --- | --- | --- |
| Access Token 검증 실패 | [공통 사항](../README.md#인증이-필요한-api의-access-token-검증) 참고 | |
| 공지 없음 | 404 Not Found | `NOTICE-1001` |

## [7] 관련 문서

- [NOTICE_LIST_001](NOTICE_LIST_001.md) — 공지 목록 조회
- [`design/ubiquitous-language.md`](../../design/ubiquitous-language.md) — 공지
