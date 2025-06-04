전역 함수
-----------

### 개요

라이믹스는 PHP의 전역 함수처럼 어디에서나 사용할 수 있는 여러 유틸리티 함수를 제공합니다.
이 함수들은 편리한 코딩을 돕고, PHP 버전 차이에 따른 호환성 문제를 완화하는 등
코어와 서드파티 모듈에서 광범위한 역할을 수행합니다.

전역 함수들은 `common/functions.php`와 `common/legacy.php` 파일에 선언되어 있습니다.
역사적인 이유로 파일이 두 개로 나뉘어 있고, 새로운 함수는 `common/functions.php`에만 추가되지만,
`common/legacy.php`에 있는 함수라고 해서 반드시 사용을 피해야 하는 것은 아닙니다.
단, 아래에서 `deprecated`로 표시된 함수들은 대안을 찾는 것을 권장합니다.

#### config()

```
function config(
	string $key,
	mixed $value = null
): mixed
```

시스템 설정을 불러오거나 저장합니다.
`$value`가 `null`이 아닌 경우 설정을 저장하고, 그렇지 않으면 설정 값을 반환합니다.

`$key`는 점(`.`)으로 구분된 문자열로 작성할 수 있습니다.
예를 들어, `config('locale.default_lang')`은 기본 언어를 반환합니다.
`config('locale.default_lang', 'en')`은 기본 언어를 영어로 변경합니다.

변경한 설정은 영구적으로 저장되지 않으므로 해당 요청이 끝나면 원상복구됩니다.
설정을 영구적으로 저장하려면 `Rhymix\Framework\Config::save()` 메소드를 사용해야 합니다.

#### lang()

```
function lang(
	string $code,
	mixed $value = null
): string|null|ArrayObject
```

다국어 코드를 불러오거나 저장합니다.
`$value`가 `null`이 아닌 경우 다국어 코드를 저장하고, 그렇지 않으면 다국어 코드를 반환합니다.

`$code`는 점(`.`)으로 구분된 문자열로 작성할 수 있습니다.
이 때 첫 번째 부분에는 모듈, 애드온, 또는 플러그인 이름을 넣되,
`common/lang` 디렉토리에 있는 다국어 코드는 모듈명을 `common`으로 지정합니다.

예를 들어, `lang('board.board_management')`은
`board` 모듈의 다국어 코드 중 `board_management`를 읽어, 한국어 기준 "게시판 관리"를 반환합니다.

해당 코드를 "게시판 관리자"로 변경하려면
`lang('board.board_management', '게시판 관리자')`라고 하면 됩니다.
변경한 다국어 코드는 영구적으로 저장되지 않으므로 해당 요청이 끝나면 원상복구됩니다.

요청한 다국어 코드가 배열이나 객체인 경우 `ArrayObject`를 반환합니다.
배열이나 객체의 특정 키를 읽으려면 점(`.`)으로 구분하여 한 단계를 추가합니다.
예를 들어 `lang('common.filter.isnull')`은
`common/lang/ko.php`에서 `$lang->filter['isnull']`의 값을 읽습니다.

요청한 다국어 코드가 존재하지 않는 경우 `$code`를 그대로 반환합니다.
코드가 그대로 노출되므로, 누락된 다국어 코드를 쉽게 확인할 수 있습니다.

#### array_first()

```
function array_first(
	array $array
): mixed
```

주어진 배열의 첫 번째 요소를 반환합니다.
배열이 비어 있는 경우 `false`를 반환하는데,
첫 번째 요소의 값이 `false`인 경우와 혼동하지 않도록 주의해야 합니다.

배열의 내부 포인터가 첫 번째 요소로 이동하는 부작용이 있으니,
`foreach`와 같은 반복문 안에서는 사용하지 않는 것이 좋습니다.

#### array_first_key()

```
function array_first_key(
	array $array
): string|int|null
```

주어진 배열의 첫 번째 키를 반환합니다.
배열이 비어 있는 경우, `null`을 반환합니다.

배열의 내부 포인터가 첫 번째 요소로 이동하는 부작용이 있으니,
`foreach`와 같은 반복문 안에서는 사용하지 않는 것이 좋습니다.

#### array_last()

```
function array_last(
	array $array
): mixed
```

주어진 배열의 마지막 요소를 반환합니다.
배열이 비어 있는 경우 `false`를 반환하는데,
마지막 요소의 값이 `false`인 경우와 혼동하지 않도록 주의해야 합니다.

배열의 내부 포인터가 첫 번째 요소로 이동하는 부작용이 있으니,
`foreach`와 같은 반복문 안에서는 사용하지 않는 것이 좋습니다.

#### array_last_key()

```
function array_last_key(
	array $array
): string|int|null
```

주어진 배열의 마지막 키를 반환합니다.
배열이 비어 있는 경우, `null`을 반환합니다.

배열의 내부 포인터가 첫 번째 요소로 이동하는 부작용이 있으니,
`foreach`와 같은 반복문 안에서는 사용하지 않는 것이 좋습니다.

#### array_escape()

```
function array_escape(
	array $array,
	bool $double_escape = true
): array
```

배열의 모든 키와 값을 이스케이프합니다.
`$double_escape`가 `true`인 경우, 이미 이스케이프된 문자열이라도 다시 이스케이프합니다.

다차원 배열인 경우, 하위 배열의 모든 키와 값도 재귀적으로 이스케이프합니다.

#### array_flatten()

```
function array_flatten(
	array $array,
	bool $preserve_keys = true
): array
```

다차원 배열을 일차원 배열로 단순화합니다.

`$preserve_keys`가 `true`인 경우, 원래 배열의 키를 유지하되, 키가 중복되는 경우에는 마지막 값을 유지합니다.
`$preserve_keys`가 `false`인 경우, 키는 0부터 시작하는 연속된 숫자로 재설정됩니다.

#### class_basename()

```
function class_basename(
	string|object $class
): string
```

클래스명 또는 오브젝트의 클래스명에서 네임스페이스를 제외한 순수 클래스 이름을 반환합니다.
예를 들어, `Rhymix\Framework\Helpers\DBHelper` 클래스라면 `DBHelper`를 반환합니다.

#### clean_path()

```
function clean_path(
	string $path
): string
```

