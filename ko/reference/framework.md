라이믹스 프레임워크
-----------------

라이믹스 프레임워크는 모듈과 테마 등을 구동할 수 있도록 하는 핵심 기능을 제공합니다.
일부 기능은 모듈에 위임하기도 하지만, 라이믹스 프레임워크에 포함된 대부분의 클래스와 함수들은
특정한 모듈에 종속되지 않고 독립적으로 사용할 수 있습니다.

라이믹스 프레임워크는 크게 아래의 부분들로 이루어져 있습니다.

1. `Rhymix\Framework` 네임스페이스에 소속된 클래스들
2. XE 1.x에서 물려받은 레거시 클래스들
  - 이 클래스들은 서드파티 자료의 하위 호환성을 위해 유지하고 있으나, 점차 `Rhymix\Framework`로 흡수되고 있으며
    이미 껍데기만 남아 alias 역할을 수행하고 있는 것도 있습니다.
  - 장기적으로, `Context`를 제외한 모든 전역 클래스는 지원이 중단될 예정입니다.
3. 전역 함수들
  - XE 1.x에서 물려받은 것도 있고, 라이믹스에서 추가된 것도 있습니다.
  - `@deprecated`로 표기되지 않은 것은 신규 자료에서도 자유롭게 사용할 수 있습니다.
4. 공통 상수들

### Rhymix\Framework

이 네임스페이스에는 아래와 같은 클래스들이 포함되어 있습니다.

(*)가 붙은 클래스는 인스턴스를 만들어 사용하는 클래스입니다.
나머지는 모두 `static`으로 사용할 수 있습니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| [Cache](framework/Cache.md) | 처리 속도 향상을 위한 캐시 처리를 담당합니다.
| [Calendar](framework/Calendar.md) | 달력 UI 제작에 도움이 되는 유틸리티 함수들을 제공합니다.
| [Config](framework/Config.md) | 시스템 설정 로딩, 변환, 저장을 담당합니다.
| [Cookie](framework/Cookie.md) | 쿠키를 굽고 가져오는 기능을 제공합니다.
| [DateTime](framework/DateTime.md) | 날짜/시간 형식 변환과 표준 시간대 처리를 담당합니다.
| [DB](framework/DB.md) (*) | DB 접속과 관리, XML 쿼리 및 커스텀 쿼리 실행을 담당합니다.
| [Debug](framework/Debug.md) | 디버그와 관련된 기능을 제공합니다.
| [Exception](framework/Exception.md) (*) | 라이믹스 내에서 예외 처리에 사용하는 공통 클래스이며, `\Exception`을 상속합니다.
| [Formatter](framework/Formatter.md) | 텍스트와 HTML의 상호 변환, SCSS 컴파일 및 압축, 합치기 등의 기능을 담당합니다.
| [HTTP](framework/HTTP.md) | API, 크롤링, 파일 다운로드 등 외부 리소스를 가져오는 기능을 제공합니다.
| [i18n](framework/i18n.md) | 표준화된 국가 및 언어 목록을 제공합니다.
| [Image](framework/Image.md) | 이미지 포맷 감지 및 변환 기능을 제공합니다.
| [Korea](framework/Korea.md) | 국내 전화번호 유효성 확인, 형식 변환 등 대한민국에서 유용한 기능을 제공합니다.
| [Lang](framework/Lang.md) (*) | 라이믹스의 다국어 기능을 담당합니다.
| [Mail](framework/Mail.md) (*) | 이메일을 발송합니다.
| [MIME](framework/MIME.md) | 파일 타입 파악, 확장자 처리 등을 담당합니다.
| [Pagination](framework/Pagination.md) | 페이지 분할에 도움이 되는 유틸리티 함수들을 제공합니다.
| [Password](framework/Password.md) | 다양한 형식의 비밀번호 암호화 및 검증과 관련된 기능을 담당합니다.
| [Push](framework/Push.md) (*) | 푸시 알림을 발송합니다.
| [Queue](framework/Queue.md) | 비동기 작업을 생성, 관리, 실행합니다.
| [Request](framework/Request.md) (*) | 하나의 HTTP 요청을 표현합니다. `Router`에서 이 클래스의 인스턴스를 반환합니다.
| [Router](framework/Router.md) | 짧은주소를 처리합니다.
| [Security](framework/Security.md) | 랜덤 문자열 생성, 암호화, 전자서명, CSRF 방어 등 보안에 필요한 기능을 담당합니다.
| [Session](framework/Session.md) | 세션 처리를 담당합니다.
| [SMS](framework/SMS.md) (*) | SMS를 발송합니다.
| [Storage](framework/Storage.md) | 파일과 디렉토리를 안전하게 관리하는 데 도움이 되는 유틸리티 함수들을 제공합니다.
| [Template](framework/Template.md) (*) | 템플릿을 불러와서 컴파일하고 출력합니다.
| [Timer](framework/Timer.md) | 어떤 작업의 소요시간을 측정할 수 있습니다.
| [UA](framework/UA.md) | `User-Agent`의 해석과 이에 따른 몇 가지 유용한 구분을 제공합니다.
| [URL](framework/URL.md) | URL을 해석하고 변환하는 함수들을 제공합니다.

