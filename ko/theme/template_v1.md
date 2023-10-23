템플릿 문법 v1
------------

라이믹스의 전신인 XE 1.x에서 사용하던 템플릿 문법으로,
대부분의 기능을 HTML 태그, 속성, 주석 등의 형태로 구현하려고 하는 것이 특징입니다.

라이믹스에서 정식 지원하는 문법이고, 지원을 종료할 계획도 없습니다.
그러나 더이상 새 기능이 추가되거나 눈에 띄는 변경은 없을 예정입니다.
특정한 기능 사용시 버그가 잦다는 지적이 예전부터 있었지만, bug-for-bug 호환성 유지를 위해 수정하지 않습니다.
라이믹스 2.2 이후 신규 자료에는 더이상 v1 문법을 사용하지 않을 것을 권장합니다.

### 데이터 출력

변수나 그 밖의 PHP 표현식을 출력할 때는 한 쌍의 중괄호를 사용합니다.

	[변수 출력 예시]
    {$name}
	{$oDocument->getTitleText()}
	{$array[$key]}

	<!-- 라이믹스 2.0.23 이상 지원 -->
	{$object->$prop[0]}

첫 중괄호 직후에 공백이 있거나, 표현식 중간에 줄바꿈이 발생하면 작동하지 않습니다.
HTML 파일에 흔히 포함되어 있는 JS 함수 선언을 템플릿 문법으로 오인하지 않기 위한 조치로 추정됩니다.

	[작동하지 않는 예시]
	{ $name }

#### 변수의 유효 범위(스코프)

템플릿에서 사용하는 모든 변수는 `Context`에서 가져오고, `Context`에 저장됩니다.
모듈이나 위젯 등에서 `Context::set('foo', $value)`로 셋팅하였거나 `$_GET['foo']`로 들어온 URL 파라미터의 값은
템플릿에서 `$foo` 변수로 참조할 수 있고, 반대로 템플릿에서 `$foo`라는 변수를 조작하면
다른 자료에서 `Context::get('foo')`로 그 값을 불러올 수 있습니다.

위의 예시에 등장하는 `$var`, `$oDocument`, `$array`, `$key`, `$object`, `$prop` 등의 변수들은 모두
실제로는 `Context::get('var')`, `Context::get('oDocument')` 등으로 해석된다는 뜻입니다.
(`$_SERVER` 등의 초전역변수와 `$this`는 예외입니다.)
즉, `Context`를 통해 서로 다른 자료들과 템플릿 사이에 서로 데이터를 주고받을 수 있습니다.

이렇게 모든 템플릿이 하나의 스코프를 공유하는 시스템은 종종 버그를 일으키기도 하지만,
XE 1.x용 자료와 하위호환성을 유지하는 데 필수적인 장치이므로 변경하지 않습니다.
그 대신, 과거 `register_globals`와 같은 문제가 발생하지 않도록 URL 파라미터 필터링,
자동 escape 등의 안전장치를 강화하고 있습니다.

#### 출력 필터

라이믹스 1.8.31 이상, XE 1.11.0 이상에서는 데이터를 출력할 때 필터를 거쳐 가공하도록 할 수 있습니다.
함수(함수(함수(변수))) 형태로 겹겹이 둘러싸는 것보다 간결한 표현이 나오는 것은 물론,
보안과 직결되는 escape 처리도 손쉽게 컨트롤할 수 있습니다.

아래의 예제는 `$var`를 소문자로 변환하고 escape하여 출력합니다.

	{$var|lower|escape}

아래의 예제는 타임스탬프를 특정한 형태로 포맷하여 출력합니다. 필터명과 옵션은 `:`으로 구분합니다.

	{$timestamp|date:'n/j H:i'}

템플릿 문법 v1에서 지원하는 모든 필터의 목록은 아래의 표를 참고하십시오.

