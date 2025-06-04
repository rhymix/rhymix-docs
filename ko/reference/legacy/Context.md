Context
-------

`Context` 클래스는 라이믹스 실행 초기에 요청에 대한 정보를 수집, 정리하여 저장하고,
필요할 때 쉽게 불러와서 사용할 수 있도록 도와주는 역할을 합니다.
또한 응답과 관련된 다양한 속성을 관리하며, 모듈이 템플릿에 전달하는 전역 변수도 관리합니다.

`Context` 요청과 응답을 처리하는 데 필수적인 역할을 하고 있지만,
지나치게 많고 다양한 기능을 모아 놓아서 관리가 어려워진다는 문제점이 있으므로
라이믹스에서는 앞으로 이 클래스의 기능들을 점진적으로 분리하여 관리할 예정입니다.

예를 들어 요청과 관련된 정보는 점차 `Rhymix\Framework\Request` 클래스로 이전될 예정이며,
`<meta>` 태그 등 잡다한 응답 속성을 관리하는 클래스도 분리될 수 있습니다.

이 문서에서는 `Context` 클래스의 주요 메소드를 임의로 분류하여 설명합니다.


### 초기화

#### getInstance()

```
public static function getInstance(): self
```

싱글턴 인스턴스를 반환합니다.

XE 1.x에서는 `Context` 클래스의 싱글턴 인스턴스를 생성하여 전역 변수에 할당해 두고 사용하였지만,
라이믹스에서는 `Context` 클래스의 모든 메소드가 `static`으로 선언되어 있으므로
일반적으로 인스턴스를 다룰 필요가 없습니다.

즉, 아래와 같은 XE 1.x 형식의 코드는

```
$oContext = Context::getInstance();
$oContext->init();
```

아래와 같이 간소화할 수 있습니다.

```
Context::init();
```

현재 이 메소드는 내부적으로만 사용합니다.

#### init()

```
public static function init(): void
```

현재 요청 정보를 기반으로 `Context`를 초기화합니다.
이 메소드는 라이믹스의 초기화 과정에서 자동으로 호출되며,
라이믹스가 시작될 때 필요한 기본 설정을 수행합니다.
하나의 스크립트가 실행되는 도중에는 여러 번 호출할 수 없습니다.

#### close()

```
public static function close()
```

DB 연결을 닫고, 세션을 종료합니다.
이 메소드는 라이믹스의 종료 과정에서 자동으로 호출되며,
자잘한 뒷정리 작업을 수행합니다.
이 메소드가 호출된 후에는 세션이나 DB를 사용하는 데 어려움이 있을 수 있습니다.



### Context 변수 관련 메소드

`Context`에 저장된 변수는 전역 변수와 비슷한 역할을 하며, 모듈과 템플릿 사이,
그리고 템플릿과 템플릿 사이에서 데이터를 공유하는 데 사용됩니다.
HTTP 요청을 통해 들어온 변수는 모두 기본으로 `Context`에 저장되지만,
임의의 변수를 추가로 저장할 수 있고, HTTP 요청을 통해 들어온 변수의 값을 덮어쓸 수도 있습니다.

아래의 메소드들은 이러한 변수를 조작하는 데 사용됩니다.

#### set()

```
public static function set(
    string $key,
    mixed $val,
    bool $replace_request_arg = false
): void
```

`Context`에 변수를 생성하거나, 이미 존재하는 변수의 값을 대체합니다.

`$replace_request_arg` 인자를 `true`로 설정하면,
HTTP 요청을 통해 들어온 변수의 값을 덮어씁니다.
그렇게 하지 않더라도 `Context::get()`을 했을 때의 결과는 동일하지만,
`Context::getRequestVars()`를 통해 요청 변수만 따로 가져올 때는
덮어썼는지 여부가 차이를 낳습니다.
값을 `null` 또는 빈 문자열로 설정함으로써 HTTP 요청 변수르 삭제할 수도 있습니다.

존재하는 HTTP 요청 변수와 같은 이름의 `$key`를 사용하면
HTTP 요청 변수를 덮어쓰려는 의도로 간주하는 것이 기본값입니다.
그렇다면 `$replace_request_arg` 인자를 `true`로 설정할 일이 없지 않겠냐고 생각할 수 있지만,
역시 `Context::getRequestVars()`를 통해 요청 변수만 따로 가져올 때는 차이가 발생합니다.

#### get()

```
public static function get(string $key): mixed
```

`Context`에 저장된 변수를 가져옵니다.

#### gets()

```
public static function gets(...$keys): ?object
```

`Context`에 저장된 여러 변수를 한 번에 가져옵니다.
변수들은 객체 형태로 반환되며, 존재하지 않는 키의 값은 `null`로 설정됩니다.

가져올 변수를 각각의 인자로 전달할 수도 있고,
라이믹스 2.1.24부터는 하나의 배열에 담아 전달할 수도 있습니다.

가져올 변수가 매우 많거나, 거의 모든 HTTP 요청 변수를 가져오고 싶은 경우,
이 메소드보다는 `Context::getAll()`이나 `Context::getRequestVars()`를 사용하는 것이 더 효율적입니다.

가져올 변수를 단 한 개도 지정하지 않은 경우, `null`을 반환합니다.