(자세한 정보 추가 예정)

#### 드라이버

라이믹스 프레임워크는 다양한 방법으로 메일, SMS 등을 발송하거나,
서로 다른 서버 환경에서 캐시를 원활하게 사용하기 위한 "드라이버"를 포함합니다.

이러한 드라이버들은 `Rhymix\Framework\Drivers` 네임스페이스에 용도에 따라 분류되어 있습니다.

#### 예외 클래스

아래의 클래스들은 `Rhymix\Framework\Exceptions` 네임스페이스에 있습니다.
모두 `Rhymix\Framework\Exception` 클래스를 상속하며, 각각 용도에 따라 사용합니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| DBError              | DB 접속 오류 등
| FeatureDisabled      | 사용할 수 없는 기능입니다.
| InvalidRequest       | 잘못된 요청입니다.
| MustLogin            | 로그인이 필요합니다.
| NotPermitted         | 권한이 없습니다.
| QueryError           | DB 쿼리 오류
| SecurityViolation    | 보안정책상 허용되지 않습니다.
| TargetNotFound       | 대상을 찾을 수 없습니다.

#### 필터 클래스

아래의 클래스들은 `Rhymix\Framework\Filters` 네임스페이스에 있습니다.
필터 클래스들은 신뢰할 수 없는 외부 입력값을 처리하는 최전방에 위치하여,
라이믹스의 보안에 핵심적인 역할을 수행합니다.

| 클래스                | 설명                                                                       |
|----------------------|---------------------------------------------------------------------------|
| [FileContentFilter](framework/filters/FileContentFilter.md) | 업르도된 파일의 내용에 따른 상세 필터링
| [FilenameFilter](framework/filters/FilenameFilter.md) | 파일명 필터링
| [HTMLFilter](framework/filters/HTMLFilter.md) | XSS 공격 방지를 위한 HTML 태그 및 속성 필터링
| [IpFilter](framework/filters/IpFilter.md) | IP 주소 필터링 (허용/차단 목록)
| [MediaFilter](framework/filters/MediaFilter.md) | iframe 등으로 본문에 삽입된 외부 미디어의 URL 필터링

#### 도우미 클래스

도우미 클래스들은 `Rhymix\Framework\Helpers` 네임스페이스에 있습니다.

특별한 규칙 없이, 라이믹스 프레임워크의 다른 영역에서 클래스가 필요한 경우 임의로 선언하여 사용하고 있습니다.

#### 파서 클래스

파서 클래스들은 `Rhymix\Framework\Parsers` 네임스페이스에 있습니다.

템플릿과 쿼리를 비롯한 다양한 파일들을 해석하여 적절한 형태로 변환하는 기능을 수행합니다.

### 레거시 클래스

XE 1.x부터 존재해 온 전역 클래스들에 대한 자세한 설명은 아래의 페이지를 참고하십시오.

[레거시 클래스](legacy.md)

### 전역 함수

라이믹스에서 제공하는 전역 함수와 레거시 전역 함수들에 대한 자세한 설명은 아래의 페이지를 참고하십시오.

[전역 함수](functions.md)

### 공통 상수

#### 라이믹스 실행 환경

라이믹스 설치 경로, 버전, 접속 환경 등에 대한 정보가 필요한 경우,
다른 파일을 해석하거나 `$_SERVER` 등의 전역변수를 참조하는 것보다
아래의 상수들을 먼저 활용하는 것을 권장합니다.