이 함수는 `Rhymix\Framework\Filters\FilenameFilter::cleanPath()`의 alias입니다.
주어진 경로에서 `/./` 등 불필요한 부분을 제거하고, 경로 구분자를 표준화합니다.

#### escape()

```
function escape(
	string $str,
	bool $double_escape = true,
	bool $except_lang_code = false
): string
```

문자열에 포함된 특수문자 `<`, `>`, `&`, `"`, `'`를 HTML 엔티티로 변환(이스케이프)하여 XSS 공격을 방지합니다.

`$double_escape`가 `true`인 경우, 이미 이스케이프된 문자열이라도 다시 이스케이프합니다.

`$except_lang_code`가 `true`인 경우, 사용자 다국어 코드(`$user_lang->userLangXXXXX`)는 이스케이프하지 않습니다.

#### escape_css()

```
function escape_css(
	string $str
): string
```

문자열의 내용 중 CSS 속성 값에 사용할 수 없는 문자를 이스케이프합니다.
이 함수는 CSS 코드에서 사용되는 문자열을 안전하게 처리하기 위해 사용됩니다.

#### escape_js()

```
function escape_js(
	string $str
): string
```

문자열을 JavaScript 코드에서 안전하게 사용할 수 있도록 이스케이프합니다.

```
<script>
  var str = '<?php echo escape_js('<script>alert("XSS")</script>'); ?>';
  console.log(str); // 출력: "\u003Cscript\u003Ealert(\u0022XSS\u0022)\u003C\/script\u003E"
  alert(str); // 화면에 표시할 때는 "<script>alert("XSS")</script>" 그대로 노출
</script>
```

이 함수를 사용하면 HTML에 포함된 JavaScript에서도 안전하게 문자열을 출력할 수 있습니다.
예를 들어 `<` 문자는 `escape()` 함수를 사용하면 `&lt;`로 변환되지만,
`escape_js()` 함수를 사용하면 `\x3C` 또는 `\u003C`로 변환되어
어떤 문맥에서도 태그를 여는 의미로 잘못 해석될 여지가 없습니다.

#### escape_sqstr()

```
function escape_sqstr(
	string $str
): string
```

문자열을 작은따옴표(`'`)로 감쌌을 때 문제가 되지 않도록 이스케이프합니다.
즉, 작은따옴표(`'`)를 `\'`로 변환합니다.
SQL 쿼리나 JavaScript에서 문자열을 작은따옴표로 감쌀 때는 더 안전한 다른 함수를 사용하십시오.

#### escape_dqstr()

```
function escape_dqstr(
	string $str
): string
```

문자열을 큰따옴표(`"`)로 감쌌을 때 문제가 되지 않도록 이스케이프합니다.
즉, 큰따옴표(`"`)를 `\"`로 변환합니다.
SQL 쿼리나 JavaScript에서 문자열을 작은따옴표로 감쌀 때는 더 안전한 다른 함수를 사용하십시오.

#### explode_with_escape()

```
function explode_with_escape(
	string $delimiter,
	string $str,
	int $limit = 0,
	string $escape_char = '\\'
): array
```

문자열을 지정한 구분자로 나누되, 이스케이프 문자가 있는 경우 해당 구분자를 무시합니다.
예를 들어, `explode_with_escape(',', 'a,b\\,c,d', 0, '\\')`는
`['a', 'b,c', 'd']`를 반환합니다.

#### starts_with()

```
function starts_with(
	string $needle,
	string $haystack,
	bool $case_sensitive = true
): bool
```

문자열이 지정한 접두사로 시작하는지 확인합니다.
`$case_sensitive`가 `true`인 경우 대소문자를 구분하여 비교합니다.

이 함수는 PHP 8.0 이상에서 제공되는 `str_starts_with()`와 비슷한 기능을 하지만,
대소문자 구분 여부를 선택할 수 있습니다.
또한 파라미터의 순서가 반대이므로 주의해야 합니다.

#### ends_with()

```
function ends_with(
	string $needle,
	string $haystack,
	string bool $case_sensitive = true
): bool
```

문자열이 지정한 접미사로 끝나는지 확인합니다.
`$case_sensitive`가 `true`인 경우 대소문자를 구분하여 비교합니다.

이 함수는 PHP 8.0 이상에서 제공되는 `str_ends_with()`와 비슷한 기능을 하지만,
대소문자 구분 여부를 선택할 수 있습니다.
또한 파라미터의 순서가 반대이므로 주의해야 합니다.

#### contains()

```
function contains(
	string $needle,
	string $haystack,
	bool $case_sensitive = true
): bool
```

문자열이 다른 문자열을 포함하는지 확인합니다.
`$case_sensitive`가 `true`인 경우 대소문자를 구분하여 비교합니다.

이 함수는 PHP 8.0 이상에서 제공되는 `str_contains()`와 비슷한 기능을 하지만,
대소문자 구분 여부를 선택할 수 있습니다.
또한 파라미터의 순서가 반대이므로 주의해야 합니다.

#### is_between()

```
function is_between(
	mixed $needle,
	mixed $min,
	mixed $max,
	bool $exclusive = false
): bool
```

`$needle`이 `$min`과 `$max` 사이에 있는지 확인합니다.
`$exclusive`가 `true`인 경우, 경계값은 포함하지 않습니다.

정수 등 숫자형 데이터에 사용하는 것이 원칙이지만,
날짜를 포함하는 문자열 등 다른 데이터 타입에도 사용할 수 있습니다.
비교 가능한 타입인지는 사용자가 판단해야 합니다.
무리하게 사용할 경우, 예기치 않은 결과가 나올 수 있습니다.

#### force_range()

```
function force_range(
	mixed $input,
	mixed $min,
	mixed $max
): mixed
```

`$input`이 `$min`과 `$max` 사이에 있도록 강제합니다.
즉, `$input`이 `$min`보다 작으면 `$min`을 반환하고,
`$input`이 `$max`보다 크면 `$max`를 반환하고,
그렇지 않으면 `$input`을 그대로 반환합니다.

정수 등 숫자형 데이터에 사용하는 것이 원칙이지만,
날짜를 포함하는 문자열 등 다른 데이터 타입에도 사용할 수 있습니다.
비교 가능한 타입인지는 사용자가 판단해야 합니다.
무리하게 사용할 경우, 예기치 않은 결과가 나올 수 있습니다.