#### getAll()

```
public static function getAll(): object
```

`Context`에 저장된 모든 변수를 객체 형태로 반환합니다.

#### getRequestVars()

```
public static function getRequestVars(): object
```

모든 HTTP 요청 변수를 객체 형태로 반환합니다.
`Context::set()`을 통해 추가하거나 덮어쓴 HTTP 요청 변수도 포함되나,
일반적으로 `Context`에 추가한 변수는 포함되지 않습니다.

#### clearRequestVars()

```
public static function clearRequestVars(): void
```

`Context`에 저장된 모든 HTTP 요청 변수를 삭제합니다.

#### clearUserVars()

```
public static function clearUserVars(): void
```

`Context`에 저장된 모든 사용자 정의 변수를 삭제합니다.
HTTP 요청 변수는 삭제하지 않습니다.



### 요청 속성 관련 메소드

#### getCurrentRequest()

```
public static function getCurrentRequest(): Rhymix\Framework\Request
```

현재 요청 정보를 담고 있는 `Rhymix\Framework\Request` 오브젝트를 반환합니다.

#### getRequestUrl()

```
public static function getRequestUrl(): string
```

현재 요청된 URL을 반환합니다.
`getCurrentPageUrl()` 및 `Rhymix\Framework\URL::getCurrentURL()`과 동일한 의미입니다.

#### getRequestMethod()

```
public static function getRequestMethod(): string
```

현재 요청 형식을 반환합니다. 라이믹스가 지원하는 요청 형식은 다음과 같습니다.

- `'GET'`: GET 요청
- `'POST'`: 일반적인 POST 요청
- `'JSON'`: POST 요청으로서 JSON 응답을 요청한 경우
- `'XMLRPC'`: POST 요청으로서 XE 1.x 방식의 XML 응답을 요청한 경우
- `'JS_CALLBACK'`: XE 1.x 방식의 JSONP 응답을 요청한 경우

#### setRequestMethod()

```
public static function setRequestMethod(string $type = ''): void
```

현재 요청 형식을 지정합니다. `Context` 초기화 과정에서 자동으로 호출됩니다.
요청 형식의 자동 감지는 아래와 같은 순서로 이루어집니다.

- `$type` 인자가 비어 있지 않은 경우 해당 값으로 설정
- GET이나 POST에서 `xe_js_callback` 파라미터가 있는 경우 `JS_CALLBACK`으로 설정
- 그 밖의 GET 요청은 `GET`으로 설정
- 라이믹스의 `exec_xml()` 또는 `exec_json()` 함수로 AJAX 요청을 한 경우
  - `X-AJAX-Compat` 헤더 또는 `_rx_ajax_compat` 파라미터의 값에 따라 `JSON` 또는 `XMLRPC`로 설정
- `Accept` 헤더에 `application/json`이 포함되어 있는 경우 `JSON`으로 설정
- 요청의 body가 JSON 형식인 경우 (`Content-Type` 헤더가 `application/json`인 경우) `JSON`으로 설정
- 요청의 body가 XML 형식인 경우 (실제 body를 확인함) `XMLRPC`로 설정
- 그 밖의 POST 요청은 `POST`로 설정

하위 호환을 위해 복잡한 로직을 사용하고 있지만,
최신 라이믹스 기준으로는 `GET`, `POST`, `JSON`밖에 사용하지 않으므로
신규 자료에서는 그 밖의 형식을 고려할 필요가 없습니다.
라이믹스 프론트엔드에서 생성하는 모든 AJAX 요청은 `JSON` 형식을 취하기 때문입니다.
(`exec_xml()` 함수도 실제 데이터는 JSON이나 일반 POST로 주고받는 것이 기본값입니다.)

#### setRequestArguments()

```
public static function setRequestArguments(array $router_args = []): void
```

요청과 함께 들어온 변수들을 정리하여 `Context`에 저장합니다.
초기화 과정에서 자동으로 처리되므로, 직접 호출할 일은 거의 없습니다.

`XMLRPC` 요청시 XXE 공격을 감지하고, 코어에서 사용하는 변수를 덮어쓰려는 시도를 차단하며,
일부 변수를 정수로 변환하거나 `escape()` 처리하는 등,
일차적인 보안 필터링이 이 메소드에서 수행됩니다.

저장된 변수들은 `getRequestVars()` 메소드를 통해 가져올 수 있습니다.

#### setUploadInfo()

```
public static function setUploadInfo(): void
```

요청과 함께 업로딩된 파일 정보를 정리하여 `Context`에 저장합니다.
초기화 과정에서 자동으로 처리되므로, 직접 호출할 일은 거의 없습니다.

배열로 업로딩된 파일은 이 메소드를 통해 처리하기 쉬운 형태로 정리됩니다.
파일명에 대한 보안 필터링을 수행하고, 안전하지 않은 내용을 감지하는 등
일차적인 보안 필터링이 이 메소드에서 수행됩니다.

저장된 파일 정보는 `Context::get('폼 필드명')`을 통해
일반적인 요청 변수와 동일하게 접근할 수 있으며, 내용은 `$_FILES`의 자료 구조를 따릅니다.

#### isUploaded()

