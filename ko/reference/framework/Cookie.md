Rhymix\Framework\Cookie
-----------------------

#### get()

```
public static function get(string $name): ?string
```

Get a cookie.

#### set()

```
public static function set(
    string $name,
    string $value,
    array $options = []
): bool
```

Set a cookie.
Options may contain the following keys:
- expires (days or Unix timestamp)
- path
- domain
- secure
- httponly
- samesite
Missing options will be replaced with Rhymix security configuration
where applicable, e.g. secure and samesite.

#### remove()

```
public static function remove(
    string $name,
    array $options = []
): bool
```

Delete a cookie.
You must pass an options array with the same values that were used to
create the cookie, except 'expires' which doesn't apply here.