| 필터명      | 기능 | 옵션 |
|------------|-----|-----|
| autoescape | 자동 escape (이중 인코딩하지 않음) | |
| autolang   | 자동 escape하되, 언어코드인 경우 escape하지 않음 | |
| escape     | 강제 escape (이중 인코딩) | |
| noescape   | escape하지 않음 | |
| escapejs   | JS 문자열에 넣을 수 있는 형태로 escape | |
| json       | JSON으로 인코딩 (JS 문맥에서는 noescape와 함께 사용) | |
| strip      | strip_tags() | |
| strip_tags | strip_tags() | |
| trim       | trim() | |
| urlencode  | rawurlencode() | |
| lower      | 소문자로 변환 | |
| upper      | 대문자로 변환 | |
| nl2br      | 개행 문자를 `<br>`로 변환 (noescape 자동 적용) | |
| join       | 배열을 문자열로 합침 | 구분자, 기본값은 `,` |
| date       | 타임스탬프 포맷 | 사용할 포맷, 기본값은 `Y-m-d H:i:s` |
| format     | 숫자에 천 단위 쉼표 표시 | 소수점 이하 자릿수, 기본값은 0 |
| number_format | 위와 같음 | |
| shorten    | 숫자를 `123.4K`와 같은 형태로 표시 | 소숫점 이하 자릿수, 기본값은 2 |
| number_shorten | 위와 같음 | |
| link       | 문자열을 링크로 표시 (noescape 자동 적용) | 링크 텍스트, 기본값은 원본 문자열 |

### 조건문 및 루프문

HTML 주석 안에 `@` 기호를 붙여 `if`, `for`, `foreach`, `while` 등의 조건문과 루프문을 사용할 수 있습니다.

	[v1 권장 문법]
	<!--@if($foo)-->
		<div class="bar"></div>
	<!--@else-->
		<div class="baz"></div>
	<!--@end-->

조건문이나 루프문이 끝날 때는 일반적으로 `<!--@end-->`를 사용하나, `<!--@endif-->` 또는 `<!--@endforeach-->` 등
루프의 종류에 맞는 표현을 사용한다면 가독성이 개선됩니다. 단, 잘못 사용하더라도 오류가 나지 않습니다.

	[v1 권장 문법]
	<!--@foreach($comments as $comment)-->
		<div class="item">{$comment->getContent()}</div>
	<!--@endforeach-->

한때 조건문과 루프문을 HTML 태그로 표현하려는 시도가 있었는데, 커뮤니티에서 흔히 "신(新)문법"이라고 부릅니다.
정규식을 사용하여 신문법을 해석하는 과정에서 버그가 잦으므로, 라이믹스에서는 사용을 권장하지 않습니다.
코드가 다소 길어지더라도 일반적인 if문, foreach문 등을 사용하시기 바랍니다.

	[v1 변형 문법 (권장하지 않음)]
	<block loop="$comments => $comment">
		<div cond="$foo" class="bar"></div>
	</block>

단, 태그 전체가 아닌 하나의 속성을 표시할지 숨길지 선택하는 `|cond=""` 문법은 해석 오류가 거의 없고,
HTML 코드 중간에 거추장스러운 if문을 끼워넣는 불편을 크게 덜어 준다는 장점이 있습니다.
아래의 예제에서는 `$val === 'foo'`라는 조건이 참인 경우 `selected="selected"` 속성이 표시됩니다.

	[v1 변형 문법 (사용해도 무방)]
	<ul>
		<li value="foo" selected="selected"|cond="$val === 'foo'">FOO</li>
	</ul>

### 템플릿 인클루드

다른 템플릿 파일을 인클루드하려면 아래와 같은 문법을 사용합니다.
현재 파일을 기준으로 한 상대경로를 입력해야 하고, 확장자 `.html`은 생략할 수 있습니다.

	<include target="dir/filename.html" />

거슬러 올라가야 하는 경로가 많은 경우, "처음"을 의미하는 `^` 문자를 사용하여
라이믹스가 설치된 루트 경로를 참조할 수 있습니다.
`../../../../`를 반복하는 것보다 간결하고, 현재 템플릿의 경로와 무관하므로
파일 이동 등 유지보수가 용이합니다.

	<include target="^/common/tpl/refresh.html" />

인클루드할 때 `cond` 속성을 사용하여, 조건이 참일 때만 인클루드하도록 할 수 있습니다.

	<include target="../filename" cond="$foo" />

XE 1.4.4 미만 버전에서는 아래와 같은 HTML 주석 형태의 인클루드 문법을 사용한 적도 있으나, 더이상 권장하지 않습니다.

	[v1 초기 문법 (권장하지 않음)]
    <!--#include("header.html")-->

### 리소스 로딩

템플릿 문법을 통해 다양한 리소스(애셋)의 로딩을 지원합니다.

`<script>` 또는 `<link>` 태그를 사용하여 JS 및 CSS 파일을 직접 불러올 수도 있습니다.
그러나 여러 모듈과 스킨 등을 조합하여 사용하는 라이믹스의 특성상,
템플릿 문법을 사용하여 코어에서 리소스를 통합 관리하도록 하면 여러 가지 장점이 있습니다.