```
public static function isUploaded(): bool
```

업로딩된 파일이 있는 경우 `true`를 반환합니다.



### 응답 속성 관련 메소드

#### setResponseMethod()

```
public static function setResponseMethod(
    string $method = 'HTML',
    ?string $content_type = null
):void
```

응답 형식을 지정합니다.
응답 형식은 대개 요청 형식에 따라 기본값이 자동 입력되지만,
요청 형식과 무관하게 응답 형식을 고정하고 싶은 경우 이 메소드를 사용할 수 있습니다.

- `$method`는 다음 중 하나를 지정할 수 있습니다:
  - `'HTML'`: HTML 웹 페이지
  - `'JSON'`: JSON 응답
  - `'XMLRPC'`: XE 1.x 방식의 XML 응답
  - `'RAW'`: 그 밖의 형식 (plan text, 이미지 출력, 파일 다운로딩 등)
- `$content_type`은 `'RAW'` 응답 형식의 `Content-Type` 헤더를 지정하는 데 사용합니다.
  그 밖의 응답 형식에서는 `text/html`, `application/json`, `text/xml` 등의 기본값이 자동으로 설정되므로
  이 인자를 지정할 필요가 없습니다.

#### getResponseMethod()

```
public static function getResponseMethod(): string
```

현재 지정되어 있거나 자동 감지된 응답 형식을 반환합니다.

#### getSiteTitle()

```
public static function getSiteTitle(): string
```

현재 접속한 도메인의 사이트 제목을 반환합니다.

#### getSiteSubtitle()

```
public static function getSiteSubtitle(): string
```

현재 접속한 도메인의 사이트 부제목을 반환합니다.

#### addBrowserTitle()

```
public static function addBrowserTitle(
    string $title,
    string $delimiter = ' - '
): void
```

HTML 응답의 브라우저 제목(`<title>` 태그의 내용) 뒤에 문자열을 추가합니다.
이미 설정된 제목에 문자열을 덧붙이는 방식으로 동작합니다.
`$delimiter` 인자를 사용하여 기존 제목과 덧붙인 문자열 사이에 구분자를 삽입할 수 있습니다.

#### prependBrowserTitle()

```
public static function prependBrowserTitle(
    string $title,
    string $delimiter = ' - '
): void
```

HTML 응답의 브라우저 제목(`<title>` 태그의 내용) 앞에 문자열을 추가합니다.
이미 설정된 제목에 문자열을 덧붙이는 방식으로 동작합니다.
`$delimiter` 인자를 사용하여 기존 제목과 덧붙인 문자열 사이에 구분자를 삽입할 수 있습니다.

#### setBrowserTitle()

```
public static function setBrowserTitle(
    string $title,
    array $vars = []
): void
```

HTML 응답의 브라우저 제목(`<title>` 태그의 내용)을 대체합니다.
이미 설정된 제목이 있다면 덮어씁니다.

`$vars` 인자를 사용하여 제목에 변수를 삽입할 수 있습니다.
예를 들어 제목에 `$TITLE`이라는 변수 삽입 위치가 정의되어 있는 경우,
`$vars`에 `['TITLE' => 'My Title']`과 같이 전달하면
`My Title`이 삽입된 제목을 생성합니다.

라이믹스에서는 이 기능을 사용하여 사이트 전체에 일관성있는 제목 형태를 유지하고,
"SEO 설정" 화면에서 변수 삽입 위치를 관리할 수 있도록 하고 있습니다.

#### getBrowserTitle()

```
public static function getBrowserTitle(): string
```

현재 설정된 브라우저 제목을 반환합니다.

#### addHtmlHeader()

```
public static function addHtmlHeader(
    string $header,
    bool $prepend = false
): void
```

HTML 응답의 헤더 부분(`</head>` 직전)에 임의의 HTML 코드를 추가합니다.

`$prepend` 인자를 `true`로 설정하면, 기존에 추가된 HTML 코드 앞에 삽입됩니다.

#### getHtmlHeader()

```
public static function getHtmlHeader(): string
```

현재 추가되어 있는 HTML 헤더 코드를 반환합니다.

#### clearHtmlHeader()

```
public static function clearHtmlHeader(): void
```

현재 추가되어 있는 HTML 헤더 코드를 삭제합니다.

#### addBodyClass()

```
public static function addBodyClass(string $class_name): void
```

HTML 응답의 `<body>` 태그에 `class`를 추가합니다.

필요한 `class`를 서버단에서 미리 추가해 둘 수 있다면,
프론트엔드에서 추가하는 것보다 응답 속도를 높이거나 깜빡임을 줄이는 효과가 있습니다.

#### removeBodyClass()

```
public static function removeBodyClass(string $class_name): void
```

HTML 응답의 `<body>` 태그에서 `class`를 제거합니다.

#### getBodyClassList()

```
public static function getBodyClassList(): array
```

HTML 응답의 `<body>` 태그에 추가된 `class`의 목록을 배열로 반환합니다.

#### getBodyClass()

```
public static function getBodyClass(): string
```

HTML 응답의 `<body>` 태그에 추가된 `class`를 모두 연결하여 문자열로 반환합니다.

#### addBodyHeader()