#### base64_encode_urlsafe()

```
function base64_encode_urlsafe(
	string $str
): string
```

문자열을 URL 안전한 Base64 형식으로 인코딩합니다.
인코딩 결과를 URL에서 추가적인 변환 없이 안전하게 사용할 수 있도록
`+`, `/`, `=` 문자를 각각 `-`, `_`, 빈 문자열로 변환합니다.
(`=`는 Base64 인코딩에서 패딩 문자로 사용되므로, 삭제해도 의미 손실이 발생하지 않습니다.)

#### base64_decode_urlsafe()

```
function base64_decode_urlsafe(
	string $str
): string
```

URL 안전한 Base64 형식으로 인코딩된 문자열을 디코딩합니다.

#### number_shorten()

```
function number_shorten(
	int|float $number,
	int $significant_digits = 2
): string
```

SI 접미사를 사용하여 숫자를 간단하게 표현하며, 지정된 자릿수만큼 유효 숫자를 남깁니다.
예를 들어, `number_shorten(1234567)`은 `1.2M`을 반환하고,
예를 들어, `number_shorten(12345678)`은 `12M`을 반환합니다.

#### path2url()

```
function path2url(
	string $path
): string|false
```

`Rhymix\Framework\URL::fromServerPath()`의 alias입니다.
주어진 서버측 파일시스템 절대경로에 대응하는 URL을 반환합니다.
변환할 수 없는 경로인 경우, `false`를 반환합니다.

#### url2path()

```
function url2path(
	string $url
): string|false
```

`Rhymix\Framework\URL::toServerPath()`의 alias입니다.
주어진 URL에 대응하는 서버측 파일시스템 절대경로를 반환합니다.
변환할 수 없는 경로인 경우, `false`를 반환합니다.

#### hex2rgb()

```
function hex2rgb(
	string $hex
): array
```

16진수로 표현된 색상값의 RGB 값을 분해하여 배열로 반환합니다.
예를 들어, `hex2rgb('#ff0000')`은 `[255, 0, 0]`을 반환합니다.
`#` 문자는 생략할 수 있으며, 대소문자를 구분하지 않습니다.
`#f00`과 같이 3자리로 축약된 색상도 지원합니다.

#### rgb2hex()

```
function rgb2hex(
	array $rgb,
	bool $hash_prefix = true
): string
```

배열러 표현된 RGB 값을 16진수 색상값으로 변환합니다.
예를 들어, `rgb2hex([255, 0, 0])`은 `#ff0000`을 반환합니다.
`$hash_prefix`가 `false`인 경우, `#` 문자를 생략합니다.

#### include_in_clean_scope()

```
function include_in_clean_scope(
	string $filename
): mixed
```

주어진 파일을 인클루드하되, 현재 스코프의 변수나 함수를 오염시키지 않습니다.
인클루드한 파일에서 출력하는 내용은 그대로 출력되며,
반환값이 있는 경우 해당 값을 반환합니다.

#### include_and_ignore_errors()

```
function include_and_ignore_errors(
	string $filename
): mixed
```

주어진 파일을 인클루드하되, 인클루드한 파일에서 발생하는 PHP 오류를 무시합니다.
인클루드한 파일에서 출력하는 내용은 그대로 출력되며,
반환값이 있는 경우 해당 값을 반환합니다.

#### include_and_ignore_output()

```
function include_and_ignore_output(
	string $filename
): mixed
```

주어진 파일을 인클루드하되, 인클루드한 파일에서 출력하는 내용은 무시합니다.
반환값이 있는 경우 해당 값을 반환합니다.

#### tobool()

```
function tobool(
	mixed $input
): bool
```

주어진 값을 `boolean` 타입으로 변환합니다.
`Y`, `yes`, `True`, `on`, `1` 등은 `true`로 변환하고,
`N`, `no`, `False`, `off`, `0` 등은 `false`로 변환합니다.
판단하기 어려운 경우 PHP의 기본 타입 변환 규칙을 따릅니다.

#### countobj()

```
function countobj(
	mixed $array_or_object
): int
```

배열에 포함된 요소의 갯수나 오브젝트의 속성 갯수를 반환합니다.
과거 XE 1.x에서는 배열과 오브젝트를 구분하지 않고 `count()` 함수를 남용하는 사례가 많았는데,
이러한 레거시 코드를 라이믹스와 최신 PHP에 맞게 수정할 때 활용할 수 있는 함수입니다.
가능하면 이 함수에 의존하지 말고 각 타입에 맞는 함수를 사용하시기 바랍니다.

배열이나 오브젝트가 아닌 경우, PHP에서 참으로 취급할 만한 값이라면 `1`, 아니라면 `0`을 반환합니다.

#### utf8_check()

```
function utf8_check(
	string $str
): bool
```

문자열이 유효한 UTF-8 인코딩인지 확인합니다.

#### utf8_clean()

```
function utf8_clean(
	string $str
): string
```

문자열을 UTF-8 Normalization Form C로 정규화하고,
일반적으로 불필요하거나 문제를 일으킬 수 있는 특수문자를 제거합니다.
이 함수는 주로 사용자 입력을 필터링할 때 사용됩니다.

제거되는 문자는 BOM, Hangul Filler, Right-to-Left Override 문자,
그리고 Zalgo를 유발하는 4개 이상의 연속된 구분 표시 부호입니다.

#### utf8_mbencode()

```
function utf8_mbencode(
	string $str
): string
```

이모티콘, 일부 한자 및 희귀 외국어 등
Basic Multilingual Plane(BMP) 이상의 영역에 위치한 유니코드 문자를 HTML 이스케이프 시퀀스로 변환하여,
이러한 문자를 저장할 수 없는 데이터베이스에도 안전하게 저장할 수 있도록 합니다.
XE 1.x에서 업그레이드하여 DB의 문자셋이 `utf8mb3`인 경우, 글과 댓글에 자동으로 적용됩니다.

#### utf8_normalize_spaces()

```
function utf8_normalize_spaces(
	string $str,
	bool $multiline = false
): string
```

문자열에 포함된 줄바꿈, 탭 등 공백 문자를 모두 스페이스(`\x20`)로 변환하고,
연속된 스페이스를 1개로 줄입니다.

`$multiline`이 `true`인 경우, 줄바꿈 문자(`\n`)는 변환하지 않고 유지합니다.

