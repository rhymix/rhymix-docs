Rhymix\Framework\Filters\MediaFilter
------------------------------------

#### addPrefix()

```
public static function addPrefix(
    string $prefix,
    bool $permanently = false
): void
```

Add a prefix to the iframe whitelist.

#### addIframePrefix()

```
public static function addIframePrefix(
    string $prefix,
    bool $permanently = false
): void
```

Add a prefix to the object whitelist.

#### addObjectPrefix()

```
public static function addObjectPrefix(): void
```

Add a prefix to the object whitelist.

#### formatPrefix()

```
public static function formatPrefix(string $prefix): string
```

Format a prefix for standardization.

#### getWhitelist()

```
public static function getWhitelist(): array
```

Get the iframe whitelist.

#### getWhitelistRegex()

```
public static function getWhitelistRegex(): string
```

Get the iframe whitelist as a regular expression.

#### matchWhitelist()

```
public static function matchWhitelist(string $url): bool
```

Check if a URL matches the iframe whitelist.

#### removeEmbeddedMedia()

```
public static function removeEmbeddedMedia(
    string $input,
    string $replacement = ''
): string
```

Remove embedded media from HTML content.

#### getIframeWhitelist()

```
public static function getIframeWhitelist(): array
```

Get the iframe whitelist.

#### getIframeWhitelistRegex()

```
public static function getIframeWhitelistRegex(): string
```

Get the iframe whitelist as a regular expression.

#### matchIframeWhitelist()

```
public static function matchIframeWhitelist(string $url): bool
```

Check if a URL matches the iframe whitelist.

#### getObjectWhitelist()

```
public static function getObjectWhitelist(): array
```

Get the object whitelist.

#### getObjectWhitelistRegex()

```
public static function getObjectWhitelistRegex(): string
```

Get the object whitelist as a regular expression.

#### matchObjectWhitelist()

```
public static function matchObjectWhitelist(string $url): bool
```

Check if a URL matches the iframe whitelist.