- 코어의 CSS/JS 압축 및 합치기 기능과 자동으로 연동됩니다.
- SCSS, LESS 파일을 로딩하면 자동으로 컴파일되고, 변수를 전달할 수도 있습니다.
- `index` 속성으로 로딩 순서를 조절할 수 있습니다.
- 이미 로딩한 파일을 언로딩할 수 있습니다.

#### CSS, SCSS, LESS

아래와 같이 `<load>` 태그를 사용하여 경로를 지정하고, 여러 부가적인 속성을 선언할 수 있습니다.

- `target` : 로딩할 파일의 경로를 지정합니다. 인클루드와 마찬가지로 현재 파일을 기준으로 한
  상대경로를 사용하는 것이 원칙이지만, 필요시 `^` 문자로 라이믹스가 설치된 루트 경로를 참조할 수 있습니다.
- `media` : 이 스타일시트를 사용할 기기의 종류를 지정합니다. screen, print, braille 등
- `index` : 로딩 순서를 지정합니다.
- `vars` : SCSS나 LESS에서 사용할 변수 목록 (연관배열 또는 오브젝트)

```
<load target="styles.css" />
<load target="../styles.css" media="print" index="20" />
```

SCSS나 LESS도 동일한 문법으로 로딩합니다. 코어에서 자동으로 컴파일합니다.

	<load target="../styles.less" />
	<load target="css/styles.scss" vars="$vars" />

XE 1.4.4 미만 버전에서는 아래와 같은 문법도 사용하였으나, 더이상 권장하지 않습니다.

	<!--%import("dir1/dir2/styles.css")-->

#### JS

CSS와 동일한 문법으로 로딩합니다.

- `target` : 로딩할 파일의 경로를 지정합니다.
- `type` : 스크립트를 실행할 위치를 지정합니다. 기본값은 `head`입니다.
  - `head` : HTML 소스의 `<head>` 부분에 삽입되어, 본문 로딩 전 실행됩니다.
  - `body` : HTML 소스의 `<body>` 하단에 삽입되어, 본문 로딩 후 실행됩니다.
- `index` : 로딩 순서를 지정합니다.

```
<load target="script.js" />
<load target="dir/script.js" type="head" />
<load target="^/common/js/script.js" type="body" index="10" />
```

XE 1.4.4 미만 버전에서는 아래와 같은 문법도 사용하였으나, 더이상 권장하지 않습니다.

    <!--%import("../script.js")-->

#### JS 플러그인

CKEditor, jQuery File Upload 등 `common/js/plugins/` 폴더에 있는 라이브러리를 로딩합니다.
태그를 사용하는 문법이 존재하지 않아, XE 초창기의 주석 문법을 그대로 사용해야 합니다.

	<!--%load_js_plugin("ckeditor")-->

#### XML JS 필터

프론트엔드에서 실행되는 XML JS 필터를 로딩합니다.
두 가지 문법이 존재하며, 백엔드에서 실행되는 룰셋(ruleset)과 마찬가지로
XML JS 필터 사용은 라이믹스에서 권장하지 않습니다.

	<load target="filters/insert.xml" />
	<!--%import("filters/insert.xml")-->

#### 언어 파일

특정 모듈, 또는 특정한 폴더에 저장되어 있는 언어 파일을 로딩합니다.
파일명까지 지정할 수 있는 것처럼 보이지만, `lang.xml`을 입력하더라도 실제로는 폴더명까지만 인식합니다.

	<load target="./lang" />
	<load target="./lang/lang.xml" />

#### 언로딩

CSS 또는 JS 파일 로딩을 취소합니다. 로딩할 때 사용했던 것과 동일한 경로를 지정해야 합니다.

	<unload target="file.js" />

XE 1.4.4 미만 버전에서는 아래와 같은 문법도 사용하였으나, 더이상 권장하지 않습니다.

	<!--%unload("file.js")-->

### PHP 코드 사용

아래와 같이 `{@ ... }` 문법을 사용하여 임의의 PHP 코드를 삽입할 수 있습니다.

	{@ $foo = 'bar'}

중괄호를 사용하지만 않는다면 여러 줄의 코드를 한 번에 입력해도 무방합니다.

	{@
		$foo = 'bar';
		if ($baz):
			$rhymix = true;
		endif;
	}

위와 같은 PHP 코드에서 사용하는 변수들도 모두 `Context`를 참조합니다.
만약 `Context`를 참조하지 않는 PHP 코드가 필요하다면 일반 `<?php ... ?>` 태그를 사용하십시오.
단, 템플릿 문법 v1 특성상 중괄호 사용에 항상 주의하여야 합니다.