#### utf8_trim()

```
function utf8_trim(
	string $str
): string
```

문자열의 앞뒤에서 스페이스(`\x20`)를 비롯하여 유니코드가 인식하는 모든 공백 문자를 제거합니다.

#### is_html_content()

```
function is_html_content(
	string $str
): bool
```

문자열이 HTML 콘텐츠인지 확인합니다.

이 함수는 기본적으로 문자열의 분량 대비 적정 갯수 이상의 HTML 태그가 포함되어 있는지 확인하는 전략을 취합니다.
일반적이지 않은 형태의 콘텐츠인 경우 잘못 판단할 수 있으므로, 보조적인 수단으로만 사용하십시오.
예를 들어 이 함수를 사용하여 HTML 콘텐츠가 아니라고 판단했더라도
이스케이프 처리를 게을리하여서는 안 됩니다.

#### is_empty_html_content()

```
function is_empty_html_content(
	string $str
): bool
```

문자열이 빈 HTML인지 확인합니다.
태그와 공백 문자를 모두 제거하였을 때 남는 내용이 없다면 빈 HTML이라고 판단합니다.
예를 들어 아래의 문자열은 빈 HTML입니다.

```
<div>
	<p> </p>
	<p> </p>
</div>
```

단, 이미지나 동영상 등이 포함된 경우 빈 HTML이라고 판단하지 않습니다.



### 레거시 전역 함수

아래의 함수들은 legacy.php에서 선언하며, 라이믹스와 XE 1.x에서 공통으로 사용할 수 있습니다.
위의 목록과 마찬가지로, 사용을 피할 필요는 없습니다.
그러나 대안이 존재하는 경우가 많으므로, 새로운 코드를 작성할 때는
더 좋은 방법이 있는지 검토해 보시기 바랍니다.

#### executeQuery()

```
function executeQuery(
	string $query_id,
	array|object $args = [],
	array $column_list = [],
	string $result_type = 'auto',
	string $result_class = 'stdClass'
): Rhymix\Framework\Helpers\DBResultHelper
```

주어진 이름의 XML 쿼리를 실행하고, 결과를 반환합니다.

이 함수는 `Rhymix\Framework\DB::executeQuery()` 메소드의 alias입니다.

`$query_id`는 일반적으로 `모듈명.파일명`의 형식을 따르며, 애드온이나 위젯에 포함된 쿼리인 경우
`addons.애드온명.파일명` 또는 `widgets.위젯명.파일명`의 형식이 될 수도 있습니다.
예를 들어 `modules/document/queries/getDocument.xml` 쿼리를 실행하려면
`executeQuery('document.getDocument', ...)`라고 호출합니다.

`$args`는 쿼리에서 사용할 인자를 배열이나 객체로 전달합니다.
XE와 달리 라이믹스는 배열을 선호합니다. 쿼리 조합 과정에서 불필요한 변환을 피할 수 있어, 성능이 개선되기 때문입니다.

`$column_list`는 쿼리 결과에서 반환할 컬럼의 목록을 지정합니다.
지정하지 않을 경우 XML 쿼리에서 선언한 컬럼 목록을 사용하며, 대부분의 상황에서 이것을 권장합니다.

`$result_type`은 쿼리 결과의 타입을 지정합니다.
아래와 같은 값을 가질 수 있습니다.

- `auto`: 반환 레코드가 1개인 경우 오브젝트를 반환하고, 2개 이상인 경우 배열에 오브젝트를 담아서 반환합니다.
- `array`: 반환 레코드의 갯수와 관계없이 항상 배열에 오브젝트를 담아서 반환합니다.

`$result_class`는 각각의 반환 레코드를 담을 클래스 이름을 지정합니다.
지정하지 않을 경우 `stdClass`를 사용합니다.

#### executeQueryArray()

```
function executeQueryArray(
	string $query_id,
	array|object $args = [],
	array $column_list = [],
	string $result_class = 'stdClass'
): Rhymix\Framework\Helpers\DBResultHelper
```

이 함수는 `executeQuery()`와 비슷하지만, 항상 배열에 오브젝트를 담아서 반환합니다.
`$result_type`을 `array`로 지정한 것과 동일합니다.

#### getNextSequence()

```
function getNextSequence(): int
```

DB에서 다음 시퀀스 값을 생성하여 반환합니다.

#### getUrl()

```
function getUrl(...$args): string
```

짧은 주소를 생성합니다. 결과는 자동으로 이스케이프됩니다.

주소를 구성하는 인자들을 넘기는 방법은 3가지가 있습니다.

1\. 연관 배열을 사용하는 방법 (권장)

```
getUrl([
	'mid' => 'board',
	'category_srl' => 123,
	'page' => 2,
])
```

2\. 키와 값을 번갈아 나열하는 방법

```
getUrl(
	'category_srl', 123,
	'page', 2
)
```

XE 1.x에서 많이 사용하던 방법으로, 새 URL을 생성하지 않고,
주어진 키와 값들을 현재 페이지 URL에 추가하거나 대체합니다.
빈 문자열이나 `null`을 값으로 넣으면 해당 키는 삭제됩니다.

게시판에서 현재 검색 조건을 유지하면서 하나의 조건이 추가된 페이지를 생성하는 등,
현재 페이지 URL을 조금씩 변경하려고 할 때 유용합니다.
반면, 현재 페이지 URL에 불필요한 파라미터가 붙어 있는 경우, 그대로 유지된다는 단점이 있으므로
전혀 다른 모듈이나 서로 무관한 페이지로 이동할 때는 사용하지 않는 것이 좋습니다.

3\. 키와 값을 번갈아 나열하되, 빈 문자열을 앞에 두는 방법

```	
getUrl(
	'', // 빈 문자열
	'mid' => 'board',
	'category_srl', 123,
	'page', 2
)
```

위 2번의 단점을 보완할 수 있는 방법입니다.
빈 문자열은 XE 1.x에서 현재 URL에 붙어 있는 기존 파라미터들을 초기화하라는 의미로 사용되었는데,
이것을 라이믹스에서도 그대로 사용할 수 있습니다.
다른 페이지로 이동할 때 불필요한 파라미터가 따라오지 않는다는 장점이 있습니다.