```
public static function addBodyHeader(
    string $header,
    bool $prepend = false
): void
```

HTML 응답의 본문 상단(`<body>` 직후)에 임의의 HTML 코드를 추가합니다.

`$prepend` 인자를 `true`로 설정하면, 기존에 추가된 HTML 코드 앞에 삽입됩니다.

#### getBodyHeader()

```
public static function getBodyHeader(): string
```

현재 추가되어 있는 HTML 본문 상단 코드를 반환합니다.

#### addHtmlFooter()

```
public static function addHtmlFooter(
    string $footer,
    bool $prepend = false
): void
```

HTML 응답의 본문 하단(`</body>` 직전)에 임의의 HTML 코드를 추가합니다.

`$prepend` 인자를 `true`로 설정하면, 기존에 추가된 HTML 코드 앞에 삽입됩니다.

#### getHtmlFooter()

```
public static function getHtmlFooter(): string
```

현재 추가되어 있는 HTML 본문 하단 코드를 반환합니다.

#### addLink()

```
public static function addLink(
    string $url,
    string $rel
): void
```

HTML 응답의 `<head>` 부분에 `<link>` 태그를 추가합니다.

예: `<link rel="preconnect" href="https://webfonts.example.com" />`

#### getLinks()

```
public static function getLinks(): array
```

현재 추가되어 있는 `<link>` 태그의 목록을 배열로 반환합니다.

#### getMetaTag()

```
public static function getMetaTag(?string $name = null): array|string|null
```

현재 HTML 응답에 추가된 `<meta>` 태그를 가져옵니다.

- `$name` 인자를 지정하면, 해당 이름의 메타 태그를 문자열로 반환합니다.
- `$name` 인자를 지정하지 않으면, 현재 추가된 모든 메타 태그를 배열로 반환합니다.

#### addMetaTag()

```
public static function addMetaTag(
    string $name,
    string $content,
    bool $is_http_equiv = false,
    bool $is_before_title = true
): void
```

현재 HTML 응답에 `<meta>` 태그를 추가합니다.

- `$name`은 메타 태그의 이름입니다.
- `$content`는 메타 태그의 내용입니다.
- `$is_http_equiv` 인자를 `true`로 설정하면, `$name`을 `name` 속성이 아닌 `http-equiv` 속성에 넣습니다.
- `$is_before_title` 인자를 `true`로 설정하면, `<title>` 태그보다 위에 표시됩니다. 그렇지 않으면 `<title>` 태그보다 아래에 표시됩니다.

#### getMetaImages()

```
public static function getMetaImages(): array
```

현재 HTML 응답에 추가된 OpenGraph 이미지 목록을 가져옵니다.

#### addMetaImage()

```
public static function addMetaImage(
    string $filename,
    int $width = 0,
    int $height = 0
): void
```

현재 HTML 응답에 OpenGraph 이미지 메타 태그를 추가합니다.

OpenGraph 이미지 메타 태그는 아래와 같은 형태를 띠는 3개의 태그입니다.

```
<meta property="og:image" content="https://example.com/files/attach/image.png" />
<meta property="og:image:width" content="1920" />
<meta property="og:image:height" content="1080" />
```

위와 같은 태그를 추가하려면 아래의 코드를 사용합니다.

```
Context::addMetaImage('./files/attach/image.png', 1920, 1080);
```

이미지 경로는 라이믹스 설치 경로를 기준으로 하는 상대경로임에 유의하십시오.
`$width`나 `$height`를 지정하지 않은 경우, 이미지 파일을 읽어와서 자동으로 계산합니다.
따라서 이미지 파일은 라이믹스가 접근할 수 있는 경로에 있어야 하고, 외부 URL은 사용할 수 없습니다.

#### getOpenGraphData()

```
public static function getOpenGraphData(): array
```

현재 HTML 응답에 추가된 (이미지를 제외한) OpenGraph 메타데이터를 가져옵니다.

#### addOpenGraphData()

```
public static function addOpenGraphData(
    string $name,
    array $content
): void
```

현재 HTML 응답에 (이미지를 제외한) OpenGraph 메타데이터를 추가합니다.

`$name`과 `$content`를 구분하여 전달하는 방법은 2가지가 있습니다.

```
Context::addOpenGraphData('og:title', 'My Page Title');
Context::addOpenGraphData('og:description', 'This is a description of my page.');
```

```
Context::addOpenGraphData('og', [
    'title' => 'My Page Title',
    'description' => 'This is a description of my page.'
]);
```

OpenGraph 메타데이터는 `property` 속성을 사용하는 것이 특징입니다.
트위터 SEO 정보처럼 `property` 속성이 아닌 `name` 속성을 사용해야 하는 경우에는
일반적인 `addMetaTag()` 메소드를 사용하십시오.

#### setCanonicalURL()

```
public static function setCanonicalURL(string $url): void
```

현재 HTML 응답에 `<link rel="canonical">` 태그를 추가합니다.

#### getCanonicalURL()

```
public static function getCanonicalURL(): string
```

현재 HTML 응답에 추가된 `<link rel="canonical">` 태그의 URL을 반환합니다.

#### setCacheControl()

```
public static function setCacheControl(
    int $ttl = 0,
    bool $public = true
): void
```

