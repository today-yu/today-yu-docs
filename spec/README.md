# 기능 명세서

기능 단위로 동작, 입력·출력, 예외를 정의합니다.

- 파일명은 `{컨텍스트}_{기능/화면}_{번호}.md` 형식의 기능 ID를 사용합니다.
- 같은 기능의 변형은 번호를 증가시켜 별도 문서로 작성합니다.
- 용어는 [`design/ubiquitous-language.md`](../design/ubiquitous-language.md)를 따릅니다.

## 기능 목록

### Auth

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [AUTH_SIGNIN_001](auth/AUTH_SIGNIN_001.md) | 일반 로그인 | `POST /api/v1/auth/sign-in` |
| [AUTH_SIGNOUT_001](auth/AUTH_SIGNOUT_001.md) | 로그아웃 | `POST /api/v1/auth/sign-out` |
| [AUTH_REISSUE_001](auth/AUTH_REISSUE_001.md) | 인증 토큰 재발급 | `POST /api/v1/auth/reissue` |

### Member

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [MEMBER_VERIFICATION_ISSUE_001](member/MEMBER_VERIFICATION_ISSUE_001.md) | 이메일 인증 코드 발급 | `POST /api/v1/auth/email-verification/issue` |
| [MEMBER_VERIFICATION_VERIFY_001](member/MEMBER_VERIFICATION_VERIFY_001.md) | 이메일 인증 코드 검증 | `POST /api/v1/auth/email-verification/verify` |
| [MEMBER_SIGNUP_001](member/MEMBER_SIGNUP_001.md) | 일반 회원가입 | `POST /api/v1/auth/sign-up` |
| [MEMBER_ME_001](member/MEMBER_ME_001.md) | 내 정보 조회 | `GET /api/v1/members/me` |
| [MEMBER_PASSWORD_RESET_ISSUE_001](member/MEMBER_PASSWORD_RESET_ISSUE_001.md) | 비밀번호 초기화 인증 코드 발급 | `POST /api/v1/auth/password-reset-verification/issue` |

### Info

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [INFO_LIST_001](info/INFO_LIST_001.md) | 정보 목록 조회 | `GET /api/v1/infos` |
| [INFO_DETAIL_001](info/INFO_DETAIL_001.md) | 정보 상세 조회 | `GET /api/v1/infos/{infoId}` |
| [INFO_UPDATE_REQUEST_LIST_001](info/INFO_UPDATE_REQUEST_LIST_001.md) | 정보 수정 요청 목록 조회 | `GET /api/v1/infos/update-requests` |
| [INFO_UPDATE_REQUEST_DETAIL_001](info/INFO_UPDATE_REQUEST_DETAIL_001.md) | 정보 수정 요청 상세 조회 | `GET /api/v1/infos/update-requests/{requestId}` |
| [INFO_UPDATE_REQUEST_SUBMIT_001](info/INFO_UPDATE_REQUEST_SUBMIT_001.md) | 정보 수정 요청 제출 | `POST /api/v1/infos/{infoId}/update-requests` |
| [INFO_REGISTRATION_REQUEST_LIST_001](info/INFO_REGISTRATION_REQUEST_LIST_001.md) | 정보 등록 요청 목록 조회 | `GET /api/v1/infos/registration-requests` |
| [INFO_REGISTRATION_REQUEST_DETAIL_001](info/INFO_REGISTRATION_REQUEST_DETAIL_001.md) | 정보 등록 요청 상세 조회 | `GET /api/v1/infos/registration-requests/{requestId}` |
| [INFO_REGISTRATION_REQUEST_SUBMIT_001](info/INFO_REGISTRATION_REQUEST_SUBMIT_001.md) | 정보 등록 요청 제출 | `POST /api/v1/infos/registration-requests` |
| [INFO_HELPFUL_MARK_REGISTER_001](info/INFO_HELPFUL_MARK_REGISTER_001.md) | '도움이 되는 정보에요' 표시 | `PUT /api/v1/infos/{infoId}/helpful-mark` |
| [INFO_HELPFUL_MARK_CANCEL_001](info/INFO_HELPFUL_MARK_CANCEL_001.md) | '도움이 되는 정보에요' 해제 | `DELETE /api/v1/infos/{infoId}/helpful-mark` |

### Scrap

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [SCRAP_LIST_001](scrap/SCRAP_LIST_001.md) | 스크랩 목록 조회 | `GET /api/v1/infos/scraps` |
| [SCRAP_REGISTER_001](scrap/SCRAP_REGISTER_001.md) | 스크랩 등록 | `PUT /api/v1/infos/{infoId}/scrap` |
| [SCRAP_CANCEL_001](scrap/SCRAP_CANCEL_001.md) | 스크랩 해제 | `DELETE /api/v1/infos/{infoId}/scrap` |

### Notice

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [NOTICE_LIST_001](notice/NOTICE_LIST_001.md) | 공지 목록 조회 | `GET /api/v1/notices` |
| [NOTICE_DETAIL_001](notice/NOTICE_DETAIL_001.md) | 공지 상세 조회 | `GET /api/v1/notices/{noticeId}` |

### Inquiry

| 기능 ID | 기능명 | API |
| --- | --- | --- |
| [INQUIRY_LIST_001](inquiry/INQUIRY_LIST_001.md) | 문의 목록 조회 | `GET /api/v1/inquiries` |
| [INQUIRY_DETAIL_001](inquiry/INQUIRY_DETAIL_001.md) | 문의 상세 조회 | `GET /api/v1/inquiries/{inquiryId}` |
| [INQUIRY_SUBMIT_001](inquiry/INQUIRY_SUBMIT_001.md) | 문의 작성 | `POST /api/v1/inquiries` |
| [INQUIRY_UPDATE_001](inquiry/INQUIRY_UPDATE_001.md) | 문의 수정 | `PUT /api/v1/inquiries/{inquiryId}` |
| [INQUIRY_DELETE_001](inquiry/INQUIRY_DELETE_001.md) | 문의 삭제 | `DELETE /api/v1/inquiries/{inquiryId}` |

## 공통 사항

### 성공 응답

```json
{
  "message": "성공 메시지",
  "data": { }
}
```

- 응답 데이터가 없는 경우 `data`는 `null`입니다.

### 실패 응답

```json
{
  "errorCode": "AUTH-1003",
  "message": "에러 메시지",
  "errors": { "필드명": "검증 실패 메시지" },
  "data": { }
}
```

- `errors`, `data`는 값이 있을 때만 포함됩니다.

## 기능 명세서 구성

1. 접근 권한
2. 사전 조건
3. 입력
4. 처리 로직
5. 정상 출력
6. 예외
7. 관련 문서