그러나 2번과 마찬가지로 가독성이 떨어지고, 목록을 별도로 조합하여 사용하기 어렵다는 단점이 있으므로,
가능하면 1번 방식으로 변환하는 것을 권장합니다.

#### getNotEncodedUrl()

```
function getNotEncodedUrl(...$args): string
```

짧은 주소를 생성하되, 이스케이프를 적용하지 않습니다.
HTML이 아닌 JavaScript나 헤더 등에 URL을 사용할 때 유용합니다.

인자를 넘기는 방법은 `getUrl()`과 같습니다.

#### getFullUrl()

```
function getFullUrl(...$args): string
```

짧은 주소를 생성하되, 도메인을 포함하는 전체 주소를 생성합니다.

인자를 넘기는 방법은 `getUrl()`과 같습니다.

#### getNotEncodedFullUrl()

```
function getNotEncodedFullUrl(...$args): string
```

짧은 주소를 생성하되, 도메인을 포함하는 전체 주소를 생성하고, 이스케이프하지 않습니다.

인자를 넘기는 방법은 `getUrl()`과 같습니다.

#### getCurrentPageUrl()

```
function getCurrentPageUrl(
	bool $escape = true
): string
```

현재 페이지의 URL을 반환합니다.
`$escape`가 `true`인 경우, 결과를 HTML 이스케이프합니다.

#### getInternalDateTime()

```
function getInternalDateTime(
	?int $timestamp = null,
	string $format = 'YmdHis'
): string
```

주어진 유닉스 타임스탬프를 [내부 표준 시간대](../../misc/timezone.md)에 맞추어
주어진 형식으로 반환합니다.

#### getDisplayDateTime()

```
function getDisplayDateTime(
	?int $timestamp = null,
	string $format = 'YmdHis'
): string
```

주어진 유닉스 타임스탬프를 [디스플레이 표준 시간대](../../misc/timezone.md)에 맞추어
주어진 형식으로 반환합니다.

#### getMonthName()

```
function getMonthName(
	int $month,
	bool $short = true
): string
```

주어진 월 번호(1~12)에 해당하는 영문 월 이름을 반환합니다.
`$short`가 `true`인 경우, 짧은 형식(예: "Jan")으로 반환하고,
`false`인 경우, 긴 형식(예: "January")으로 반환합니다.

#### getTimeGap()

```
function getTimeGap(
	string $date,
	string $format = 'Y.m.d')
: ?string
```

주어진 시간이 현재 시간으로부터 24시간 이내인 경우
`Rhymix\Framework\DateTime::getRelativeTimestamp()` 메소드를 사용하여
"15분 전"과 같이 쉽게 읽을 수 있는 문자열을 반환합니다.
24시간을 초과한 경우, `$format`의 형식대로 반환합니다.

#### debugPrint()

```
function debugPrint(
	mixed $value,
	...$values
): void
```

주어진 값(들)을 디버그 정보에 추가합니다.
`Rhymix\Framework\Debug::addEntry()` 메소드의 alias입니다.

여러 값을 넘길 수 있습니다. 이 경우 디버그 정보에 각각 추가됩니다.

#### getNumberingPath()

```
function getNumberingPath(
	int $no,
	int $size = 3
): string
```

숫자를 3자리씩 나누어 분산 저장할 수 있는 경로를 생성합니다.
결과는 `001/002/003/`과 같이 슬래시 문자가 뒤에 붙는 형태를 띱니다.

#### hexrgb()

```
function hexrgb(
	string $hex
): array
```

`hex2rgb()` 함수와 동일한 기능입니다.

#### isCrawler()

```
function isCrawler(
	?string $user_agent = null
): bool
```

주어진 user-agent가 로봇인지 판단합니다.
`$user_agent`가 `null`인 경우, 현재 요청의 user-agent를 사용합니다.
로봇이라고 판단되는 경우, `true`를 반환하고, 그렇지 않으면 `false`를 반환합니다.

최근에는 일반 브라우저로 위장하고 일반 브라우저처럼 행동하는 로봇이 많으므로,
이 함수의 반환값을 지나치게 신뢰해서는 곤란합니다.

#### zdate()

```
function zdate(
	string $str,
	string $format = 'Y-m-d H:i:s',
	bool $conversion = false
): ?string
```

`getDisplayDateTime()` 함수와 비슷하지만, 유닉스 타임스탬프가 아닌
`YYYYMMDDHHMMSS` 또는 `YYYY-MM-DD HH:MM:SS` 형식의 문자열도 처리할 수 있습니다.
시간 부분이 없는 `YYYYMMDD` 형식을 넘길 경우, 시간은 0시 0분 0초로 간주합니다.

#### ztime()

```
function ztime(
	string $str
): ?int
```

위의 `zdate()`에서 문자열을 유닉스 타임스탬프로 변환하는 데 사용하는 함수입니다.
XE 1.x에서 널리 사용하던 `YYYYMMDDHHMMSS`,
대부분의 DB에서 표준으로 채택한 `YYYY-MM-DD HH:MM:SS`와 같이 형식이 정해져 있는 문자열인 경우,
이 함수를 사용하면 `strtotime()`보다 더 정확한 해석을 보장할 수 있습니다.



### Deprecated

아래의 함수들은 여러 모듈에서 여전히 사용되지만, 새로운 코드에서는 사용을 피하는 것을 권장합니다.
비슷한 기능을 제공하는 함수라도, 라이믹스 프레임워크 클래스나 위에 나열된 신규 함수를 사용하는 것보다
기능이 제한적이거나 성능이 떨어질 수 있기 때문입니다.

#### getModule()

```
function getModule(
	string $module_name,
	string $type = 'view',
	string $kind = ''
): ?ModuleObject
```

특정 모듈에 소속된 특정 클래스의 인스턴스를 반환합니다.
클래스를 찾을 수 없는 경우 `null`을 반환합니다.

`$module_name`은 모듈명입니다.

`$type`은 클래스의 종류를 지정하며, `model`, `view`, `controller` 등의 값을 가집니다.

`$kind`는 관리자용 모듈인 경우 `admin`을 지정합니다.

위의 세 변수를 조합하여 XE 1.x 방식의 모듈 클래스명을 찾게 됩니다.
예를 들어 `board` 모듈의 `view` 종류, `admin` 종류라면 `BoardAdminView` 클래스가 됩니다.