브라우저 캐시를 컨트롤하기 위한 `Cache-Control` 헤더를 설정합니다.

- `$ttl`은 캐시의 유효 기간을 초 단위로 지정하며, 0인 경우 `no-cache`로 설정됩니다.
- `$public`이 `true`인 경우, `public` 키워드가 추가되어 공용 캐시에 응답이 저장될 수 있습니다.
  그렇지 않은 경우, `private` 키워드가 추가됩니다.

별도로 설정하지 않으면 라이믹스에서 출력하는 모든 페이지의 `$ttl`은 0으로 설정됩니다.

#### setCorsPolicy()

```
public static function setCorsPolicy(
    string $origin = '*',
    array $methods = [],
    array $headers = [],
    int $max_age = 0
): void
```

이 요청에 대한 CORS(Cross-Origin Resource Sharing) 정책을 설정합니다.

- `$origin`은 허용할 출처를 지정하며, `'*'`을 사용하면 모든 출처를 허용합니다.
- `$methods`는 허용할 HTTP 메소드를 배열로 지정합니다.
- `$headers`는 허용할 헤더를 배열로 지정합니다.
- `$max_age`는 preflight 요청의 캐시 유효 기간을 초 단위로 지정합니다.

#### redirect()

```
public static function redirect(
    string $url,
    int $status_code = 302,
    int $ttl = 0
): void
```

특정 URL로 리다이렉트하는 `Location` 헤더를 출력합니다.

- `$url`은 리다이렉트할 URL을 지정합니다.
- `$status_code`는 HTTP 상태 코드를 지정하며, 기본값은 302입니다. 필요시 301, 307, 308 등으로 변경하십시오.
- `$ttl`은 브라우저 캐시의 유효 기간을 초 단위로 지정합니다.
  0이 아닌 값을 입력할 경우, 리다이렉트가 캐싱되어 예상치 못한 동작을 할 수 있으므로 주의해야 합니다.



### URL 처리 관련 메소드

#### getUrl()

```
public static function getUrl(
    int $num_args = 0,
    array $args_list = [],
    ?string $domain = null,
    bool $encode = true,
    bool $autoEncode = false
): string
```

이 메소드는 `getUrl()` 및 그와 비슷한 역할을 하는 전역 함수들이 공통적으로 사용하는 함수입니다.
일반적으로 이 메소드를 직접 호출할 필요는 없습니다.

#### getDefaultUrl()

```
public static function getDefaultUrl(
    ?object $site_module_info = null,
    ?bool $use_ssl = null
): string
```

현재 접속한 도메인의 기본 URL을 반환합니다.

XE 1.x에서는 기본 URL이 매우 중요한 개념이었으나, 라이믹스에서는 그렇지 않습니다.
홈 페이지로 링크되는 상대경로가 필요하다면 `RX_BASEURL` 상수로 갈음할 수 있고,
도메인을 포함한 절대경로가 필요하다면 `getFullUrl('')`이나
`Rhymix\Framework\URL::getCurrentDomainURL(\RX_BASEURL)`을 사용할 수도 있습니다.

즉, 기본 URL은 `module`이나 `act`와 같은 변수가 하나도 붙지 않은 상태로
`\RX_BASEURL`만 남겨진 최소한의 URL일 뿐, 특별한 개념이 아닙니다.

#### getRequestUri()

```
public static function getRequestUri(
    int $ssl_mode = \FOLLOW_REQUEST_SSL,
    ?string $domain = null
): string
```

기본 URL을 반환한다는 점에서 `getDefaultUrl()`과 유사하지만,
다른 도메인의 기본 URL을 반환하거나, SSL을 적용 또는 제거한 URL을 반환할 수 있다는 점에서
기본 URL을 가공하여 반환하는 메소드라고 할 수 있습니다.

원래의 목적은 AJAX 요청이나 폼을 제출할 수 있는 URL을 생성하는 것이지만,
현재 이러한 목적으로 URL을 별도로 생성할 필요는 없으므로 (프론트엔드에서 자동으로 처리하므로)
기본 URL이라는 개념과 함께 필요가 없어지고 있는 메소드입니다.

SSL 모드를 변경할 수 있는 기능도 일부 `act`에만 SSL을 적용할 수 있던 XE 1.x와 달리,
라이믹스에서는 항상 사용하거나 전혀 사용하지 않는 두 가지 옵션만 제공하기 때문에
큰 의미가 없다고 할 수 있겠습니다. 이 기능과 관련된 `FOLLOW_REQUEST_SSL` 등의 상수들 역시
하위 호환 목적 외에는 사용하지 않습니다.



### 언어 및 다국어 관련 메소드

#### loadLangSupported()

```
public static function loadLangSupported(): array
```

라이믹스에서 지원하는 언어 목록을 반환합니다.

#### loadLangSelected()

```
public static function loadLangSelected(): array
```

관리자가 활성화시킨 언어 목록을 반환합니다.

#### loadLang()

```
public static function loadLang(
    string $path,
    ?string $plugin_name = null
): void
```

주어진 경로에서 언어 파일을 로딩합니다.

