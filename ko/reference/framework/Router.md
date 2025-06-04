Rhymix\Framework\Router
-----------------------

#### getRewriteLevel()

```
public static function getRewriteLevel(): int
```

Return the currently configured rewrite level.
0 = None
1 = XE-compatible rewrite rules only
2 = Full rewrite support

#### parseURL()

```
public static function parseURL(
    string $method,
    string $url,
    int $rewrite_level
): Rhymix\Framework\Request
```

Extract request arguments from the current URL.

#### getURL()

```
public static function getURL(
    array $args,
    int $rewrite_level
): string
```

Create a URL for the given set of arguments.
