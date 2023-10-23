템플릿 문법 v2
------------

라이믹스 2.1.8부터 프리뷰 형식으로 제공되고, 라이믹스 2.2부터 정식 지원할 템플릿 문법입니다.

Laravel Blade 문법을 기반으로, 라이믹스의 구조와 기능에 맞게 여러 지시자(directive)가 추가되었습니다.
또한 기존의 v1 문법 중 안정성과 편리함이 검증된 요소들도 상당수 그대로 유지하여,
필요에 따라 자유롭게 섞어쓸 수 있습니다.

확장자는 `.html`과 `.blade.php` 중 자유롭게 선택할 수 있습니다.
후자를 선택할 경우, VSCode, PHPStorm, IntelliJ 등 대부분의 에디터(IDE)에서
Blade 플러그인을 설치하여 문법 하이라이팅과 자동 완성의 혜택을 볼 수 있습니다.

### v1 문법과 다른 점

#### Blade 문법과 v1 호환 문법 병용

템플릿 문법 v2는 Blade 10.x와 호환되도록 하는 것을 목표로 자체 구현된 템플릿 컴파일러를 사용하며,
프리뷰 공개 시점 현재 Blade 10.x의 문법과 기능 중 [일부](#미지원_기능)를 제외하고 대부분 지원합니다.
동시에 v1 문법처럼 HTML 태그나 주석을 사용하는 조건문과 루프문의 형태도 지원합니다.
두 가지 문법은 서로 충돌하지 않으므로, 하나의 템플릿 엔진에서 동시에 사용할 수 있습니다.

예를 들어 Blade의 if문 좌우에 `<!--` `-->` 주석을 붙이거나,
데이터를 출력하는 `{{ ... }}` 문법에서 중괄호를 하나씩만 쓰면
대부분의 v1 템플릿에서 익숙하게 써온 문법이 되는 것을 볼 수 있습니다.

```
[v2 정규 문법]
@if ($condition)
	<div class="comment">{{ $comment }}</div>
@endif
```

```
[v1 호환 문법]
<!--@if($condition)-->
	<div class="comment">{$comment}</div>
<!--@end-->
```

아래의 문서에서는 Blade 스타일의 문법을 "v2 정규 문법"으로,
v1과 호환되거나 유사한 문법을 "v1 호환 문법"으로 소개합니다.

#### 문맥에 맞는 자동 이스케이프

템플릿에서 출력하는 모든 데이터는 자동으로 escape 처리되고, 이 기능을 일괄적으로 해제하는 옵션은 제공하지 않습니다.
꼭 필요한 곳에서만 `noescape` 필터나 `{!! $var !!}` 문법을 사용하여 개별적으로 해제할 수 있습니다.

단, 이미 escape된 데이터를 이중 인코딩하지는 않습니다.

`<script>` 태그 안에서 자바스크립트 변수에 데이터를 출력할 때는 자동으로 문맥을 인식하여,
HTML이 아닌 JS 문법에 맞는 형태로 escape합니다.
예를 들어 `<`는 HTML 문맥에서는 `&lt;`로 변환하고, JS 문맥에서는 `\u003C`로 변환합니다.
HTML 문맥에서 JSON을 출력할 때는 모든 `"`를 `&quot;`로 변환하지만, JS 문맥에서는 그대로 출력합니다.

#### 항상 일관성있는 Context 참조

템플릿 v1에서는 `<?php ?>` 태그를 사용하여 PHP 코드를 직접 작성하면
`Context`를 참조하지 않는 로컬 변수가 생성되어,
`{@ ...}`와 같은 템플릿 문법에서 사용하는 변수들과 연동되지 않는 불편이 있었습니다.

템플릿 v2에서는 템플릿 파일에서 사용하는 모든 변수가 항상 일관성있게 `Context`를 참조합니다.
(아래에서 설명하는 예외가 있으나, 예외 역시 항상 일관성있게 적용됩니다.)

#### &lt;block&gt;, loop, cond 속성 미지원

XE 1.4.4 버전 이후 수많은 템플릿 해석 오류의 원인이 되어 온
`<block>` 가상 태그, `loop` 속성, `cond` 속성을 지원하지 않습니다.
더이상 임의의 태그에서 이러한 속성을 사용할 수 없고,
사용시 출력물에 소스가 그대로 남거나 오류가 발생할 수 있습니다.

`<include>`에서는 `cond` 속성을 지원하지만,
Blade 문법과 유사한 `if`, `when`, `unless`로 변경할 것을 권장합니다.

하나의 속성을 표시할지 숨길지 결정하는 `|cond=""` 문법은 계속 지원하지만,
불가피한 경우가 아니라면 더욱 강력한 Blade 스타일의
`@class`, `@style`, `@selected` 등의 지시자를 활용할 것을 권장합니다.

#### 강력한 부가 기능

네임스페이스를 포함하거나 아주 긴 클래스명을 자주 참조할 경우,
alias 처리하여 간결하게 쓸 수 있습니다.

템플릿 파일을 인클루드할 때 변수 목록을 함께 넘기면 일반 인클루드가 아닌 컴포넌트로 취급하여,
`Context`의 전역변수를 공유하지 않고 해당 변수들만 사용하여 렌더링하도록 할 수 있습니다.

코어에서 통합 관리하는 리소스 로딩 기능, 상대경로 자동 변환, 출력 필터 등
v1의 핵심 기능들도 대부분 그대로 가져왔습니다.

Laravel Blade가 제공하는 `@auth`, `@can` 등의 지시자를 라이믹스의 권한 체계에 맞게 재해석하여,
게시판 관리자인지, 댓글 쓰기 권한이 있는지 등을 손쉽게 체크할 수 있습니다.

부가 기능은 이 매뉴얼 하단에서 더 자세히 설명합니다.

### 버전 표기 방법

확장자가 `.blade.php`인 템플릿 파일은 모두 v2로 인식합니다.

확장자가 `.html`인 파일에서 v2 문법 사용을 원한다면 최상단에 버전을 표기해야 합니다.
버전 표기 방법은 2가지가 있습니다.

```
[v2 정규 문법]
@version(2)
```

```
[v1 호환 문법]
<config version="2" />
```

### 데이터 출력

변수나 그 밖의 PHP 표현식을 출력할 때는 두 쌍의 중괄호를 사용하는 것이 Blade 문법의 표준입니다.

	[v2 정규 문법]
	{{ $var }}
	{{ $oDocument->getNickName() }}

단, v1처럼 중괄호를 하나만 쓰는 것도 허용합니다. 이 때, 중괄호 바로 안쪽에 공백이 있어서는 안 됩니다.

	[v1 호환 문법]
	{$var}
	{$oDocument->getNickName()}

중괄호를 하나만 쓸 때는 클로져 등 중괄호가 포함되는 코드를 사용할 수 없고,
중괄호 안에서 줄바꿈을 할 수 없다는 제한이 있습니다.
그러나 일반적인 변수를 출력할 때는 특별한 차이가 없으므로, 두 가지 문법 중 어느 한 쪽을 더 권장하지는 않습니다.

#### 변수의 유효 범위(스코프)

v1을 염두에 두고 만들어진 많은 자료들과 호환성을 유지하기 위해,
v2 템플릿에서 사용하는 변수들도 모두 `Context`와 연동됩니다.

모듈이나 위젯 등에서 `Context::set('foo', $value)`로 셋팅하였거나
`$_GET['foo']`로 들어온 URL 파라미터의 값은 템플릿에서 `$foo` 변수로 참조할 수 있고,
반대로 템플릿에서 `$foo`라는 변수를 조작하면 다른 자료에서 `Context::get('foo')`로 변경된 값을 불러올 수 있습니다.
즉, `Context`를 공용 state 저장소로 사용합니다.

단, 아래와 같은 변수는 `Context`와 연동되지 않습니다.

- 변수와 함께 인클루드한 템플릿 파일 내에서 사용하는 변수 ([인클루드시 변수 전달](#인클루드시_변수_전달) 참고)
- `$_SERVER`, `$_SESSION`, `$GLOBALS` 등의 초전역변수(superglobals)
- `Template` 클래스의 인스턴스를 가리키는 `$this`
- 변수 앞에 `\`를 붙여 `\$foo`와 같이 표기하면 로컬 변수가 됩니다.
  클로져를 선언할 때도 이 방법을 사용해야 합니다.

아래의 예시에서 `$list`는 `Context`에 소속된 변수이고, `\$i`는 클로져 내부에서 사용하는 로컬 변수입니다.
```
[클로져 사용 예시]
{{ implode(', ', array_map(function(\$i) { return \$i * 1000; }, $list)) }}
```

> [주의]
> 템플릿 컴파일러가 내부 데이터 처리를 위해 사용하는 로컬 변수는 `$__tmp` 등 2개의 언더바로 시작하는 것이 많습니다.
> 템플릿 내에서 로컬 변수 사용시 컴파일러 내부 변수와 충돌하지 않도록, 2개의 언더바로 시작하는 이름은 피하시기 바랍니다.

#### 출력 필터

v1과 동일한 출력 필터를 지원합니다.
v1과 다른 점은 필터를 구분하는 `|` 문자 좌우에 공백을 허용한다는 것입니다.

아래의 예제는 `$var`를 소문자로 변환하고 escape하여 출력합니다.

	{{ $var|lower|escape }}

아래의 예제는 타임스탬프를 특정한 형태로 포맷하여 출력합니다. 필터명과 옵션은 `:`으로 구분합니다.

	{$timestamp|date:'n/j H:i'}

필터 이외의 의미로 `|` 문자를 사용할 경우, 잘못 해석되지 않도록 주의해야 합니다.
v2 템플릿 문법은 아래와 같이 3가지 예외를 허용합니다.

- 2개를 붙여 OR의 의미를 갖는 `||` 연산자를 사용할 경우
- 문자열 안에 단독으로 넣어 `'|'`로 표기할 경우 (예: `join` 필터의 구분자로 사용할 경우)
- `\|`로 이스케이프할 경우

v2에서 지원하는 모든 필터의 목록은 아래의 표를 참고하십시오.

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

#### 조건문

Blade 스타일의 조건문과 루프문은
PHP의 [alternative syntax for control structures](https://www.php.net/manual/en/control-structures.alternative-syntax.php)를 기반으로, 앞에 `@` 문자를 붙여 구분하는 것이 원칙입니다.
이것은 XE와 라이믹스가 그동안 써온 템플릿 문법 v1과 매우 흡사한데,
v1의 조건문과 루프문 문법에서 좌우의 주석을 지우면 Blade 스타일의 지시자(directive)와 동일한 형태가 되기 때문입니다.

이러한 유사점 덕분에, v1처럼 주석을 사용하더라도 정상 인식하도록 되어 있습니다.
즉, 좌우의 주석을 삭제한 후 동일하게 처리합니다.
`@if`와 괄호 사이에 띄어쓰기를 하지 않더라도 무방하고,
`@endif`를 `@end`로 축약해서 써도 문제가 없습니다.

```
[v2 정규 문법]
@if ($condition1)
    <div class="foo"></div>
@elseif ($condition2)
	<div class="bar"></div>
@else
	<div class="baz"></div>
@endif
```

```
[v1 호환 문법]
<!--@if($condition1)-->
    <div class="foo"></div>
<!--@elseif($condition2)-->
	<div class="bar"></div>
<!--@else-->
	<div class="baz"></div>
<!--@end-->
```

> [주의]
> 괄호를 사용하는 Blade 스타일의 모든 지시자는 정규식(PCRE)을 사용하여 해석됩니다.
> 여는 괄호와 짝이 맞는 괄호를 찾는 것이 이 정규식의 핵심 기능으로,
> 복잡한 자료 구조를 즉석에서 선언하거나 문자열 안에 괄호를 넣는 등 해석을 방해하면
> v1 템플릿의 `loop`, `cond` 속성과 마찬가지로 오작동할 수 있습니다.
> 이것은 Laravel Blade 공식 문서에서도 경고하고 있는 부분입니다.
> 복잡한 자료 구조는 별도로 선언한 후 변수만 넘기는 것을 권장합니다.

#### foreach 루프

동일한 규칙을 따르며, v1 호환 문법도 마찬가지입니다.

```
[v2 정규 문법]
@foreach ($array as $key => $val)
	<div class="item">{{ $val->name }}</div>
@endforeach
```

v1에서 사용하던 `<block loop="...">` 문법은 지원하지 않습니다.

#### forelse 루프

Blade에서 도입한 문법으로, if문과 foreach문을 결합한 형태를 띱니다.
배열이 비어 있는 경우 표시할 내용을 쉽게 지정할 수 있습니다.
중간에 사용하는 지시자는 `@else`가 아니라 `@empty`임에 유의하십시오.

위와 마찬가지로, 이것도 좌우에 주석을 붙여 v1 문법처럼 사용할 수 있습니다.
`@endforelse`도 `@end`로 축약할 수 있으나,
얼핏 보았을 때 다른 루프문과 혼동할 가능성이 있으므로 축약하여 쓰는 것을 권장하지 않습니다.

```
@forelse ($array as $key => $val)
	<div class="item">{{ $val->name }}</div>
@empty
	<div class="noitem">항목이 없습니다.</div>
@endforelse
```

#### 그 밖의 주요 루프문

for문, while문, switch문 등 PHP에서 지원하는 루프문이라면 모두 사용할 수 있고,
Blade와 마찬가지로 `@continue`, `@break`, `@default`를 지원하므로
switch문처럼 복잡한 구조의 루프도 PHP를 사용하지 않고 템플릿 문법만으로 구현할 수 있습니다.

단, 복잡한 루프는 PHP 코드를 직접 사용하는 것이 더 효율적인 경우도 있으니
적절히 활용하시기 바랍니다.

```
@for ($i = 0; $i < 10; $i++)
	@if ($condition)
		@continue
	@endif
	<div class="count">{{ $i }}</div>
@endfor
```

```
@while (true)
	<div class="forever"></div>
	@if ($condition)
		@break
	@endif
@endfor
```

```
@switch ($color)
	@case ('red')
		<div class="red"></div>
		@break
	@case ('blue')
		<div class="blue"></div>
		@break
	@default
		<div class="default"></div>
@endswitch
```

#### 편의를 위한 조건문

Blade 10.x에서 지원하는 조건문은 `@hasSection`을 제외하고 모두 지원합니다.
`@auth`, `@can` 등 일부는 라이믹스의 권한 관리 체계에 맞추어 재해석하였습니다.

라이믹스에서 유용하게 쓸 수 있는 조건문도 몇 가지 추가되었습니다.
내부적으로는 모두 if문으로 컴파일되므로 필요시 `@else`나 `@elseif`를 끼워 사용할 수 있고,
마치는 지시자는 모두 `@end`로 축약할 수 있습니다.

```
@isset ($foo)
    Context에 $foo 변수가 존재합니다.
@endisset
```

```
@unset ($foo)
    Context에 $foo 변수가 존재하지 않습니다.
@endunset
```

```
@empty ($foo)
    Context에 $foo 변수가 존재하지 않거나, 빈 값입니다.
@endempty
```

```
@env ('foo')
    foo라는 환경변수($_ENV['foo'])가 존재합니다.
@endenv
```

```
@admin
    최고관리자입니다.
@endadmin
```

```
@auth('admin')
    최고관리자입니다.
@endauth
```

```
@auth('manager')
    게시판 관리자입니다.
@endauth
```

```
@auth
    로그인한 회원입니다.
@endauth
```

```
@guest
    로그인하지 않았습니다.
@endguest
```

```
@can ('view')
    글읽기 권한이 있습니다. ($grant->view가 있습니다.)
@endcan
```

```
@cannot ('view')
    글읽기 권한이 없습니다. ($grant->view가 없습니다.)
@endcannot
```

```
@canany (['view', 'write_comment'])
    글읽기, 댓글쓰기 중 최소 1개의 권한이 있습니다.
@endcanany
```

```
@desktop
    PC입니다.
@enddesktop
```

```
@mobile
    모바일입니다.
@endmobile
```

### 템플릿 인클루드

다른 템플릿 파일을 인클루드하려면 `@include` 지시자를 사용합니다.
경로는 현재 템플릿 파일을 기준으로 하는 상대경로입니다.

	[v2 정규 문법]
	@include ('dir/filename')

단, 많은 디렉토리를 거슬러 올라가야 하는 경우 `^` 문자를 사용하여
라이믹스 설치 디렉토리를 기준으로 하는 경로를 작성할 수 있습니다.
이것은 CSS, JS 등의 리소스를 로딩할 때도 동일합니다.

	[v2 정규 문법]
	@include ('^/common/tpl/default_layout')

v1처럼 태그를 사용하는 문법도 허용하며, 이 경우 `src` 속성 대신 `target`을 써도 정상 인식합니다.
(HTML에서 널리 사용하는 속성에 다른 의미를 부여하지 말자는 취지로,
`target`은 더이상 권장하지 않으나 지원을 중단하지는 않았습니다.)

	[v1 호환 문법]
	<include src="dir/filename" />
	<include target="dir/filename" />

#### 조건부 인클루드

특정 조건이 충족될 때에만 인클루드하도록 할 수 있습니다.

- `@includeIf` : 파일이 존재하는 경우에만 인클루드하고, 존재하지 않더라도 오류를 표시하지 않습니다.
- `@includeWhen` : 첫 번째 파라미터로 전달한 조건이 참인 경우에만 인클루드합니다.
- `@includeUnless` : 첫 번째 파라미터로 전달한 조건이 참인 경우에만 인클루드합니다.

```
[v2 정규 문법]
@includeIf ('../dir/filename)
@includeWhen ($condition, '../dir/filename')
@includeUnless ($condition, '../dir/filename')
```

그 중 2가지 조건은 v1 호환 문법으로도 구현할 수 있습니다. v1 문법의 `cond`에 해당합니다.
편의를 위해 `when` 속성을 `cond`로 쓸 수도 있고, `if`로 쓸 수도 있습니다.
단, 파일이 존재하는 경우에만 인클루드한다는 조건은 지원하지 않습니다.

```
[v1 호환 문법]
<include src="../dir/filename" when="$condition">
<include src="../dir/filename" unless="$condition">
```

#### 인클루드시 변수 전달

다른 템플릿을 인클루드할 때 연관배열이나 오브젝트를 전달하면,
인클루드된 템플릿은 `Context`를 참조하지 않고 전달받은 데이터만을 사용하게 됩니다.

예를 들어 아래와 같이 A 파일이 B 파일을 인클루드했을 때, B 파일 내의 `$title`과 `$content`는
일반적인 템플릿에서와 달리 `Context`를 참조하지 않습니다.
전달받은 연관배열의 해당 키를 참조하며, 만약 존재하지 않는 키를 참조할 경우 경고가 발생합니다.

이 방법을 사용하여 외부의 다른 변수에 영향을 받지 않는 독립적인 컴포넌트를 구현할 수 있고,
호출하는 쪽에서는 해당 컴포넌트 내에서 사용하는 모든 변수를 통제할 수 있게 됩니다.

```
[A.blade.php]

@include ('B', ['title' => '제목', 'content' => '내용'])
```

```
[B.blade.php]

<article>
	<h1>{{ $title }}</h1>
	<div class="content">
		{{ $content|noescape }}
	</div>
</article>
```

변수를 전달하여 인클루드된 템플릿에서 또다른 템플릿을 인클루드할 경우,
자식 템플릿은 부모 템플릿의 변수들을 상속받습니다.
부모 템플릿과 마찬가지로, 자식 템플릿도 `Context`를 참조하지 않습니다.

만약 인클루드할 때 또다시 변수를 전달한다면,
자식 템플릿은 부모 템플릿으로부터 상속받은 변수들과 직접 전달받은 변수들의 합집합을 갖게 되며,
같은 이름의 변수가 있다면 직접 전달받은 변수가 우선합니다.

단, 인클루드된 템플릿에서 `Context::get()` 등의 메소드를 사용하여
자신이 직접 전달받지 않은 외부의 데이터에 접근하는 것을 막지는 않습니다.
그러한 시도를 할 경우 눈에 잘 띄게 될 뿐입니다.

v1 호환 문법에서도 `vars` 속성을 사용하여 변수를 전달할 수 있고, 조건부 인클루드와 함께 쓸 수도 있습니다.

```
[v1 호환 문법]
<include target="filename" vars="$vars" />
<include src="filename" if="$vars" vars="$vars" />
```

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

CSS 리소스를 로딩하려면 `@load` 지시자를 사용합니다.
로딩할 파일의 경로는 인클루드와 마찬가지로 현재 파일을 기준으로 한 상대경로를 사용하는 것이 원칙이지만,
필요시 `^` 문자로 라이믹스가 설치된 루트 경로를 참조할 수 있습니다.

	[v2 정규 문법]
	@load ('styles.css')
	@load ('styles.css', 'print', 20)

SCSS나 LESS도 동일한 문법으로 로딩합니다. 코어에서 자동으로 컴파일하고, 변수를 전달할 수도 있습니다.

	[v2 정규 문법]
	@load ('../styles.less')
	@load ('css/styles.scss', $vars)

파라미터의 순서는 파일명, media, 로딩 순서, 변수 순이며, media와 로딩 순서는 생략할 수 있습니다.
단, media와 로딩 순서는 각각 문자열과 정수로 전달해야 하며 변수를 사용할 수 없습니다.

v1 호환 문법을 사용하면 각 파라미터의 역할을 더 분명하게 표현할 수 있습니다.
(`src` 속성은 `target`으로 써도 정상 인식합니다.)
표현상의 분명한 이점이 있으므로, 이러한 문법의 사용을 굳이 피해야 할 필요는 없습니다.

	[v1 호환 문법]
	<load src="^/modules/foo/bar/styles.scss" media="screen" vars="$vars" />
	<load target="../../../styles.css" index="-3" />

#### JS

CSS와 동일한 문법으로 로딩합니다. 단, `media` 대신 `type`을 전달하고, 변수를 전달하는 기능은 없습니다.
`type`은 스크립트를 실행할 위치를 의미하는데, 기본값은 `head`입니다.

- `head` : HTML 소스의 `<head>` 부분에 삽입되어, 본문 로딩 전 실행됩니다.
- `body` : HTML 소스의 `<body>` 하단에 삽입되어, 본문 로딩 후 실행됩니다.

```
[v2 정규 문법]
@load ('script.js')
@load ('dir/script.js', 'body')
@load ('^/common/js/script.js", 'body', 10)
```

v1 호환 문법도 사용할 수 있습니다.

	[v1 호환 문법]
	<load src="script.js" type="body" index="10">
	<load target="../../foo/script.js" />

#### JS 플러그인

CKEditor 등 `common/js/plugins/` 폴더에 있는 라이브러리를 로딩합니다.
JS를 로딩하는 방법과 동일하며, 라이믹스 설치 위치를 기준으로 하는 경로만 정확하게 지정해 주면 됩니다.
(plugin.load 파일이 있는 디렉토리까지만 적으면 됩니다.)

```
[v2 정규 문법]
@load ('^/common/js/plugins/ckeditor/')
```

```
[v1 호환 문법]
<load src="^/common/js/plugins/ckeditor/" />
```

#### 언어 파일

특정 모듈, 또는 특정한 폴더에 저장되어 있는 언어 파일을 로딩합니다.
언어 파일들이 저장되어 있는 디렉토리를 지정합니다.

```
[v2 정규 문법]
@load ('../lang/')
```

```
[v1 호환 문법]
<load src="../lang/" />
```

#### 기타

XML JS 필터 로딩은 지원하지 않습니다. 템플릿 문법 v2를 사용하여 제작하는 신규 자료에서는
룰셋(ruleset)이나 XML JS 필터를 사용하는 것을 권장하지 않습니다.

이미 로딩한 리소스를 언로딩하려면 `@unload` 지시자 또는 `<unload>` 태그를 사용합니다.

```
[v2 정규 문법]
@unload ('foo/bar.js')
```

```
[v1 호환 문법]
<unload src="foo/bar.js" />
```

### HTML 속성 작성 도우미

#### class 목록 작성

조건에 따라 HTML 태그에 각각 다른 `class` 목록을 나열해야 하는 일이 자주 있습니다.
기존의 템플릿에서는 `class` 속성 중간에 if문을 끼워넣거나,
다양한 경우의 수에 해당하는 여러 속성값을 미리 준비해 두는 등의 불편이 있었습니다.

Blade 스타일의 템플릿 문법은 `@class` 지시자를 사용하여 `class` 목록을 손쉽게 작성하는 방법을 제공합니다.

	<div @class([
		'project-item',
		'project-complete' => $is_complete,
		'featured' => $is_featured,
		'awarded' => AwardModel::isAwarded($project),
	])></div>

`@class` 지시자에 하나의 배열을 전달합니다.
이 배열에는 `class`명이 단순한 값으로 들어 있을 수도 있고, 키/값 형태로 들어 있을 수도 있습니다.
위 예시의 `project-item`처럼 단순한 값으로 들어 있는 `class`명은 그대로 출력됩니다.
반면, 키/값 형태로 들어 있는 것은 값이 참인 경우에만 출력됩니다.
참인지 거짓인지는 배열에 넣은 변수나 함수의 값에 따라 판단하므로, 어떤 로직이라도 적용할 수 있습니다.

만약 위의 코드에서 `$is_complete`와 `$is_featured`가 참이고, 마지막 조건이 거짓이라면
아래와 같은 결과가 출력될 것입니다.

	<div class="project-item project-complete featured"></div>

#### style 속성 작성

위와 비슷하게 `@style` 지시자를 사용하여 `style` 속성을 작성할 수 있습니다.

	<div @style([
		'background-color: white',
		'border: 2px' => $thickBorder,
		'border: 1px' => $thinBorder,
		'border-radius: 4px' => $isRound,
	])></div>

만약 위의 코드에서 `$thinBorder`만 참이라면, 아래와 같은 결과가 출력될 것입니다.

	<div style="background-color: white; border: 1px"></div>

단, 반드시 필요한 경우를 제외하면 스타일은 별도의 CSS 또는 SCSS 파일에서 작성하는 것을 권장합니다.

#### 불리언 속성 작성

`<input type="checkbox" checked>`에서 `checked`와 같은 속성을 불리언(boolean) 속성이라고 합니다.
일반적으로 값을 가지지 않고, 값을 가진다 해도 `checked="checked"`와 같이 이름을 반복할 뿐이며,
값과 관계없이 속성이 존재하는지 안 하는지, 0과 1만으로 상태를 구분하기 때문입니다.

템플릿 문법 v1에서는 `|cond=""` 문법을 사용하여 이런 속성들을 쉽게 켜고 끌 수 있었습니다.
템플릿 문법 v2에서도 같은 문법을 지원합니다.
`cond` 대신 `if`, `when`, `unless`를 사용하여 더 간결하게 표현하거나 조건을 뒤집을 수도 있습니다.

	[v1 호환 문법]
	<input type="checkbox" checked|cond="$is_checked">
	<input type="text" disabled="disabled"|cond="$is_disabled">
	<select>
		<option value="1" selected|cond="$val == 1">ONE</option>
		<option value="2" selected|cond="$val == 2">ONE</option>
		<option value="3" selected|cond="$val == 3">ONE</option>
	</select>

그러나 더 간결한 Blade 스타일의 문법도 지원합니다.
사용법은 위에서 본 `@class`와 비슷하나,
불리언 문법에서는 참인지 거짓인지 판단할 수 있는 하나의 변수나 표현식만 전달하면 됩니다.

	[v2 정규 문법]
	<input type="checkbox" @checked($is_checked)>
	<input type="text" @disabled($is_disabled)>
	<select>
		<option value="1" @selected($val == 1)>ONE</option>
		<option value="2" @selected($val == 2)>ONE</option>
		<option value="3" @selected($val == 3)>ONE</option>
	</select>

비슷하게 활용할 수 있는 지시자로는 `@checked`, `@selected`, `@disabled`, `@readonly`, `@required`가 있습니다.
각각 이름과 같은 HTML 속성에 대응합니다.
모두 HTML 폼에서 입력란의 상태를 변경하는 데 흔히 사용하는 속성들입니다.

Blade 스타일의 문법이 지원하지 않는 속성에 조건을 걸어야 한다면,
위와 같은 v1 호환 문법이나 if문을 사용할 수 있습니다.

### PHP 코드 사용

라이믹스 템플릿 v2에서 PHP 코드를 직접 실행하는 방법은 3가지가 있습니다.

첫째는 Blade 방식으로, `@php` `@endphp` 지시자 사이에 PHP 코드를 작성하는 것입니다.

	[v2 정규 문법]
	@php
		Hello::world();
		$foo = 'bar';
	@endphp

둘째는 템플릿 문법 v1에서 사용하던 방식으로, `{@ ... }` 안에 PHP 코드를 작성하는 것입니다.
중괄호를 사용하는 문법의 특성상, 중괄호를 포함하는 코드는 작성할 수 없습니다.

	[v1 호환 문법]
	{@
		Hello::world();
		$foo = 'bar';
	}

셋째는 PHP 언어에서 기본 제공하는 `<?php ?>` 태그를 사용하는 것입니다.

	[PHP 기본 문법]
	<?php
		Hello::world();
		$foo = 'bar';
	?>

세 가지 방법은 100% 동일한 효과를 낳습니다.
템플릿 문법 v1과 달리, 어떤 방법을 사용하더라도 모든 변수는 `Context`를 참조합니다.
(인클루드시 변수를 전달하여 해당 변수만 사용하도록 강제한 경우 제외)

### 주석

일반적인 HTML 주석은 결과물에 그대로 출력됩니다.
결과물에 포함되지 않는 주석을 작성하려면 아래의 두 가지 문법 중 하나를 사용하면 됩니다.
이러한 주석은 템플릿 해석 과정에서 삭제됩니다.

```
[Blade 호환 문법]
{{-- 이 주석은 출력되지 않습니다 --}}
```

```
[v1 호환 문법]
<!--// 이 주석은 출력되지 않습니다 -->
```

### 부가 기능

#### 클래스명 줄여 쓰기

라이믹스 프레임워크와 주요 모듈들이 네임스페이스를 도입하면서,
해당 기능을 가져다가 쓰기 위해 호출해야 하는 클래스명이 점점 길어지고 있습니다.
네임스페이스를 사용하는 모듈이라면 `use` 구문을 사용하여 적절한 이름으로 alias 처리할 수 있지만,
모든 템플릿은 전역 스코프에서 실행되므로 이 방법을 사용할 수 없습니다.

그래서 라이믹스 템플릿 v2 문법은 클래스명을 alias 처리하는 방법을 제공합니다.

```
[v2 정규 문법]
@use('Rhymix\Framework\Filters\HTMLFilter', 'HTMLFilter')
@use('Rhymix\Framework\Mail', 'Mail')

<article>
	{!! HTMLFilter::clean($content) !!}
</article>

@php
	$mail = new Mail();
@endphp
```

이 기능은 v1 호환 문법 형태로도 제공됩니다. 단, v1에 없는 기능이므로 실제로 v1과 호환되지는 않습니다.

```
[v1 호환 문법]
<use class="Rhymix\Framework\Filters\HTMLFilter" as="HTMLFilter" />
<use class="Rhymix\Framework\Mail" as="Mail" />

<article>
	{HTMLFilter::clean($content)|noescape}
</article>

<?php
	$mail = new Mail();
?>
```

실제로 전역 스코프에 alias가 생성되는 것은 아닙니다.
템플릿 내에서 해당 명칭을 사용하는 부분을 일괄 치환하는 것뿐이므로,
유사한 이름을 여러 가지 사용하면 오작동할 수 있습니다.

#### 리소스 경로 자동 조정

템플릿 소스에 포함된 이미지, 동영상 등의 상대경로는 해석 과정에서 절대경로로 변환되어,
실제 페이지가 표시되는 다양한 URL에서 항상 정확한 경로를 가리키게 됩니다.

예를 들어 `modules/board/skins/example/list.blade.php`에서 아래와 같은 코드를 사용하였다면

    <img src="img/icon.png" alt="아이콘" />

해석 및 출력 과정에서 아래와 같이 변환됩니다.

    <img src="/modules/board/skins/example/img/icon.png" alt="아이콘" />

따라서 레이아웃이나 스킨 제작자는 사용자가 그 자료를 정확히 어떤 경로에 설치할지 신경쓰지 않고,
자료 내부의 디렉토리 구조만 감안하여 코드를 작성하면 됩니다.

이 기능은 대부분의 `src` 속성, `srcset` 속성, `poster` 속성에 적용됩니다.

#### 폼 필드 자동 주입

템플릿 문법 v2는 `<form>` 태그에 사용자가 작성하지 않은 필드를 임의로 주입하지 않습니다.
원하지 않는 필드 주입을 막기 위한 `rx-autoform` 및 `no-error-return-url` 속성도 필요하지 않습니다.

단, `@csrf` 지시자를 사용하여 CSRF 토큰을 포함하는 hidden input을 손쉽게 주입할 수 있습니다.

	<form action="{\RX_BASEURL}" method="post">
		@csrf
		...
	</form>

#### JSON 출력

`@json`을 사용하여 데이터를 JSON으로 출력할 수 있습니다.
`json` 출력 필터를 사용하는 것과 동일한 효과입니다.

HTML의 일부로 출력하는지, 자바스크립트 변수로 출력하는지 자동으로 판단하여
각각 문맥에 맞는 형태로 인코딩합니다.
예를 들어 HTML 속성에서는 `"`를 `&quot;`로 인코딩해야 하지만, JS 변수에서는 `"`를 그대로 둡니다.

```
[HTML 문맥]
<div class="foo" data-params="@json($params)"></div>
```

```
[JS 문맥]
<script>
	const params = @json($params);
</script>
```

#### 번역문 로딩

`@lang`을 사용하여 현재 사용중인 언어의 번역문을 쉽게 불러올 수 있습니다.
라이믹스의 `lang()` 전역 함수와 동일한 기능으로, 다른 모듈의 언어 파일도 불러올 수 있기 때문에
v1 템플릿에서 자주 보이는 `{$lang->foo}`보다 강력합니다.

일반적인 방법으로 사용할 경우, 해당 메시지를 선언한 모듈이 아직 로딩되지 않았다면 번역이 되지 않습니다.

	@lang('msg_file_not_found')

그러나 아래와 같이 모듈명을 지정하여 사용하면, file 모듈이 로딩되지 않았더라도 강제 로딩 후 번역할 수 있습니다.
특정 경로의 언어 파일을 별도로 불러오는 함수나 지시자를 사용할 필요가 없는 것입니다.

	@lang('file.msg_file_not_found')

#### URL 생성

`@url`을 사용하면 `getUrl()`류의 함수를 쉽게 호출할 수 있습니다.
문맥에 맞게 인코딩된 결과를 출력합니다.

신규 자료에서는 연관배열 형태를 사용할 것을 권장합니다.

	@url('')
	@url('act', 'dispMemberInfo')
	@url('', 'mid', $mid, 'act', 'dispBoardWrite')
	@url(['mid' => $mid, 'act' => 'dispBoardWrite'])

#### 기타 유용한 지시자

##### @dump, @dd

`@dump`를 사용하면 1개 또는 여러 개의 변수를 `var_dump()`합니다. 흔히 "변수를 찍어본다"라고 하는 디버깅 작업입니다.

	@dump($var);
	@dump($foo, $bar)

`@dd`를 사용하면 변수를 `var_dump()`한 후 강제종료합니다. ("Dump and Die"의 줄임말이라고 합니다.)

	@dd($var)
	@dd($foo, $bar)

##### @once ... @endonce

`@once ... @endonce`에 둘러싸인 템플릿 코드는 루프나 인클루드로 인해 여러 번 호출되더라도 1번만 실행됩니다.
특정한 라이브러리를 로딩하는 코드 등, 여러 번 실행되어서는 곤란한 경우에 사용할 수 있습니다.

	@once
		... 외부 라이브러리 로딩 또는 초기화 작업 ...
	@endonce

##### @error ... @enderror

특정한 이름의 validation 에러가 있는 경우를 의미하는 조건문입니다.
필요시 `@else`나 `@elseif`를 끼워넣을 수 있습니다.

	[v2 정규 문법]
	@error ('modules/foo/tpl/1')
		에러가 있습니다.
	@enderror

위의 코드는 흔히 사용하는 아래의 코드와 동일한 기능입니다.

	[v1 코드 예시]
	<!--@if($XE_VALIDATOR_ID == 'modules/foo/tpl/1' && $XE_VALIDATOR_MESSAGE)-->
		에러가 있습니다.
	<!--@endif-->

실제 에러 메시지 내용은 여전히 `$XE_VALIDATOR_MESSAGE` 변수에 담겨 있습니다.

##### @verbatim ... @endverbatim

`@verbatim ... @endverbatim`에 둘러싸인 부분의 템플릿 코드는 해석하지 않습니다.

	@verbatim
		@if ($foo)
			{{ $bar }}
		@endif
	@endverbatim

위의 예시에서 조건문은 실행되지 않고, `$bar` 변수의 내용이 출력되지도 않습니다.
마치 이 매뉴얼처럼 템플릿 코드가 그대로 출력됩니다.
템플릿 코드 그 자체를 프론트엔드에 전달하려고 하는 경우 유용하게 사용할 수 있습니다.

##### @fragment ... @endfragment

`@fragment ... @endfragment`에 들러싸인 템플릿 코드는
템플릿을 컴파일하고 실행할 때는 별다른 특색 없이 그대로 출력되지만,
실행이 끝난 후 해당 부분만 별도로 추출할 수 있습니다.

```
@fragment ('foo')
	이 부분만 추출해 볼게요
@endfragment
@fragment ('bar')
	이 부분도 따로 가져올 수 있어요
@endfragment
```

```
$template = new Rhymix\Framework\Template('dir', 'filename');
$output = $template->compile();
$foo = $template->getFragment('foo')
$bar = $template->getFragment('bar')
```

##### @push, @prepend, @stack

`@push`와 `@prepend`는 `@fragment`처럼 출력물의 일부분을 추출하는 데 사용하지만,
하나의 이름으로 하나의 내용만 저장할 수 있는 `@fragment`와 달리,
같은 이름으로 추출한 내용을 계속 덧붙일 수 있습니다.

	@push ('foo')
		이 내용을 추가할게요.^^
	@endpush
	@push ('foo')
		이 내용도 추가하고 싶어요~
	@endpush
	@push ('foo')
		이 내용도 있어요!
	@endpush

이렇게 여러 차례 내용을 추가한 후, `@stack`을 사용하여 가져올 수 있습니다.

	@stack ('foo')

이렇게 하면 지금까지 쌓인 내용이 모두 출력됩니다.

	이 내용을 추가할게요.^^
	이 내용도 추가하고 싶어요~
	이 내용도 있어요!

`@prepend`도 비슷한 기능이지만, `@push`와 달리 내용을 앞에 끼워넣습니다.
`@prepend`로 추가한 내용은 `@stack`을 호출했을 때 역순으로 나옵니다.
같은 이름으로 `@push`와 `@prepend`를 번갈아 사용하여
어떤 것은 뒤에, 어떤 것은 앞에 끼워넣도록 할 수도 있습니다.

`@pushIf`와 `@prependIf`는 조건부로 내용을 추출합니다.

	@pushIf ($condition, 'foo')
		조건이 참이어야 이 내용이 추가됩니다.
	@endPushIf
	@prependIf ($condition, 'foo')
		조건이 참이어야 이 내용을 앞에 끼워넣습니다.
	@endPrependIf

`@pushOnce`와 `@prependOnce`는 이미 추가한 내용을 중복으로 추가하지 않습니다.

Laravel에서는 CSS, JS 로딩하는 코드를 `@push`로 추가하고,
마지막에 레이아웃에서 `@stack`으로 한 번에 불러오는 방법을 추천합니다.
그러나 라이믹스에서는 CSS, JS 리소스를 관리하는 다른 방법이 있으므로,
이런 용도로 `@stack`을 사용할 필요는 없습니다.

### 미지원 기능

템플릿 문법 v2는 Blade 10.x의 문법과 지시자를 대부분 그대로 따르고,
라이믹스에 필요한 지시자와 v1 호환 문법을 추가했습니다.

단, Laravel Blade를 그대로 사용하는 것은 아니라는 점에 유의해야 합니다.
PHP 지원 범위 문제, 서로 다른 출처의 여러 모듈과 스킨 등을 조합하여 사용하는 라이믹스의 특성,
`Context` 변수 참조 문제, 일부 v1 문법 유지 등의 이유로
Laravel 프레임워크의 코드를 그대로 사용하기에는 어려움이 있어,
"Blade 스타일"의 문법만 차용하고 실제 컴파일러는 자체 구현했습니다.
따라서 세부적인 해석 방식에서 Laravel Blade와 차이가 있을 수 있습니다.

특히 Blade 10.x의 기능 중 템플릿 상속 및 컴포넌트화와 관련된 지시자는 적용되지 않았습니다.
`@extends`, `@yield`, `@section`, `@show`, `@inject`, `@slot` 등이 여기에 해당됩니다.

하나의 개발팀이 하나의 코드베이스를 통합 관리한다고 가정하는 대다수의 웹 프레임워크와 달리,
라이믹스는 각각의 모듈과 스킨을 독립적으로 개발하고 독립적으로 배포할 수 있어야 합니다.
전문 개발자가 아닌 사용자나 자료 제작자가 많기 때문에,
단지 스킨에서 사용할 컴포넌트를 제작하기 위해 별도의 `.php` 파일에서 클래스를 선언해야 하거나
터미널에서 명령을 실행해야 하는 개발 방식은 받아들이기 곤란합니다.

이런 특성을 고려할 때 Laravel Blade와 같은 컴포넌트 설계는 라이믹스와 맞지 않다고 판단하였으며,
적절한 대안을 마련할 때까지 컴포넌트 관련 기능의 적용은 보류합니다.

단, [인클루드시 변수 전달](#인클루드시_변수_전달) 기능을 활용하여
상당히 독립적인 컴포넌트와 같은 템플릿을 구현할 수 있습니다.