`$plugin_name`에는 모듈이나 애드온 등, 로딩할 언어 파일을 제공하는 자료의 이름을 넣습니다.
필수는 아니지만, 다국어 코드 분류에 도움이 되며,
`모듈명.언어코드` 형식으로 특정 모듈의 다국어 코드를 불러오는 기능을 사용할 때 필요합니다.

모듈 등 일반적인 확장 기능을 로딩할 때는 이 메소드가 자동으로 호출되므로,
별도의 과정 없이 해당 모듈이 제공하는 다국어 코드를 자유롭게 사용할 수 있습니다.

#### setLangType()

```
public static function setLangType(string $lang_type = 'ko'): void
```

현재 사용자에게 적용할 언어 코드를 지정합니다.

#### getLangType()

```
public static function getLangType(): string
```

현재 사용자에게 적용된 언어 코드를 반환합니다.

#### getLang()

```
public static function getLang(string $code): mixed
```

주어진 다국어 코드에 해당하는 언어 문자열을 반환합니다.
`lang()` 전역 함수와 같은 역할이지만, 다국어 코드를 추가하는 기능은 `setLang()`으로 분리되어 있습니다.

#### setLang()

```
public static function setLang(
    string $code,
    string $val
): void
```

주어진 다국어 코드에 해당하는 언어 문자열을 설정합니다.
`lang()` 전역 함수와 같은 역할이지만, 다국어 코드를 추가하는 부분만 담당합니다.

#### replaceUserLang()

```
public static function replaceUserLang(
    string $string,
    bool $fix_double_escape = false
): string
```

주어진 문자열에 포함된 사용자 정의 다국어 코드(`$user_lang->userLangXXXXX`)를 변환합니다.

XE 1.x 방식의 사용자 정의 다국어 코드에는 `>` 문자가 포함되어 있어서,
문맥에 따라서는 이것이 `&gt;`로 변환되어 정상 인식하지 못하는 경우가 있습니다.
이 때는 `$fix_double_escape` 인자를 `true`로 설정하여
`&gt;`를 `>`로 되돌린 후, 사용자 정의 다국어 코드를 변환하도록 할 수 있습니다.



### 프론트엔드 애셋 관련 메소드

#### loadFile()

```
public static function loadFile(array $args): void
```

```
public static function loadFile(...$args): void
```

HTML 응답에서 사용할 CSS (LESS, SCSS 포함) 및 JavaScript 애셋을 로딩합니다.
인자는 하나의 배열로 전달하거나, 각각의 인자로 전달할 수 있습니다.
모든 인자는 `FrontEndFileHandler::loadFile()` 메소드로 전달됩니다.

자세한 내용은 [FrontEndFileHandler](./FrontEndFileHandler.md) 문서를 참조하십시오.

#### unloadFile()

```
public static function unloadFile(
    string $file,
    string $unused = '',
    string $media = 'all'
): void
```

CSS (LESS, SCSS 포함) 및 JavaScript 애셋을 제거합니다.
`$unused` 인자는 사용하ㅣ 않으며, `$media` 인자는 CSS에서만 사용합니다.
모든 인자는 `FrontEndFileHandler::unloadFile()` 메소드로 전달됩니다.

자세한 내용은 [FrontEndFileHandler](./FrontEndFileHandler.md) 문서를 참조하십시오.

#### unloadAllFiles()

```
public static function unloadAllFiles(string $type = 'all'): void
```

자세한 내용은 [FrontEndFileHandler](./FrontEndFileHandler.md) 문서를 참조하십시오.

#### addJsFile()

```
public static function addJsFile(
    string $file,
    string $unused1 = '',
    string $unused2 = '',
    int $index = 0,
    string $type = 'head',
    bool $isRuleset = false,
    ?string $autoPath = null
): void
```

오래 전 XE 1.x에서 사용하던 메소드입니다. `loadFile()` 메소드를 사용하십시오.

#### unloadJsFile()

```
public static function unloadJsFile($file)
```

`unloadFile()` 메소드를 사용하십시오.

#### unloadAllJsFiles()

```
public static function unloadAllJsFiles(): void
```

`unloadAllFiles()` 메소드를 사용하십시오.

#### addJsFilter()

```
public static function addJsFilter(
    string $path,
    string $filename
): void
```

자바스크립트에서 사용할 XML 필터를 추가합니다.

#### getJsFile()

```
public static function getJsFile(
    string $type = 'head',
    bool $finalize = false
): array
```

현재 로딩된 자바스크립트 파일 목록을 반환합니다.

- `$type` 인자를 사용하여 `head` 또는 `body`에 로딩된 파일만 가져올 수 있습니다.
- `$finalize` 인자를 `true`로 설정하면, 압축과 합치기 등의 설정을 모두 적용한 결과를 반환합니다.

#### addCSSFile()

```
public static function addCSSFile(
    string $file,
    string $unused1 = '',
    string $media = 'all',
    string $unused2 = '',
    int $index = 0
): void
```

오래 전 XE 1.x에서 사용하던 메소드입니다. `loadFile()` 메소드를 사용하십시오.

#### unloadCSSFile()

```
public static function unloadCSSFile(
    string $file,
    string $unused = '',
    string $media = 'all'
): void
```

`unloadFile()` 메소드를 사용하십시오.

#### unloadAllCSSFiles()