이 함수를 비롯하여 아래에 나열된 함수들은 모두 몇 가지 치명적인 단점이 있으므로 사용을 권장하지 않습니다.

1. 정해진 종류의 클래스 외에는 찾을 수 없습니다.
모듈의 구조가 점점 복잡해지고, 모델이나 컨트롤러를 하나의 클래스만으로 구현하기 어려운데도
이 함수에 의존하여 불러오려면 임의로 클래스를 추가할 수 없기 때문에
간혹 1만 줄이 넘는 비대한 클래스가 생성되기도 합니다.

2. 반환값의 클래스를 유추하기 어렵기 때문에, 대부분의 IDE에서 자동완성이 제대로 작동하지 않습니다.
현대적인 자동완성과 AI 기능을 활용할 수 없어서 개발 생산성이 떨어지는 것은 물론,
정의되지 않은 클래스나 메소드를 사용하려고 시도하는 등 명백한 오류를 찾아내기도 어렵게 됩니다.

이러한 문제 때문에 라이믹스에서는 각 모듈 클래스의 싱글턴 인스턴스를 직접 생성하는 것을 추천합니다.
`BoardAdminView::getInstance()`를 사용하여 클래스명을 직접 참조하는 것입니다.
`ModuleObject`를 상속하는 모든 클래스는 `getInstance()` 메소드를 통해 싱글턴을 생성할 수 있습니다.

#### getController()

```
function getController(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'controller')`와 동일합니다.

#### getAdminController()

```
function getAdminController(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'controller', 'admin')`과 동일합니다.

#### getView()

```
function getView(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'view')`와 동일합니다.

#### getAdminView()

```
function getAdminView(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'view', 'admin')`과 동일합니다.

#### getModel()

```
function getModel(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'model')`과 동일합니다.

#### getAdminModel()


```
function getAdminModel(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'model', 'admin')`과 동일합니다.

#### getAPI()

```
function getAPI(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'api')`와 동일합니다.

#### getMobile()

```
function getMobile(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'mobile')`과 동일합니다.

#### getWAP()

```
function getWAP(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'wap')`과 동일합니다.

#### getClass()

```
function getClass(
	string $module_name
): ?ModuleObject
```

이 함수는 `getModule($module_name, 'class')`와 동일합니다.

비슷한 종류의 다른 함수들과 달리, `getClass('board')`라고 호출했을 때
`BoardClass` 클래스가 아닌 `Board` 클래스를 찾습니다.
즉, 모듈명과 동일한 이름의 클래스를 찾는 함수입니다.

#### setUserSequence()

```
function setUserSequence(int $seq): void
```

생성된 시퀀스 값을 세션에 저장합니다. `getNextSequence()` 함수에서 내부적으로 사용합니다.

#### checkUserSequence()

```
function checkUserSequence(int $seq): bool
```

세션에 저장된 시퀀스 값을 주어진 값과 비교합니다.

#### getAutoEncodedUrl()

```
function getAutoEncodedUrl(...$args): string
```

`getUrl()` 함수와 비슷하지만, 생성된 URL을 이스케이프하며
`$double_escape`를 `false`로 지정한 것과 같은 결과를 반환합니다.
혼란을 일으키기 쉬운 특이한 조합이므로, 사용을 권장하지 않습니다.

#### getSiteUrl()

```
function getSiteUrl(...$args): string
```

`getUrl()` 함수와 비슷하지만, XE 1.x에서 사용하던 가상 사이트의 도메인을 포함합니다.
라이믹스에서는 XE 1.x 방식의 가상 사이트를 지원하지 않으므로, 이 함수는 사용하지 않습니다.

#### getNotEncodedSiteUrl()

```
function getNotEncodedSiteUrl(...$args): string
```

`getUrl()` 함수와 비슷하지만, XE 1.x에서 사용하던 가상 사이트의 도메인을 포함합니다.
라이믹스에서는 XE 1.x 방식의 가상 사이트를 지원하지 않으므로, 이 함수는 사용하지 않습니다.

#### getFullSiteUrl()

```
function getFullSiteUrl(...$args): string
```

`getUrl()` 함수와 비슷하지만, XE 1.x에서 사용하던 가상 사이트의 도메인을 포함합니다.
라이믹스에서는 XE 1.x 방식의 가상 사이트를 지원하지 않으므로, 이 함수는 사용하지 않습니다.

#### getScriptPath()

```
function getScriptPath(): string
```

이 함수는 `RX_BASEURL` 상수와 동일한 값을 반환합니다.

#### getRequestUriByServerEnviroment()

```
function getRequestUriByServerEnviroment(): string
```

이 함수는 `$_SERVER['REQUEST_URI']`에서 몇몇 특수문자를 인코딩한 결과를 반환합니다.
인코딩하는 특수문자의 종류에 일관성이 없고, 용도에 따라 `escape()`, `escape_js()`, `urlencode()` 등의
함수를 사용하는 것이 더 안전하므로, 이 함수는 사용하지 않는 것이 좋습니다.

#### isSiteID()

```
function isSiteID(string $domain): bool
```

주어진 값이 유효한 변수명으로 사용할 수 있는 문자열인지 확인합니다.

#### cut_str()

```
function cut_str(
	string $string,
	int $cut_size = 0,
	string $tail = '...')
: string
```

문자열을 일정한 길이로 자르고, 뒤에 `$tail`을 붙입니다.
`$cut_size`가 0인 경우, 문자열을 자르지 않고 그대로 반환합니다.
한글은 영문이나 숫자보다 2배의 길이를 차지한다고 가정하여,
`$cut_size`가 10인 경우, 한글은 5자, 영문이나 숫자는 10자로 잘립니다.

지금은 2000년대보다 한글과 영문 폰트가 훨씬 다양해졌고,
CSS를 사용하여 문자열을 일정한 길이까지만 표시하는 방법이 널리 사용되고 있으므로,
이 함수를 사용하여 문자열을 임의로 자르는 것은 권장하지 않습니다.

#### get_time_zone_offset()

```
function get_time_zone_offset(string $timezone): int
```

XE 1.x에서 사용하던 `+09:00` 형식의 표준 시간대 offset 명칭을 초 단위로 바꾸어 반환합니다.

#### zgap()

```
function zgap(?int $timestamp = null): int
```

