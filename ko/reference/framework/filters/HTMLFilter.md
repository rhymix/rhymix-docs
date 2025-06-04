Rhymix\Framework\Filters\HTMLFilter
-----------------------------------

#### prependPreFilter()

```
public static function prependPreFilter(callable $callback): void
```

Prepend a pre-processing filter.

#### appendPreFilter()

```
public static function appendPreFilter(callable $callback): void
```

Append a pre-processing filter.

#### prependPostFilter()

```
public static function prependPostFilter(callable $callback): void
```

Prepend a post-processing filter.

#### appendPostFilter()

```
public static function appendPostFilter(callable $callback): void
```

Append a post-processing filter.

#### clean()

```
public static function clean(
    string $input,
    $allow_classes = false,
    bool $allow_editor_components = true,
    bool $allow_widgets = false
): string
```

Filter HTML content to block XSS attacks.

#### fixRelativeUrls()

```
public static function fixRelativeUrls(string $content): string
```

Convert relative URLs to absolute URLs in HTML content.
This is useful when sending content outside of the website,
such as e-mail and RSS, where relative URLs might not mean the same.
This method also removes attributes that don't mean anything
when sent outside of the website, such as editor component names.
This method DOES NOT check HTML content for XSS or other attacks.

#### getHTMLPurifier()

```
public static function getHTMLPurifier(?array $allowed_classes = null): HTMLPurifier
```

Get an instance of HTMLPurifier.