```
public static function unloadAllCSSFiles(): void
```

`unloadAllFiles()` 메소드를 사용하십시오.

#### getCSSFile()

```
public static function getCSSFile(bool $finalize = false): array
```

현재 로딩된 CSS 파일 목록을 반환합니다.

`$finalize` 인자를 `true`로 설정하면, 압축과 합치기 등의 설정을 모두 적용한 결과를 반환합니다.

#### getJavascriptPluginInfo()

```
public static function getJavascriptPluginInfo(string $plugin_name): object
```

자바스크립트 플러그인에 대한 정보를 반환합니다.
자바스크립트 플러그인은 `common/js/plugins/` 디렉토리에 있는 라이브러리들로,
CKEditor, jQuery UI 등이 있습니다.

#### loadJavascriptPlugin()

```
public static function loadJavascriptPlugin(string $plugin_name): void
```

자바스크립트 플러그인을 로딩합니다.

하나의 플러그인을 구성하는 모든 필수 파일을 `loadFile()` 메소드를 통해 로딩하고,
플러그인에 다국어 파일이 포함되어 있다면 그것도 로딩합니다.



### 세션 처리 관련 메소드

아래의 메소드들은 "세션 시작 지연" 기능을 사용하는 경우에만 의미가 있습니다.
그 밖의 경우에는 `init()` 메소드에서 세션을 자동으로 시작하기 때문입니다.

#### getSessionStatus()

```
public static function getSessionStatus(): bool
```

세션이 시작되었는지 확인합니다.

`Rhymix\Framework\Session::isStarted()`의 alias입니다.

#### checkSessionStatus()

```
public static function checkSessionStatus(bool $force = false): void
```

세션이 아직 시작되지 않았다면 시작할 필요가 있는지 확인하고, 필요시 강제로 시작합니다.

`Rhymix\Framework\Session::checkStart($force)`의 alias입니다.




### 기타 유틸리티 메소드

#### displayErrorPage()

```
public static function displayErrorPage(
    string $title = 'Error',
    string $message = '',
    int $status = 500,
    string $location = ''
): void
```

간단한 에러 페이지를 출력합니다.

#### isInstalled()

```
public static function isInstalled(): bool
```

라이믹스가 설치되어 있는지 확인합니다.
아직 설치가 완료되지 않은 상태를 구분하는 데 사용할 수 있습니다.

#### isLocked()

```
public static function isLocked(): bool
```

사이트 잠금 기능을 사용중인지 확인합니다.

#### isAllowRewrite()

```
public static function isAllowRewrite(): int
```

`Rhymix\Framework\Router::getRewriteLevel()`의 alias입니다.

#### isDefaultPlugin()

```
public static function isDefaultPlugin(
    string $plugin_name,
    string $type
): bool
```

주어진 모듈, 애드온, 레이아웃, 위젯 등이 라이믹스에 기본 포함된 자료인지 확인합니다.

- `$plugin_name`은 모듈, 애드온, 레이아웃, 위젯 등의 이름입니다.
- `$type`은 자료의 종류로, `'module'`, `'addon'`, `'layout'`, `'widget'` 등을 지정합니다.

#### isBlacklistedPlugin()

```
public static function isBlacklistedPlugin(
    string $plugin_name,
    string $type = ''
): bool
```

라이믹스에서 호환성 문제로 사용이 제한된 자료인지 확인합니다.

- `$plugin_name`은 모듈, 애드온, 레이아웃, 위젯 등의 이름입니다.
- `$type`은 자료의 종류로, `'module'`, `'addon'`, `'layout'`, `'widget'` 등을 지정합니다.

#### isReservedWord()

```
public static function isReservedWord(string $word): bool
```

라이믹스 내부에서 예약어로 사용되는 단어인지 확인합니다.
이러한 단어는 모듈, 애드온, 레이아웃, 위젯 등의 이름이나 사용자 아이디로 사용할 수 없습니다.

#### setValidatorMessage()

```
public static function setValidatorMessage(
    string $id,
    string $message,
    string $type = 'info'
): void
```

내부적으로 사용하는 함수입니다. 에러 메시지를 세션에 저장하여, 다음 화면에 표시하도록 합니다.



### Deprecated

#### loadDBInfo()

```
public static function loadDBInfo(?array $config = null): void
```

시스템 설정 중 DB 접속과 관련된 부분을 XE 1.x와 호환되는 형식으로 재구성하여 `db_info` 속성에 저장합니다.
이 속성을 참조하는 기존 자료가 많기 때문에 제공하는 메소드로, 추후 삭제될 수 있습니다.

#### setDBInfo()

```
public static function setDBInfo(object $db_info): void
```

`db_info` 속성에 DB 접속 정보를 저장합니다.

#### getDBInfo()

```
public static function getDBInfo(): object
```

`db_info` 속성의 값을 반환합니다.

#### getDBType()

```
public static function getDBType()
```

현재 사용 중인 DB 타입을 반환합니다.
라이믹스를 신규 설치한 경우 항상 `'mysql'`을 반환하며,
XE 1.x에서 업그레이드한 경우 `'mysqli'`, `'mysql_innodb'` 등 유사한 문자열을 반환할 수도 있습니다.

