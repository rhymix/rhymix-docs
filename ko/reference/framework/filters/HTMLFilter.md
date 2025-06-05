Rhymix\Framework\Filters\HTMLFilter
-----------------------------------

이 클래스는 HTMLPurifier를 사용하여 HTML 콘텐츠를 필터링하는 기능을 제공합니다.
이를 통해 XSS 공격을 방지하고 HTML 태그를 표준에 맞도록 정리하여,
사이트의 보안을 강화하고 HTML 페이지의 태그 구조가 깨지는 것을 방지합니다.

#### prependPreFilter()

```
public static function prependPreFilter(callable $callback): void
```

실행 순서의 맨 앞에 프리 필터를 추가합니다.
프리 필터는 HTML 콘텐츠를 필터링하기 전에 실행되는 콜백 함수로,
HTMLPurifier 적용 전에 콘텐츠를 수정할 수 있습니다.

#### appendPreFilter()

```
public static function appendPreFilter(callable $callback): void
```

실행 순서의 맨 뒤에 프리 필터를 추가합니다.

#### prependPostFilter()

```
public static function prependPostFilter(callable $callback): void
```

실행 순서의 맨 앞에 포스트 필터를 추가합니다.
포스트 필터는 HTML 콘텐츠를 필터링한 후에 실행되는 콜백 함수로,
HTMLPurifier를 거쳐 나온 콘텐츠를 추가로 가공할 수 있습니다.

#### appendPostFilter()

```
public static function appendPostFilter(callable $callback): void
```

실행 순서의 맨 뒤에 포스트 필터를 추가합니다.

#### clean()

```
public static function clean(
    string $input,
    array|bool $allow_classes = false,
    bool $allow_editor_components = true,
    bool $allow_widgets = false
): string
```

HTML 콘텐츠를 필터링하여 XSS 공격을 방지하고,
HTML 태그를 표준에 맞게 정리합니다.

`$allow_classes` 매개변수에 배열을 넘기면 해당 클래스만 허용합니다.
`false`를 넘기면 시스템 설정에서 허용하는 클래스 목록을 참조하여 필터링합니다.
HTMLPurifier는 기본적으로 본문에 HTML 클래스를 허용하지 않습니다.
클래스를 사용해서 로그인 버튼을 공격자의 링크로 덮어쓰는 등, 다양한 공격이 가능하기 때문입니다.

`$allow_editor_components`가 `true`인 경우, 에디터 컴포넌트를 허용합니다.

`$allow_widgets`가 `true`인 경우, 라이믹스 위젯 코드를 허용합니다.
위젯 코드는 관리자만 삽입할 수 있으므로, 이 매개변수는 `false`가 기본값입니다.

#### fixRelativeUrls()

```
public static function fixRelativeUrls(string $content): string
```

본문에 포함된 상대 URL(사이트 내부 URL)을 절대 URL로 변환합니다.
이것은 이메일을 발송하거나 RSS 피드를 발행하는 등, HTML 콘텐츠를 사이트 외부로 전송할 때 유용합니다.

이 메소드는 사이트 외부로 전송했을 때 의미가 없는 속성(예: 에디터 컴포넌트 이름)도 제거합니다.

이 메소드는 HTML 콘텐츠에 대한 XSS 공격이나 다른 공격을 검사하지 않습니다.
신뢰할 수 없는 HTML 콘텐츠에 이 메소드를 적용하려는 경우,
`clean()`을 먼저 호출한 후에 이 메소드를 호출하는 것을 권장합니다.

#### getHTMLPurifier()

```
public static function getHTMLPurifier(?array $allowed_classes = null): HTMLPurifier
```

내부적으로 사용하는 HTMLPurifier 인스턴스를 반환합니다.
