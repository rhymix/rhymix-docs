Rhymix\Framework\Filters\MediaFilter
------------------------------------

`HTMLFilter`에서 허용되는 `<iframe>` 외부 미디어의 URL 화이트리스트를 관리하는 클래스입니다.

### 현재 지원하는 메소드

#### addPrefix()

```
public static function addPrefix(
    string $prefix,
    bool $permanently = false
): void
```

특정한 URL 접두사를 동적으로 화이트리스트에 추가합니다.
시스템 설정을 영구적으로 변경하지 않고 임시로 특정 도메인을 허용할 수 있습니다.

URL 접두사 추가 후 시스템 설정을 영구적으로 변경하려면
`$permanently` 매개변수를 `true`로 지정합니다.

URL 접두사는 `http://`, `https://` 등을 포함하지 않아야 하고,
허용할 URL이 여러 가지라면 그 중 공통되는 부분까지만 지정해야 합니다.
예를 들어 YouTube 동영상 삽입 코드의 공통되는 접두사 중 하나는 `www.youtube.com/embed/`입니다.

#### formatPrefix()

```
public static function formatPrefix(string $prefix): string
```

일반적인 URL을 위에서 설명한 접두사 형식으로 변환합니다.
`http://`, `https://` 등을 제거하는 기본적인 작업만 수행하므로,
불필요한 내용이 포함되었다면 직접 수정해야 합니다.

#### getWhitelist()

```
public static function getWhitelist(): array
```

현재 적용된 화이트리스트 배열을 반환합니다.

#### getWhitelistRegex()

```
public static function getWhitelistRegex(): string
```

현재 적용된 화이트리스트를 한 번에 매칭할 수 있는 정규표현식(regular expression)을 반환합니다.
이 정규표현식을 `preg_match()`와 같은 함수에 사용할 수 있습니다.

#### matchWhitelist()

```
public static function matchWhitelist(string $url): bool
```

주어진 URL이 현재 화이트리스트의 적용을 받는지 확인합니다.

#### removeEmbeddedMedia()

```
public static function removeEmbeddedMedia(
    string $input,
    string $replacement = ''
): string
```

주어진 HTML 문자열에서 모든 `<object>` 및 `<embed>` 태그를 `$replacement`로 치환합니다.
XE 1.x에서 `<object>` 태그와 비슷한 역할을 하던 `multimedia_link` 에디터 컴포넌트도 제거합니다.
URL 접두사에 따라 허용 여부를 판단하는 기능은 수행하지 않고, 일괄 치환만 지원합니다.

이러한 태그를 제거하는 기능이 존재하는 이유는 `<iframe>`과 `<object>`를 별도로 허용하던 XE 1.x와 달리,
라이믹스는 현대의 추세에 맞추어 모든 외부 미디어를 `<iframe>`으로 삽입한다고 간주하기 때문입니다.

### 레거시 메소드

아래는 `<iframe>`과 `<object>`를 각각 필터링하던 과거의 방식에서 사용하던 메소드들입니다.
현재 라이믹스에서는 `<iframe>`을 기준으로 작성된 하나의 화이트리스트를 모든 외부 미디어에 동일하게 적용하므로,
태그에 따라 구분하는 아래의 메소드들은 하위 호환을 위한 alias로만 제공하고 있습니다.

#### addIframePrefix()

```
public static function addIframePrefix(
    string $prefix,
    bool $permanently = false
): void
```

이 메소드는 `addPrefix()`의 alias입니다.

#### addObjectPrefix()

```
public static function addObjectPrefix(): void
```

이 메소드는 동작하지 않습니다.
(`<object>`를 위한 화이트리스트가 따로 존재하지 않으므로, 추가할 수 없습니다.)

#### getIframeWhitelist()

```
public static function getIframeWhitelist(): array
```

이 메소드는 `getWhitelist()`의 alias입니다.

#### getIframeWhitelistRegex()

```
public static function getIframeWhitelistRegex(): string
```

이 메소드는 `getWhitelistRegex()`의 alias입니다.

#### matchIframeWhitelist()

```
public static function matchIframeWhitelist(string $url): bool
```

이 메소드는 `matchWhitelist()`의 alias입니다.

#### getObjectWhitelist()

```
public static function getObjectWhitelist(): array
```

이 메소드는 `getWhitelist()`의 alias입니다.

#### getObjectWhitelistRegex()

```
public static function getObjectWhitelistRegex(): string
```

이 메소드는 `getWhitelistRegex()`의 alias입니다.

#### matchObjectWhitelist()

```
public static function matchObjectWhitelist(string $url): bool
```

이 메소드는 `matchWhitelist()`의 alias입니다.