| 상수                    | 설명                                                                     |
|------------------------|-------------------------------------------------------------------------|
| `RX_VERSION`           | 라이믹스 버전
| `RX_MICROTIME`         | 라이믹스 실행 시작 시간 (유닉스 타임스탬프, 마이크로초까지)
| `RX_TIME`              | 라이믹스 실행 시작 시간 (유닉스 타임스탬프, 정수)
| `RX_BASEDIR`           | 라이믹스가 설치된 서버 파일시스템 절대경로 (마지막 슬래시 있음)
| `RX_BASEURL`           | 라이믹스가 설치된 URL의 절대경로 (마지막 슬래시 있음)
| `RX_REQUEST_URL`       | 현재 요청한 URL에서 `RX_BASEURL`을 제외한 나머지
| `RX_CLIENT_IP_VERSION` | 현재 방문자의 IP 주소가 IPv4인 경우 `4`, IPv6인 경우 `6`
| `RX_CLIENT_IP`         | 현재 방문자의 IP 주소
| `RX_SSL`               | 현재 요청이 SSL/TLS를 사용하는 경우 `true`, 그렇지 않으면 `false`
| `RX_POST`              | 현재 요청이 POST 요청인 경우 `true`, 그렇지 않으면 `false`
| `RX_WINDOWS`           | 윈도우 서버인 경우 `true`, 그렇지 않으면 `false`

#### 상태값, 유효한 문자의 범위 등

문서나 댓글의 상태를 표현하는 데 사용할 수 있는 값들입니다.
현재 `PUBLIC`과 `SECRET`이 코어에서 구현되어 있으며,
나머지 값들은 서드파티 자료에서 공통으로 사용하기 위한 표준으로서 미리 선언해 둔 것입니다.

| 상수                    | 설명                                                                     |
|------------------------|-------------------------------------------------------------------------|
| `RX_STATUS_TEMP`       | 0
| `RX_STATUS_PRIVATE`    | 10
| `RX_STATUS_PUBLIC`     | 1
| `RX_STATUS_SECRET`     | 2
| `RX_STATUS_EMBARGO`    | 3
| `RX_STATUS_TRASH`      | 4
| `RX_STATUS_CENSORED`   | 5
| `RX_STATUS_CENSORED_BY_ADMIN` | 6
| `RX_STATUS_DELETED`    | 7
| `RX_STATUS_DELETED_BY_ADMIN` | 8
| `RX_STATUS_OTHER`      | 9
| `DIGITS`               | 0123456789
| `XDIGITS`              | 0123456789abcdef
| `ALPHABETS`            | ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
| `UPPER`                | ABCDEFGHIJKLMNOPQRSTUVWXYZ
| `LOWER`                | abcdefghijklmnopqrstuvwxyz
| `CR`                   | \r
| `CRLF`                 | \r\n
| `LF`                   | \n

#### XE 호환을 위해 제공하는 상수

아래의 상수들은 더이상 사용하지 않는 것을 권장합니다.

| 상수                    | 설명                                                                     |
|------------------------|-------------------------------------------------------------------------|
| `__XE__`               | true (인클루드 전 미리 정의할 필요 없음)
| `__ZBXE__`             | true (인클루드 전 미리 정의할 필요 없음)
| `__XE_VERSION__`       | `RX_VERSION`과 동일
| `__XE_VERSION_ALPHA__` | false
| `__XE_VERSION_BETA__`  | false
| `__XE_VERSION_RC__`    | false
| `__XE_VERSION_STABLE__` | true
| `__XE_MIN_PHP_VERSION__` | 최소 PHP 버전 (라이믹스 2.1.24 현재 `7.4.0`)
| `__XE_RECOMMEND_PHP_VERSION__` | 권장 PHP 버전 (라이믹스 2.1.24 현재 `7.4.0`)
| `__ZBXE_VERSION__`     | `RX_VERSION`과 동일
| `_XE_LOCATION_`        | ko
| `_XE_PACKAGE_`         | XE
| `_XE_PATH_`            | `RX_BASEDIR`과 동일
| `Y`                    | 문자열 `'Y'`
| `N`                    | 문자열 `'N'`
| `FOLLOW_REQUEST_SSL`   | 0
| `ENFORCE_SSL`          | 1
| `RELEASE_SSL`          | 2