현재 사용중인 디스플레이 표준 시간대와 내부 표준 시간대의 차이를 초 단위로 반환합니다.

서머타임의 영향으로, 이 차이는 시기에 따라 달라질 수 있습니다.
예를 들어 서울과 뉴욕은 여름에는 13시간, 겨울에는 14시간 차이가 납니다.
따라서 정확한 `$timestamp`를 넘겨야 해당 시점의 차이를 알 수 있고,
한 시점에 계산한 차이를 다른 시점에 그대로 적용하여서도 안 됩니다.

`$timestamp`가 `null`인 경우, 현재 시간을 사용합니다.

#### getEncodeEmailAddress()

```
function getEncodeEmailAddress(string $email): string
```

이메일 주소를 무단으로 수집하기 어렵도록 임의의 HTML 이스케이프 시퀀스로 변환하는 함수입니다.
2025년 현재는 크롤러가 발전하여 사실상 무의미한 기능입니다.

#### delObjectVars()

```
function delObjectVars(
	object $target_obj,
	object $del_obj
): object
```

`$target_obj`의 속성들 중 `$del_obj`에 정의된 것과 같은 속성은 삭제합니다.

#### getDestroyXeVars()

```
function getDestroyXeVars(
	array|object $vars
): array|object
```

`$vars`의 속성 또는 배열 키 중, XE 1.x에서 흔히 사용하던 GET/POST 변수명을 제거합니다.
라이믹스에서 흔히 사용하는 변수도 몇 가지 추가되어, 현재 적용되는 삭제 대상은 다음과 같습니다.

- `error_return_url`
- `success_return_url`
- `ruleset`
- `xe_validator_id`
- `_filter`
- `_rx_ajax_compat`
- `_rx_ajax_form`
- `_rx_csrf_token`

#### detectUTF8()

```
function detectUTF8(
	string $string,
	bool $return_convert = false,
	bool $urldecode = true
): string|bool
```

문자열이 정상적인 UTF-8 인코딩인지 확인합니다.

`$return_convert`가 참인 경우, UTF-8이 아니더라도 곧바로 `false`를 반환하지 않고
문자열이 EUC-KR (CP949) 인코딩인지 다시 한 번 확인한 후, EUC-KR이라고 판단되면 UTF-8로 변환합니다.

`$urldecode`가 참인 경우, 일단 `URL` 디코딩을 시도합니다.

변수의 인코딩 방식을 알지 못한 상태에서 임의로 디코딩을 시도하는 불안정한 기능이므로,
이 함수는 사용을 권장하지 않습니다.

#### htmlHeader()

```
function htmlHeader(): void
```

HTML 헤더를 출력합니다.

반환하는 것이 아니고 출력한다는 점에 주의하십시오.
AJAX가 발전하지 않았던 XE 1.x 초반에 설계된 것으로 추정되는 함수입니다.

#### htmlFooter()

```
function htmlFooter(): void
```

HTML 푸터를 출력합니다.

반환하는 것이 아니고 출력한다는 점에 주의하십시오.
AJAX가 발전하지 않았던 XE 1.x 초반에 설계된 것으로 추정되는 함수입니다.

#### alertScript()

```
function alertScript(?string $msg = null): void
```

주어진 문자열을 `alert()`하는 자바스크립트 코드를 출력합니다.

반환하는 것이 아니고 출력한다는 점에 주의하십시오.
AJAX가 발전하지 않았던 XE 1.x 초반에 설계된 것으로 추정되는 함수입니다.

#### closePopupScript()

```
function closePopupScript(): void
```

팝업창을 닫는 자바스크립트 코드를 출력합니다.

반환하는 것이 아니고 출력한다는 점에 주의하십시오.
AJAX가 발전하지 않았던 XE 1.x 초반에 설계된 것으로 추정되는 함수입니다.

#### reload()

```
function reload(bool $isOpener = false): void
```

화면을 새로고침하는 자바스크립트 코드를 출력합니다.

반환하는 것이 아니고 출력한다는 점에 주의하십시오.
AJAX가 발전하지 않았던 XE 1.x 초반에 설계된 것으로 추정되는 함수입니다.

#### handleError()

```
function handleError(
	int $errno,
	string $errstr,
	string $file,
	int $line,
	array $context
): void
```

이 함수는 `Rhymix\Framework\Debug::addError()` 메소드의 alias입니다.

#### getMicroTime()

```
function getMicroTime(): float
```

이 함수는 `microtime(true)`와 동일합니다.

#### json_encode2()

```
function json_encode2(mixed $data): string|false
```

이 함수는 `json_encode()`와 동일하며, JSON을 지원하지 않던 시기 XE 1.x의 흔적입니다.

#### url_decode()

```
function url_decode(string $str): string
```

이 함수는 HTML 엔티티 디코딩과 URL 디코딩을 모두 수행하려고 시도합니다.
위에서 등장한 `detectUTF8()` 함수와 같은 이유로 권장하지 않습니다.

#### blockWidgetCode()

```
function blockWidgetCode(string $content): string
```

주어진 문자열에서 위젯 코드를 무력화시킵니다.

#### checkCSRF()

```
function checkCSRF(): bool
```

이 함수는 `Rhymix\Framework\Security::checkCSRF()` 메소드의 alias입니다.

#### stripEmbedTagForAdmin()

```
function stripEmbedTagForAdmin(
	string &$content,
	int $writer_member_srl
): void
```

이 함수는 최고관리자 권한이 있는 사용자가 다른 사용자의 콘텐츠를 열람할 때
`Rhymix\Framework\Filters\MediaFilter::removeEmbeddedMedia()` 메소드를 호출하여
타인이 작성한 콘텐츠에 포함된 임베드 미디어를 제거합니다.

#### recurciveExposureCheck()

```
function recurciveExposureCheck(
	array &$menu
): void
```

이 함수는 사용자 권한에 따른 메뉴 노출 여부를 재귀적으로 확인합니다.

#### removeHackTag()

```
function removeHackTag(
	string $content
): string
```

이 함수는 `Rhymix\Framework\Filters\HTMLFilter::clean()` 메소드의 alias입니다.

#### removeSrcHack()

```
function removeSrcHack(
	array $match
): string
```

이 함수는 항상 `'Array'`를 반환합니다.

#### purifierHtml()