#### getRouteInfo()

```
public static function getRouteInfo(): Rhymix\Framework\Request
```

현재 요청 정보를 반환합니다. `getCurrentRequest()`의 alias입니다.

#### getSSLStatus()

```
public static function getSSLStatus(): string
```

현재 도메인의 SSL 설정을 반환합니다. 현재 요청의 속성을 반환하는 것이 아니니 주의하십시오.
SSL을 사용하는 경우 `'always'`, 그렇지 않은 경우 `'none'`을 반환합니다.

#### getJSCallbackFunc()

```
public static function getJSCallbackFunc()
```

`xe_js_callback` 파라미터로 받은 JSONP 콜백 함수명을 반환합니다.
라이믹스에서는 사실상 사용할 일이 없습니다.

#### getConfigFile()

```
public static function getConfigFile(): string
```

라이믹스에서는 사용하지 않습니다. 존재하지 않거나 빈 파일 경로를 반환할 가능성이 높습니다.

#### getFTPConfigFile()

```
public static function getFTPConfigFile(): string
```

라이믹스에서는 사용하지 않습니다. 존재하지 않거나 빈 파일 경로를 반환할 가능성이 높습니다.

#### isFTPRegisted()

```
public static function isFTPRegisted(): bool
```

FTP 정보가 등록되어 있는지 확인합니다.
라이믹스에서는 사용하지 않습니다.

#### getFTPInfo()

```
public static function getFTPInfo(): object
```

등록된 FTP 정보를 반환합니다.
라이믹스에서는 사용하지 않습니다.

#### addSSLAction()

```
public static function addSSLAction(string $action): void
```

이 메소드는 사용하지 않습니다.

#### addSSLActions()

```
public static function addSSLActions(array $action_array): void
```

이 메소드는 사용하지 않습니다.

#### subtractSSLAction()

```
public static function subtractSSLAction(string $action): void
```

이 메소드는 사용하지 않습니다.

#### getSSLActions()

```
public static function getSSLActions(): array
```

이 메소드는 사용하지 않으며, 호출시 항상 빈 배열을 반환합니다.

#### isExistsSSLAction()

```
public static function isExistsSSLAction(string $action): bool
```

이 메소드는 사용하지 않으며, 항상 `false`를 반환합니다.

#### checkSSO()

```
public static function checkSSO()
```

구 버전에서 SSO 처리를 담당하던 메소드로,
현재 이 기능은 `Rhymix\Framework\Session` 클래스로 이전되었습니다.

이 메소드는 no-op으로, 항상 `true`를 반환합니다.

#### normalizeFilePath()

```
public static function normalizeFilePath(string $file): string
```

파일 경로에서 불필요하거나 위험한 특수 문자를 제거하는 메소드였으나,
현재 라이믹스에서는 사용하지 않습니다.

#### getAbsFileUrl()

```
public static function getAbsFileUrl(string $file): string
```

파일 경로를 변환하는 데 사용하는 메소드였으나,
현재 라이믹스에서는 사용하지 않습니다.

#### pathToUrl()

```
public static function pathToUrl(string $path): string
```

라이믹스에서 변경된 경로 체계에 대응하지 못하는 XE 1.x 방식의 함수이므로, 사용을 권장하지 않습니다.
`Rhymix\Framework\URL::fromServerPath()`를 사용하십시오.

#### convertEncoding()

```
public static function convertEncoding(object $source_obj): object
```

이 메소드는 사용하지 않습니다.

문자열의 인코딩 변환이 필요하다면 `mb_convert_encoding()` 함수를 사용하십시오.

#### checkConvertFlag()

```
public static function checkConvertFlag(
    mixed &$val,
    ?string $key = null,
    ?string $charset = null
): ?bool
```

이 메소드는 사용하지 않습니다.

문자열의 인코딩 변환이 필요하다면 `mb_convert_encoding()` 함수를 사용하십시오.

#### doConvertEncoding()

```
public static function doConvertEncoding(
    mixed &$val,
    ?string $key = null,
    string $charset = 'CP949'
): void
```

이 메소드는 사용하지 않습니다.

문자열의 인코딩 변환이 필요하다면 `mb_convert_encoding()` 함수를 사용하십시오.

#### convertEncodingStr()

```
public static function convertEncodingStr(string $str): string
```

이 메소드는 사용하지 않습니다.

문자열의 인코딩 변환이 필요하다면 `mb_convert_encoding()` 함수를 사용하십시오.

#### transContent()

```
public static function transContent(string $content): string
```

라이믹스에서는 사용하지 않으며, 하위 호환을 위해 존재합니다.
전달한 문자열에 어떤 가공도 하지 않고 그대로 반환합니다.

#### encodeIdna()

```
public static function encodeIdna(string $domain): string
```

한글 도메인을 IDNA(퍼니코드) 형식으로 변환합니다.

이 메소드는 `Rhymix\Framework\URL::encodeIdna()`의 alias입니다.

#### decodeIdna()

```
public static function decodeIdna(string $domain): string
```

IDNA(퍼니코드) 도메인을 원래의 형식으로 변환합니다.

이 메소드는 `Rhymix\Framework\URL::decodeIdna()`의 alias입니다.
