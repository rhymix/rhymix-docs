테마 제작
--------

라이믹스로 테마(레이아웃, 모듈 스킨, 위젯 스킨)를 제작할 때는 2가지 버전의 템플릿 문법 중 하나를 선택할 수 있고,
각각의 버전 내에서도 다양한 표현 방법이 있으므로 원하는 스타일을 따를 수 있습니다.

라이믹스 2.2 이상을 대상으로 신규 제작하는 자료라면 템플릿 문법 v2를 권장합니다.
템플릿 문법 v1은 구 버전과 XE 1.x에 맞추어 제작된 자료와 bug-for-bug 호환성을 유지할 목적으로 계속 지원합니다.

### 템플릿 문법 v2

[자세히 보기](template_v2.md)

라이믹스 2.1.8부터 프리뷰 형식으로 제공되고, 라이믹스 2.2부터 정식 지원할 템플릿 문법입니다.

Laravel Blade 문법을 기반으로, 라이믹스의 구조와 기능에 맞게 확장하였습니다.
주요 로직을 최대한 짧고 간결하게 표현하는 것을 목표로 합니다.
출력하는 모든 데이터를 문맥에 따라 자동으로 escape하므로 보안성이 뛰어나고,
자주 사용하는 기능들을 쉽게 끌어쓸 수 있도록 `@lang`, `@url` 등 다양한 지시자(directive)를 제공합니다.

확장자는 `.html`과 `.blade.php` 중 자유롭게 선택할 수 있습니다.
후자를 선택할 경우, 대부분의 에디터(IDE)에서 Blade 문법 하이라이팅과 자동 완성의 혜택을 볼 수 있습니다.

	[v2 정규 문법 예시] [Blade]

	@include('header')
	@load('comment.scss', $vars)

	@foreach ($comments as $comment)
		@if ($comment->isAccessible())
			<div @class(['comment', 'secret' => $comment->isSecret()])>
				{{ $comment->getContent() }}
			</div>
		@endif
	@endforeach

	<script>
		const data = @json($data);
	</script>

마이그레이션 편의를 위해 템플릿 문법 v1에서 사용하던 HTML 태그나 주석 방식도 일부 지원합니다.
조건문 좌우에 주석을 붙이거나, 중괄호를 하나만 쓰거나, `@endif`를 `@end`로 축약하더라도 정상 인식합니다.
v1으로 작성된 대부분의 템플릿은 비교적 쉽게, 점진적으로 v2로 변환할 수 있습니다.
v1 템플릿에서 v2 템플릿을 인클루드하거나, v2에서 v1을 인클루드할 수도 있습니다.

	[v2 대체 문법 예시] [HTML]

	<!--@if ($comment->isAccessible())-->
		<div class="comment <!--@if($comment->isSecret())-->secret<!--@end-->">
			{$comment->getContent()}
		</div>
	<!--@end-->

### 템플릿 문법 v1

[자세히 보기](template_v1.md)

XE 1.x에서 사용하던 템플릿 문법으로, 대부분의 기능을 HTML 태그와 주석의 형태로 구현하려고 시도합니다.
라이믹스에서도 8년간 지속적으로 기능이 강화되어 왔으나,
v2 문법 정식 공개 후에는 더이상 기능이 추가되지 않을 예정입니다.

그러나 기존 자료에 사용할 경우 여전히 100% 지원하며, 지원을 종료할 계획도 없습니다.

	[v1 문법 예시] [HTML]
	
	<include target="header" />
	<load target="comment.scss" vars="$vars" />

	<block loop="$comments => $comment">
		<!--@if($comment->isAccessible())-->
			<div class="comment"|cond="!$comment->isSecretddddd()" class="comment secret"|cond="$comment->isSecret()">
				{$comment->getContent()}
			</div>
		<!--@end-->
	</block>
	
	<script>
		const data = {json_encode($data)};
	</script>

### 버전 구분 방법

확장자가 `.blade.php`인 파일은 모두 v2로 인식합니다.

확장자가 `.html`인 파일에서 v2 문법 사용을 원한다면 최상단에 버전을 표기해야 합니다.
버전 표기 방법은 2가지가 있습니다.

```
[v2 정규 문법]
@version(2)
```

```
[v2 대체 문법]
<config version="2" />
```

확장자가 `.html`인 파일에서 버전을 표기하지 않으면 v1 템플릿으로 취급합니다.