```
function purifierHtml(
	string &$content
): void
```

이 함수는 `Rhymix\Framework\Filters\HTMLFilter::clean()` 메소드의 alias입니다.

#### checkXmpTag()

```
function checkXmpTag(
	string $content
): string
```

이 함수는 주어진 변수를 문자열로 변환하여 그대로 반환합니다.

#### checkUploadedFile()

```
function checkUploadedFile(
	string $file,
	?string $filename = null
): bool
```

이 함수는 `Rhymix\Framework\Filters\FileContentFilter`로 대체되어
이제는 항상 `true`를 반환합니다.

#### mysql_pre4_hash_password()

```
function mysql_pre4_hash_password(
	string $password
): string
```

주어진 문자열을 구 버전 MySQL의 비밀번호 해시 방식으로 단방향 암호화합니다.
이 해시 방식은 안전하지 않으므로, 다른 시스템과 연동하기 위해 반드시 필요한 경우를 제외하면 사용을 권장하지 않습니다.

#### changeValueInUrl()

```
function changeValueInUrl(
	string $key,
	string $requestKey,
	string $dbKey,
	string $urlName = 'success_return_url'
): void
```

#### utf8RawUrlDecode()

```
function utf8RawUrlDecode(
	string $source
): string
```

#### _code2utf()

```
function _code2utf(
	string $num
): string
```

#### writeSlowlog()

```
function writeSlowlog(): void
```

#### flushSlowlog()

```
function flushSlowlog(): void
```

#### requirePear()

```
function requirePear(): void
```



### Polyfill 함수

아래의 함수들은 PHP 버전이나 확장 설치 여부에 따라 사용할 수 없는 함수들을
라이믹스의 지원 범위 전체에서 일관성있게 사용할 수 있도록 제공합니다.
해당 함수가 기본 제공되는 환경에서는 선언하지 않습니다.

#### str_starts_with()

```
function str_starts_with(
	string $haystack,
	string $needle
): bool
```

PHP 8.0 미만 버전에서도 `str_starts_with()` 함수를 사용할 수 있도록 합니다.

이 함수는 항상 대소문자를 구분합니다.
대소문자 구분 없는 문자열 시작 비교가 필요하다면
라이믹스에서 제공하는 `starts_with()` 함수를 사용하시기 바랍니다.

#### str_ends_with()

```
function str_ends_with(
	string $haystack,
	string $needle
): bool
```

PHP 8.0 미만 버전에서도 `str_ends_with()` 함수를 사용할 수 있도록 합니다.

이 함수는 항상 대소문자를 구분합니다.
대소문자 구분 없는 문자열 시작 비교가 필요하다면
라이믹스에서 제공하는 `ends_with()` 함수를 사용하시기 바랍니다.

#### str_contains()

```
function str_contains(
	string $haystack,
	string $needle
): bool
```

PHP 8.0 미만 버전에서도 `str_contains()` 함수를 사용할 수 있도록 합니다.

이 함수는 항상 대소문자를 구분합니다.
대소문자 구분 없는 문자열 시작 비교가 필요하다면
라이믹스에서 제공하는 `contains()` 함수를 사용하시기 바랍니다.

#### iconv()

```
function iconv(
	string $in_charset,
	string $out_charset,
	string $str
): string|false
```

`iconv` 확장이 설치되어 있지 않고 `mbstring` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

단, 이 함수보다는 `mb_convert_encoding()` 함수를 사용하는 것을 권장합니다.

#### iconv_strlen()

```
function iconv_strlen(
	string $str,
	?string $charset = null
): int
```

`iconv` 확장이 설치되어 있지 않고 `mbstring` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

단, 이 함수보다는 `mb_strlen()` 함수를 사용하는 것을 권장합니다.

#### iconv_strpos()

```
function iconv_strpos(
	string $haystack,
	string $needle,
	int $offset,
	?string $charset = null
): int|false
```

`iconv` 확장이 설치되어 있지 않고 `mbstring` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

단, 이 함수보다는 `mb_strpos()` 함수를 사용하는 것을 권장합니다.

#### iconv_substr()

```
function iconv_substr(
	string $str,
	int $offset,
	?int $length = null,
	?string $charset = null
): string
```

`iconv` 확장이 설치되어 있지 않고 `mbstring` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

단, 이 함수보다는 `mb_substr()` 함수를 사용하는 것을 권장합니다.

#### mb_strlen()

```
function mb_strlen(
	string $str,
	?string $charset = null
): int
```

`mbstring` 확장이 설치되어 있지 않고 `iconv` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

`mb_strlen()` 함수보다 성능이나 안정성이 떨어질 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.

#### mb_strpos()

```
function mb_strpos(
	string $haystack,
	string $needle,
	int $offset = 0,
	?string $charset = null
): int|false
```

`mbstring` 확장이 설치되어 있지 않고 `iconv` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

`mb_strpos()` 함수보다 성능이나 안정성이 떨어질 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.

#### mb_substr()

```
function mb_substr(
	string $str,
	int $offset,
	?int $length = null,
	?string $charset = null
): string
```

`mbstring` 확장이 설치되어 있지 않고 `iconv` 확장만 설치되어 있는 경우,
양쪽 함수를 모두 사용할 수 있도록 polyfill을 제공합니다.

`mb_substr()` 함수보다 성능이나 안정성이 떨어질 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.

#### mb_substr_count()

```
function mb_substr_count(
	string $haystack,
	string $needle,
	?string $charset = null
): int
```

`mbstring` 확장이 설치되어 있지 않은 경우를 위한 polyfill입니다.

인코딩에 따라서는 정확하지 않은 결과가 나올 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.

#### mb_strtoupper()

```
function mb_strtoupper(
	string $str,
	?string $charset = null
): string
```

`mbstring` 확장이 설치되어 있지 않은 경우를 위한 polyfill입니다.

인코딩에 따라서는 정확하지 않은 결과가 나올 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.

#### mb_strtolower()

```
function mb_strtolower(
	string $str,
	?string $charset = null
): string
```

`mbstring` 확장이 설치되어 있지 않은 경우를 위한 polyfill입니다.

인코딩에 따라서는 정확하지 않은 결과가 나올 수 있으므로,
가능하면 `mbstring` 확장이 설치된 서버에서 라이믹스를 사용할 것을 권장합니다.