### 주석

일반적인 HTML 주석은 결과물에 그대로 출력됩니다.
결과물에 포함되지 않는 주석을 작성하려면 아래와 같이 `<!--//`로 시작하도록 하면 됩니다.
이러한 주석은 템플릿 해석 과정에서 삭제됩니다.

    <!--// 이 주석은 출력되지 않습니다 -->

### 부가 기능

#### 자동 escape 설정

`<config>` 태그를 사용하여 이후에 등장하는 모든 데이터 출력 코드(중괄호)에
`htmlspecialchars()` 함수를 자동 적용하도록 설정할 수 있습니다.

    <config autoescape="on" />

파일 최상단에 이 태그를 쓰면 모든 데이터 출력 코드에 일괄 적용되므로,
일일이 `autoescape` 필터를 붙여주어야 하는 불편을 덜 수 있습니다.

단, 이 때 적용되는 필터는 이중 인코딩을 하지 않는 점을 참고하시기 바랍니다.
이중 인코딩이 필요한 경우, 개별적으로 `escape` 필터를 사용해야 합니다.

#### 리소스 경로 자동 조정

템플릿 소스에 포함된 이미지, 동영상 등의 상대경로는 해석 과정에서 절대경로로 변환되어,
실제 페이지가 표시되는 다양한 URL에서 항상 정확한 경로를 가리키게 됩니다.

예를 들어 `modules/board/skins/example/list.html`에서 아래와 같은 코드를 사용하였다면

    <img src="img/icon.png" alt="아이콘" />

해석 및 출력 과정에서 아래와 같이 변환됩니다.

    <img src="/modules/board/skins/example/img/icon.png" alt="아이콘" />

따라서 레이아웃이나 스킨 제작자는 사용자가 그 자료를 정확히 어떤 경로에 설치할지 신경쓰지 않고,
자료 내부의 디렉토리 구조만 감안하여 코드를 작성하면 됩니다.

이 기능은 대부분의 `src` 속성에 적용되며, 라이믹스 2.0.3 이후에는 `srcset` 속성에도 적용됩니다.

#### 폼 필드 자동 주입

템플릿 소스에 `<form>` 태그가 포함된 경우, 흔히 사용하는 필드들을 자동으로 추가해 주는 기능입니다.
XE 1.x 시절부터 제공되어 온 기능이므로 계속 제공하고 있으나, 여기에 의존하는 것은 권장하지 않습니다.
폼 제출에 필요한 필드가 누락되지 않았는지 체크하는 것은 제작자의 몫입니다.

폼에 `ruleset` 속성이 존재하는 경우, 이를 해석하여 해당 룰셋을 로딩합니다.

```
[BEFORE]
<form method="post" ruleset="insertFoo">
    ...
</form>
```

```
[AFTER]
<form method="post">
    <input type="hidden" name="ruleset" value="insertFoo" />
	....
</form>
```

폼에 `mid`와 `act` 필드가 존재하지 않는 경우, 현재 화면의 mid와 act를 기준으로 hidden input을 자동 추가합니다.

```
[BEFORE]
<form method="post">
    ...
</form>
```

```
[AFTER]
<form method="post">
	<input type="hidden" name="mid" value="{$mid}" />
	<input type="hidden" name="act" value="{$act}" />
	...
</form>
```

폼에 `error_return_url` 필드가 존재하지 않는 경우, 현재 화면의 주소를 기준으로 hidden input을 자동 추가합니다.

```
[BEFORE]
<form method="post">
    ...
</form>
```

```
[AFTER]
<form method="post">
	<input type="hidden" name="error_return_url" value="{getRequestUriByServerEnviroment()}" />
	...
</form>
```

GET 요청인 경우에는 이것 때문에 원하지 않는 파라미터가 덕지덕지 붙어서 URL이 더러워지는 경향이 있기 때문에,
특정한 속성을 추가하여 이 기능을 끄는 것을 지원합니다.

- `no-error-return-url="true"` : `error_return_url` 필드를 주입하지 않습니다.
- `rx-autoform="false"` : 폼 필드를 자동으로 주입하는 모든 기능을 끕니다. (라이믹스 2.0.15 이상)

예를 들어 아래와 같은 폼은 일체의 추가 필드를 주입하지 않고 그대로 사용하므로,
폼 제출시 `/search?q=검색어`라는 깔끔한 URL로 연결됩니다.

```
<form action="/search" method="get" rx-autoform="false">
    <input type="search" name="q" value="" />
	<button type="submit">{$lang->cmd_search}</button>
</form>
```
